---
title: "CertBot申请证书"
date: 2026-04-13T10:50:32+08:00
draft: false
categories:
 - Linux
tags:
 - certbot
 - Cloudflare
featured_image: 
---

1、安装certbot
```
apt install certbot python3-certbot-dns-cloudflare
```
<!--more-->
2、设置cloudflare Token

```
vi .cerbot

# cloudflare token
dns_cloudflare_api_token = your_cloudflare_token

chmod 600 .cerbot
```

3、申请证书

```
certbot certonly --dns-cloudflare --dns-cloudflare-credentials ~/.certbot -d example.com
```

4、部署脚本

```
#!/bin/bash

# 定义目标域名和目标目录
DOMAIN="example.com"  # 替换为你的域名
DEST_DIR="/path/to/destination"  # 替换为目标目录

# 检查目标目录是否存在，不存在则创建
if [ ! -d "$DEST_DIR" ]; then
    mkdir -p "$DEST_DIR"
fi

# 从 Certbot 的默认目录复制证书文件到目标目录
cp "/etc/letsencrypt/live/$DOMAIN/privkey.pem" "$DEST_DIR/privkey.pem"
cp "/etc/letsencrypt/live/$DOMAIN/fullchain.pem" "$DEST_DIR/fullchain.pem"

# 设置目标目录中的文件权限
chmod 600 "$DEST_DIR/privkey.pem" "$DEST_DIR/fullchain.pem"

echo "证书已成功复制到 $DEST_DIR"
```

6、调用脚本 
```
certbot renew --deploy-hook "/path/to/your/script.sh"
```
