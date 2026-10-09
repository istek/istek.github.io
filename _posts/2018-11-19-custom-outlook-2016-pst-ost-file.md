---
title: 'Outlook 2016 pst/ost邮件数据文件迁移实现'
date: 2018-11-19 21:47:29
type: post
categories:
  - Windows
tags:
  - Outlook
---
当使用outlook 2016新建Email账户的时候，其数据文件（.ost文件）总是被保存在C盘默认目录`C:\Users\用户名\AppData\Local\Microsoft\Outlook`下，这样占用C盘的空间。在默认状态下进入Outlook，发现设置选项中关于.ost文件的保存位置，这似乎是无法修改的。笔者在网上进行了一系列的搜索，如何来修改默认的ost存储位置，按照网上的做法，大家有的用控制面板里面的“邮件”来修改，我试过了，发现在win10+office2016下面无相关选项；有的说是修改注册表，添加`ForceOstPath`键值，还是不行。

几经辗转，笔者终于找到了可行且容易的解决方法，效果看图：  
![](https://img.261540.xyz/piclist/2026_10_20261009153315179.png)
<!--more-->
具体操作流程如下：

1、下载`Office自定义工具（OCT）` 微软官方下载页面：`https://www.microsoft.com/en-us/download/details.aspx?id=49030`；大家根据自己的操作系统版本选择32位或者64位，后面以64位为例子来做讲解。

2、下载完后，运行64位的`admintemplates_x64_4297-1000_en-us.exe`，会提示让你选择解压文件夹，找个文件夹解压即可，例如`d:\tmp`；

3、进入你解压的`d:\tmp`文件夹，进入`admx`文件夹，复制`outlk16.admx`文件和`zh-cn`文件夹到`C:\Windows\PolicyDefinitions`，如果你的系统盘不是C，请修改为你自己的盘符；

![](https://img.261540.xyz/piclist/2026_10_20261009153222990.png)

4、WIN键+R，输入`gpedit.msc`启动组策略，按照下图所示，修改路径！

![](https://img.261540.xyz/piclist/2026_10_20261009153406329.png)

![](https://img.261540.xyz/piclist/2026_10_20261009153438320.png)

5、经过上述配置。在Outlook 2016中新添加的邮件账户时，其邮件ost文件会存储在你所指定的目录下。但是对已经存在的邮件账户是无效的（因为在此之前是没有配置组策略的），只能先删除掉已经存在的邮件账户，然后重新添加（过程中请注意相关的备份）。

重置office2016
------------

```bash
C:\Program Files (x86)\Microsoft Office\Office14\outlook.exe /importprf .\.prf
```

变更office365更新通道
---------------

![](https://img.261540.xyz/piclist/2026_10_20261009153535900.png)

![](https://img.261540.xyz/piclist/2026_10_20261009153606486.png)
