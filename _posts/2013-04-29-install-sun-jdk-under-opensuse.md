---
id: 1480
title: openSUSE 12.3安装sun jdk 1.6.0_45
date: 2013-04-29T02:44:57+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1480
permalink: /install-sun-jdk-under-opensuse/
categories:
  - 我的世界
tags:
  - jdk
  - opensuse
---
openSUSE 12.3自带的是openjdk 1.7.0，而我们需要的是sun jdk，搜索了一下网上的文章，多数都是抄袭，都没有验证过。接下来，主要是安装的步骤。

<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#1sun_jdk">1.下载sun jdk</a>
    </li>
    <li>
      <a href="#2sun_jdk">2.安装sun jdk</a>
    </li>
    <li>
      <a href="#3">3.配置环境变量</a>
    </li>
    <li>
      <a href="#4">4.调整优先级</a>
    </li>
    <li>
      <a href="#5">5.测试验证</a>
    </li>
  </ul>
</div>

#### <span id="1sun_jdk">1.下载sun jdk</span> {#1sunjdk}

这步就不用我说了吧，去sun官方下载rpm.bin包，按自己的架构选择（X86,X86_64）。

#### <span id="2sun_jdk">2.安装sun jdk</span> {#2sunjdk}

    chmod a+x jdk-6u45-linux-x64-rpm.bin  
    zypper in jdk-6u45-linux-x64-rpm.bin  
    

首先它会解压bin包，然后会有几个rpm包，然后就开始自动安装。

#### <span id="3">3.配置环境变量</span> {#3}

编辑`/etc/profile`文件，在文件末尾添加如下环境变量：

    JAVA_HOME=/usr/java/jdk1.6.0_45 JRE_HOME=/usr/java/jdk1.6.0_45/jre PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib  
    export JAVA_HOME JRE_HOME PATH CLASSPATH  
    

添加后，我们使设置生效`source /etc/profile`，之后通过`env|sort`命令查看我们刚才添加的环境变量，是否存在。

#### <span id="4">4.调整优先级</span> {#4}

因为系统里面已经内置了openjdk，所以我们选择调整openjdk和sunjdk的优先级，让sun jdk成为默认选择。ok，我们开始。(以X86_64为例)

    #/usr/sbin/update-alternatives --install "/usr/bin/java" "java" "/usr/java/jdk1.6.0_45/bin/java" 88 
    #/usr/sbin/update-alternatives --install "/usr/bin/javac" "javac" "/usr/java/jdk1.6.0_45/bin/javac" 88 
    #/usr/sbin/update-alternatives --install "/usr/lib64/browser-plugins/javaplugin.so" "javaplugin" "/usr/java/jdk1.6.0_45/jre/lib/amd64/libnpjp2.so" 88 #/usr/sbin/update-alternatives --config java
    

选择咱们添加的sun jdk

    #/usr/sbin/update-alternatives --config javaplugin
    

同理，选择我们自己添加的sun java web插件。

#### <span id="5">5.测试验证</span> {#5}

    #java -version
    java version "1.6.0_45"  
    Java(TM) SE Runtime Environment (build 1.6.0_45-b06)  
    Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01, mixed mode)  
    

浏览器验证： 
  
打开firefox，在地址栏输入`about:plugins`，然后你会看到如下图的内容。 
  
![java-plugin](https:///www.bestzhou.org/wp-content/uploads/2013/08/52713f9370ecc3bbcfa6168f54098c97.png?w=720)

_2013年4月30日增加第三小节的内容_