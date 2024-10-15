---
title: 在ubuntu下nginx的安装与卸载
date: 2018-03-28 11:30:00
tags: [ubuntu, nginx]
categories: 技术整理
---

# 简单说明

nginx（发音同engine x）是一个异步框架的 Web服务器，也可以用作反向代理，负载平衡器 和 HTTP缓存。Nginx是一款免费的开源软件，根据类BSD许可证的条款发布。一大部分Web服务器使用Nginx，通常作为负载均衡器。（摘自[维基百科](https://zh.wikipedia.org/wiki/Nginx)）

<!---------------more----------->
## 在ubuntu上安装与使用nginx

> sudo apt-get update
> sudo apt-get install nginx

### 启动nginx
> sudo /etc/init.d/nginx start

### 停止nginx
> sudo /etc/init.d/nginx stop

### 重启nginx
> sudo /etc/init.d/nginx restart

### 配置nginx
> sudo vi /etc/nginx/sites-available/default

## 在ubuntu上删除nginx

### 通过--purge参数删除配置文件
> sudo apt-get --purge remove nginx

### 自动删除不使用的软件包（全局）
> sudo apt-get autoremove

### 罗列并删除相关软件(sudo apt-get --purge remove xxx)
> dpkg --get-selections | grep nginx

