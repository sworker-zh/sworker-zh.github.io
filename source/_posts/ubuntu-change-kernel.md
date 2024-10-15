---
title: ubuntu更换内核并开启bbr加速
date: 2018-04-08 11:01:00
tags: [ubuntu, bbr, vps]
categories: 技术借鉴
---

# Ubuntu更改内核并开启TCP-BBR实现高效单边加速

## 前言

国内由于墙的原因，很多人选择用国外VPS搭建梯子来实现科学上网，但是由于距离的原因，网速经常十分感人，所以就想用其余的方法来实现网速的增加。之前大家会选择锐速来实现这样的功能，但是现在锐速的价格稍微有点点高啦。所以横空出世的拥塞控制算法BBR就广受好评啦。

<!----------------more------------------>

## 检查内核

由于BBR是集成在内核中的，所以只有当内核版本在4.9以上的操作系统才能开启BBR加速。

### 查看当前系统内核
```
uname -a
```
#### 注：如果内核版本已经符合要求，则可以跳过以下步骤

### 下载合适内核，[更多内核](http://kernel.ubuntu.com/~kernel-ppa/mainline)
```
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9/linux-image-4.9.0-040900-generic_4.9.0-040900.201612111631_amd64.deb
```

### 安装内核
```
dpkg -i linux-image-4.9*.deb
```

### 查看已经安装的内核
```
dpkg -l | grep linux-image
```

### 卸载旧内核
```
apt-get purge 旧内核
```

#### 旧内核也可以不用卸载，Ubuntu会默认启动4.9

### 更新grub系统引导文件并重启
```
update-grub
reboot
```

## 开启与关闭BBR

### 修改系统变量
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
```

### 保存生效
```
sysctl -p
```

### 执行
```
sysctl net.ipv4.tcp_available_congestion_control
```

如果结果入下，则开启成功。
```
root@linux:~# sysctl net.ipv4.tcp_available_congestion_control
net.ipv4.tcp_available_congestion_control = bbr cubic reno
```

### 查看
```
lsmod | grep bbr
```

### 关闭
```
sed -i "net.core.default_qdisc=fq/d" /etc/sysctl.conf
sed -i "net.ipv4.tcp_congestion_control=bbr/d" >> /etc/sysctl.conf
sysctl -p
```
执行上述代码后要重启VPS才能关闭bbr。

## [参考](https://www.longsays.com/2118.html)