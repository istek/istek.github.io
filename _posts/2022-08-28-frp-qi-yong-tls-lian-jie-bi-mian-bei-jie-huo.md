---
title: 'frp启用TLS连接，避免被截获'
date: 2022-08-28 16:24:35
type: post
category:
 - Linux
tags:
 - frp
---

frp自版本0.25.0后，就支持 frpc 和 frps 之间使用TLS协议。
为了端口复用，frp 会发送第一个字节“0x17”以创建 TLS 连接。
在`[common]`配置小节增加`tls_enable = true`后，就可以启用该特性。
若强制frps只接受TLS连接，则需要在`[common]`小节配置`tls_only = true`。

You will need **a root CA cert** and **at least one SSL/TLS certificate**. It **can** be self-signed or regular (such as Let's Encrypt or another SSL/TLS certificate provider).

If you using `frp` via IP address and not hostname, make sure to set the appropriate IP address in the Subject Alternative Name (SAN) area when generating SSL/TLS Certificates.

需要注意的点：

创建frps证书时，注意vps_ip和vps_domain两个参数，必须是真实的VPSip和域名。
<!--more-->
## 准备openssl配置文件

一般情况下，该配置文件默认位置是`/etc/pki/tls/openssl.cnf`，如果找不到该文件可以使用如下配置文件。

```bash
cat > my-openssl.cnf << EOF
[ ca ]
default_ca = CA_default
[ CA_default ]
x509_extensions = usr_cert
[ req ]
default_bits        = 2048
default_md          = sha256
default_keyfile     = privkey.pem
distinguished_name  = req_distinguished_name
attributes          = req_attributes
x509_extensions     = v3_ca
string_mask         = utf8only
[ req_distinguished_name ]
[ req_attributes ]
[ usr_cert ]
basicConstraints       = CA:FALSE
nsComment              = "OpenSSL Generated Certificate"
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid,issuer
[ v3_ca ]
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints       = CA:true
EOF
```


## 创建ca证书

```bash
openssl genrsa -out ca.key 2048
openssl req -x509 -new -nodes -key ca.key -subj "/CN=example.ca.com" -days 5000 -out ca.crt
```

## 创建frps服务端证书

```bash
openssl genrsa -out server.key 2048

openssl req -new -sha256 -key server.key \
    -subj "/C=XX/ST=DEFAULT/L=DEFAULT/O=DEFAULT/CN=server.com" \
    -reqexts SAN \
    -config <(cat my-openssl.cnf <(printf "\n[SAN]\nsubjectAltName=DNS:localhost,IP:VPS_IP,DNS:vps_domain")) \
    -out server.csr

openssl x509 -req -days 365 -sha256 \
	-in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
	-extfile <(printf "subjectAltName=DNS:localhost,IP:VPS_IP,DNS:vps_domain") \
	-out server.crt
```

## 创建frpc客户端证书
```bash
openssl genrsa -out client.key 2048

openssl req -new -sha256 -key client.key \
    -subj "/C=XX/ST=DEFAULT/L=DEFAULT/O=DEFAULT/CN=client.com" \
    -reqexts SAN \
    -config <(cat my-openssl.cnf <(printf "\n[SAN]\nsubjectAltName=DNS:client.com,DNS:example.client.com")) \
    -out client.csr

openssl x509 -req -days 365 -sha256 \
    -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
	-extfile <(printf "subjectAltName=DNS:client.com,DNS:example.client.com") \
	-out client.crt
```

## 更新ca证书

**Linux**
```bash
cp ca.crt /usr/local/share/ca-certificates

update-ca-certificates
```

**Windows**
将ca证书导入受信任的根证书机构即可。

## frps 服务端配置文件样例

```ini
[common]
bind_addr = 0.0.0.0
bind_port = 7000
log_level = info
log_max_days = 3
log_file = /var/log/frp/frp.log

authentication_method=token
token=123456789

max_pool_count = 2000
allow_ports = 2000-50000

tls_only = true
tls_cert_file = /usr/local/frp/server.crt
tls_key_file = /usr/local/frp/server.key
tls_trusted_ca_file = /usr/local/frp/ca.crt
```

frpc客户端配置文件样例

```ini
[common]
server_addr=vps_ip
server_port=vps_frps_端口
protocol=tcp
token=123456789
log_file = frpc.log
pool_count = 500
tls_enable = true
tls_cert_file = "D:\ProgramFiles\frp\tls\xzcu.crt"
tls_key_file = "D:\ProgramFiles\frp\tls\xzcu.key"
tls_trusted_ca_file = "D:\ProgramFiles\frp\tls\ca.crt"

[tcpsrv]
type=tcp
local_ip = 127.0.0.1
local_port = 8899
remote_port= 12345
use_compression = true
```
