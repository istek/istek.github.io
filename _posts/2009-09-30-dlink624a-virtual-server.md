---
title: DLINK624+A和虚拟服务器
date: 2009-09-30T06:15:28+00:00
layout: post
categories:
  - 分享
tags:
  - 'D-Link'
---

这两天要在自己的台式机上做几个试验，于是昨天晚上在硬盘上安装了Windows 2003 sp2，但是设置好IIS之后，无论是内网还是公网，都无法访问我的台式机，非常懊恼，因为我已经在我的dlink 624+a上设置了virtual server HTTP，也就是所谓的端口映射，全部都是默认值，没有修改什么，可怎么都无法访问。

今天在单位上搜索了一天的相关资料，一无所获，包括在一些大的论坛求证，没人回答我，相当的遗憾。于是，晚上重新安装了一次系统，将路由器甩开，直接将ADSL接到台式机上，然后从公网访问，通了，这样就说明了所有的问题都集中在路由器上了。

于是，打开dlink的官方论坛，找一些帮忙，都没有说明虚拟服务器如何设置，后来又在百度上搜索了一下，发现一些2007年的帖子，也是虚拟服务器无法设置，之后通过升级Firmware解决了。于是，打开Dlink的官方网站，寻找最新的firmware版本，发现一个2008年4月的patch11的版本，抱着试一下的态度，下载了firmware文件，然后升级路由器，一路都很顺利，之后请自己的朋友尝试打开我的公网IP，okay，那久违的几个字终于出现了。

如果你也使用这个路由器，可以考虑升级firmware，当然前提是你要是用虚拟服务器，因为我看了一下版本说明，patch11只是解决了虚拟服务器和动态dns的连接问题，其他没有什么变化，所以无此需求的朋友可以不用升级。累死我了。

这会已经开始部署Exchange 2003了。