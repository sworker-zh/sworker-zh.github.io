---
title: ssh-key和git
date: 2018-03-29 14:30:00
tags: [git, ssh]
categories: 技术整理
---

# 简单说明

现在流行的基于git进行[版本控制](https://zh.wikipedia.org/wiki/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)的软件源代码托管服务有很多，而这些服务需要通过[ssh](https://zh.wikipedia.org/wiki/Secure_Shell)的方式进行连接。ssh可以通过用户名+密码的方式或ssh-key的方式进行连接，而每次输入用户名和密码会比较繁琐，所以配置好ssh密钥后会省下不少麻烦。

<!----------------more-------------->

## 安装git

### 在Windows下进行安装

首先到[下载页面](https://git-scm.com/download/win)下载相关的安装包，然后进行安装。

### 在ubuntu下进行安装

> sudo apt-get update
> sudo apt-get install git

## 配置git

安装完git客户端后，需要对其进行简单的配置。主要就是配置用户名和邮箱。

### 配置用户名和邮箱
> git config --global user.name "Your Name"
> git config --global user.email "Your Email"

## 生成新的SSH key

1.打开新的Git Bash

2.运行下列代码，在双引号里面填上自己的邮箱

> $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
这将以上面的邮箱作为标签创建一个新的ssh key
Generating public/private rsa key pair.

3.当你被询问将key存放在何处的时候（"Enter a file in which to save the key"），按下回车键，这个是将文件保存在默认路径下。

> Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[press enter]

4.接下来就是被要求输入密码，如果不需要直接按两下“Enter”键。

> Enter passphrase (empty for no passphrase):[Type a passphrase]
> Enter same passphrase again:[Type passphrase again]

## 备注

这样新的sshkey就被创建好了，在windows操作系统中，文件被在存放在/c/user/you/.ssh/id_rsa中。