---
title: OpenCV-Python（三）：OpenCV中画画函数
date: 2018-03-22 09:30:00
tags: OpenCV-Python 
categories: 技术翻译
---

目标
----

> * 利用OpenCV画出不同的形状
> * 将会学到以下函数：cv2.line(),cv2.circle(),cv2.rectangle(),cv2.ellipse(),cv2.putText()等

### 编码

在上述代码中，你将会用到以下常见的参数

> * img：你希望在哪里画出这个图像
> * color：图形的颜色，通过元组表示RGB，例如（255， 0， 0）代表蓝色
> * thickness：表示线或者圆等物体的厚度。如果参数为-1表示关闭这个功能，将画出圆。默认厚度为1
> * lineType：线的形状。

<!-------------------more------------------->

### 画一条线

为了画一条线，你需要从起点坐标画到终点坐标。我们将创建一个黑色背景的图并在上面从左上角到右下角画一条蓝色的线。

``` python
import numpy as np
import cv2

#创建一个黑色背景图
img = np.zeros((512, 512, 3), np.uint8)

#从左上角到右下角画一条5像素的蓝线
img = cv2.line(img, (0,0),(511,511),(255,0,0),5)
cv2.imshow('ShowImg', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```


将鼠标作为画笔
============

目标
-----

> * 学会如何在OpenCV中获取鼠标事件
> * 学会cv2.setMouseCallback()这个函数

### 简单的模型

我们将创建一个简单的应用，这个应用实现了当你在自己创建的画布上双击鼠标的时候，画布上会自动生成一个圆。

首先，我们创建一个当鼠标事件激发的时候被调用的回调函数。所谓的鼠标事件可以是任何与鼠标相关的事件，比如左击鼠标、双击鼠标等。它为我们提供每个鼠标在触发事件时鼠标的坐标（x，y）
。根据这个事件和坐标，我们可以完成任何自己想完成的事情。为了展示当前所有的鼠标事件，在控制台运行以下代码就可以实现。

``` python
import cv2
events = [i for i in dir(cv2) if 'EVENT' in i]
print events
```

创建的回调函数跟其余函数格式一样，唯一不同的是它所实现的功能。我们的鼠标回调函数只做一件事，那就是当我们双击鼠标的时候它能够画出一个圆。以下就是实现代码。

``` python
import cv2
import numpy as np

#创建一个鼠标回调函数
def draw_circle(event, x, y, flags, param):
if event == cv2.EVENT_LBUTTONDBLCLK:
cv2.circle(img, (x, y), 100, (255,0,0), -1)

img = np.zeros((512, 512, 3), np.uint8)
cv2.namedWindow('image')
cv2.setMouseCallback('image', draw_circle)

while(1):
cv2.imshow('image', img)
if cv2.waitKey(20) & 0xFF == 27:
break
cv2.destroyAllWindows()
```

更加高级的模型

现在我们创建更好玩的应用程序。在这个程序中，我们通过拖拽鼠标的方式像在画画软件上一样画出矩形或者圆形（这个取决于我们的选择）。也就是说，我们的鼠标事件回调函数包含两个方面，一个是画矩形而另一个是画圆形。这个程序将对我们创建和理解一些互动的应用程序很有帮助。

``` python
import cv2
import numpy as np

drawing = False
mode = True
ix, iy = -1, -1

def draw_circle(event, x, y, flags, param):
global ix, iy, drawing, mode

if event == cv2.EVENT_LBUTTONDOWN:
drawing = True
ix, iy = x, y

elif event == cv2.EVENT_MOUSEMOVE:
if drawing == True:
if mode == True:
cv2.rectangle(img, (ix, iy), (x, y), (0, 255, 0), -1)
else:
cv2.circle(img, (x, y), 5, (0,0,255), -1)

elif event == cv2.EVENT_LBUTTONUP:
drawing == False:
if mode == True:
cv2.rectangle(img, (ix, iy), (x, y), (0, 255, 0), -1)
else:
cv2.circle(img, (x, y), 5, (0,0,255), -1)
```

接着，我们必须将这个回调函数跟OpenCV的窗体进行绑定。在主循环函数中，我们需要设置一个‘m’键用于切换画矩形和画圆形。

``` python
img = np.zeros((512, 512, 3), np.uint8)
cv2.namedWindow('image')
cv2.setMouseCallback('image', img)

while(1):
cv2.imshow('image')
k = cv2.waitKey(1) & 0xFF
if k == ord('m'):
mode = not mode
elif k == 27:
break

cv2.destroyAllWindows()
```