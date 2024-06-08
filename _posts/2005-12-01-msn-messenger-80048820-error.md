---
id: 98
title: MSN Messenger 80048820错误
date: 2005-12-01T06:24:04+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=98
permalink: /msn-messenger-80048820-error/
categories:
  - 我的世界
tags:
  - windows
---
我的MSN最近一段时间只要是在办公室里，就是无法连接网络，从网络上查了一下，主要存在的问题可能有：1，系统时间是否设置正确。2，可能没有注册系统的控件。据说是MSN Messenger 7.5的一个Bug，不知道是否是真实的。

对于系统注册控件：

<div class="UBBPanel">
  <div class="UBBTitle">
    ![引用内容](images/quote.gif) 引用内容
  </div>
  
  <div class="UBBContent">
    在WINDOWS的”开始”菜单下选择”运行”,然后依次键入:
  </div>
</div>

我的MSN最近一段时间只要是在办公室里，就是无法连接网络，从网络上查了一下，主要存在的问题可能有：1，系统时间是否设置正确。2，可能没有注册系统的控件。据说是MSN Messenger 7.5的一个Bug，不知道是否是真实的。

对于系统注册控件：

<div class="quote">
  在WINDOWS的”开始”菜单下选择”运行”,然后依次键入: regsvr32 softpub.dll 回车</p> 
  
  <p>
    regsvr32 mssip32.dll 回车
  </p>
  
  <p>
    regsvr32 intippki.dll 回车
  </p>
</div>