---
title: 计算力扣银行的钱（leetcode1716）
author: 
tags: 
       - 算法

category: 
       - 算法

date: 2022-01-15 17:45:09
---
#### 前言

easy题我重拳出击！

#### 题目

Hercy 想要为购买第一辆车存钱。他 每天 都往力扣银行里存钱。

最开始，他在周一的时候存入 1 块钱。从周二到周日，他每天都比前一天多存入 1 块钱。在接下来每一个周一，他都会比 前一个周一 多存入 1 块钱。

给你 n ，请你返回在第 n 天结束的时候他在力扣银行总共存了多少块钱。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/calculate-money-in-leetcode-bank
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
点击跳转题目https://leetcode-cn.com/problems/calculate-money-in-leetcode-bank/

#### 思路

简单的等差数列公式。

#### 代码

else语句中
第一部分为所有完整周的原始形态的值，
第二部分为所有完整周的增加部分的值，
第三部分为剩余天数的值。
 
```js 
class Solution {
    
public:
    int totalMoney(int n) {
    
        if(n<=7){
    
            return (1+n)*n/2;
        }
        else{
    
            return (n/7*28 + 7*(n/7-1)*(n/7)/2 + (1+n-n/7*7 + 2*(n/7))*(n-n/7*7)/2);
        }
    }
};
```