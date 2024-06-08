---
id: 1967
title: WINDOWS下部署WEBLOGIC11G集群手册
date: 2017-12-02T22:56:28+00:00
author: Stanley
layout: post
guid: http://www.bestzhou.org/?p=1967
permalink: /deploy-weblogic11g-windows/
categories:
  - 我的世界
tags:
  - weblogic
  - windows
---
我们项目实施部经常会遇到部署weblogic的问题，中间件我们平时主要使用tomcat和weblogic，但是从稳定性、内存消耗、应用管理部署等方面看，weblogic更胜一筹，可惜的是，虽然可以从oracle官方网站下载到安装程序，但是漏洞补丁的修复，仍然需要购买服务才能获得，否则很尴尬，一旦被局方扫描到漏洞后，处理起来很费劲，例如我们公司就没有购买weblogic服务。

从工作中发现很多同事都不会部署weblogic，先不说rhel linux了，就是Windows很多人也不会，因此，在同事的要求下，写了这篇文档给他们，还有贴图，一副一副截图，然后贴到word里面，很累，环境的话，我是在我的PC虚拟机里面装了两台windows 2003 server。这里面的内容，我就再不贴图了，太多了，文末有完整版的pdf文件供下载学习。<!--more-->

<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#i">一、安装前准备</a><ul>
        <li>
          <a href="#1">1、操作系统的要求</a>
        </li>
        <li>
          <a href="#2JDK">2、JDK</a>
        </li>
        <li>
          <a href="#3">3、节点信息</a>
        </li>
        <li>
          <a href="#4">4、配置集群应用的必要条件</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Weblogic11g">二、安装Weblogic11g</a><ul>
        <li>
          <a href="#1-2">1、准备工作</a>
        </li>
        <li>
          <a href="#2">2、环境变量</a>
        </li>
        <li>
          <a href="#3WEBLOGIC">3、安装WEBLOGIC</a><ul>
            <li>
              <a href="#Node1">Node1:</a>
            </li>
            <li>
              <a href="#Node2">Node2：</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#4-2">4、新建域</a><ul>
            <li>
              <a href="#Node1-2">Node1：</a>
            </li>
            <li>
              <a href="#Node2-2">Node2：</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Weblogic">三、启动Weblogic集群</a><ul>
        <li>
          <a href="#1-3">1、启动顺序</a>
        </li>
        <li>
          <a href="#2-2">2、启动步骤</a><ul>
            <li>
              <a href="#i-2">管理机</a>
            </li>
            <li>
              <a href="#i-3">代理服务器</a>
            </li>
            <li>
              <a href="#1-4">集群节点1</a>
            </li>
            <li>
              <a href="#2-3">集群节点2</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#3-2">3、通过管理机检查节点信息</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#i-4">四、名词解释</a><ul>
        <li>
          <a href="#Domain">Domain</a>
        </li>
        <li>
          <a href="#Server">Server</a>
        </li>
        <li>
          <a href="#Domain_and_Server">Domain and Server的关系</a>
        </li>
        <li>
          <a href="#i-5">为什么需要代理服务器？</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

## <span id="i">一、安装前准备</span>

&nbsp;

### <span id="1">1、操作系统的要求</span>

建议操作系统至少为Windows Server 2008以上，在本例中使用了WINDOWS SERVER 2003 x86操作系统。

因为微软已经于2015年1月终止了对WINDOWS SERVER 2008的支持，所以目前如果想要使用微软的系统补丁，则必须使用windows server 2012以后的操作系统。

### <span id="2JDK">2、JDK</span>

JDK使用oracle jdk即可，版本至少1.6以上，本例中使用JDK 1.6 update 45 x86。

JDK的位数要求：必须匹配操作系统的位数，若使用WINDOWS server 2008 X86，则JDK版本也应该使用jdk x86，反之，若windows server 2008 x86_64,则JDK使用 jdk x64版本。

Windows操作系统的版本可以查看系统信息获取，如下图：

### <span id="3">3、节点信息</span>

设有两个节点组建为一个集群，节点信息如下：

<table>
  <tr>
    <td width="142">
      <strong>名称</strong>
    </td>
    
    <td width="142">
      <strong>节点名</strong>
    </td>
    
    <td width="142">
      <strong>IP</strong>
    </td>
    
    <td width="142">
      <strong>掩码</strong>
    </td>
  </tr>
  
  <tr>
    <td width="142">
      节点1
    </td>
    
    <td width="142">
      Node1
    </td>
    
    <td width="142">
      192.168.31.188
    </td>
    
    <td width="142">
      255.255.255.0
    </td>
  </tr>
  
  <tr>
    <td width="142">
      节点2
    </td>
    
    <td width="142">
      Node2
    </td>
    
    <td width="142">
      192.168.31.189
    </td>
    
    <td width="142">
      255.255.255.0
    </td>
  </tr>
</table>

**节点角色：**

<table>
  <tr>
    <td width="142">
      <strong>节点</strong>
    </td>
    
    <td width="142">
      <strong>端口</strong>
    </td>
    
    <td width="142">
      <strong>角色</strong>
    </td>
    
    <td width="142">
      <strong>受管服务器名称</strong>
    </td>
  </tr>
  
  <tr>
    <td width="142">
      节点1
    </td>
    
    <td width="142">
      7001
    </td>
    
    <td width="142">
      管理机
    </td>
    
    <td width="142">
      AdminServer
    </td>
  </tr>
  
  <tr>
    <td width="142">
      节点1
    </td>
    
    <td width="142">
      8080
    </td>
    
    <td width="142">
      受管服务器
    </td>
    
    <td width="142">
      Node1
    </td>
  </tr>
  
  <tr>
    <td width="142">
      节点1
    </td>
    
    <td width="142">
      80
    </td>
    
    <td width="142">
      代理服务器
    </td>
    
    <td width="142">
      proxy
    </td>
  </tr>
  
  <tr>
    <td width="142">
      节点2
    </td>
    
    <td width="142">
      8080
    </td>
    
    <td width="142">
      受管服务器
    </td>
    
    <td width="142">
      Node2
    </td>
  </tr>
</table>

### <span id="4">4、配置集群应用的必要条件</span>

  * 集群中的所有Server必须位于同一网段，并且必须是IP广播(UDP)可到达的；
  * 集群中的所有Server必须使用相同的版本,包括Service Pack；
  * 集群中的Server必须使用永久的静态IP地址。动态IP地址分配不能用于集群环境。如果服务器位于防火墙后面，而客户机位于防火墙外面，那么服务器必须有公共的静态IP地址，只有这样，客户端才能访问服务器；
  * 要以CLUSTER方式运行，必须有包含CLUSTER许可的LICENSE（从oracle网站上下载的版本就可以进行Cluster配置）；

## <span id="Weblogic11g">二、安装Weblogic11g</span>

### <span id="1-2">1、准备工作</span>

在node1和node2两台服务器安装JDK，配置系统环境变量。

首先，安装jdk，运行jdk-6u45-windows-i586.exe，安装界面

点击“下一步”

安装路径保持默认，除非你有特殊要求，继续点击“下一步”开始安装JDK，安装完之后弹出安装JRE对话框，

除非对安装路径有特殊要求，否则请保持默认，点击“下一步”开始安装JRE

点击“关闭”完成安装。

### <span id="2">2、环境变量</span>

首先，鼠标右键桌面上的“我的电脑”，点击“属性”

打开系统属性对话框

点击“高级”标签，然后点击“环境变量”

打开“环境变量”对话框

然后点击“系统变量”下的新建，新建两个系统变量，修改一个系统变量

JAVA\_HOME= C:\Program Files\Java\jdk1.6.0\_45

classpath= .;%JAVA\_HOME%\lib;%JAVA\_HOME%\lib\tools.jar

_注意：分号前有一个点_

修改PATH变量，在变量值最后追加

配置完成后，注销，重新登陆Windows，打开CMD命令提示符。

输入java -version，返回版本信息，则说明环境变量配置正确。

### <span id="3WEBLOGIC">3、安装WEBLOGIC</span>

#### <span id="Node1">Node1:</span>

打开命令行提示符，进入weblogic安装程序所在目录，在本例中” c:/software”

使用命令运行weblogic11g安装界面

**X86****使用的命令：**

java -jar wls1036_generic.jar

**X86_64****使用的命令：**

java -D64 -jar wls1036_generic.jar

运行命令以后弹出weblogic安装界面

_注意：不要关闭命令提示符，否则安装界面消失！_

点击“下一步”开始安装

修改中间件目录到你想安装的位置，本例中安装位置为C：/weblogic，然后点击“下一步”

取消勾选“我希望通过My Oracle Suport接收安全更新”

点击“YES”，同意不接收安全更新

勾选“我希望不接收配置中存在安全问题”，点击“继续”，

按照典型配置进行安装，点击“下一步”

默认勾选了我们安装的JDK1.6.0_45，点击“下一步”

点击“下一步”

保持默认，点击“下一步”

点击“下一步”开始安装

等待完成安装

不要勾选“运行Quickstart”，点击“完成”关闭安装对话框。

#### <span id="Node2">Node2：</span>

按以上步骤在节点2安装Weblogic。

### <span id="4-2">4、新建域</span>

#### <span id="Node1-2">Node1：</span>

**配置要点：配置管理服务器，受管服务器，集群配置**

从开始菜单，选择“Configuration Wizard”

之后弹出域配置对话框

选择“创建新的WebLogic域”，点击“下一步”

默认即可，点击“下一步”

域名可以填写你想要的域名，比如cbsp\_domain，cmcs\_domain等等，我这里保持默认，点击“下一步”

配置管理员用户名和密码，密码我们这里使用gsdx_123，然后点击“下一步”

选择“生产模式”，点击“下一步”

这里勾选“管理服务器”，“受管服务器，集群和计算机”，点击“下一步”

配置管理服务器的IP和监听端口，按照节点信息的描述，这里监听端口使用7001

添加受管服务器proxy，node1和node2，点击“下一步”

新建集群，集群名称本例中是cluster，集群消息传送模式选择multicast（多播），其他不变，点击“下一步”

将node1和node2添加到集群cluster，点击“下一步”

勾选“创建HTTP代理”，代理服务器选择“proxy”，点击“下一步”

保持默认，点击“下一步”

点击“创建”开始创建域

点击“完成”，关闭域配置对话框。

#### <span id="Node2-2">Node2：</span>

**配置要点：仅配置受管服务器，个别配置需要与节点1****配置一致**

点击开始菜单，点击“Configuration Wizard”打开域配置对话框

之后，弹出域配置对话框

选中“创建新的WebLogic”域，点击“下一步”

使用默认设置，点击“下一步”

配置域名和域目录，**需要注意必须和节点****1****的域名、域目录保持一致**，点击“下一步”

配置管理员用户名和密码，**必须与节点****1****的用户名和密码一致**，点击“下一步”

&nbsp;

选择“生产模式”，点击“下一步”

选择“受管服务器，集群和计算机”，点击“下一步”

添加受管服务器节点2的信息，点击“下一步”

保持默认，点击“下一步”

保持默认，点击“下一步”

点击“创建”，开始创建域

点击“完成”，退出域配置对话框。

Weblogic的集群安装部分已经完成。

## <span id="Weblogic">三、启动Weblogic集群</span>

### <span id="1-3">1、启动顺序</span>

  * 管理机->代理服务器->受管服务器1->受管服务器2
  * 管理机->受管服务器1->受管服务器2->代理服务器

以上是两种启动顺序，从启动顺序可以发现，管理机必须首先启动，之后启动代理服务器还是受管服务器没有顺序要求。

### <span id="2-2">2、启动步骤</span>

#### <span id="i-2">管理机</span>

从开始菜单点击“Start Admin Server For Weblogic Server Domain”，弹出DOS启动窗口

然后会提示输入用户名和密码，输入weblogic和密码gsdx_123，这个用户名和密码就是新建域的时候设置的用户名和密码。

看到红框中的信息，表示管理机启动成功。

#### <span id="i-3">代理服务器</span>

在c:\weblogic\user\_projects\domains\base\_domain\bin目录创建代理服务器启动脚本start_proxy.cmd。

startManagedWebLogic.cmd proxy <http://192.168.31.188:7001>

从内容可以发现，受管服务器的启动脚本就是“startManagedWebLogic.cmd 受管服务器名称 管理机地址”

双击start_proxy.cmd启动代理服务器，记得输入用户名和密码

看到以上信息，说明代理服务器已经启动成功。

#### <span id="1-4">集群节点1</span>

通过代理服务器的脚本，我们可以很轻松的创建节点1的启动脚本。

依然双击执行start_node1.cmd

到这个状态，通过信息可以看到，节点1正在等待与其他集群节点同步信息。

#### <span id="2-3">集群节点2</span>

现在启动节点2，start_node2.cmd脚本内容如下

&nbsp;

双击start_node2.cmd启动节点2

从提示信息可以看到，节点2也在等待与集群其他节点同步信息，同步完成后，节点就上线了。

### <span id="3-2">3、通过管理机检查节点信息</span>

登陆管理机，打开浏览器，访问http://192.168.31.188:7001/console 或者点击节点1服务器开始菜单中的“Admin Server Console”

输入用户名和密码，点击“登录”，进入控制台页面

点击“环境”，“服务器”可以看到一共有1个管理机，3个受管服务器

并且，node1和node2属于集群cluster，四个服务器的状态都是running（运行中），健康状况都是OK。

在部署下可以发现，代理服务器应用已经处于运行中

至此，WEBLOGIC集群安装就算完成了，接下来就是一些优化工作和项目部署了。

## <span id="i-4">四、名词解释</span>

### <span id="Domain">Domain</span>

Domain是WebLogic Server实例的基本管理单元。所谓Domain就是，由配置为Administrator Server的WebLogic Server实例管理的逻辑单元，这个单元是有所有相关资源的集合。

### <span id="Server">Server</span>

Server是一个相对独立的，为实现某些特定功能而结合在一起的单元。例如本例中的代理服务器，node1服务器，node2服务器，管理服务器。

### <span id="Domain_and_Server">Domain and Server的关系</span>

一个Domain 可以包含一个或多个WebLogic Server实例，甚至是Server集群。一个Domain中有且只能有一个Server 担任管理Server的功能，其它的Server具体实现一个特定的逻辑功能。

本例中管理服务器就是担任管理Server的AdminServer，其他服务器均为受管服务器，实现特地的功能。

### <span id="i-5">为什么需要代理服务器？</span>

项目部署到集群后，需要一个特定的服务器用来处理客户发起的请求，将客户的请求分发给集群中的节点服务器。那么，代理服务器就是承担这个功能的服务器，它收到客户发来的请求，将请求转发给节点1或者节点2去处理，节点1或者节点2处理完成后返回给客户。它的负载算法主要有循环法，基于权重，随机等几种算法。

本例中，客户发送请求给代理服务器 http://192.168.31.188，代理服务器收到请求后发送给节点1 http://192.168.31.188:8080,然后节点1处理客户信息，返回给客户。对于客户而言，客户不用管后端是由哪个节点来处理，他只面对代理服务器。

* * *

本文档的下载地址： [点我下载](https://pan.baidu.com/s/1hrC5EJE) （提取码：qivb）