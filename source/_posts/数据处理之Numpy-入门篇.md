---
title: 数据处理之Numpy-入门篇
author: 
tags: 
       - python

category: 
       - 其它

date: 2021-04-06 09:26:31
---
## 实验一、数据处理之Numpy

### 一、实验目的

#### 1. 了解numpy库的基本功能

#### 2. 掌握Numpy库的对数组的操作与运算

### 二、实验工具：

#### 1. Anaconda

#### 2. Numpy

### 三、Numpy简介

Numpy 的英文全称为 Numerical Python，指Python 面向数值计算的第三方库。Numpy 的特点在于，针对 Python 内建的数组类型做了扩充，支持更高维度的数组和矩阵运算，以及更丰富的数学函数。Numpy 是 Scipy.org 中最重要的库之一，它同时也被 Pandas，Matplotlib 等我们熟知的第三方库作为核心计算库。
NumPy（Numeric Python）提供了许多高级的数值编程工具，如：矩阵数据类型、矢量处理，以及精密的运算库。专为进行严格的数字处理而产生。多为很多大型金融公司使用，以及核心的科学计算组织如：Lawrence Livermore，NASA用其处理一些本来使用C++，Fortran或Matlab等所做的任务。
Numpy包括了：1、一个强大的N维数组对象Array；2、比较成熟的（广播）函数库；3、用于整合C/C++和Fortran代码的工具包；4、实用的线性代数、傅里叶变换和随机数生成函数。Numpy和稀疏矩阵运算包scipy配合使用更加方便。

### 四、实验内容

#### 1. 数组的创建（创建全0数组，全1数组，随机数数组）

```js 
import numpy as np

a = np.ones(5, int)

b = np.zeros(5, int)

f = np.random.randint(0, 10, 6)
print("全1数组：\n", a)

print("全0数组:\n", b)

print("随机数数组:\n", f)
```

![image.png](../images/39083d12-6736-4043-bbbe-8f2e9a858ffb.png)

#### 2. 数组的属性（查看数组的维度，数组元素的个数）

```js 
import numpy as np

a = np.ones(5, int)

print("全1数组维度：\n", a.ndim)

print("全1数组元素个数：\n", a.shape)
```

![image.png](../images/804a573c-9673-4cfc-85f0-16bdd075fb84.png)

#### 3. 数组的维度操作（将数组的行变列，返回最后一个元素，返回第2到第4个元素，返回逆序的数组）

```js 
import numpy as np

c = np.arange(9).reshape(3, 3)
print("转置前：\n", c)
d = c.T
print("转置后：\n", d)

e = np.arange(10)
print("数组为：\n", e)
print("最后一个元素为：\n", e[-1])
print("第2到第4元素为：\n", e[1:4])
print("逆序数组为：\n", e[::-1])
```

![image.png](../images/c305a0fc-95de-49da-8323-4848537aaa7c.png)

#### 4. 数组的合并（数组的水平合并，垂直合并，深度合并）

```js 
import numpy as np

c = np.arange(-9, 0).reshape(3, 3)
d = np.arange(0, 9).reshape(3, 3)
print("第一个数组为：\n", c)
print("第二个数组为：\n", d)

print("水平合并：\n", np.hstack((c, d)))
print("垂直合并：\n", np.hstack((c, d)))
print("深度合并：\n", np.hstack((c, d)))
```

![image.png](../images/4a236cbd-4c66-4bf6-ac38-2664a1027261.png)

#### 5. 数组的拆分（数组的水平拆分，垂直拆分，深度拆分）

```js 
import numpy as np

c = np.arange(-9, 0).reshape(3, 3)
print("数组为：\n", c)

print("水平拆分为：\n", np.hsplit(c, 3))
print("垂直拆分为：\n", np.vsplit(c, 3))

d = np.arange(8).reshape(2, 2, 2)
print("待深度拆分数组为：\n", d)
print("深度拆分为：\n", np.dsplit(d, 2))
```

![image.png](../images/af2ece3b-1895-4bd7-8fd7-f0372f66111a.png)
![image.png](../images/b2d52458-0e15-4e7d-9480-1800755cb85d.png)
![image.png](../images/f5202819-abe1-400d-a6f2-dcf0a3fcc9f4.png)

#### 6. 数组运算（与常的四则运算，与数组的四则运算，判断数组是否相等）

```js 
import numpy as np

a = np.arange(4)
b = np.arange(4, 8)
print("两个数组分别为：\n", a, b)
print("a+2为：\n", a + 2)
print("a+b为：\n", a+b)
print("a-b为：\n", a-b)
print("a*b为：\n", a*b)
print("a/b为：\n", a/b)
```

![image.png](../images/e78eea16-0e8b-474d-95c7-327e0e2e806d.png)

#### 7. 数组的常用函数（数组所有元素的和、积、平均值、最大值、最小值、元素替换、方差、标准差）

```js 
import numpy as np

a = np.arange(7)
print("数组为：", a)
print("数组所有元素的和为：", a.sum())
print("数组所有元素的积为：", a.prod())
print("数组所有元素的平均值为：", a.mean())
print("数组所有元素的最大值为：", a.max())
print("数组所有元素的最小值为：", a.min())
print("数组所有元素的元素小于3的元素替换为3，大于4的元素替换为4：", a.clip(2, 5))
print("数组所有元素的方差为：", a.var())
print("数组所有元素的标准差为：", a.std())
```

![image.png](../images/8b115823-a4ed-416e-95f2-5fffad4d6649.png)

### 五、实验总结（写出本次实验的收获，遇到的问题等）

学习到了numpy库中的一些函数的使用方法。受益良多，感觉到python库的强大之处，日后一定多加练习，以求对python的常用库的使用更加熟练。