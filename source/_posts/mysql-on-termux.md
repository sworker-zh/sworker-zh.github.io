---
title: 在termux上运行mysql
date: 2019-05-06 15:04:00
tags: [mysql, termux, mariadb]
categories: 技术整理
---

# 本地安装

1. 安装termux并更新
> * apt update
> * apt upgrade 

2. 安装Mariadb(mysql)
> * pkg install mariadb

3. 进入模拟etc目录进行配置操作
> * cd /data/data/com.termux/files/usr/etc/

4. 创建文件夹（如果上述目录中存在该文件夹则跳过）
> * mkdir my.cnf.d

5. 安装数据库
> * mysql_install_db

<!------------------------more----------------------------->

6. 安装好以后，开启数据库
> * mysqld_safe -u root &

7. 本地测试是否创建成功
> * mysql -u root 

8. 通过查看目前存在的数据库判断是否创建并登录成功
> * show databases;

如果一切顺利，则能看到以下结果：

![](http://ww1.sinaimg.cn/large/902f6897ly1g2rmfz4iwtj20cn08275k.jpg)

# 配置文件

### 以上安装的方式是默认没有密码的，只能本地操作，所以要修改配置文件

1. 准备修改配置文件
> * mysql_secure_installation

2. 第一处暂停，这个地方要求输入root密码，由于是第一次修改配置文件，此时还没有密码，直接回车（enter for none）

3. 第二处暂停，询问是否设置root密码。输入‘y’并回车。

4. 第三处暂停，第一次输入设置的root密码，并回车。

5. 第四次暂停，第二次输入root密码（与第一次保持一致），并回车。

6. 第五次暂停，询问是否删除匿名用户，输入‘y’并回车。

7. 第六次暂停，询问是否不允许用root从远程登录。如果需要从远程登录输入‘n’并回车否则输入‘y’并回车。

8. 第七次暂停，询问是否删除test数据库，这个按需要选择。

9. 最后询问是否重建权健表，当然是‘y’啦。

# 设置远程登录

### 至此，通过root登录mysql的时候需要输入密码了，但如果想远程（局域网）登录的话还是会有问题的，接下来解决这个问题。

1. 本地登录数据库
> * mysql -u root -p
> * Enter password:

 - 密码为刚刚设置的密码

 2. 切换数据库为mysql
 > * USE mysql;

 3. 从user表中查看主机、用户和密码
 > * select host, user, password from user;

 4. 将localhost的host值改为%
 > * UPDATE user SET host='%' WHERE host='localhost';

 5. 刷新用户权限表
 > * FLUSH PRIVILEGES;

 ![](http://ww1.sinaimg.cn/large/902f6897ly1g2rn2m8x7oj20d80eyq3e.jpg)

 完成以上操作就可以在局域网内远程登录操作数据库啦，随身的mysql数据库。