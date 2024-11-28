---
title: openwrt无法识别USB千兆网卡
date: 2022-02-16 16:24:35
type: post
category:
 - Linux
tags:
 - OpenWRT
---

最近几天不知道怎么回事，自己的openwrt好端端的就出现问题了，网络不通了，可以发现是dhcp不工作，没有下发ip，可问题是最近我没有动过路由器啊，很奇怪。

于是乎，重置路由器，不行。

只能换一个其他版本的openwrt了，于是换了一个版本，发现usb网卡拔插以后，只有如下信息：
<!--more-->
```
[  524.423197] usb 2-1: USB disconnect, device number 2
[  525.442946] usb 2-1: new SuperSpeed Gen 1 USB device number 3 using xhci_hcd
```

然后用lsusb命令查看结果如下：

```
root@OPENWRT 23:03 ~# lsusb -t
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/1p, 5000M
    |__ Port 1: Dev 3, If 0, Class=, Driver=, 5000M
    |__ Port 1: Dev 3, If 1, Class=, Driver=, 5000M
```

显然，driver是空的，没有驱动啊，于是google了一下，说是要安装` kmod-usb-net-rtl8152`这个软件包才行，于是在软件包里面搜索了一下，发现有两个包：

- kmod-usb-net-rtl8152
- kmod-usb-net-rtl8152-vendor

第一个包，无法安装，因为缺少必要的一个固件包，如下图：

![image.png](https://img.zhoutao.ren/202209102149781.jpeg)

于是只能安装第二个包，安装完成以后，重新拔插一次USB网卡，识别出来了

```
opkg install  kmod-usb-net-rtl8152-vendor

```

然后再看一下`lsusb`的输出信息

```
root@OPENWRT 23:12 modules.d# lsusb -t
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/1p, 5000M
    |__ Port 1: Dev 5, If 0, Class=, Driver=r8152, 5000M

```

Great! Driver有内容了！

-----
Tip: lsusb命令属于`usbutils`这个包
```
opkg install usbutils
```

顺便推荐一个固件，精简但非常好用的固件，安装好后，apps非常少，然后你可以根据需要在老板提供的软件仓库中搜索安装，用什么安装什么，非常方便。

https://bf.supes.top/