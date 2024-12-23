---
title: U盘量产
date: 2009-09-20T21:15:08+00:00
layout: post
categories:
  - Windows
tags:
  - USB
---

昨天给本子装Ubuntu，重新格式化了自己的U盘，Kingston Data Traveler 4G，之后用老毛桃U盘PE制作工具，重新给U盘灌系统，结果做好后，笔记本上U盘不能启动，总是出现下面的错误。
```
Try (hd0,0):Fat32
```
然后就没有任何反应了，搜索了相关主题，发现有人说要对U盘进行量产。我是第一次听说给U盘做量产，不理解量产的概念，最后翻阅了一部分的帖子，终于敢动手做了，某人说了U盘是搞不坏的，放心折腾。哈哈。

首先，下载[Chip Genius工具](http://cid-574f8e37bafc4791.skydrive.live.com/self.aspx/.Public/ChipGenius-v2.64.zip)，查看自己的U盘芯片型号和芯片制造商。

运行之后，如下图：
<!--more-->
图中G盘就是我的U盘了，芯片制造商是慧荣（SMI），芯片型号是SM321/SM324，有了这些信息就可以去找你需要的量产工具了，因为**量产工具必须得使用符合自己芯片制造商和芯片型号**。

如果您的U盘也是SMI 321/324这种，可以下载这个[量产工具](http://cid-574f8e37bafc4791.skydrive.live.com/self.aspx/.Public/release-test%5E_sm32x%5E_i0414.zip)。运行工具，程序界面如下图：

一般量产的大致过程是：

1. 点settings，密码默认是两个空格，选择default.ini文件；
2. 如果你要做一个USB-CDROM，那么要勾选make auto run，然后选择你要写入的ISO文件；
3. 其他选择不用做任何调整，直接点Start；
4. 量产完成之后，会在“Start”的上面出现一个蓝色OK图标，这表示量产成功，否则就是出错了；
5. 完成之后，必须拔下U盘，重新插上，才能识别新的盘符。

我的盘做好后，出现两个盘符，一个是做的USB-CDROM，大约占1G（一个ISO文件怎么会占1G啊？！），还有一个usb-hdd,总空间是3G，大家看下图。

G盘就是做出来的一个USB-CDROM，而I盘是剩余的那3G做出来的一个USB-HDD。其实，量产很容易，只要你阅读了那些相关的内容之后，做到胸有成竹，是不会有什么问题的，当然，如果出了问题，请教Google，肯定是一个不错的选择。