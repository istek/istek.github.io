---
title: 'openwrt使用openssh'
date: 2024-01-16 21:24:35
type: post
category:
 - Linux
tags:
 - OpenWRT
---

其实完全可以使用openssh替换掉openwrt中的dropbear，毕竟咱们用的软路由不会在意那一点内存的。

我使用的是immortalwrt，可以根据自己使用的版本进行更换。
<!--more-->
首先，修改root密码
```
passwd
```

其次，修改dropbear的监听端口，避免与openssh监听端口冲突，其次呢，以防万一openssh启动不了，不至于进不了控制台。
```
uci set dropbear.@dropbear[0].Port=2222
uci commit dropbear
/etc/init.d/dropbear restart
```

测试一下2222端口是否能够登录成功，也可以通过终端工具再开一个窗口。
```
ssh -p 2222 root@192.168.1.1
```

安装openssh
```
opkg update
opkg install openssh-server openssh-client openssh-sftp-server
```

修改ssh配置文件，允许root登录

```
vi /etc/ssh/sshd_config
```
找到`#PermitRootLogin without-password`这行，替换为`PermitRootLogin yes`,之后保存退出。

然后设置开机自启并启动服务

```
/etc/init.d/sshd enable
/etc/init.d/sshd start
```

最后用终端程序，从22端口连接路由器吧，应该没有问题了，可以连上了。

接下来就可以停用dropbear，甚至于删除它。

```
/etc/init.d/dropbear disable
/etc/init.d/dropbear stop
opkg remove dropbear
```
