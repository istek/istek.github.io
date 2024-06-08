---
id: 1492
title: WIN7配置jdk
date: 2014-08-30T06:02:23+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1492
permalink: /configura-jdk-under-win7/
categories:
  - 我的世界
tags:
  - jdk
  - windows
---
## 配置环境变量 {#}

  1. 安装好后，直接右击【我的电脑】–【属性】，在弹出的对话框中选择【高级系统设置】 
  
      ![wpid-c48f3c05ea1841979db2ddaaa929bd45_6609c93d70cf3bc73733e052d100baa1cd112a581.jpg](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214652/1449229709_mz3hel.jpg?w=720)
  2. 在弹出的对话框中选择【高级】选项卡下的【环境变量】 
  
     ![wpid-c48f3c05ea1841979db2ddaaa929bd45_b2de9c82d158ccbf00ccbd9f19d8bc3eb13541261.jpg](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214652/14492297091_bddgxb.jpg?w=720)
  3. 在弹出的对话框，单击【系统变量】下的【新建】按钮，在弹出的对话框中填入变量名 **JAVA_HOME</strong>,变量值：`C:Program FilesJavajdk1.7.0_04` 
  
    <img src="https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214650/1449229710_syzsig.jpg?w=720" alt="wpid-c48f3c05ea1841979db2ddaaa929bd45_3812b31bb051f81977082cf4dab44aed2e73e75b1.jpg&#8221; title=&#8221;&#8221; /> </li> 
    
      * 按照同样的方式创建系统变量**classpath**，变量名为**classpath**，变量值为：`.;%JAVA_HOME%lib;%JAVA_HOME%libtools.jar`**(注意开头的.和;)** 
      * 还要在已有的系统变量`path`的变量值的**最后**加入以下变量值：`;%JAVA_HOME%bin;%JAVA_HOME%jrebin` 
      * 配置结束 </ol> 
    
    ## 验证是否成功 {#}
    
      1. 在D盘下新建文件夹test，并在文件夹中新建记事本文件，改名为Hello.java 并写入以下代码，保存： 
    
        public class Hello{  
        public static void main(String[]args){ System.out.println("Hello world!"); } }  
        
    
    2. 点击开始按钮，在搜索栏中输入`cmd`，直接按回车，打开命令窗口<img src="https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214651/14492297101_tga6b3.jpg?w=720" alt="wpid-c48f3c05ea1841979db2ddaaa929bd45_3bf33a87e950352a83100ab65343fbf2b2118b1e1.jpg&#8221; title=&#8221;&#8221; /> 
  
    3. java文件位置是 `D:/testHello.java`在命令窗口直接输入 `d:`来打开D盘 
  
    4. 输入`cd test`命令进入test文件夹 
  
    5. 输入`javac Hello.java`和`java Hello`，结果如下所示说明配置成功<img src="https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214649/1449229711_r36hfu.jpg?w=720" alt="wpid-c48f3c05ea1841979db2ddaaa929bd45_0e2442a7d933c89578feecded11373f0830200961.jpg&#8221; title=&#8221;&#8221; />