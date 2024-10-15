---
title: OpenCV-Python（二）：从处理视频开始
date: 2018-03-22 8:12:00
tags: OpenCV-Python 
categories: 技术翻译
---

# 从视频开始

### 目标

> * 学习读取、显示以及保存视频
>
> * 学习从摄像头上读取并显示视频
>
> * 将学到cv2.VideoCapture\(\),cv2.VideoWriter\(\)函数

### 从摄像头获取视频文件

经常的，我们需要通过摄像头抓取实时的画面。OpenCV为此提供了简单的接口。让我们通过摄像头（我使用的是笔记本自带的摄像头）来采集画面吧。我将它转换成了灰度视频，让我们从这个简单的测试开始吧。

为了采集视频，你需要先创建一个视屏采集\(VideoCapture\)对象。它的参数可以是摄像头的索引值或者摄像头的名字。如果通过索引值选择，记得索引值是从0开始的。最终，不要忘了释放资源（采集的工具）。

<!------------more--------------------->

```
import numpy as np
import cv2

cap = cv2.VideoCapture(0)

while(True):
#一个画面一个画面的采集
ret, frame = cap.read()

#我们窗体上显示的画面来源
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

#显示返回的画面
cv2.imshow('frame', gray)
if cv2.waitKey(1) & 0xFF == ord('q'):
break

#当所有的完成后，释放资源
cap.release()
cv2.destroyAllWindow()
```

cap.read\(\)返回一个布尔值（True/False）。如果一个帧能够被正确的读取，这个函数将返回真。所以，你可以通过检查这个函数的返回值来检查视频的结尾。



有的时候cap可能无法完成初始化。在这情况下，这段代码将返回错误。你可以通过cap.isOpened\(\)这个函数来检查它是否被成功的初始化，如果返回值为真，则表示初始化成功，否则使用cap.open\(\)。



你可以通过cap.get\(propId\)这个方法来获取当前视频的相关信息，其中propId的值是从0到18的。其中有些值是可以通过cap.set\(propId, value\)这个函数进行修改的。



比如说，我可以通过cap.get\(3\)和cap.get\(4\)这两个函数来获取当前帧的宽度和高度。在默认情况下，它将返回640\*480。但如果我想修改成320\*240，那么只需要用ret=cap.set\(3,320\)和ret=cap.set\(4,240\)就可以了。



#### 注意

如果在使用过程当中出现了错误，请先确保这个摄像头在其余的摄像头驱动程序中能够正常工作（比如在Linux）操作系统上。



### 通过视频文件播放视频



这个跟从摄像头获取文件是一样的，只需在创建视频对象的时候将索引号换成视频文件的名字就好了。当然，在播放视频文件的时候，在cv2.waitKey\(\)中需要填入适当的时间。如果这个时间太短，视频将会播放的很快，如果这个时间太长，这个视频将会播放的很慢（这也就是你如何以慢速播放视频的方法）。在正常情况下，25毫秒是比较好的。

```
import numpy as np
import cv2

cap = cv2.VideoCapture('vtest.avi')

while(cap.isOpened()):
ret, frame = cap.read()

grap = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

cv2.imshow('frame', gray)
if cv2.waitKey(1) & 0xFF == ord('q'):
break

cap.release()
cv2.destroyAllWindows()
```

### 保存视频



我们想将一帧一帧采集的视频进行保存，对于保存图片来说这很简单，只需要使用cv2.imwrite\(\)。如果想要保存视频，则需要多做几步。



我们先创建一个VideoWriter的对象。我们需要设定好输出的文件名（例如：output.avi）。我们还需要指定FourCC编码（具体细节在下一章）。接着是设置每秒播放的帧数（fps），最后是颜色（Color）标记。如果返回值为真，编码器需要颜色类，否则以灰色的表示。



ForCC指定视频编码方式以4字节为一位。相关信息可以在fourcc.org上查找。这个是由平台决定的。下列编码方式在我电脑上好使。



> * 在Fedora上：DIVX, XVID, MJPG, X264, WMV1, WMV2\(XVID是较好的选择，MJPG会导致快速播放视频，X264提供很小的画面\)

> * 在windows上：DIVX（被最多的测试和添加）

> * 在OSX上：还不太清楚



FourCC编码以cv2.VideoWriter\_fourcc\('M', 'J', 'P', 'G'\)或者cv2.VideoWriter\_fourcc\(\*'MJPG'\)的方式编写



以下是从摄像头获取视频，翻转每个画面并保存的代码。

```
#coding:utf-8
import numpy as np
import cv2

cap = cv2.VideoCapture(0)

#定义编码方式以及创建VideoWriter的对象
fourcc = cv2.VideoWriter_fourcc(*'XVID')
out = cv2.VideoWriter('output.avi', fourcc, 20.0, (640, 480))

while(cap.isOpened()):
ret, frame = cap.read()
if ret == True：
frame = cv2.flip(frame, 0)

out.write(frame)

cv2.imshow('frame', frame)
if cv2.waitKey(1) & 0xFF == ord('q'):
break
else:
break

cap.release()
out.release()
cv2.destroyAllWindows()
```