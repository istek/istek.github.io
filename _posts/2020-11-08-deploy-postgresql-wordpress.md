---
title: 部署WordPress+PostgreSQL
date: 2020-11-08 09:24:35
type: post
category:
 - Linux
tag:
 - WordPress
---

想起来PostgreSQL这么优秀的开源数据库，难道Wordpress不支持吗？于是乎一番搜索，发现Wordpress原生是不支持PostgreSQL的，只能通过插件的形式更换数据库，这个插件名叫[PG4WP](https://github.com/gmercey/PG4WP "PG4WP")。

## 准备工作

首先，备份数据库，并且，导出XML文件，XML文件是用来在新安装的wordpress中导入的，因为mysql导出的SQL，无法直接用到PostgreSQL的。

导出完成后，reinstall操作系统吧，装好后，可以使用oneinstack之类的脚本，或者，自己安装PostgreSQL，php，nginx这些，还比脚本之类的快一点，效率为上，直接手工apt安装即可。

域名提前做好解析。

## 1. 安装必需的库

```bash
apt install curl wget gnupg2 ca-certificates lsb-release socat git
```

## 2. 安装PostgreSQL

```bash
# 引入postgreSQL官方源
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# 导入密钥签名:
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

# 更新软件包库:
apt-get update

# 安装PostgreSQL.
# 如果要安装特定版本，那就用'postgresql-12' 或者 'postgresql':
apt-get -y install postgresql
```

## 3. 修改PostgreSQL本地认证

```bash
vi /etc/postgresql/12/main/pg_hba.conf

//对于local的认证修改为md5

local   all             all                                     md5

```

## 4. 启动数据库

```bash
systemctl enable postgresql
systemctl start postgresql
```

## 5.安装php

```bash
apt install php7.3 php7.3-curl php7.3-fpm php7.3-gd php7.3-json php7.3-mbstring \
php7.3-mysql php7.3-opcache php7.3-pgsql php7.3-xml php7.3-xmlrpc php7.3-zip
```

## 6. 安装nginx

```bash
# 添加nginx官方源
echo "deb http://nginx.org/packages/debian `lsb_release -cs` nginx" \
    | tee /etc/apt/sources.list.d/nginx.list

# 导入源签名密钥
curl -fsSL https://nginx.org/keys/nginx_signing.key | apt-key add -

# 更新软件仓库
apt update

# 安装nginx
apt install nginx
```

## 7. 安装acme.sh

```bash
curl  https://get.acme.sh | sh
```

## 8. 生成dhparam文件

```bash
mkdir /etc/nginx/ssl
openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048
```

## 9. 创建普通用户

```bash
useradd -m -d /web www
mkdir /web/{wwwroot,wwwlogs}
chown -R www.www /web
```

## 10. 修改配置文件

### nginx配置文件

```ini
user  www www;

worker_processes auto;
worker_cpu_affinity auto;

error_log  /var/log/nginx/nginx_error.log  crit;

pid        /var/run/nginx.pid;

#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 51200;

events
    {
        use epoll;
        worker_connections 51200;
        multi_accept off;
        accept_mutex off;
    }

http
    {
        include       mime.types;
        default_type  application/octet-stream;

        server_names_hash_bucket_size 128;
        client_header_buffer_size 32k;
        large_client_header_buffers 4 32k;
        client_max_body_size 50m;

        sendfile on;
        sendfile_max_chunk 512k;
        tcp_nopush on;

        keepalive_timeout 60;

        tcp_nodelay on;

        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
        fastcgi_buffer_size 64k;
        fastcgi_buffers 4 64k;
        fastcgi_busy_buffers_size 128k;
        fastcgi_temp_file_write_size 256k;

        gzip on;
        gzip_min_length  1k;
        gzip_buffers     4 16k;
        gzip_http_version 1.1;
        gzip_comp_level 2;
        gzip_types     text/plain application/javascript application/x-javascript text/javascript text/css application/xml application/xml+rss;
        gzip_vary on;
        gzip_proxied   expired no-cache no-store private auth;
        gzip_disable   "MSIE [1-6]\.";

        #limit_conn_zone $binary_remote_addr zone=perip:10m;
        ##If enable limit_conn_zone,add "limit_conn perip 10;" to server section.

        server_tokens off;
        access_log off;

	include /etc/nginx/conf.d/*.conf;
}
```

### php.conf

```
vi /etc/nginx/php.conf
```

```nginx
location ~ [^/]\.php(/|$)
        {
            fastcgi_pass  unix:/run/php/php7.3-fpm.sock;
            fastcgi_index index.php;
            include fastcgi_params;
            fastcgi_split_path_info ^(.+?\.php)(/.*)$;
            set $path_info $fastcgi_path_info;
            fastcgi_param PATH_INFO       $path_info;
            try_files $fastcgi_script_name =404;
        }
```

### `/etc/nginx/fastcgi_params`文件

新增一行在最后一行

```nginx
fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
```

### php-fpm

编辑`/etc/php/7.3/fpm/pool.d/www.conf`文件，使用如下配置即可

```ini
[www]
listen = /run/php/php7.3-fpm.sock
listen.backlog = -1
listen.allowed_clients = 127.0.0.1
listen.owner = www
listen.group = www
listen.mode = 0666
user = www
group = www
pm = dynamic
pm.max_children = 10
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 6
pm.max_requests = 1024
pm.process_idle_timeout = 10s
request_terminate_timeout = 100
request_slowlog_timeout = 0
slowlog = /var/log/slow.log
```

## 8. 启动nginx，php-fpm

```bash
systemctl enable php7.3-fpm
systemctl enable nginx
systemctl restart php7.3-fpm
systemctl restart nginx
```

## 9. 签发ssl证书

```bash
source .bashrc
acme.sh --issue -d www.mydomain.com --standalone
```

如无意外，应该会看到签发的证书的具体位置，例如`/root/.acme.sh/www.mydomain.com/fullchain.cer`和`/root/.acme.sh/www.mydomain.com/www.mydomain.com.key`。但是我们还需要将证书安装到nginx的ssl目录，而不是直接使用上面目录的证书，会有权限问题，再者目录会发生变化。

```bash
acme.sh --install-cert -d www.example.com \
--key-file       /etc/nginx/ssl/www.mydomain.com.key \
--fullchain-file /etc/nginx/ssl/fullchain.cer \
--reloadcmd     "service nginx force-reload"
```

## 10. 给nginx增加虚拟主机

新建配置文件 `vi /etc/nginx/conf.d/mydomain.conf`

```nginx
server
        {
        listen          80;
        server_name www.mydomain.com mydomain.com;
        return 301 https://www.mydomain.com$request_uri;
}

server
        {
        listen 443 ssl http2;
        #listen [::]:443 ssl http2;
        server_name     www.mydomain.com mydomain.com;
        index index.html index.htm index.php default.html default.htm default.php;
        root  /web/wwwroot/www.mydomain.com;

    location / {
        try_files $uri $uri/ /index.php?$args;
        index  index.php;
    }

        ssl_certificate /etc/nginx/ssl/fullchain.cer;
        ssl_certificate_key /etc/nginx/ssl/www.mydomain.com.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers "TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
        ssl_session_cache builtin:1000 shared:SSL:10m;
        ssl_dhparam /etc/nginx/ssl/dhparam.pem;

        #error_page   404   /404.html;

        # Deny access to PHP files in specific directory
        location ~ /(wp-content|uploads|wp-includes|images)/.*\.php$ { deny all; }

        include php.conf;

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires      30d;
        }

        location ~ .*\.(js|css)?$
        {
            expires      12h;
        }

        location ~ /.well-known {
            allow all;
        }

        location ~ /\.
        {
            deny all;
        }

        access_log  /web/wwwlogs/mydomain.log;
}
```

## 11. 测试虚拟主机是否工作正常

新建一个phpinfo测试脚本`vi /web/wwwroot/www.mydomain.com/t.php`

```php
<?php3
    phpinfo();
?>
```

用浏览器打开 <http://www.mydomain.com/t.php> 验证

- 是否如我们期待跳转到https
- 是否能解析php脚本

如无意外，你应该能看到地址栏的小锁子了，并且PHP成功解析。

![file](https://i.loli.net/2020/11/07/XkQfMKvlWwd6DEy.png)

## 12. 为Wordpress创建数据库

```bash
su - postgres
psql
postgres=# create database your_database;
postgres=# create user your_user with password ‘your_password’;
postgres=# grant all privileges on database your_database to your_user;
postgres=# \q
```

OK. 至此数据已经成功创建了。现在你就可以试试登录数据库。

```bash
psql -U your_user -W
Password:
psql (12.4 (Debian 12.4-1.pgdg100+1))
Type "help" for help.

your_database=>
your_database=> \q
```

## 13. 下载最新的wordpress版本

```bash
cd /web/wwwroot/www.mydomain.com
wget https://wordpress.org/latest.tar.gz -O - | tar -zvx
mv wordpress/* ./
rm -fr wordpress
```

## 14. 安装pg4wg插件

```bash
cd /web/wwwroot/www.mydomain.com/wp-content
git clone https://github.com/gmercey/PG4WP.git
cp PG4WP/db.php ./db.php
```

## 15. 调整目录和文件的权限

因为nginx和php-fpm都是以www用户权限跑的，所以需要将web目录下的文件和目录，调整权限，否则无法安装，更新插件，主题等。

```bash
chown -R www.www /web
```

## 16.配置wordpress数据库信息

默认会有一份wp-config-sample.php的文件，cp一份作为我们配置的起点。

```bash
vi wp-config-sample.php wp-config.php
```

修改如下内容：

```php
/** The name of the database for WordPress */
define('DB_NAME', 'your PostgreSQL database name will go here');
/** MySQL database username */
define('DB_USER', 'your PostgreSQL database username will go here');
/** MySQL database password */
define('DB_PASSWORD', 'your PostgreSQL database password will go here');
/** MySQL hostname */
define('DB_HOST', 'localhost');
```

## 17. 安装wordpress

用浏览器打开`https://www.mydomain.com/wp-admin/install.php`启动wordpress安装程序。

> 需要注意一下，一定要安装php-mysql 这个扩展，如果不装，wordpress安装开始就报错了。

## 18. 导入xml

安装完成后，就开始导入吧！


**全文结束**