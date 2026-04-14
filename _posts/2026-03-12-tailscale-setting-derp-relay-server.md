---
title: "Tailscale-自建Derp中继服务器"
date: 2026-03-12T15:29:32+08:00
draft: false
categories:
 - Linux
tags:
 - Tailscale
featured_image: 
---

# 1. Tailscale 与 DERP 简介
## Tailscale 是什么？

Tailscale 是一个基于 Wireguard 的带有多种网络工具的 P2P 组网工具。得益于其 P2P 的特性，Tailscale 还可以进行内网穿透，打破 NAT 的限制直达另一台主机。

## DERP 是什么？

`DERP`是一个 Tailscale 自行开发的中继服务。当所处网络环境难以穿透（如校园网、移动大内网、4G、5G等）时，所有流量都会经由 DERP 中转至目标地址。
<!--more-->
![](https://www.xiaobaby.eu.org/file/AgACAgEAAyEGAASQfha-AAMiadyLT3V30EecRdb7XfoEQCsQDdEAAhwMaxvWRuhGoCoF6d_EjY0BAAMCAAN4AAM7BA.png)

在默认情况下，Tailscale 官方已经提供了环大陆的官方 DERP 服务（见下方`图 1`），但是由于中国大陆的网络连通性等问题，官方并未提供大陆的 DERP 节点。为了确保大陆的打通成功率，我们需要自建一个 DERP 服务，来帮助我们“打洞”。

![](https://www.xiaobaby.eu.org/file/AgACAgEAAyEGAASQfha-AAMjadyLm5x5gFVxjYxywD5oncBmt7oAAh0MaxvWRuhGXAABKQqYeC4IAQADAgADeAADOwQ.png)


# 2. 安装 Docker 与 Docker compose

这段请自行参考 Docker 官方网站与其他作者写的部署教程，毕竟每个发行版都不尽相同。
```
curl -fsSL https://get.docker.com | bash -s docker

或者

curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

# 3. 在 DERP 节点服务中部署 Tailscale 客户端
先安装

```
curl -fsSL https://tailscale.com/install.sh | sh
```
登录

```
tailscale login
```

# 4. 启动 Docker 镜像

可用的docker-compose.yaml

```
services:  
  derper:
    image: fredliang/derper
    restart: always
    ports:
      - 13443:443
      - 3478:3478/udp
    environment:
      DERP_DOMAIN: "derp.example.com"
      DERP_CERT_MODE: "manual"
      DERP_ADDR: ":443"
      DERP_VERIFY_CLENTS: "true"
    volumes:
      - /var/run/tailscale/tailscaled.sock:/var/run/tailscale/tailscaled.sock
      - /home/derp:/app/certs
```

注意：本地路径 /home/derp 保存与域名相同名称的ssl证书和私钥，例如`derp.example.com.cer`,`derp.example.com.key`。

# 5. 配置 tailscale ACL 

```
	"derpMap": {
		"OmitDefaultRegions": false,//这里改成true就是只用自己的derp互联，建议不改，如果要跨网可以改成true
		"Regions": {
			"900": {
				"RegionID":   900,
				"RegionCode": "85",
				"RegionName": "HK",
				"Nodes": [{
					"Name":     "myderp",
					"RegionID": 900,
					"HostName": "derp.example.com",
					"DERPPort": 13443,
					"STUNPort": 3478 //这里改成stun的端口
				}],
			},
		},
	},
```
# 6. 测试
重新连接Tailscale，然后测试
```
tailscale netcheck
```
