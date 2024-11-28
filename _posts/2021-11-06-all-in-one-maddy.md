---
title: 'Maddy：多合一邮件服务器'
date: 2021-11-06 20:24:35
type: post
category:
 - Linux
tags:
 - Maddy
---

我自己有几台VPS，平时都是吃灰状态，或者，除了翻墙，也再没做什么用途，感觉有点浪费，偶然，我发现了这个Maddy这个好东西，免去了自己配置postfix，dovecat等等之类的服务，仅仅需要配置一个文件就可以了。是不是很容易？

而且，还可以让自己的VPS发挥一点作用，对于长期持有的域名，真的不失为一个很好的用途，你们说呢？

> Maddy Mail Server implements all functionality required to run a e-mail server. It can send messages via SMTP (works as MTA), accept messages via SMTP (works as MX) and store messages while providing access to them via IMAP. In addition to that it implements auxiliary protocols that are mandatory to keep email reasonably secure (DKIM, SPF, DMARC, DANE, MTA-STS).
>It replaces Postfix, Dovecot, OpenDKIM, OpenSPF, OpenDMARC and more with one daemon with uniform configuration and minimal maintenance cost.

[Maddy](https://github.com/foxcpp/maddy)是一款用Go语言开发的邮件服务器，它实现了运行电子邮件服务器所需的所有功能。

Maddy用一个具有统一配置和最低维护成本的守护进程取代了Postfix、Dovecot、OpenDKIM、OpenSPF、OpenDMARC 等程序。
<!--more-->

## 准备工作

- 一台25端口没有被封的VPS
- 一个域名
- Maddy程序，最新版本：[0.5.2](https://github.com/foxcpp/maddy/releases/tag/v0.5.2)

## 域名配置

登录你的域名控制面板，新增几条DNS解析记录，这里我以CloudFlare为例。

**注意：以下例子中的IP和域名请替换为你自己的！**

首先添加一条A记录，指向你的VPS，名称这里以mail为例。

![](https://cdn.jsdelivr.net/gh/istek/img/202111061951559.png)

然后，添加一条MX记录，指向上一步的那个域名

![](https://cdn.jsdelivr.net/gh/istek/img/202111061955146.png)

再添加一条TXT记录，名称`_dmarc`，记录值：`v=DMARC1; p=none; rua=mailto:webmaster@example.com`

![](https://cdn.jsdelivr.net/gh/istek/img/202111062011695.png)

再添加一条TXT记录，名称`@`，记录值：`v=spf1 mx ~all`

![](https://cdn.jsdelivr.net/gh/istek/img/202111062014380.png)

DNS配置完了，先别关闭浏览器窗口，最后还要增加一个DKIM的TXT记录。

## 安装前检查

务必确认VPS的`25`，`465`，`587`，`993`，`143`端口没有被占用以及这些端口有没有被服务器商家屏蔽。

有一些VPS的DEBIAN10 默认安装了exim4，需要卸载掉。

```
apt remove exim4
apt autoremove
```

## 安装Maddy

首先，安装必需的软件包

```
apt -y update
apt -y install acl zstd wget certbot
```

下载最新的maddy包并解压

```
wget https://hub.fastgit.org/foxcpp/maddy/releases/download/v0.5.2/maddy-0.5.2-x86_64-linux-musl.tar.zst
unzstd maddy-0.5.2-x86_64-linux-musl.tar.zst
tar -xvf maddy-0.5.2-x86_64-linux-musl.tar
cd maddy-0.5.2-x86_64-linux-musl/
```

创建需要用到的目录，复制相应的文件到对应的目录：

```
mkdir /etc/maddy
cp maddy.conf /etc/maddy
cp maddy maddyctl /usr/local/bin
cp systemd/*.service /etc/systemd/system
```

新增maddy用户，主要是为了跑maddy服务，请不要使用root用户跑服务！

```
useradd -mrU -s /sbin/nologin -d /var/lib/maddy -c "maddy mail server" maddy
```

## 申请SSL证书

```
certbot certonly --standalone --agree-tos --no-eff-email --email xxxxx@example.com -d mail.example.com
```

配置 ACL权限，允许maddy用户读取证书及证书私钥。

```
setfacl -R -m u:maddy:rx /etc/letsencrypt/{live,archive}
```

## 配置maddy

```
nano /etc/maddy/maddy.conf
```

修改如下的配置

```ini
$(hostname) = mail.example.com
$(primary_domain) = example.com
$(local_domains) = $(primary_domain)

tls file /etc/letsencrypt/live/mail.example.com/fullchain.pem /etc/letsencrypt/live/mail.example.com/privkey.pem
```

## 启动maddy

启动并设置开机自启动服务
```
systemctl enable --now maddy.service
```

检查服务状态

```
systemctl status maddy.service
```

## 配置DKIM

在第一次成功启动maddy后，maddy会生成dkim密钥文件，需要在DNS解析配置一下。

```
cat /var/lib/maddy/dkim_keys/example.com_default.dns
```

内容类似如下：

```
v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAx9mN47......
```

打开Cloudflare新增一条TXT记录，名称`default._domainkey`，记录值就是`/var/lib/maddy/dkim_keys/example.com_default.dns`的内容。

![](https://cdn.jsdelivr.net/gh/istek/img/202111062111627.png)

## 增加邮箱用户

先增加一个邮箱用户

```
maddyctl creds create webmaster@example.com
```

还需要创建一个imap本地存储账户与之关联，电子邮件的地址务必与用户账号的地址保持一致：

```
maddyctl imap-acct create webmaster@example.com
```

## 安装完成 

## 附：EMAIL客户端配置

用户名：webmaster.com
密码：（增加邮箱账号时新建的密码）
IMAP: mail.example.com:993 SSL/TLS
SMTP: mail.example.com:465 SSL/TLS
