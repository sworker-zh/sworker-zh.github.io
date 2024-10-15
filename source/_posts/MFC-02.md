---
title: MFC窗口创建及消息机制等
date: 2019-05-07 22:42:00
tags: [c++, MFC]
categories: 技术整理
---

# 简介
最基础的MFC窗口需要CWinApp应用程序类和CFrameWnd窗口框架类。

## 创建过程
1. 新建空的win32项目，并添加“mfc.cpp”和“mfc.h”两个源文件
2. 在“mfc.h”中包含头文件“afxwin.h”（mfc的头文件）
```
#include <afxwin.h>
```
3. 创建应用程序类MyApp，继承自CWinApp
```
class MyApp : public CWinApp
{
public:
    virtual BOOL InitInterface();
}
```
<!--------------------------more--------------------------->
4. 在类MyApp中添加程序入口函数“InitInterface”，该方法为虚函数
5. 创建窗口框架类MyFrame，继承自CFrameWnd
```
class MyFrame : public CFrameWnd
{
public:
    MyFrame();
}
```
6. 在CFrameWnd的构造函数中创建窗口
```
Create(NULL, TEXT("MFC"));
```
7. 在入口函数中实例化新的窗口，并进行显示和更新操作
```
MyFrame * frame = new MyFrame;
frame->ShowWindow(SW_SHOWNORMAL);
frame->UpdateWindow();
```
### 注意点
1. 上述过程没有创建应用程序对象，该操作应该在“mfc.h”中进行。创建的应用程序对象是全局的而且只有一个。
2. 想要运行该项目，必须右击项目文件，选择“属性”--“常规”--“MFC的使用”--“在共享DLL中使用MFC”。

![](http://ww1.sinaimg.cn/large/902f6897ly1g2sxr7lojej20cr0gg0t4.jpg)

![](http://ww1.sinaimg.cn/large/902f6897ly1g2sxrn7t9tj20o10g7mxy.jpg)

3. 相关不了解的函数可以查阅《VC++之MFC类库中文手册》，在“索引”中查找时要加上类名。

# 消息映射
消息映射是一个将消息和成员函数相互关联的表。

1. 声明宏，写到.h文件中
```
DECLARE_MESSAGE_MAP()
```
2. 分界宏，写到.cpp中
```
BEGIN_MESSAGE_MAP(theClass, baseClass)
...
END_MESSAGE_MAP()
```
3. 找消息宏，写到分界宏中间
4. 函数原型声明到.h中
5. 函数的实现写到.cpp中

## 以在MyFrame里面添加一个鼠标左键按下弹出坐标对话框为例
1. 在MyFrame中添加声明宏，提供消息映射机制

![](http://ww1.sinaimg.cn/large/902f6897ly1g2t51deoazj208x03iq2q.jpg)

2. 在mfc.h中添加分界宏，并添加左键按下的消息宏

![](http://ww1.sinaimg.cn/large/902f6897ly1g2t53pkdhtj208j023we9.jpg)

3. 在mfc.h中声明函数原型

![](http://ww1.sinaimg.cn/large/902f6897ly1g2t55tmgzwj209202m3ya.jpg)

4. 在mfc.cpp中实现

![](http://ww1.sinaimg.cn/large/902f6897ly1g2t599wb2jj20cc03h3yb.jpg)

# 结尾
不管是在学习还是在工作中，不可能记住所有的API，不过要记得大概然后能快速查看文档。查文档也是非常重要的一环，加油。