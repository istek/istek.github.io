---
title: "Cloudflare Tunnels无法建立隧道的解决办法"
date: 2024-11-26T20:50:32+08:00
draft: false
categories:
 - Linux
tags:
 - Cloudflare
featured_image: 
---

今天在使用CloudFlare的tunnel，给NAS搞一个公网访问，结果发现从NAS里面运行docker，Cloudflare客户端无法建立连接，错误如下：

```
2024-11-26T09:05:39Z ERR Failed to dial a quic connection error="failed to dial to edge with quic: timeout: no recent network activity" connIndex=0 event=0 ip=198.18.0.50
2024-11-26T09:05:39Z INF Retrying connection in up to 2s connIndex=0 event=0 ip=198.18.0.50
2024-11-26T09:05:45Z ERR Failed to dial a quic connection error="failed to dial to edge with quic: timeout: no recent network activity" connIndex=0 event=0 ip=198.18.0.49
2024-11-26T09:05:45Z INF Retrying connection in up to 4s connIndex=0 event=0 ip=198.18.0.49
2024-11-26T09:05:54Z ERR Failed to dial a quic connection error="failed to dial to edge with quic: timeout: no recent network activity" connIndex=0 event=0 ip=198.18.0.50
2024-11-26T09:05:54Z INF Retrying connection in up to 8s connIndex=0 event=0 ip=198.18.0.50
```
<!--more-->
网上查询了一下，大家的说法是NAT，防火墙，再就是客户端的协议方式，最后查了一下自己的网络，排除了NAT，防火墙问题，于是就按照下面的方式修改协议，在docker命令中增加`--protocol http2`即可，此时强制指定协议为http2，使用的是TCP，这样就不会被运营商阻断了。当然，也可以设置为--protocol auto ，开启自动切换，默认依然是quic，但是失败后可自动切换到http2。

```
sudo docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --protocol http2 --token <YOUR_TOKEN>
```

心想这样应该就可以了吧，结果又报了下面的错误：

```
2024-11-26T09:11:24Z ERR Unable to establish connection with Cloudflare edge error="TLS handshake with edge error: EOF" connIndex=0 event=0 ip=198.18.0.49
2024-11-26T09:11:24Z ERR Serve tunnel error error="TLS handshake with edge error: EOF" connIndex=0 event=0 ip=198.18.0.49
2024-11-26T09:11:24Z INF Retrying connection in up to 16s connIndex=0 event=0 ip=198.18.0.49
2024-11-26T09:11:32Z ERR Unable to establish connection with Cloudflare edge error="TLS handshake with edge error: EOF" connIndex=0 event=0 ip=198.18.0.50
2024-11-26T09:11:32Z ERR Serve tunnel error error="TLS handshake with edge error: EOF" connIndex=0 event=0 ip=198.18.0.50
2024-11-26T09:11:32Z INF Retrying connection in up to 32s connIndex=0 event=0 ip=198.18.0.50
2024-11-26T09:11:41Z ERR Unable to establish connection with Cloudflare edge error="TLS handshake with edge error: EOF" connIndex=0 event=0 ip=198.18.0.49
2024-11-26T09:11:41Z ERR Serve tunnel error error="TLS handshake with edge error: EOF" connIndex=0 event=0 ip=198.18.0.49
```

又是查网络发现这个[issue](https://github.com/cloudflare/cloudflared/issues/376)说的靠谱，我家里是软路由透明代理，DNS应该是使用了旁路由的DNS，导致了这个问题，于是我把NAS的dns地址手动指定为我的主路由地址，再次运行docker，查看日志，it works!

```
2024-11-26T09:20:02Z INF Initial protocol http2
2024-11-26T09:20:02Z INF ICMP proxy will use A.A.A.A as source for IPv4
2024-11-26T09:20:02Z INF ICMP proxy will use 0000::0000:0000:0000:0000 in zone eth0 as source for IPv6
2024-11-26T09:20:07Z INF Starting metrics server on 127.0.0.1:45025/metrics
2024-11-26T09:20:08Z INF Registered tunnel connection connIndex=0 connection=xxxxxxxxx event=0 ip=198.41.200.193 location=sjc07 protocol=http2
```