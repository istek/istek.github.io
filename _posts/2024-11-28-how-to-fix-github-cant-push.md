---
title: "Github无法push代码报Connection closed"
date: 2024-11-28T10:50:32+08:00
draft: false
categories:
 - Windows
tags:
 - GitHub
featured_image: 
---

今天在更新博客文章时，发现无法push代码，错误如下：

```
Connection closed by 140.82.116.3 port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
<!--more-->
Google了一下错误信息，反正肯定是和防火墙有关，禁止了我们访问GitHub的22端口，通过查看GitHub的文档，可以使用HTTPS端口进行push代码。

## 测试HTTPS端口的SSH是否可行

首先，要测试通过 HTTPS 端口的 SSH 是否可行，请运行以下 SSH 命令：：
```
ssh -T -p 443 git@ssh.github.com
```
返回如下信息，表示你可以使用这个办法去规避22端口无法push的问题：
```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

## 修改SSH配置文件

打开`.ssh/config`文件，增加如下内容：
```
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
```

## 测试

### Clone代码

```
git clone ssh://git@ssh.github.com:443/YOUR-USERNAME/YOUR-REPOSITORY.git
```

### Push代码

```
git push
```
