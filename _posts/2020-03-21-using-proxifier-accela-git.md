---
title: 使用Proxifier加速git
date: 2020-03-21 22:36:45
type: post
category:
  - Windows
tag:
  - Proxifier
---
基于众所周知的原因，gitlab，github要上传下载代码实在是太慢了，很多人使用``git config``配置全局代理的方式加速上传下载。

今天我想分享一种不一样的加速方式，就是通过proxifier软件。

因为我这边需要使用proxifier将连接到办公室的网络代理到本地，所以平时在家里都是经常打开这个软件的。

proxifier可以从我的网盘下载，或者，从百度上搜索下载，很方便的。安装完成后，运行程序，如下图：

![](https://cdn.jsdelivr.net/gh/istek/img/20200321203102.png)

然后添加代理服务器，保存一下。

![](https://cdn.jsdelivr.net/gh/istek/img/20200321223948.png)

接下来就是配置规则了，我研究了一下，原本以为是通过``git.exe```上传下载的，结果发现是```ssh.exe``，既然知道了，配置一条根据应用程序代理的规则，很简单了。

![](https://cdn.jsdelivr.net/gh/istek/img/20200321224053.png)

配置以后，试试吧，是不是速度飞起来了？ 🙂