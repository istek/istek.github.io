---
id: 524
title: 如何从web.config中取得ConnectionString
date: 2006-10-14T07:55:53+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=524
permalink: /how-do-i-get-the-connectionstring-from-the-web-config/
categories:
  - 我的世界
tags:
  - 'c#'
---
通过使用ConfigurationManager，你可以从Web.Config中取得ConnectionString。

Web.Config connectionStrings 配置部分: 

    <connectionStrings>  
    <add name="MyConnectionString" connectionString="Data Source=myDBServer;database=myDB;Integrated Security=True;" providerName="System.Data.SqlClient"/>  
    </connectionStrings>  
    

aspx 文件: 

    <asp:SqlDataSource ID="DS" runat="server" ConnectionString="<%$ ConnectionStrings:MyConnectionString %>  
    

代码中: 
  
C#代码: 

    string strConnString = ConfigurationManager.ConnectionStrings["MyConnectionString"].ConnectionString;  
    

VB.NET代码 

    Dim strConnString As String = ConfigurationManager.ConnectionStrings("MyConnectionString").ConnectionString