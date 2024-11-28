---
title: 使用Github+jsDelivr+PicGo搭建图床
date: 2020-03-10 21:48:14
type: post
category:
  - Windows
tag:
  - PicGo
---
- GitHub：全球最大开源托管站，微软旗下。
- jsDelivr：免费、稳定的CDN
- PicGo：开源的图片上传程序，支持win、linux等系统。

一、Github设置
----------

### 1.新建仓库

新建一个public仓库。

### 2.获取token

打开 [https://github.com/settings/tokens，点击右侧的](https://github.com/settings/tokens%EF%BC%8C%E7%82%B9%E5%87%BB%E5%8F%B3%E4%BE%A7%E7%9A%84) Generate new token

二、PicGo设置
---------

看下图

![](https://cdn.jsdelivr.net/gh/istek/img/img/20200310213521.png)

三、引用地址
------

前缀为 <https://cdn.jsdelivr.net/gh/xxx/yyy> ，后面xxx/yyy是github用户名和仓库