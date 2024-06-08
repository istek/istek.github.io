---
id: 1028
title: Ubuntu 8.10安装永中Office个人版2009
date: 2008-10-20T08:47:06+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1028
permalink: /ubuntu-8-10-install-evermore-office-personal-edition-2009/
categories:
  - 我的世界
tags:
  - Ubuntu
---
从网络上找到了8.04的安装方法，其实在8.10下面安装没有什么区别，不过我的桌面效果开了，安装对话框很正常，没有出现“白屏”和看不到字的情况。

1、在永中<span onclick="tagshow(event)">官方</span>网站<span onclick="tagshow(event)">下载</span>一个压缩包，这里需要注意的是一定要<span onclick="tagshow(event)">下载</span>for linux版本，如果不幸<span onclick="tagshow(event)">下载</span>了for win版本的，则以下步骤无效！

下载永中集成Office

下载:<http://218.90.147.70/EverMore/EIOPersonal/EIOffice>_Personal_Lin.tar.gz

2、将tar.gz包下载到主目录，然后解压缩

3、打开解压缩后的<span onclick="tagshow(event)">文件</span>夹，看到n多<span onclick="tagshow(event)">文件</span>，其中有setup<span onclick="tagshow(event)">安装</span><span onclick="tagshow(event)">文件</span>，这些都不用管了，看看此时的路径为：/home/用户名/EIOffice_Personal

4、在<span onclick="tagshow(event)">终端</span>中输入：cd EIOffice_Personal,然后回车

5、在终端中输入：<span onclick="tagshow(event)">sudo</span> chmod +x setup，然后回车，添加登录用户的执行权限

6、在终端中输入：sudo ./setup ，然后回车，

下面的解决办法先预留着，如果出现问题，可以使用。

在安装完成后，如果开启“桌面效果”启动也可能出现空界面的问题，下面是在网上搜索到的<span onclick="tagshow(event)">解决</span>办法。

sudo gedit /<span onclick="tagshow(event)">usr</span>/bin/eio

在第一行下加入

export AWT_TOOLKIT=MToolkit