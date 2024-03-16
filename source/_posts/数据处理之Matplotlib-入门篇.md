---
title: 数据处理之Matplotlib-入门篇
author: 
tags: 
       - matplotlib

category: 
       - 其它

date: 2021-04-06 09:30:59
---
## 实验二、数据处理之Matplotlib

### 一、实验目的

#### 1. 了解matplotlib库的基本功能

#### 2. 掌握matplotlib库的使用方法

### 二、实验工具：

#### 1. Anaconda

#### 2. Numpy，matplotlib

### 三、Matplotlib简介

Matplotlib 包含了几十个不同的模块， 如 matlab、mathtext、finance、dates 等，而 pylot 则是我们最常用的绘图模块

### 四、实验内容

#### 1.绘制正弦曲线，并设置标题、坐标轴名称、坐标轴范围

```js 
import numpy as np
import matplotlib.pyplot as plt
from pylab import mpl
mpl.rcParams['font.sans-serif'] = ['FangSong']
mpl.rcParams['axes.unicode_minus'] = False
x=np.arange(0, 2*np.pi, 0.01)
y=np.sin(x)
plt.plot(x, y)
plt.title(u'正弦曲线', fontdict={'size': 20})
plt.xlabel(u'弧度', fontdict={'size': 16})
plt.ylabel(u'正弦值', fontdict={'size': 16})
plt.axis([-0.1*np.pi, 2.1*np.pi, -1.1, 1.1])
plt.show()
```

![在这里插入图片描述](../images/e00e15b8-cf47-495a-bb18-6a9a4fa0623b.png)

#### 2. 同一坐标系中绘制多种曲线并通过样式、宽度、颜色加以区分

```js 
import numpy as np
import matplotlib.pyplot as plt
from pylab import mpl
mpl.rcParams['font.sans-serif'] = ['FangSong']
mpl.rcParams['axes.unicode_minus'] = False
x = np.linspace(-4, 4, 200)
f1 =  np.power(10, x)
f2 = np.power(np.e, x)
f3 = np.power(2, x)
plt.plot(x, f1, 'r', ls='-', linewidth=2, label='$10^x$')
plt.plot(x, f2, 'b', ls='--', linewidth=2, label='$e^x$')
plt.plot(x, f3, 'g', ls=':', linewidth=2, label='$2^x$')
plt.axis([-4, 4, -0.5, 8])
plt.title('幂函数曲线', fontsize=16)
plt.legend(loc='lower right')
plt.show()
```

![在这里插入图片描述](../images/e48c8dd6-036b-4848-84d4-10469e4b4708.png)

#### 3.绘制多轴图，即将多幅子图绘制在同一画板。

```js 
import numpy as np
import matplotlib.pyplot as plt
plt.subplot(221)
x = np.arange(0, 2*np.pi, 0.01)
y = np.cos(x)
plt.plot(x, y)
plt.subplot(222)
plt.axis([-1, 2, -1, 2])
plt.axvline(x=0, ymin=0, linewidth=4, color='g')
plt.axvline(x=1.0, ymin=-0.5, ymax=0.5, linewidth=4, color='y')
plt.show()
```

![image.png](../images/92e24ad4-0f7a-48fc-9a4c-f80ce9499c87.png)

#### 4.直方图的绘制(数据自己定义）

```js 
import numpy as np
import matplotlib.pyplot as plt
bins = np.arange(-3, 10, 3)
plt.hist(bins)
plt.show()
```

![image.png](../images/517a7374-5d40-42b0-9d08-e23c660e211c.png)

#### 5.绘制散点图

```js 
import numpy as np
import matplotlib.pyplot as plt
x = np.random.rand(30)
y = np.random.rand(30)
area = np.pi*(15*np.random.rand(30))**2
color = 2*np.pi*np.random.rand(30)
plt.scatter(x, y, s=area, c=color, alpha=0.5, cmap=plt.cm.hsv)
plt.show()
```

![在这里插入图片描述](../images/bb907d89-2d42-45fe-bfb9-88d85bec2f67.png)

#### 6.绘制盒状图

```js 
import numpy as np
import matplotlib.pyplot as plt
data = np.random.randn(200)
fig, (ax2) = plt.subplots(1, figsize=(8, 6))
ax2.boxplot(data)
plt.show()
```

![在这里插入图片描述](../images/2e965599-4959-44c8-8ec7-5612434816ca.png)

#### 7.尝试matplotlib库的其它功能，如2D,3D等

```js 
import numpy as np
import matplotlib.pyplot as plt
y,x = np.ogrid[-2:2:200j, -3:3:300j]
z = x*np.exp(-x**2 - y**2)
extent = [np.min(x), np.max(x), np.min(y), np.max(y)]
plt.subplot(211)
cs = plt.contour(z, 10, extent=extent)
plt.clabel(cs)
plt.subplot(111)
plt.contourf(x.reshape(-1), y.reshape(-1), z, 20)
plt.show()
```

![在这里插入图片描述](../images/030dd6b6-ddc1-4332-8b47-8ca16d732510.png)

### 五、实验总结（写出本次实验的收获，遇到的问题等)

了解到matplotlib库不是只要你安装了numpy就有了这个库，刚开始做的时候因为没有导入matplotlib库而频频报错。在网上搜索了简易的安装matplotlib库的办法，直接在电脑cmd里面敲两行命令安装即可，不用再很繁琐的在电脑上还要配置环境变量。