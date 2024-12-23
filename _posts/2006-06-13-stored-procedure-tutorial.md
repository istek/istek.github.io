---
title: 存储过程的教程
date: 2006-06-13T12:36:35+00:00
layout: post
categories:
  - Windows
tags:
  - Oracle
---

什么是存储过程呢？

定义：

将常用的或很复杂的工作，预先用SQL语句写好并用一个指定的名称存储起来, 那么以后要叫数据库提供与已定义好的存储过程的功能相同的服务时,只需调用execute,即可自动完成命令。

讲到这里,可能有人要问：这么说存储过程就是一堆SQL语句而已啊？

Microsoft公司为什么还要添加这个技术呢?

那么存储过程与一般的SQL语句有什么区别呢?

存储过程的优点：

1. 存储过程只在创造时进行编译，以后每次执行存储过程都不需再重新编译，而一般SQL语句每执行一次就编译一次,所以使用存储过程可提高数据库执行速度。
2. 当对数据库进行复杂操作时(如对多个表进行Update,Insert,Query,Delete时），可将此复杂操作用存储过程封装起来与数据库提供的事务处理结合一起使用。
3. 存储过程可以重复使用,可减少数据库开发人员的工作量
4. 安全性高,可设定只有某此用户才具有对指定存储过程的使用权

存储过程的种类：

1.系统存储过程：以sp_开头,用来进行系统的各项设定.取得信息.相关管理工作,

如 sp_help就是取得指定对象的相关信息

2.扩展存储过程 以XP_开头,用来调用操作系统提供的功能

exec master..xp_cmdshell &#8216;ping 10.8.16.1&#8217;

3.用户自定义的存储过程,这是我们所指的存储过程

常用格式
```
Create procedure procedue_name

\[@parameter data_type\]\[output\]

[with]{recompile|encryption}

as

sql_statement
```
解释:

output：表示此参数是可传回的

with {recompile|encryption}

recompile:表示每次执行此存储过程时都重新编译一次

encryption:所创建的存储过程的内容会被加密

如:

表book的内容如下

编号 书名 价格

001 C语言入门 $30

002 PowerBuilder报表开发 $52

实例1:查询表Book的内容的存储过程
```
create proc query_book

as

select * from book

go

exec query_book
```
实例2:加入一笔记录到表book,并查询此表中所有书籍的总金额
```
Create proc insert_book

@param1 char(10),@param2 varchar(20),@param3 money,@param4 money output

with encryption ———加密

as

insert book(编号,书名，价格） Values(@param1,@param2,@param3)

select @param4=sum(价格) from book

go
```
执行例子:
```
declare @total_price money

exec insert_book &#8216;003&#8217;,&#8217;Delphi 控件开发指南&#8217;,$100,@total_price

print &#8216;总金额为&#8217;+convert(varchar,@total_price)

go
```
存储过程的3种传回值:

1. 以Return传回整数
2. 以output格式传回参数
3. Recordset

传回值的区别:

output和return都可在批次程式中用变量接收,而recordset则传回到执行批次的客户端中

实例3：设有两个表为Product,Order,其表内容如下：

Product

|产品编号| 产品名称 |客户订数 |
|--------|----------|---------|
|001 |钢笔| 30 |
|002 |毛笔| 50 |
|003 |铅笔| 100 |

order

|产品编号| 客户名 |客户订金 |
|--------|----------|---------|
|001 |南山区| \$30 |
|002 |罗湖区| \$50 |
|003 | 宝安区| \$4 |

请实现按编号为连接条件,将两个表连接成一个临时表,该表只含编号.产品名.客户名.订金.总金额,

总金额=订金*订数,临时表放在存储过程中

代码如下:
```
Create proc temp_sale

as

select a.产品编号,a.产品名称,b.客户名,b.客户订金,a.客户订数* b.客户订金 as总金额

into #temptable from Product a inner join order b on a.产品编号=b.产品编号

if @@error=0

print &#8216;Good&#8217;

else

print &#8216;Fail&#8217;

go
```