---
id: 1970
title: 使用油猴脚本直链下载百度网盘内容
date: 2017-12-02T23:34:51+00:00
author: Stanley
layout: post
guid: http://www.bestzhou.org/?p=1970
permalink: /tampermonkey/
image: https://upyun.esesr.net/wp-files/2017/12/TIM截图20171202230702.png?_upt=a00873c51512613230
categories:
  - 我的世界
tags:
  - Tampermonkey
  - 软件分享
---
<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#i">一、描述</a>
    </li>
    <li>
      <a href="#Tempermonkey">二、安装Tempermonkey</a>
    </li>
    <li>
      <a href="#i-2">三、用户脚本</a>
    </li>
    <li>
      <a href="#i-3">四、如何使用</a>
    </li>
    <li>
      <a href="#i-4">五、后记</a>
    </li>
  </ul>
</div>

## <span id="i">一、描述</span>

油猴脚本就是浏览器的一个扩展程序，通过这个扩展程序，可以调用用户编写的脚本程序，所有的脚本程序都是使用JavaScript编写的。

因为我使用的是chrome浏览器，所以使用的油猴脚本扩展程序是**Tampermonkey**<sup id="fnref-1970-1"><a href="#fn-1970-1" class="jetpack-footnote">1</a></sup>。
  
<!--more-->

## <span id="Tempermonkey">二、安装Tempermonkey</span>

官方网站在[这里](http://tampermonkey.net/)，点击前往打开首页，如下图：

![](https://ws1.sinaimg.cn/large/622271cdgy1fm2tr349thj21gu0kjac2.jpg)

Tampermonkey支持的浏览器主要有Chrome，Microsoft Edge，Safari，Opera Next（chrome内核）和Firefox，涵盖了所有平台。

若您的浏览器也是Chrome点击_Chrome_标签下的 _Tempermonkey Stable_下的 **下载** 安装这个扩展，请看下图

![](https://ws1.sinaimg.cn/large/622271cdgy1fm2tzlcu8zj20xl0k2jtd.jpg)

安装以后，chrome扩展中就可以看到这个扩展程序了，如下图：

![](https://ws1.sinaimg.cn/large/622271cdgy1fm2tvldrwuj20nm0dhgme.jpg)

![](https://ws1.sinaimg.cn/large/622271cdgy1fm2tx2qdhej20c806idg8.jpg)

OK，Tempermonkey已经安装成功了，接下来我们需要给Tempermonkey增加用户脚本，让它帮我们干活。

## <span id="i-2">三、用户脚本</span>

用户脚本主要是从三个网站下载

  * [GreasyFork](https://greasyfork.org/zh-CN) 
  * [Userscripts.org](http://userscripts-mirror.org/)
  * [OpenUserJS](https://openuserjs.org/)
  * [Github/Gist](https://gist.github.com/search?l=javascript&q=%22user.js%22)

我们从GreasyFork下载用户脚本，首先打开[GreasyFork](https://greasyfork.org/zh-CN)，如下图

![](https://ws1.sinaimg.cn/large/622271cdgy1fm2u19yee1j21gv0qd78t.jpg)

搜索**百度网盘直接下载助手**这个关键字

![](https://ws1.sinaimg.cn/large/622271cdgy1fm2u266ceqj20ur0i60vp.jpg)

点击搜索结果第一条的百度网盘直接下载助手，打开页面，会有一个绿色的大大的`安装此脚本`按钮，点击即可安装成功。

![](https://ws1.sinaimg.cn/large/622271cdgy1fm2u2lecswj20tr0ib40b.jpg)

OK，用户脚本安装完了。

## <span id="i-3">四、如何使用</span>

我给咱们实施部注册了一个网盘，里面有实施需要的所有软件，包括oracle数据库，db2数据库，weblogic10，tomcat6，tomcat7，jdk1.6，jdk1.7以及操作系统的ISO。

| 网站            | 用户名       | 密码             |
| ------------- | --------- | -------------- |
| pan.baidu.com | \***\**** | &#8212;&#8212; |

**请各位同事不要将此百度盘资料泄露出去，也请不要随意修改密码。**

Tempermonkey安装好，用户脚本也安装好，什么设置都不需要做，现在就可以使用百度盘拿到直链链接，然后使用你喜欢的
  
aria2，迅雷，idm等等工具就可以满速下载了。

![](https://ws1.sinaimg.cn/large/622271cdly1fm2u6w091lj21h60qaq5i.jpg)

鼠标移动到`下载助手`，会显示一个下拉菜单，然后将鼠标移到`外链下载`->`显示链接`，然后点击`显示链接`

![](https://ws1.sinaimg.cn/large/622271cdly1fm2u464v30j20sj0bbaaz.jpg)

点击`显示链接`后的窗口

![](https://ws1.sinaimg.cn/large/622271cdly1fm2u4v18wej20n907iaaz.jpg)

图中1-7的地址都是外链地址，都可以用来下载这个文件。比如我打开aria2下载工具，复制第一个链接地址，然后再aria2下载工具中新建一个下载项目，粘贴地址，然后开始下载，你会发现，下载速度很快很快，是不是很爽，不用什么破解程序。

![](https://ws1.sinaimg.cn/large/622271cdly1fm2u5ay2p9j20gz08zdg0.jpg)

## <span id="i-4">五、后记</span>

其实油猴脚本可以干的事情很多，比如一些视频网站总是需要VIP才能看，OK，也有这种相应的用户脚本，只要安装后，优酷QQ爱奇艺等等网站的VIP视频随你看。

也可以用来去除搜索结果页面的广告，重新处理搜索结果页面等等。

我写这篇文档也是希望将这个技巧分享给各位同事，希望大家以后不要再为了下载实施软件而犯难了。

_本文使用markdown工具[typora](https://typora.io/)。_

<li id="fn-1970-1">
  <strong>Tampermonkey</strong> 是一款免费的浏览器扩展和最为流行的用户脚本管理器，它适用于 <a href="http://tampermonkey.net/#"><strong>Chrome</strong></a>, <a href="http://tampermonkey.net/#"><strong>Microsoft Edge</strong></a>, <a href="http://tampermonkey.net/#"><strong>Safari</strong></a>, <a href="http://tampermonkey.net/#"><strong>Opera Next</strong></a>, 和 <a href="http://tampermonkey.net/#"><strong>Firefox</strong></a>。&#160;<a href="#fnref-1970-1">&#8617;</a> </fn></footnotes>