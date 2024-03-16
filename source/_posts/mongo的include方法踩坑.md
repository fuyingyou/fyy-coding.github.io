---
title: mongo的include方法踩坑
author: 
tags: 
       - mongodb

category: 
       - 数据库

date: 2023-08-21 00:20:00
---
#### 前言

又是不认识自己代码的一天

### 问题

```js 
Query query = new Query();
if(StringUtils.isNotNull(reqVO.getFieldLimitList()) && reqVO.getFieldLimitList().size() > 0){
    
	for(String filedName : reqVO.getFieldLimitList()){
    
		query.fields().include(filedName);
	}
}
```

看到 **query.fields().include(filedName);** 这行代码，

**以为效果是**： 如果数据没有这个字段就把这条数据筛掉，加上自己当时写的注释有歧义，所以误解了。

**实际效果是**： include只是对返回的结果集进行了处理，使得只返回结果集的某些字段，并不会减少结果。

如果需要查询时，不返回某字段为空的数据，正确做法是：
```js 
Query query = new Query();
if(StringUtils.isNotNull(reqVO.getFieldLimitList()) && reqVO.getFieldLimitList().size() > 0){
    
	query.addCriteria(Criteria.where(MongoConsts.fieldName).exists(true));
}
```