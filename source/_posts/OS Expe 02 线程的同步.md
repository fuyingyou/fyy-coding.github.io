---
title: OS Expe 02 线程的同步
author: 
tags: 
       - c++

category: 
       - 其它

date: 2020-04-21 21:42:31
---
## 实验二:线程的同步

### 一、实验目的

1）进一步掌握windows系统环境下线程的创建和撤销
2）熟悉windows系统提供的线程同步API（是WINDOWS提供给应用程序与操作系统的接口）
3）使用windows系统提供的线程同步API解决实际问题

### 二、实验准备

##### 1. 进程同步机制的主要任务：

对多个相关进程在执行次序上进行协调，使并发执行的诸进程之间能够按照一定的规则（或时序）共享系统资源，并能很好地相互合作，从而使程序执行具有可再现性。

##### 2. 线程和进程的发展历程

```js 
在20世纪60年代中期，人们在设计多道程序OS时，引入了进程的概念，从而解决了在单处理机环境下的程序并发执行问题。此后在长达20年的时间里，在多道程序OS中一直以进程作为能拥有资源和独立调度的（运行）的基本单位。

直到80年代中期，人们又提出了比进程更小的基本单位 线程 的概念，试图用它来提高程序并发执行的程度，以进一步改善系统的服务质量。特别是在进入20世纪90年代后，多处理机系统得到迅速发展，由于线程能更好的提高程序的并发执行程度，因而近几年推出的多处理机OS无一例外地都引入了进程，用以改善OS的性能。
```

由于**线程具有许多传统进程所具有的特征**，所以又称之为轻型进程或进程元，相应的，把传统进程称之为重型进程。传统进程相当于只有一个线程的任务，

##### 3. 等待对象函数

等待一个对象 等待多个对象 WaitForSingleObject() WaitForMultipleObjects() 在指定时间内等待一个对象 在指定时间内等待多个对象

**原型：**

```js 
DWORD WaitForSingleObject(
	HANDLE hHandle,	//对象句柄
	DWORD dwMilliseconds //等待时间，以毫秒为单位
);
```

**原型：**

```js 
DWORD WaitForMultipleObjects(
	DWORD nCount,				//句柄数组中的句柄数
	const HANDLE *lpHandles,	//指向对象句柄数组的指针
	BOOL fWaitAll				//等待类型 1/true表示等待所有的任务完成后进行下一个操作，0/flase 只等待任何一个的完成
	DWORD dwMilliseconds 		//等待时间，以毫秒为单位
);
```

**可等待的对象列表**

* Change notification：变化通知
* Console input：控制台输入
* Events：事件
* Job：作业
* Mutex：互斥信号量
* Process：进程
* **Semaphore：计数信号量** （*本次主要用到）*
* **Thread：线程** *（本次主要用到）*
* Wait-able timer：定时器

#### 如何去等待一个对象

1. 我们需要立一个Flag，用于在主子线程之间相互告知运行状态。
1. Flag = 信号量
1. 创建一个信号量
```js 
hHandle1 = CreateSemaphore(NULL, 0，1, "SemaphoreName1");//创建一个信号量（安全标识符，信号量初始态，信号量最大值，信号量名称）
```

1. 打开一个信号量
```js 
hHandle1 = OpenSemaphore( SYNCHRONIZE ISEMAPHORE_ MODIFY_ STATE, NULL,"SemaphoreName1" );//（访问标志，继承标志，信号量名）
```

1. 释放信号量
```js 
rc = ReleaseSemaphore (hHandle1, 1, NULL)（我们需要释放哪一个信号量，对信号量进行增几的操作，信号量要增加数值地址）;
```

1. 等待单个对象
```js 
dRes = WaitForSingleObject(hHandle1 , INFINITE); // 主线程无限期地等待子线程结束（信号量的句柄，如果没有释放无限运行）  如果对操作时长有限制，可在第二个参数设置等待的最大值
```

1. 等待多个对象
```js 
dRes = WaitForMultiple0bjects(3, hHandles, 1, INFINITE); //第三个参数为1或true时，等待数组中所有对象完成，为0或者false时满足一个任务结束就可继续执行
```

### 三、实验内容

**实验一 线程的同步之等待单个对象**
子线程 主线程 用于制作麻辣香锅，制作用时5秒 等待子线程执行完毕，打印“麻辣香锅上菜完毕，请开动吧！”

**实验二 线程的同步之等待多个对象**

子线程1 子线程2 子线程3 主线程 用于制作麻辣香锅，制作用时5秒 用于制作什锦菇，制作用时3秒 用于制作米饭，制作用时6秒 等待多个子线程执行完毕，打印”所有菜品上菜完毕，请开动吧！“

#### 实验要求

能正确使用等待对象、WaitForSingleObject（）或WaitForMultipleObject（)及信号量对象CreateSemaphore（）、OpenSemaphore（）、ReleaseSemaphore（）等系统调用，进一步理解线程的同步。

#### 实验指导

1.在Microsoft visual C++6.0环境下建立一个MFC支持的控制台文件，编写C程序。
2.在程序中使用CreateSemaphore（NULL，0，1，”SemaphoreName1”）创建一个名为“SemaphoreName1”的信号量，信号量的初始值为0。
之后使用
```js 
OpenSemaphore（SYNCHRONIZE|SEMAPHORE_MODIFY_STARTE,NULL, ”SemaphoreName1）
```

打开该信号量，这里访问标志使用“SYNCHRONIZE|SEMAPHORE_MODIFY_STARTE”，
以便之后可以使用WaitForSingleObject（）等待该信号量及使用ReleaseSemaphore（）释放该信号量，然后创建一个子线程。

3.主线程创建子线程后调用WaitForSingleObject（hHandle1，INFINITE），这里等待时间设置为INFINITE表示一直等待下去，直到该信号量被唤醒为止。

4.子线程结束，调用ReleaseSemaphore（hHandle1，1，NULL）释放信号量，使信号量的值加1。

#### 源程序

**实验一主要内容及代码**

* 创建一个信号量并打开，创建一个线程，主线程等待子线程结束，释放信号量。
```js 
static HANDLE hHandle1 = NULL;

void chef()
{
    
	printf("麻辣香锅开始制作，预计等待时间5秒。\n");
	Sleep(5000);
	printf("麻辣香锅制作完成！\n");

	BOOL rc;
	DWORD err;

	rc = ReleaseSemaphore(hHandle1,1,NULL);
	err = GetLastError();
	
	printf("ReleaseSemaphore err=%d\n",err);
	if(rc == 0){
    	
		printf("Semaphore Release Fail!\n");
	}else{
    	
		printf("Semaphore Release Success! rc=%d\n",rc);	
	}
}


int _tmain(int argc, TCHAR* argv[], TCHAR* envp[])
{
    

	int nRetCode = 0;

	DWORD dRes,err;

	hHandle1 = CreateSemaphore(NULL,0,1,"SemaphoreName1");//创建一个信号量
	if(hHandle1 == NULL){
    
		printf("Semaphore Create Fail!\n");
	}else{
    
		printf("Semaphore Create Success!\n");
	}
	
	hHandle1 = OpenSemaphore(SYNCHRONIZE|SEMAPHORE_MODIFY_STATE,
		NULL,
		"SemaphoreName1");
	if(hHandle1 == NULL){
    
		printf("Semaphor Open Fail!\n");
	}
	else{
    
		printf("Semaphor Open Success!\n");
	}

	HANDLE handle1 = NULL;
	DWORD ThreadID1 = NULL;

	handle1 = CreateThread((LPSECURITY_ATTRIBUTES) NULL,
				0,
				(LPTHREAD_START_ROUTINE) chef,
				(LPVOID) NULL,
				0,
				&ThreadID1);

	dRes = WaitForSingleObject(hHandle1,INFINITE);

	err = GetLastError();

	if(err == 0){
    
		printf("麻辣香锅上菜完毕，请开动吧。\n");
	}
	else{
    
		printf("WaitForSingleObject err = %d\n",err);
	}

	return nRetCode;
}
```

**效果图**
![在这里插入图片描述](https://gitee.com/fuyingyou/picgo/raw/master/img_algorithm/202403151953256.png)

**实验二主要内容及代码**

* 创建三个线程，当三个线程都执行完毕时，执行主线程
```js 
void chef(int meal_code)
{
    

	if(meal_code == 0){
    
		Sleep(5000);
		printf("麻辣香锅制作完成！\n");
	}
	else if(meal_code == 1){
    
		Sleep(3000);
		printf("什锦菇制作完成！\n");
	}
	else if(meal_code == 2){
    
		Sleep(6000);
		printf("米饭制作完成！\n");
	}
	
}


int _tmain(int argc, TCHAR* argv[], TCHAR* envp[])
{
    

	int nRetCode = 0;

	DWORD dRes,err;

	HANDLE handle1 = NULL;
	HANDLE handle2 = NULL;
	HANDLE handle3 = NULL;

	DWORD ThreadID1 = NULL;
	DWORD ThreadID2 = NULL;
	DWORD ThreadID3 = NULL;

	int a = 0;
	int b = 1;
	int c = 2;

	handle1 = CreateThread((LPSECURITY_ATTRIBUTES) NULL,
				0,
				(LPTHREAD_START_ROUTINE) chef,
				(LPVOID) a,
				0,
				&ThreadID1);

	handle2 = CreateThread((LPSECURITY_ATTRIBUTES) NULL,
				0,
				(LPTHREAD_START_ROUTINE) chef,
				(LPVOID) b,
				0,
				&ThreadID2);

	handle3 = CreateThread((LPSECURITY_ATTRIBUTES) NULL,
				0,
				(LPTHREAD_START_ROUTINE) chef,
				(LPVOID) c,
				0,
				&ThreadID3);

	HANDLE hHandles[3];
	hHandles[0] = handle1;
	hHandles[1] = handle2;
	hHandles[2] = handle3;

	dRes = WaitForMultipleObjects(3,hHandles,0,INFINITE);

	err = GetLastError();

	if(err == 0){
    
		printf("所有菜品上菜完毕，请开动吧。\n");
	}
	else{
    
		printf("WaitForSingleObject err = %d\n",err);
	}

	return nRetCode;
}
```

**效果图**
![在这里插入图片描述](https://gitee.com/fuyingyou/picgo/raw/master/img_algorithm/202403151953257.png)
但是当我们的选择等待的对象为任意一个，即第三个参数为0时

```js 
dRes = WaitForMultipleObjects(3,hHandles,0,INFINITE);
```

**效果图为：**
![在这里插入图片描述](https://gitee.com/fuyingyou/picgo/raw/master/img_algorithm/202403151953258.png)

### 四、实验总结

实验完成了主、子线程的同步，主线程创建子线程后，主线程塞，让子线程先执行，等子线程执行完后，由子线程唤醒主线程。是使我们了解如何使用使用等待对象WaitForSingleObject（）或WaitForMultipleObjects（)及信号量对象CreateSemaphore（）、OpenSemaphore（）、ReleaseSemaphore（）等系统调用，进一步理解线程的同步。