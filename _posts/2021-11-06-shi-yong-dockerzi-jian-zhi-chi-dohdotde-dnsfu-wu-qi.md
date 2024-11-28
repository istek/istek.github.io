---
title: '使用 DOCKER 自建支持 DOH、DOT 的 DNS 服务器'
date: 2021-11-06 16:24:35
type: post
category:
 - Linux
tags:
 - AdGuard
---

DoH (DNS over HTTPS) 和 DoT (DNS over TLS) 有效避免了运营商的 DNS 监听和劫持，本文记录在公网搭建一个可以去广告的 DNS 服务器（借助 Adguard Home）。

## 前期准备

- 公网服务器一台
- 域名一个，并且指向该公网服务器
- 域名指向该公网服务器，并申请好 TLS 证书（可使用 Let’s Encrypt 申请）
<!--more-->
## Docker 部署
Docker Hub 镜像地址：https://hub.docker.com/r/adguard/adguardhome

安装好 Docker 后下载镜像，以下任选其一

```
docker pull adguard/adguardhome:latest  # 稳定版
docker pull adguard/adguardhome:edge  # 最新的版本，可能不稳定
```

新建配置目录和工作目录

```
mkdir /root/adguardhome/{workdir,confdir}
```

启动镜像，完成后进入配置页面：HostIP:3000

```
docker run --name adguardhome \
  -v /root/adguardhome/workdir:/opt/adguardhome/work \
  -v /root/adguardhome/confdir:/opt/adguardhome/conf \
  --restart always \
  -p 443:443 -p 853:853 -p 3000:3000 -p 53:53/udp -p 53:53/tcp \
  -d adguard/adguardhome:edge
```

注意，第一次进入配置页面需要配置监听端口，配置好后注意修改容器启动时端口的映射，并以新的命令重新启动容器

## 上游服务器设置

进入 Adguard Home 控制台，找到「设置 - DNS 设置」

「上游 DNS 服务器」可填入公共 DNS 服务器

![](https://cdn.jsdelivr.net/gh/istek/img/202111061223825.png)

国内外常用的公共 DNS 如下：

|DNS 服务器地址|	类型|	提供商|
|-------|-------|-------|
|https://dns.alidns.com/dns-query|DNS over HTTPS|阿里 DNS
|tls://dns.alidns.com|DNS over TLS|阿里 DNS
|https://doh.360.cn/dns-query|DNS over HTTPS|360 DNS
|119.29.29.29|常规 DNS|DNSPod
|182.254.116.116|常规 DNS|DNSPod
|223.5.5.5|常规 DNS|阿里 DNS
|223.6.6.6|常规 DNS|阿里 DNS
|114.114.114.114|常规 DNS|114 DNS
|https://dns.quad9.net/dns-query|DNS over HTTPS|Quard9

「Bootstrap DNS 服务器」填入一到两个常规 或者 运行商 DNS 服务器就可以，如`223.5.5.5`和`119.29.29.29`。

在 「DNS 服务设定中」，勾选「使用客户端的子网地址（EDNS）」，可以使查询的客户端得到离其最近的网站 IP 解析。

## DoH & DoT 设置
进入「设置 - 加密设置」

勾选「启用加密」，填写准备的域名，「HTTPS 端口」和「DNS-over-TLS 端口」均可自定义，如有改正记得重新修改容器启动时的端口映射配置。

证书可以选择「设置证书路径」和「粘贴证书内容」，按要求配置。

配置完成后可以使用「`https://域名`」访问控制台

## 去广告设置

进入「过滤器 - DNS 封锁清单」

附一些常用的规则：

- HalfLife，规则合并自 EasylistChina、EasylistLite、CJX’sAnnoyance 合并规则：`https://gitee.com/halflife/list/raw/master/ad.txt`
- EasyPrivacy，隐私保护：`https://easylist-downloads.adblockplus.org/easyprivacy.txt`
- I don’t care about cookies，我不关心 Cookie 的问题，屏蔽网站的 cookies 相关的警告：`https://www.i-dont-care-about-cookies.eu/abp/`
- anti-AD，自称是目前中文区命中率最高的广告过滤列表：`https://gitee.com/privacy-protection-tools/anti-ad/raw/master/easylist.txt`
- 乘风广告、视频过滤规则：`https://gitee.com/xinggsf/Adblock-Rule/raw/master/rule.txt`、`https://gitee.com/xinggsf/Adblock-Rule/raw/master/mv.txt`

至此配置完成。

使用普通 DNS 解析，在客户端填入服务器的 IP 即可，若要使用 DoH 和 DoT 则需使用另外的工具。

最新版的 Chrome 已经支持使用 DoH 解析，在 Chrome 中找到「设置 - 安全 - 使用安全 DNS」，按规则填入域名即可

![](https://cdn.jsdelivr.net/gh/istek/img/202111061236829.png)
