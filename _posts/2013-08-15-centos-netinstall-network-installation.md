---
id: 1482
title: 网络安装CentOS 5.9
date: 2013-08-15T23:20:38+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1482
permalink: /centos-netinstall-network-installation/
categories:
  - 我的世界
tags:
  - CentOS
  - netinstall
---
<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#1">1. 下载网络安装镜像</a>
    </li>
    <li>
      <a href="#2_CentOSCD">2. 刻录镜像文件与使用CentOS安装CD引导计算机</a>
    </li>
    <li>
      <a href="#3_GRUB_NetInstall">3. 在GRUB菜单中选择网络安装 (NetInstall)或直接按回车键开始引导系统</a>
    </li>
    <li>
      <a href="#4_CentOS_59">4. 启动CentOS 5.9安装工具</a>
    </li>
    <li>
      <a href="#5_Chinese_SimplifiedEnglish">5. 选择语言（Chinese Simplified简体中文或者English英语）</a>
    </li>
    <li>
      <a href="#6">6. 选择键盘类型（保持默认即可）</a>
    </li>
    <li>
      <a href="#7_HTTP">7. 选择安装方式（这里我们选择HTTP）</a>
    </li>
    <li>
      <a href="#8_TCPIP">8. 配置TCP/IP设置</a>
    </li>
    <li>
      <a href="#9_CentOS_59URL_CentOS">9. 设置网络安装CentOS 5.9的URL (软件源站点和CentOS目录)</a>
    </li>
    <li>
      <a href="#10">10. 开始下载安装工具</a>
    </li>
    <li>
      <a href="#11_CentOS">11. CentOS安装欢迎屏幕</a>
    </li>
    <li>
      <a href="#12">12. 进行分区操作</a>
    </li>
    <li>
      <a href="#13">13. 配置启动引导工具</a>
    </li>
    <li>
      <a href="#14">14. 配置网络</a>
    </li>
    <li>
      <a href="#15">15. 选择时区</a>
    </li>
    <li>
      <a href="#16_root">16. 设置root密码</a>
    </li>
    <li>
      <a href="#17">17. 选择需要安装的软件包</a>
    </li>
    <li>
      <a href="#18">18. 开始安装已选择的软件包</a>
    </li>
    <li>
      <a href="#19">19. 安装结束，重启</a>
    </li>
    <li>
      <a href="#20">20. 系统成功启动至命令提示符</a>
    </li>
  </ul>
</div>

#### <span id="1">1. 下载网络安装镜像</span> {#1}

**选择这里的镜像:**

  * [i386 version](http://mirrors.sohu.com/centos/5.9/isos/i386/CentOS-5.9-i386-netinstall.iso) 
  * [x86_64 version](http://mirrors.sohu.com/centos/5.9/isos/x86_64/)

**下载ISO镜像文件** 

  * CentOS-5.9-i386-netinstall.iso 
  * CentOS-5.9-x86_64-netinstall.iso

#### <span id="2_CentOSCD">2. 刻录镜像文件与使用CentOS安装CD引导计算机</span> {#2centoscd}

检查CentOS镜像的MD5，之后使用你喜欢的工具刻录至光盘。最后，使用CentOS安装光盘引导计算机。 

#### <span id="3_GRUB_NetInstall">3. 在GRUB菜单中选择网络安装 (NetInstall)或直接按回车键开始引导系统</span> {#3grubnetinstall}

[![Centos 5.9 Netinstall boot](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/35b240bfa6c894775e882f9c380bd853.png?resize=560%2C420 "Centos 5.9 Netinstall boot")](https://i1.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.6-netinstall-boot.png "Centos 5.9 Netinstall boot")

#### <span id="4_CentOS_59">4. 启动CentOS 5.9安装工具</span> {#4centos59}

[![Booting CentOS 5.9 Installer](https://i2.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/e840bdb0d402680e8e2edaf9ab3f1b00.png?resize=560%2C384 "Booting CentOS 5.9 Installer")](https://i1.wp.com/media.if-not-true-then-false.com/2010/03/02-centos-network-installation-booting.png "Booting CentOS 5.9 Installer")

#### <span id="5_Chinese_SimplifiedEnglish">5. 选择语言（Chinese Simplified简体中文或者English英语）</span> {#5chinesesimplifiedenglish}

[![Choose a Language](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/4bf5f0077cbaf7c809ba71c8bdc4d6ca.png?resize=400%2C354 "Choose a Language")](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/4bf5f0077cbaf7c809ba71c8bdc4d6ca.png "Choose a Language")

#### <span id="6">6. 选择键盘类型（保持默认即可）</span> {#6}

[![Select keyboard type](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/6ccb70516bc95fbde240038f55006575.png?resize=400%2C332 "Select keyboard type")](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/6ccb70516bc95fbde240038f55006575.png "Select keyboard type")

#### <span id="7_HTTP">7. 选择安装方式（这里我们选择HTTP）</span> {#7http}

[![Select Network Installation Method](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/505d663465a1107c4a90cb1948be4dce.png?resize=400%2C301 "Select Network Installation Method")](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/505d663465a1107c4a90cb1948be4dce.png "Select Network Installation Method")

#### <span id="8_TCPIP">8. 配置TCP/IP设置</span> {#8tcpip}

（勾选Enable IPv4 support,如果没有IPv6网络，可不勾选Enable IPv6 Support）

[![Configure TCP/IP settings](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/101121c9043cae25a95438ef09575737.png?resize=512%2C305 "Configure TCP/IP settings")](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/101121c9043cae25a95438ef09575737.png "Configure TCP/IP settings")

#### <span id="9_CentOS_59URL_CentOS">9. 设置网络安装CentOS 5.9的URL (软件源站点和CentOS目录)</span> {#9centos59urlcentos}

**Web site name:** 
   
mirror.sohu.com

**CentOS directory for i386:** 
   
/centos/5.9/os/i386/ 
   
or 
   
/centos/5/os/i386

**CentOS directory for x86_64:** 
   
/centos/5/os/x86_64 
   
or 
   
/centos/5.9/os/x86_64

[![CentOS 5 Netinstall url (http setup)](https://i2.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/1859f06c394f5559aa78752870bed1a6.png?resize=560%2C310 "CentOS 5 Netinstall url (http setup)")](https://i2.wp.com/media.if-not-true-then-false.com/2010/03/centos-5-netinstall-url-http-setup.png "CentOS 5 Netinstall url (http setup)")

#### <span id="10">10. 开始下载安装工具</span> {#10}

[![Downloading Installer](https://i2.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/b68f4296cf3b67447c0bca133c36a3b0.png?resize=526%2C109 "Downloading Installer")](https://i2.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/b68f4296cf3b67447c0bca133c36a3b0.png "Downloading Installer")

#### <span id="11_CentOS">11. CentOS安装欢迎屏幕</span> {#11centos}

[![CentOS 5.9 Welcome to CentOS](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/2491b8027f3edd12e82686cb66e23078.png?resize=560%2C420 "CentOS 5.9 Welcome to CentOS")](https://i0.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-welcome-to-centos.png "CentOS 5.9 Welcome to CentOS")

#### <span id="12">12. 进行分区操作</span> {#12}

若打算修改分区大小，文件系统等选项，请选择“Create custom layout”选项. 
  
[![CentOS 5.9 Select partitioning type](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/43b823b86d55ffe7c4c43216fb456795.png?resize=560%2C420 "CentOS 5.9 Select partitioning type")](https://i0.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-select-partitioning-type.png "CentOS 5.9 Select partitioning type")

现在就可以开始修改分区了. 
  
[![CentOS 5.9 Partition layout](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/2ece2c5f1ed69faad1b80ac58db58d43.png?resize=560%2C420 "CentOS 5.9 Partition layout")](https://i0.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-partition-layout.png "CentOS 5.9 Partition layout")

#### <span id="13">13. 配置启动引导工具</span> {#13}

[![CentOS 5.9 Bootloader setup](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/cac1ecc898e80df2736c2c9c2e0ea0e3.png?resize=560%2C420 "CentOS 5.9 Bootloader setup")](https://i2.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-bootloader-setup.png "CentOS 5.9 Bootloader setup")

#### <span id="14">14. 配置网络</span> {#14}

[![CentOS 5.9 Network setup](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/d110f9d249901362a66d01675207701b.png?resize=560%2C420 "CentOS 5.9 Network setup")](https://i2.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-network-setup.png "CentOS 5.9 Network setup")

#### <span id="15">15. 选择时区</span> {#15}

[![CentOS 5.9 Timezone setup](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/aee41d88f6ed82d0424b4d9e3ab7401a.png?resize=560%2C420 "CentOS 5.9 Timezone setup")](https://i1.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-timezone-setup.png "CentOS 5.9 Timezone setup")

#### <span id="16_root">16. 设置root密码</span> {#16root}

[![CentOS 5.9 Set root password](https://i2.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/b88abd80220d82cc23eae31ad0ccec70.png?resize=560%2C420 "CentOS 5.9 Set root password")](https://i1.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-set-root-password.png "CentOS 5.9 Set root password")

#### <span id="17">17. 选择需要安装的软件包</span> {#17}

[![CentOS 5.9 Select installation type](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/cf11eaefa2b2fd9d200a2261085a6fea.png?resize=560%2C420 "CentOS 5.9 Select installation type")](https://i2.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-select-installation-type.png "CentOS 5.9 Select installation type")

#### <span id="18">18. 开始安装已选择的软件包</span> {#18}

[![CentOS 5.9 Installing selected packages](https://i0.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/4a39de7529b95cdd8399ab6ff102dfcc.png?resize=560%2C420 "CentOS 5.9 Installing selected packages")](https://i0.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-installing-centos-5.7.png "CentOS 5.9 Installing selected packages")

#### <span id="19">19. 安装结束，重启</span> {#19}

[![CentOS 5.9 Installation complete](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/3bac40f27fe3f2fd7d4b85e8490de35d.png?resize=560%2C420 "CentOS 5.9 Installation complete")](https://i0.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.7-installation-complete.png "CentOS 5.9 Installation complete")

#### <span id="20">20. 系统成功启动至命令提示符</span> {#20}

[![CentOS 5.9 Shell Login](https://i1.wp.com/www.bestzhou.org/wp-content/uploads/2013/08/d398475a36340fc856074fe1d565cca6.png?resize=560%2C311)](https://i0.wp.com/media.if-not-true-then-false.com/2010/03/centos-5.9-shell-login.png)