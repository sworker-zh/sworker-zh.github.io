---
title: QCustomPlot画图基础
date: 2019-08-15 10:15:00
tags: QT
categories: 技术整理
---

# QCutomPlot简介

## 官网网址及介绍
[https://www.qcustomplot.com/](https://www.qcustomplot.com/)

```
QCustomPlot is a Qt C++ widget for plotting and data visualization. It has no further dependencies and is well documented. This plotting library focuses on making good looking, publication quality 2D plots, graphs and charts, as well as offering high performance for realtime visualization applications. Have a look at the Setting Up and the Basic Plotting tutorials to get started.

QCustomPlot can export to various formats such as vectorized PDF files and rasterized images like PNG, JPG and BMP. QCustomPlot is the solution for displaying of realtime data inside the application as well as producing high quality plots for other media.
```
<!------------------more---------------------------------------->
其主要就是用来创建各种二维图，而且能够保存为很多文件格式。在QT下，是一个非常好用的画图库。

# 画图基本步骤

## 基本步骤是根据官网[Tutorials](https://www.qcustomplot.com/index.php/tutorials/settingup)完成的。

### 1. 先[下载](https://www.qcustomplot.com/index.php/download)需要的文档。里面包含了编程所需的.h和.cpp文件以及例子等文档。我选择了最新的下载，毕竟最新的解决了已知bug。
### 2. 在QT中新建一个WidgetApplication。
![](http://ww1.sinaimg.cn/large/005ULVkugy1g606q5k8glj30j209wwf1.jpg)
### 3. 将下载下来的“qcustomplot.h”和“qcustomplot.cpp”这两个文件添加到项目中
![](http://ww1.sinaimg.cn/large/005ULVkugy1g606x8e0iyj30iw09djrz.jpg)
### 4. 非常重要的一点--要在.pro文件中添加“printsupport”。添加的位置在“QT += widgets”的后面
![](http://ww1.sinaimg.cn/large/005ULVkugy1g6070omtw6j30ko0920t8.jpg)
### 5. 在ui中添加一个widget控件，然后将其进行提升，提升为QCustomPlot
![](http://ww1.sinaimg.cn/large/005ULVkugy1g6076fvnqfj30j60dhmx8.jpg)
### 6. 在“mainwidget.h”中包含一下“qcustomplot.h”头文件，这时就可以编译运行了，如果不出意外，得到
![](http://ww1.sinaimg.cn/large/005ULVkugy1g607fyy9yjj30db0b9mx0.jpg)
至此，准备工作都已经完成，接下来就是在坐标系上画图了。

## 正式画一个简单的图
### 先上效果图
![](http://ww1.sinaimg.cn/large/005ULVkugy1g607qy13wdj30o50cs3z5.jpg)

## 步骤
> 1. 创建x轴和y轴所需要的数据
> 2. 创建一个“画布”
> 3. 在画布上将x轴和y轴对应的数据设置好
> 4. 非必须，由于默认情况下x和y轴都是从“原点"(0,0)开始，所以为了显示全部图片，可以设置一下轴的范围
> 5. 将图画出

# 以上就是简单的画图，接着就是深入了解了。
# 通过设置可以让画的图被拖拽，通过鼠标中间键（滚轮）缩放，通过轴缩放等操作，但无法通过矩形选区缩放。
