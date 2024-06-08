---
id: 1170
title: 在Ubuntu 9.04配置itshideen
date: 2009-09-02T05:15:49+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1170
permalink: /in-ubuntu-9-04-configuration-itshideen/
categories:
  - 我的世界
tags:
  - itshidden
  - Ubuntu
---
To use the VPN service provided by [ItsHidden.com](http://itshidden.com/), complete the following steps (that were partially found in [this Ubuntu forum thread](http://ubuntuforums.org/showthread.php?t=1224517)):

要使用itsHidden.com提供的VPN服务，请根据下面的步骤操作：

  1. Install the [PPTP](http://en.wikipedia.org/wiki/Point-to-point_tunneling_protocol) plug-in for [Network Manager](https://help.ubuntu.com/community/NetworkManager#VPN%20support)//安装nm的PPTP插件 $ sudo apt-get install network-manager-pptp 
  2. Restart Network Manager//重启NM服务 $ sudo killall NetworkManager $ sudo NetworkManager & 
  3. Create the VPN connection //创建VPN链接 
  4. In the “Advanced…” settings, enable “Use Point-to-Point encryption (MPPE)”//在VPN创建窗口，点击“高级”，勾选“使用点对点加密”

附图如下：

<img src="https://i1.wp.com/a.imagehost.org/0152/Screenshot_5.png?resize=720%2C312 "VPN窗口"" alt="" />