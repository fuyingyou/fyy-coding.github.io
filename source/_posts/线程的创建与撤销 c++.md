---
title: 线程的创建与撤销 c++
author: 
tags: 
       - c++

category: 
       - 其它

date: 2020-04-18 16:55:46
---
## 线程的创建与撤销

### 一、目的

(1)熟悉windows系统提供的线程创建与撤销系统调用.
(2)掌握windows系统环境下线程的创建与撤销方法.

### 二、准备

##### 1. 线程的创建

CeateThread()完成线程的创建.它在调用进程的地址空间上创建一个线程,执行指定的函数,并返回新建立的线程的句柄.
原型:
```js 
HANDLE CeateThread(
	LPSECURITY_ATTRIBUTES lpThreadAttributes,
	DWORD dwStackSize,
	LPSECURITY_START_ROUTINE lpStartAddress,
	LPVOID lpparameter,
	DWORD dwCreationFlags,
	LPDWORD lpThreadId
);
```

参数说明:

1. lpThreadAttributes:为线程指定安全属性.为NULL时,线程得到一个默认的安全描述符.
1. dwStackSize:线程堆栈的大小.其值为0时,其大小与调用该线程的线程堆栈大小相同.
1. lpStartAddress:指定线程要执行的函数.
1. lpparameter:函数中要传递的参数.
1. dwCreationFlags:指定线程创建后所处的状态.若为CRRATE_SUSPENDED,表示创建后出于挂起状态,用ResumeThread()激活线程才可以执行.若该值为0，表示线程创建后立即执行.
1. lpThreadId:用一个32位的变量接受系统返回的线程标识符.若该值设为NULL,系统不返回线程标识符.
返回值:
如果线程创建成功,将返回线程的句柄;如果失败,系统返回NULL,可以调用函数GetLastError查询失败的原因.
用法举例:
```js 
static HANDLE hHandle1=NULL; //用于存储线程返回句柄的变量
DWORD dwThreadID1;           //用于存储线程标识符的变量
//创建一个名为ThreadName1的线程
hHandle1=CeateThread((LPSECURITY_ATTRIBUTES)) NULL
                     0,
					 (LPSECURITY_START_ROUTINE)ThreadName1,
					 (LPDWORD)NULL,
					 0,&dwThreadID1);
```

##### 2. 撤销线程

ExitThread()用于撤销当前进程.
原型:
```js 
VOID ExitThread(
DWORD dwExitCode);   //线程返回码
```

参数说明:
dwExitCode:指定线程返回码,可以调用GetExitCodeThread()查询返回码的含义.
返回值:
该函数没有返回值.
用法举例:

```js 
ExitThread(0);
```

##### 3.终止线程

TerminateThread()用于终止当前线程.该函数与ExitThread()的区别在于,ExitThread()在撤销线程时将该线程所拥有的资源全部归还给系统,而TerminateThread()不归还资源.
原型:
```js 
BOOL TerminateThread(
HANDLE hHandle,       //线程句柄
DWORD dwExitCode);    //线程返回码
```

参数说明:
(1)hThread:要终止线程的线程句柄.
(2)dwExitCode:指定线程返回码,可以调用GetExitCodeThread()查询返回码的含义.
返回值:
函数调用成功,将返回一个非0值;若失败,返回0，可以调用函数GetLastError()查询失败的原因.

##### 4.挂起线程

Sleep()用于挂起当前正在执行的线程.
原型:
```js 
VOID Sleep(DWORD dwMilliseconds);
```

参数说明:
dwMilliseconds;指定挂起时间,单位为ms(毫秒).
返回值:
该函数没有返回值.

##### 5.关闭句柄

函数CloseHandle()用于关闭已打开的对象的句柄,其作用与释放动态申请的内存空间类似,这样可以释放系统资源,使线程安全运行.
原型:
```js 
BOOL CloseHandle(HANDLE hObject);
```

参数说明:
hObject:已打开对象的句柄.
返回值:
如果函数调用成功,则返回值为非0值;如果函数调用失败,则返回值为0.若要得到更多的错误信息,调用函数GetLastError()查询.

### 三、内容

使用系统调用CreatThread()创建一个子线程,并在子线程中显示;Thread is Running!.为了能让用户清楚地看到线程的运行情况,使用Sleep()使线程挂起5s,之后使用ExitThread(0)撤销进程.

#### 要求

能正确使用CreatThread(),ExitThread()及Sleep()等系统调用,进一步理解进程与线程理论.

#### 指导

本实验在WindowsXP,Microsoft Visual C++ 6.0环境下实现,利用Windows SDK提供的API完成程序的功能.实验在Windows XP环境下安装由于WindowsXP,Microsoft Visual C++ 6.0是一个集成开发环境,其中包含了Windows SDK 所有工具和定义,所以安装了WindowsXP,Microsoft Visual C++ 6.0后不用特意安装SDK.试验中所有的API是操作系统提供的用来进行应用程序开发的系统功能接口.

1. 首先启动安装好的,Microsoft Visual C++ 6.0.
1. 在,Microsoft Visual C++ 6.0环境下选择File->new命令,然后在Project选项卡中选择Win32 Console Application建立一个控制台工程文件.
1. 由于CreatThread()等函数是Microsoft Windows操作系统的系统调用,因此,在下图中选择An application that supports MFC,之后单击Finish按钮.

![在这里插入图片描述](../images/3e8b77cd-3dac-468c-94a0-0da4b15803a3.png)

1. 创建一个单线程操作并观看结果
![在这里插入图片描述](../images/cdecf844-c7c0-4b59-8eb0-a90db0e708fb.png)
![在这里插入图片描述](../images/f01c088e-7663-4d75-aaef-4df7554944fe.png)
1. 创建一个多线程操作，并观看结果
![在这里插入图片描述](../images/f42ae8df-444c-4e5d-9fb8-f168dd28c1d1.png)
![在这里插入图片描述](../images/9309b62e-e0a3-408d-99aa-1a4f18a836b3.png)

#### 源程序

```js 
// test1.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include "test1.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#undef THIS_FILE
static char THIS_FILE[] = __FILE__;
#endif

/
// The one and only application object

CWinApp theApp;

using namespace std;



void eatApple(int apple_number)
{
    
	Sleep((3-apple_number)*1000);
	printf("i'm eating apple #%d.\n", apple_number);
}


int _tmain(int argc, TCHAR* argv[], TCHAR* envp[])
{
    
	HANDLE handle1 = NULL;
	HANDLE handle2 = NULL;
	HANDLE handle3 = NULL;
	DWORD ThreadID1 = NULL;
	DWORD ThreadID2 = NULL;
	DWORD ThreadID3 = NULL;

	int nRetCode;

	int a = 0;
	int b = 1;
	int c = 2;
	
	handle1 = CreateThread((LPSECURITY_ATTRIBUTES) NULL,
		0,
		(LPTHREAD_START_ROUTINE) eatApple,
		(LPVOID) a,
		0,
		&ThreadID1);
	
	handle2 = CreateThread((LPSECURITY_ATTRIBUTES) NULL,
		0,
		(LPTHREAD_START_ROUTINE) eatApple,
		(LPVOID) b,
		0,
		&ThreadID2);
	
	handle3 = CreateThread((LPSECURITY_ATTRIBUTES) NULL,
		0,
		(LPTHREAD_START_ROUTINE) eatApple,
		(LPVOID) c,
		0,
		&ThreadID3);
	Sleep(10000);
	return nRetCode;
}
```

### 四、总结

在Windows系统中进程是资源的拥有者,线程是系统调用的单位.进程创建后,其主线程也随即被创建.但是单线程只能执行完一个之后再执行另外一个线程，而多线程在一定程度上是多个线程一起执行的。