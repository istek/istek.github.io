---
id: 1436
title: CentOS安装php加速软件Zend Optimizer 3.3.9
date: 2012-07-05T19:40:33+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1436
permalink: /centos-php-zend-optimizer-3-3-9-for-php-5-2/
categories:
  - 我的世界
tags:
  - CentOS
  - php
---
**引言：** 
   
php程序代码被加密过后，必须安装解密软件Zend Optimizer才能进行使用，比如Shopex等php程序，下面我们安装Zend Optimizer 3.3.9（针对php5.2,X之前的版本,php5.3.X需要安装Zend Guard），操作如下：

<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#1Zend_optimizer">1、下载Zend optimizer</a>
    </li>
    <li>
      <a href="#2Zend_optimizer">2、安装Zend optimizer</a>
    </li>
    <li>
      <a href="#3Zend_optimizer">3、配置Zend optimizer</a>
    </li>
    <li>
      <a href="#4web">4、重启web服务器</a>
    </li>
  </ul>
</div>

### <span id="1Zend_optimizer">1、下载Zend optimizer</span> {#1zendoptimizer}

    cd /home  
    wget http://downloads.zend.com/optimizer/3.3.9/ZendOptimizer-3.3.9-linux-glibc23-i386.tar.gz #32位  
    wget http://downloads.zend.com/optimizer/3.3.9/ZendOptimizer-3.3.9-linux-glibc23-x86_64.tar.gz #64位  
    

### <span id="2Zend_optimizer">2、安装Zend optimizer</span> {#2zendoptimizer}

    mkdir /usr/zend #建立Zend Optimizer安装目录  
    tar xvfz ZendOptimizer-3.3.9-linux-glibc23-i386.tar.gz #解压安装文件  
    cp /home/ZendOptimizer-3.3.9-linux-glibc23-i386/data/5_2_x_comp/ZendGuardLoader.so /usr/zend #拷贝文件到安装目录  
    rm -rf /home/ZendOptimizer-3.3.9-linux-glibc23-i386* #删除安装包  
    

### <span id="3Zend_optimizer">3、配置Zend optimizer</span> {#3zendoptimizer}

    cp /etc/php.ini /etc/php.inibak #修改之前先备份  
    vi /etc/php.ini #编辑文件  
    

在最后位置添加以下内容

    [Zend Optimizer]
    zend_optimizer.optimization_level=15 zend_extension="/usr/zend/ZendOptimizer.so"  
    

### <span id="4web">4、重启web服务器</span> {#4web}

    /etc/init.d/httpd restart