---
id: 336
title: 关于引用控件值的问题
date: 2006-04-26T03:34:11+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=336
permalink: /in-regard-to-the-issue-of-control-values/
categories:
  - 我的世界
tags:
  - 'c#'
---
昨天在写类的时候，遇到了一个问题，就是如何在类中引用form里面控件的问题。我问了几个朋友，但是给出的方法都存在问题。下面是刘工给我的一个解决办法。

1。首先需要在Program.cs里面，main()函数以外，声明如下语句：

<div class="UBBPanel">
  <div class="UBBTitle">
    <img src="images/code.gif" alt="程序代码" title="" /> 程序代码
  </div>
  
  <div class="UBBContent">
    public static Form1 frmMain;
  </div>
</div>

通常在main()函数之上。 

昨天在写类的时候，遇到了一个问题，就是如何在类中引用form里面控件的问题。我问了几个朋友，但是给出的方法都存在问题。下面是刘工给我的一个解决办法。

1。首先需要在Program.cs里面，main()函数以外，声明如下语句：

<div class="UBBPanel">
  <div class="UBBTitle">
    <img src="images/code.gif" alt="程序代码" title="" /> 程序代码
  </div>
  
  <div class="UBBContent">
    public static Form1 frmMain;
  </div>
</div>

通常在main()函数之上。 
  
2。在main()函数内声明如下语句 
  
<textarea class="code" cols="50" rows="10">frmMain = new Form1(); <br /> Application.Run(frmMain); <br /> 或者 <br /> Application.Run(frmMain = new Form1()); <br /> </textarea> 
  
3。在win app窗体内将需要传递值的控件的modifiers属性改为Public，因为只有是public访问修饰符时，才可以在任何地方访问。 
  
4。在类中调用控件值。使用下面的代码：

<div class="UBBPanel">
  <div class="UBBTitle">
    <img src="images/code.gif" alt="程序代码" title="" /> 程序代码
  </div>
  
  <div class="UBBContent">
    Program.frmMain.txtBox1.Text…..
  </div>
</div>

代码仅供参考，如果哪位朋友有更好的方法，我愿闻其详。:-)