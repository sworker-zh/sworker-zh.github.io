---
title: 在ubuntu下安装nodejs
date: 2018-03-28 10:30:00
tags: [ubuntu, nodejs]
categories: 技术整理
---

# nodejs的安装

## 简单说明

nodejs作为轻量级的服务器，现在的使用也是非常常见的。但是在ubuntu下进行安装的时候，如果使用自带的“sudo apt-get install nodejs”和“sudo apt-get install nodejs-legacy”的话出现的问题就是不是最新版本的，而hexo需要的版本是要在4以上的，所以要采取以下安装方式。

<!-------------more---------------->

### 通过curl方法安装

#### 安装8.x版本
> curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
> sudo apt-get install -y nodejs

#### 安装9.x版本
> curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
> sudo apt-get install -y nodejs

### 通过压缩文件进行安装

#### 下载nodejs压缩文件
> wget https://nodejs.org/dist/v9.1.0/node-v9.1.0-linux-x64.tar.xz

#### 解压
> tar -xvf node-v9.1.0-linux-x64.tar.xz

#### 将node和npm设置为全局模式
> sudo ln /home/ubuntu/node-v9.1.0-linux-x64/bin/node /usr/local/bin/node
> sudo ln /home/ubuntu/node-v9.1.0-linux-x64/bin/npm /usr/local/bin/npm