---
title: 'BuyPass 免费域名 SSL 证书ACME API申请方法'
date: 2022-08-01 16:24:35
type: post
category:
 - Linux
tags:
 - "Let's Encrypt"
---

BuyPass 是挪威的一家 CA 机构，提供数字证书、安全认证产品等多种服务。目前 BuyPass 提供了类似 Let's Encrypt 的基于 ACME/Certbot 的证书自动签发服务 - BuyPass GO Free SSL，免费！

与 Let's Encrypt 主要的不同点在于 BuyPass 证书每次签发有效期是 <strong>180 天</strong>（ Let's Encrypt 是 90 天）。另外 BuyPass 不支持签发泛域名证书，但支持多域名。

由于目前 Let's Encrypt OCSP 存在问题（ocsp.int-x3.letsencrypt.org 被干扰），严重拖慢网站速度，因此可以考虑替换为 BuyPass。
<!--more-->
## 利用 ACME 申请

### 下载 ACME
```
curl https://get.acme.sh | bash
```

### 注册账号
```
./acme.sh --server https://api.buypass.com/acme/directory --register-account -m 'YOUR_EMAIL'
```

### 申请证书

可以选择手动 DNS 验证：

```
./acme.sh --server https://api.buypass.com/acme/directory --issue -d vircloud.net -d www.vircloud.net --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please
//添加完 DNS 验证后执行
./acme.sh --server https://api.buypass.com/acme/directory --renew -d vircloud.net -d www.vircloud.net --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please
```

也可以选择自动 DNS 验证（以 CloudFlare 为例）：[关于这部分的信息，可以参与[DNSAPI](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)]

```
export CF_Key="****"   #填写 CloudFlare 的 Global API Key
export CF_Email="root@example.com" #填写 CloudFlare 注册邮箱
./acme.sh --server https://api.buypass.com/acme/directory --issue -d vircloud.net -d www.vircloud.net --dns dns_cf
```
或者 HTML 验证：
```
./acme.sh --server https://api.buypass.com/acme/directory --issue -d vircloud.net -d www.vircloud.net --webroot  /var/www/example.com/public_html/
```

### 吊销证书

BuyPass 支持手动吊销证书，命令如下：

```
acme.sh --revoke -d vircloud.net -d www.vircloud.net --server 'https://api.buypass.com/acme/directory'
```

## Certbot申请

### certbot 安装

```
apt install certbot
```

### 注册账号

```
certbot register -m 'YOUR_EMAIL' --agree-tos --server 'https://api.buypass.com/acme/directory'
```

### 申请证书

可以选择DNS验证

```
certbot certonly --standalone --email 'YOUR_EMAIL' -d vircloud.net -d www.vircloud.net --server 'https://api.buypass.com/acme/directory'
```

也可以选择网页HTML验证

```
certbot certonly --webroot -w /var/www/example.com/public_html/ -d vircloud.net -d www.vircloud.net --server 'https://api.buypass.com/acme/directory'
```

然后根据提示，到对应的目录就可以找到证书。

### 自动续签

如果申请证书时选择了 HTML 验证，那么就可以实现自动续签。

手动执行续签：
```
certbot renew
```

计划任务续签
```
crontab -e

30 */12 * * * /usr/bin/certbot renew -n -q >> /var/log/cerbot-renew.log
```

