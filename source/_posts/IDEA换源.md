---
title: IDEA换源
author: 
tags: 
       - Java学习

category: 
       - Java学习

date: 2021-04-06 09:13:33
---
#### IDEA打开setting，搜索maven

如图：
![](https://gitee.com/fuyingyou/picgo/raw/master/img_algorithm/202403151953435.png)

右侧两个override

* 第一个是配置文件的路径重写
* 第二个是仓库路径重写

更改为自己的setting.xml的路径即可

setting.xml的内容
```js 
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                        https://maven.apache.org/xsd/settings-1.0.0.xsd">
    <mirrors>
        <mirror>
			<id>aliyunmaven</id>
			<mirrorOf>*</mirrorOf>
			<name>阿里云公共仓库</name>
			<url>https://maven.aliyun.com/repository/public</url>
		</mirror>
    </mirrors>
</settings>
```