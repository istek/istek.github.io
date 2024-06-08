---
title: "WIN10 ONEDRIVE无法运行"
date: 2021-09-10 16:24:35
type: post
categories:
 - Windows
tags:
 - OneDrive
---

最近发现自己的OneDrive 办公室和家里的都无缘无故的无法运行，即便是卸载以后重新安装，也是无法运行，于是乎，查了一下，发现是因为组策略中的“禁止使用OneDrive进行文件存储”的设置导致的。

如下图：

![image.png](https://pic.rmb.bdstatic.com/bjh/792f1b1fdbd1e8ba55981fd4ef1271c1.jpeg)
<!--more-->
**解决办法**：

1. 打开开始-运行-输入`regedit`，回车

![image.png](https://pic.rmb.bdstatic.com/bjh/8c350808744fdab1ab3d308585d02a5a.jpeg)

2. 打开注册表管理器，依次`HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\OneDrive`的文件夹。

![image.png](https://pic.rmb.bdstatic.com/bjh/4392fbe013a4bc44f9f239db418d0622.jpeg)

3. 将`DisableFileSyncNGSC`的DWORD值改为`0`。

![image.png](https://pic.rmb.bdstatic.com/bjh/6afeaeaea04c9604e625cae8182bad03.jpeg)

4. 重启电脑之后，启动OneDrive。
