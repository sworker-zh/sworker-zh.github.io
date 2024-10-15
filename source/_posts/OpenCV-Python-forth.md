---
title: OpenCV-Python（四）：通过滑块修改颜色
date: 2018-03-24 10:30:00
tags: OpenCV-Python 
categories: 技术翻译
---

目标
----

> * 学习将滑块绑定到OpenCV的窗体
> * 学习cv2.getTrackbarPos(), cv2.createTrackbar()等函数

### 代码部分

我们将创建一个显示由你指定颜色的应用程序。这个应用程序上有一个显示颜色的窗体和三个调色的滑块，这三个滑块分别对应着RGB三色的值。当你滑动滑块的时候，窗体的颜色也会随之改变。在默认情况下，窗体的颜色是黑色的。

对于函数cv2.getTrackbarPos()函数而言，各参数的含义如下：
1. 滑块的名字
2. 相关联的窗体的名字
3. 默认参数
4. 最大值
5. 回调函数，每次滑块值被改变是被调用

回调函数将滑块的位置作为默认参数。在我们的案例中，由于函数并没有执行任何操作，所以我们简单的带过。

另外一点，滑块可以被当做按钮或者开关使用。在默认情况下，OpenCV并没有按钮相关的函数，因此，你可以将这个当做相关的函数。在我们的应用程序中，我们创建了一个开关用于控制这个程序是否有效。当开关为ON时，可以调整颜色，否则窗体一直为黑色。
<!------------------more------------>

``` python
#coding:utf-8
import cv2
import numpy as np

def nothing(x):
pass

img = np.zeros((300, 512, 3), np.uint8)
cv2.namedWindow('image')

cv2.createTrackbar('R', 'image', 0, 255, nothing)
cv2.createTrackbar('G', 'image', 0, 255, nothing)
cv2.createTrackbar('B', 'image', 0, 255, nothing)

switch = '0 : OFF\n1 :ON'
cv2.createTrackbar(switch, 'image', 0, 1, nothing)

while(1):
cv2.imshow('image', img)
k = cv2.waitKey(1) & 0xFF
if k == 27:
break

r = cv2.getTrackbarPos('R', 'image')
g = cv2.getTrackbarPos('G', 'image')
b = cv2.getTrackbarPos('B', 'image')
s = cv2.getTrackbarPos(switch, 'image')

if s == 0:
img[:] = 0
else:
img[:] = [b,g,r]

cv2.destroyAllWindows()
```

### 练习
创建一个可变颜色的画板应用，利用前一章学到的画画技巧。