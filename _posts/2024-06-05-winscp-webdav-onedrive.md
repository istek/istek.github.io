---
title: "使用WinSCP连接OneDrive"
date: 2024-06-05T19:28:35+08:00
type: post
category: 
 - Windows
tags:
 - OneDrive
---
使用WinSCP，基于WebDAV协议可以轻松地上传、管理、备份文件到OneDrive。
<!--more-->
# 准备工作
## 找到你的"OneDrive客户ID"

打开浏览器，登录OneDrive，登录以后，地址栏的地址类似如下：
https://onedrive.live.com/?id=root&cid=ABCDEFGHIJKLMNOP

这个`ABCDEFGHIJKLMNOP`就是你的客户ID，选中复制到粘贴板，我们一会要用。

![](https://www.xiaobaby.eu.org/file/db9e4f9d84bd452be882d.png)

# WinSCP 配置

启动WinSCP工具，在显示的登录对话框中，选择`WebDAV`，加密类型选择`TLS/SSL Implicit encryption`，主机名填写`d.docs.live.net`，用户名和密码填写你的OneDrive账号和密码。

*注意：如果你的OneDrive账号启用了二次验证，需要去[创建app应用密码](https://account.live.com/proofs/Manage/additional#AppPassword)*

![](https://www.xiaobaby.eu.org/file/60a4d6b91155881f2c955.png)

接下来，点击`Advanced`按钮，在`Environment` > `Directories`下面的`Remote Directory`中填写 `/ABCDEFGHIJKLMNOP`

![](https://www.xiaobaby.eu.org/file/cf52963068a48cf0fc6d6.png)

点击OK按钮确认，再次点击`Save`按钮保存设置。

![](https://www.xiaobaby.eu.org/file/5e9e9ec360a2d9b33bfba.png)

# 开始使用

双击保存的会话名称，就可以连接了。

![](https://www.xiaobaby.eu.org/file/147530184f0a90740c5d7.png)

