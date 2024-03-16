---
title: MySQL基本语法
author: 
tags: 
       - 数据库

category: 
       - mysql

date: 2023-08-28 09:55:03
---
## 基础语法

#### 别名

```js 
-- 别名 as 可省略，但中间需要空格
select name as 员工姓名, position as 职位名称 from employees;
```

#### 常量和运算

```js 
select 200, '篮球' as hobby;
select order_id, unit_price, quantity, unit_price * quantity as total_amount from orders;
```

#### 运算符

```js 
select name, age, salary from employees where age between 25 and 30;
select name, age, salary from employees where salary > 5500;
select name, age, salary from employees where name != '小张';
```

#### 空值

```js 
-- SQL查询语句
select name, age from employees where hire_date IS NULL;

select name, age from employees where hire_date IS NOT NULL;
```

#### 模糊查询 like, not like _ %

```js 
select name, age, position from employees where name like '%张%';
-- 只查询以 "张" 开头的数据行
select name, age, position from employees where name like '张%';

-- 只查询以 "张" 结尾的数据行
select name, age, position from employees where name like '%张';
-- 可以使用 not like 来查询不包含某关键字的信息。
```

#### 逻辑运算

AND OR NOT
```js 
-- SQL查询语句
select name, age, salary from employees where name like '%李%' and age < 30;
```

#### 去重

```js 
-- SQL 查询语句 使用DISTINCT关键字来找出不同的班级 ID
select distinct class_id from students;
-- DISTINCT 关键字还支持根据多个字段的组合来进行去重操作，确保多个字段的组合是唯一的。
```

#### 排序

```js 
-- SQL 查询语句 1
select name, age from students order by age asc;

-- SQL 查询语句 2
select name, score from students order by score desc;
```

#### 截断、偏移

```js 
-- LIMIT 后只跟一个整数，表示要截断的数据条数（一次获取几条）
select task_name, due_date from tasks limit 2;

-- LIMIT 后跟 2 个整数，依次表示从第几条数据开始、一次获取几条
select task_name, due_date from tasks limit 2, 2;
```

#### 条件分支

```js 
SELECT
  name,
  CASE WHEN (name = '鸡哥') THEN '会' ELSE '不会' END AS can_rap
FROM
  student;
```
 
```js 
CASE WHEN (条件1) THEN 结果1
	   WHEN (条件2) THEN 结果2
	   ...
	   ELSE 其他结果 END
```

## 函数

#### 时间

```js 
-- 获取当前日期
SELECT DATE() AS current_date;

-- 获取当前日期时间
SELECT DATETIME() AS current_datetime;

-- 获取当前时间
SELECT TIME() AS current_time;
-- 这里的日期、日期时间和时间将根据当前的系统时间来生成，实际运行结果可能会因为当前时间而不同。
```

#### 四舍五入

```js 
round(AVG(grade),2)
```

#### 字符串处理

```js 
-- 将姓名转换为大写
SELECT name, UPPER(name) AS upper_name
FROM employees;
```
 
```js 
-- 计算姓名长度
SELECT name, LENGTH(name) AS name_length
FROM employees;
```
 
```js 
-- 将姓名转换为小写并进行条件筛选
SELECT name, LOWER(name) AS lower_name
FROM employees;
```

#### 聚合

* COUNT：计算指定列的行数或非空值的数量。
当Mysql确认括号内的表达式值不可能为NULL时，实际上就是在统计行数。
所以使用条件要加一个COUNT(c.action = ‘confirmed’ OR NULL)
不为confirmed时用NULL代替，NULL不会被COUNT统计

* SUM：计算指定列的数值之和。
* AVG：计算指定列的数值平均值。
* MAX：找出指定列的最大值。
* MIN：找出指定列的最小值。
```js 
-- 使用聚合函数 COUNT 计算订单表中的总订单数
SELECT COUNT(*) AS order_num
FROM orders;
```
 
```js 
-- 使用聚合函数 COUNT(DISTINCT 列名) 计算订单表中不同客户的数量
SELECT COUNT(DISTINCT customer_id) AS customer_num
FROM orders;
```
 
```js 
-- 使用聚合函数 SUM 计算总订单金额
SELECT SUM(amount) AS total_amount
FROM orders;
```

## 分组

#### 字段分组

```js 
-- 使用分组聚合查询中每个客户的编号
SELECT customer_id
FROM orders
GROUP BY customer_id;
```
 
```js 
-- 使用分组聚合查询每个客户的下单数
SELECT customer_id, COUNT(order_id) AS order_num
FROM orders
GROUP BY customer_id;
```
 
```js 
-- 使用多字段分组查询表中 每个客户 购买的 每种商品 的总金额，相当于按照客户编号和商品编号分组
SELECT customer_id, product_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id, product_id;
```

#### having 子句

```js 
-- 使用 HAVING 子句查询订单数超过 1 的客户

SELECT customer_id, COUNT(order_id) AS order_num
FROM orders
GROUP BY customer_id
HAVING COUNT(order_id) > 1;
```
 
```js 
-- 使用 HAVING 子句查询订单总金额超过 100 的客户
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 100;
```

## 关联查询

#### CROSS JOIN

是一种简单的关联查询，不需要任何条件来匹配行，它直接将左表的 每一行 与右表的 每一行 进行组合，返回的结果是两个表的笛卡尔积。
```js 
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
CROSS JOIN departments d;
```

#### INNER JOIN

只返回两个表中满足关联条件的交集部分，即在两个表中都存在的匹配行。
```js 
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
JOIN departments d ON e.department = d.department;
```

#### OUTER JOIN

根据指定的关联条件，将两个表中满足条件的行组合在一起，并包含没有匹配的行 。
包括 LEFT OUTER JOIN 和 RIGHT OUTER JOIN 两种类型，分别表示查询左表和右表的所有行（即使没有被匹配），再加上满足条件的交集部分。有些数据库并不支持 RIGHT JOIN 语法，只需要把主表（from 后面的表）和关联表（LEFT JOIN 后面的表）顺序进行调换即可
```js 
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
LEFT JOIN departments d ON e.department = d.department;
```

## 子查询

```js 
-- 查询出订单总金额 > 200 的客户的姓名和他们的订单总金额
-- 主查询
SELECT name, total_amount
FROM customers
WHERE customer_id IN (
    -- 子查询
    SELECT DISTINCT customer_id
    FROM orders
    WHERE total_amount > 200
);
```

**exists**
用于检查主查询的结果集是否存在满足条件的记录，它返回布尔值（True 或 False），而不返回实际的数据。

```js 
-- 主查询
SELECT name, total_amount
FROM customers
WHERE EXISTS (
    -- 子查询
    SELECT 1
    FROM orders
    WHERE orders.customer_id = customers.customer_id
);
```

#### 组合查询

* **UNION** 操作：将两个或多个查询的结果集合并， 并去除重复的行 。即如果两个查询的结果有相同的行，则只保留一行。
* **UNION ALL** 操作：将两个或多个查询的结果集合并， 但不去除重复的行 。即如果两个查询的结果有相同的行，则全部保留。
```js 
SELECT name, age, department
FROM table1
UNION
SELECT name, age, department
FROM table2;
```

## 开窗函数

#### sum over

SUM(计算字段名) OVER (PARTITION BY 分组字段名)
```js 
SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    SUM(total_amount) OVER (PARTITION BY customer_id) AS customer_total_amount
FROM
    orders;
```

#### sum over order by

SUM(计算字段名) OVER (PARTITION BY 分组字段名 ORDER BY 排序字段 排序规则)
```js 
-- 计算每个客户的历史订单累计金额，并显示每个订单的详细信息

SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    SUM(total_amount) OVER (PARTITION BY customer_id ORDER BY order_date ASC) AS cumulative_total_amount
FROM
    orders;
```

#### rank

用于对查询结果集中的行进行 排名 的开窗函数。可以根据指定的列或表达式对结果集中的行进行排序，并为每一行分配一个排名。
在排名过程中，相同的值将被赋予相同的排名，而不同的值将被赋予不同的排名。
常见用法是在查询结果中查找前几名（Top N）或排名最高的行。
 
```js 
RANK() OVER (
  PARTITION BY 列名1, 列名2, ... -- 可选，用于指定分组列
  ORDER BY 列名3 [ASC|DESC], 列名4 [ASC|DESC], ... -- 用于指定排序列及排序方式
) AS rank_column
PARTITION BY 子句可选，用于指定分组列，将结果集按照指定列进行分组；
ORDER BY 子句用于指定排序列及排序方式，决定了计算 Rank 时的排序规则。
AS rank_column 用于指定生成的 Rank 排名列的别名。
```
 
```js 
SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    RANK() OVER (PARTITION BY customer_id ORDER BY total_amount DESC) AS customer_rank
FROM
    orders;
```

#### row_number

用于为查询结果集中的每一行分配唯一连续排名。
Row_Number函数为每一行都分配一个唯一的整数值，不管是否存在并列（相同排序值）的情况。
每一行都有一个唯一的行号，从 1 开始连续递增。

Row_Number 开窗函数的语法如下（几乎和 Rank 函数一模一样）

```js 
ROW_NUMBER() OVER (
  PARTITION BY column1, column2, ... -- 可选，用于指定分组列
  ORDER BY column3 [ASC|DESC], column4 [ASC|DESC], ... -- 用于指定排序列及排序方式
) AS row_number_column
PARTITION BY子句可选，用于指定分组列，将结果集按照指定列进行分组。ORDER BY 子句用于指定排序列及排序方式，决定了计算 Row_Number 时的排序规则。AS row_number_column 用于指定生成的行号列的别名。
```
 
```js 
SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY total_amount DESC) AS row_number
FROM
    orders;
```

#### lag / lead

在需要比较相邻行数据或进行时间序列分析时非常有用。

* Lag 函数用于获取当前行之前的某一列的值。Lag 函数的语法如下：
LAG(column_name, offset, default_value) OVER (PARTITION BY partition_column ORDER BY sort_column)
参数解释：
column_name：要获取值的列名。
offset：表示要向上偏移的行数。例如，offset为1表示获取上一行的值，offset为2表示获取上两行的值，以此类推。
default_value：可选参数，用于指定当没有前一行时的默认值。
PARTITION BY和ORDER BY子句可选，用于分组和排序数据。

* Lead 函数用于获取 当前行之后 的某一列的值。Lead 函数的语法如下：
LEAD(column_name, offset, default_value) OVER (PARTITION BY partition_column ORDER BY sort_column)
参数解释：
column_name：要获取值的列名。
offset：表示要向下偏移的行数。例如，offset为1表示获取下一行的值，offset为2表示获取下两行的值，以此类推。
default_value：可选参数，用于指定当没有后一行时的默认值。
PARTITION BY和ORDER BY子句可选，用于分组和排序数据。
 
```js 
SELECT 
    student_id,
    exam_date,
    score,
    LAG(score, 1, NULL) OVER (PARTITION BY student_id ORDER BY exam_date) AS previous_score,
    LEAD(score, 1, NULL) OVER (PARTITION BY student_id ORDER BY exam_date) AS next_score
FROM
    scores;
```

整理自网站SQL之母http://sqlmother.yupi.icu/#/learn（学习过，但记不住，单独查看太麻烦，所以整理给自己看）