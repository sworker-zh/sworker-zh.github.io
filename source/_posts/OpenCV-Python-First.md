---
title: OpenCV-Python（一）：从处理图片开始
date: 2018-03-21 19:30:00
tags: OpenCV-Python 
categories: 技术翻译
---

# 从处理图片开始

### 目标

> * 如何读取，显示和保存图片
>
> * 需要用到的函数：cv2.imread\(\), cv2.imshow\(\), cv2.imwrite\(\)
>
> * 可选项，学习如何通过Matplotlib显示图像

### 使用OpenCV

#### 读取一个图片

利用cv2.imread\(\)函数读取一张图片。

该函数需要两个参数：

1. 图片所在的文件路径，如果没有返回none。

2. 读片读取的方式。

> * cv2.IMREAD\_COLOR:以彩色的方式读取
>
> * cv2.IMREAD\_GRAYSCALE:以灰度的方式读取
>
> * cv2.IMREAD\_UNCHANGED:以alpha通道的方式读取

<!------------------------more------------------------------------>

#### 注意

可以用 1， 0， -1代替第二个参数。

代码如下：

```
import numpy as np
import cv2

#以色彩的方式读取图片
img = cv2.imread('test.jpg',0)
```

#### 显示一张图片

利用cv2.imshow\(\)这个函数在窗口上显示图片。显示窗口会自动适应图片的大小。

1. 第一个参数是窗口的名字，是一个string类型。

2. 第二个参数就是图片的名字。

你可以同时创建多个不同窗体名字的窗体来显示图片。

```
cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindow()
```

cv2.waitKey\(\)是一个键盘绑定函数。参数是毫秒。这个函数是在这个时间段内等待键盘事件。如果在这个时间段内键盘的按键被触发，那么程序继续。如果是0则继续等待。

cv2.destroyAllWindows\(\)直接删除我们创建的所有的窗口。如果想删除指点的窗口，那么将窗口名字作为参数传入函数cv2.destroyWindow\(\)。

#### 注意

可以通过函数cv2.namedWindow\(\)这个函数来决定窗体的大小是否可变。默认情况下，参数是cv2.WINDOW\_AUTOSIZE。如果将这个参数修改为cv2.WINDOW\_NORMAL，那么就能修改窗体的大小。

代码如下

```
cv2.namedWindow('image', cv2.WINDOW_NORMAL)
cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindow()
```

#### 保存图片

利用cv2.imwrite\(\)函数来保存图片

1. 第一个参数是保存的图片名。

2. 第二个参数是想保存的图片。

```
cv2.imwrite('saveImg.png', img)
```

### 总结

下述代码总结了通过灰度的方式读取、显示、保存图片的方式。按下‘s’保存并退出，按下ESC直接退出。

```
import numpyt as np
import cv2

img = cv2.imread('test.jpg', 0)
cv2.imshow('image', img)
k = cv2.waitKey(0)
if k == 27:
cv2.destroyAllWindow()
elif k == ord('s'):
cv2.imwrite('saveImg.jpg', img)
cv2.destroyAllWindow()
```

### 警告

如果你使用的是64位操作系统，那么需要将k = cv2.waitKey\(0\)这一行修改为 k = cv2.waitKey\(0\) & 0xFF

#### 使用Matplotlib

Matplotlib是Python的一个画图库，它提供了很多的方法。你会在接下来的几章中接触到它们。现在，先学习如何使用Matplotlib来显示一张图片。你可以通过它实现放大缩小或者保存图片等操作。

```
import numpy as np
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('test.jpg', 0)
plt.imshow(img, cmp = 'gay', interpolation = 'bicubic')
plt.xticks([]), plt.yticks([])
plt.show()
```


