---
title: Leetcode 779. 第K个语法符号
author: 
tags: 
       - 算法

category: 
       - 算法

date: 2022-10-21 13:14:05
---
##### 题目来源： Leetcode 779. 第K个语法符号https://leetcode.cn/problems/k-th-symbol-in-grammar/

##### 题目描述

我们构建了一个包含 n 行( 索引从 1 开始 )的表。首先在第一行我们写上一个 0。接下来的每一行，将前一行中的0替换为01，1替换为10。

例如，对于 n = 3 ，第 1 行是 0 ，第 2 行是 01 ，第3行是 0110 。
给定行数 n 和序数 k，返回第 n 行中第 k 个字符。（ k 从索引 1 开始）

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/k-th-symbol-in-grammar
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 个人思路

* 每一行的前半部分正好为上一行
* 每一行的后半部分正好为前半部分的反转。
* 后半部分因为相当于上一行的反转，用1-x来达到这种目的

代码
```js 
class Solution {
    
    public int kthGrammar(int n, int k) {
    
        if(n==1){
    
            return 0;
        }

		//处于前半部分还是后半部分
        if(k > Math.pow(2,n-2)){
    
            return (1-kthGrammar(n-1, k-(int)Math.pow(2,n-2)));
        }else{
    
            return kthGrammar(n-1,k);
        }
    }
}
```
 
没想到这辈子还能赶上一次绿的题。