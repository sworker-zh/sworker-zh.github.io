---
title: 用pyinstallyer打包pyqt5时遇到的问题
date: 2018-11-27 15:30:00
tags: [python, pyqt5, pyinstaller]
categories: 技术整理
---

# 简单说明

python在创建小程序的时候总是能体现出它的高效,这点在写小界面的时候也同样适用。然而,写好的程序必须在编译环境下运行的问题却是要想办法解决的。PyInstaller提供了一个解决方案,但是在打包的过程中会出现各种问题,以下记录自己遇到的问题及解决方案。

<!-------------more-------------->
## PyInstaller的使用方法

### 安装
> 1. 通过pip进行安装--pip install PyInstaller

> 2. 从网上下载安装包进行安装(既然pip那么方便,还是用上述方法安装吧)

### 使用
> 打开终端,进入程序文件所在的目录,输入pyinstaller -F fileName.py

>同时可以加入不同的参数 -c代表有console窗口,-w表示只用运行的windows窗口

## 问题及解决

### 问题一  No platform

在编译的时候如果不注意,只看最后一行"Building EXE from ... completed successfuly"的话就觉得成功了,可以运行exe文件的时候就会弹出"This application failed to start because no Qt platform ...."等消息.

这个问题是由于编译的时候没有包含框架插件所导致的。如果是本机使用,可以在程序中包含这个路径就好

> 通过QApplication.addLibraryPath(pluginsPath)实现
pluginsPath是pyqt5的path
pyqt_plugins = os.path.dirname("C:/../Python/Python36/site-packages/PyQt5/Qt/plugins/")

> 将上面提到的plugins文件夹下的platforms和imageformats两个文件拷贝的生成的exe同目录下(这个方法适用于将软件分发给别人使用)

### 问题二  No Module PyQt5.sip

虽然按照上述方法处理了,但是还是不能运行。此时用pyinstaller打包的时候添加-c选项,这样就能在弹出的console中看到相应的问题。这个console总是一闪而过,忧伤。

发现缺少PyQt5.sip的模块,选择使用pip进行安装。
> pip install PyQt5.sip

你以为这样就能结束么,并不是,可能我比较衰,还是不行。查找了好久的问题,可能时SIP的版本不匹配,所以有用pip重新安装SIP

> pip install SIP

### 问题三 无法读取相对路径内容

由于刚开始用小程序进行测试,走到这一步发现成功了,就急忙拿写好的程序进行打包,结果,又出问题了。

> "Failed to execute script ..."

再次使用-c参数进行查看,发现找不到指定文件(当时使用相对路径)。一气之下把样式表和图片路径全删了,再打包运行,好了。

找到问题就不怕了。首先尝试用绝对路径,完全没有问题,但是在分发软件的时候,这个路径可不是绝对的呀。

通过os.getcwd()的方法先获取当前路径,再用os.path.join()的方法将文件路径包含进去。

# 到此我遇到的所有问题都解决了