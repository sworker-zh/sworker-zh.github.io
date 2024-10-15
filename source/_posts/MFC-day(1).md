---
title: MFC底层窗口实现
date: 2019-05-07 10:42:00
tags: [c++, MFC]
categories: 技术整理
---

# 简要说明
MFC是微软的一个基础类库，如果在Windows平台上做GUI的开发，这是一个不错的选择。简单的记录MFC学习过程中的需要掌握或者后期需要查看的知识点。

# Windows消息机制
1. 操作系统首先捕获到来自键盘或鼠标等输入系统的消息，并将获取到的消息存放到消息队列中。
2. 应用程序一直通过GetMessage()从消息队列中获取消息。
3. 应用程序再将获取到的消息通过DispatchMessage()分派到操作系统
4. 操作系统再执行“窗口过程”

![](http://ww1.sinaimg.cn/large/902f6897ly1g2sllm57dwj20c4080749.jpg)

<!---------------more---------------------->
# Windows编程模型
1. WinMain函数的定义（WinMain函数是Windows程序的入口）
2. 创建一个窗口
3. 进行消息循环
4. 编写窗口过程函数

## 整体框架结构
![](http://ww1.sinaimg.cn/large/902f6897ly1g2sltgfew0j20k108kdfw.jpg)

# 实现步骤
## 创建win32项目
1. 新建win32项目，选择“空项目”并完成创建。
2. 添加源文件，文件名以“.c”结尾。
3. 添加头文件“windows.h”
4. 添加程序入口函数“WindowMain”

```
//WINAPI 代表__stdcall 参数的传递顺序：从右到左 以此入栈，并且在函数返回前 清空堆栈
int WINAPI WinMain(
	HINSTANCE hInstance,      // 应用程序实例句柄
	HINSTANCE hPrevInstance,  // 上一个应用程序句柄，在win32环境下，参数一般为NULL，不起作用了
	LPSTR lpCmdLine,          // char * argv[]
	int nShowCmd)             // 显示命令 最大化、最小化 正常
{
    ...
    	return 0;
}
```

## 应用程序过程
1. 设计窗口
- 首先实例化一个窗口类，再依次设置其参数
```
WNDCLASS wc;
wc.cbClsExtra = 0; //类的额外的内存
wc.cbWndExtra = 0; //窗口额外的内存
wc.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH); // 设置背景
wc.hCursor = LoadCursor(NULL, IDC_HAND);  //设置光标 如果第一个参数为NULL，代表使用体统提供的光标
wc.hIcon = LoadIcon(NULL, IDI_ERROR);
wc.hInstance = hInstance; //应用程序实例句柄 传入WinMain中的形参即可
wc.lpfnWndProc = WindowProc;// 回调函数 窗口过程
wc.lpszClassName = TEXT("WIN");//指定窗口类名称
wc.lpszMenuName = NULL; //NULL代表不使用菜单
wc.style = 0; //显示风格 0为默认
```

2. 注册窗口
- 将上述实例化的窗口类进行注册
```
RegisterClass(&wc);
```
3. 创建窗口
```
/*
lpClassName,  类名
lpWindowName, 标题名
dwStyle,      风格      WS_OVERLAPPEDWINDOW
x,            显示坐标  CW_USEDEFAULT
y,
nWidth,       宽
nHeight,      高
hWndParent,   父窗口
hMenu,        菜单
hInstance,    实例句柄
lpParam       附加值  lp一般为鼠标附加值  NULL
*/
HWND hwnd = CreateWindow(wc.lpszClassName, TEXT("WINDOWS"), WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, NULL, NULL, wc.hInstance, NULL);

```
4. 显示和更新
```
ShowWindow(hwnd, SW_SHOWNORMAL);
UpdateWindow(hwnd);
```
5. 通过循环取消息
```
/*
HWND        hwnd;    主窗口句柄
UINT        message; 具体消息名称
WPARAM      wParam;  附加消息 键盘消息
LPARAM      lParam;  附加消息 鼠标消息
DWORD       time;    消息产生时间
POINT       pt;      附加消息 鼠标消息 x y
*/
MSG msg;

while (1)
{
	/*
	LPMSG lpMsg,        消息
	HWND hWnd,          捕获窗口 填NULL代表捕获所有的窗口
	UINT wMsgFilterMin, 最小和最大的过滤的消息 一般填入0
	UINT wMsgFilterMax  填0代表捕获所有消息
	*/
	if (GetMessage(&msg, NULL, 0, 0) == FALSE)
	{
		break;
	}

	// 翻译消息
	TranslateMessage(&msg);
	// 分发消息
	DispatchMessage(&msg);
}
```
6. 消息处理（窗口过程）
```
//CALLBACK 代表__stdcall 参数的传递顺序：从右到左 以此入栈，并且在函数返回前 清空堆栈
LRESULT CALLBACK WindowProc(
	HWND hwnd,       // 消息所属的窗口句柄
	UINT uMsg,       // 具体消息名称   WM_XXXXXXXX 消息名
	WPARAM wParam,   // 键盘附加消息
	LPARAM lParam    // 鼠标附加消息
)
{
	switch (uMsg)
	{
	case WM_CLOSE:
		//所有xxxWindow为结尾的方法，都不会进入到消息队列中，而是直接执行
		DestroyWindow(hwnd);  //DestroyWindow 发送另一个消息 WM_DESTROY
		break;
	case WM_DESTROY:
		PostQuitMessage(0);
		break;
	case WM_LBUTTONDOWN:
	{
		int xPos = LOWORD(lParam);
		int yPos = HIWORD(lParam);

		char buf[1024];
		wsprintf(buf, TEXT("x = %d, y = %d"), xPos, yPos);

		MessageBox(hwnd, buf, TEXT("鼠标左键按下"), MB_OK);
		break;
	}
	case WM_KEYDOWN:
		MessageBox(hwnd, TEXT("键盘按下"), TEXT("键盘按下"), MB_OK);
		break;
	case WM_PAINT:
	{
		PAINTSTRUCT ps;
		HDC hdc = BeginPaint(hwnd, &ps);

		TextOut(hdc, 100, 100, TEXT("HELLO"), strlen("HELLO"));
		EndPaint(hwnd, &ps);
		break;
	}
	default:
		break;
}
```
### 大概流程如下
![](http://ww1.sinaimg.cn/large/902f6897gy1g2ssrgqjg6j20dt0e3tdi.jpg)

# 小结
1. 首先要对整个创建流程有个整体的把握
2. 学会查看msdn文档，不知道的参数及方法都可以从中获取
3. 学习很重要，要好好学习






































