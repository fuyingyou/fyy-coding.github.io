---
title: 分发糖果（Java实现）
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2021-04-06 09:14:32
---
### 题目

老师想给孩子们分发糖果，有 N 个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。
你需要按照以下要求，帮助老师给这些孩子分发糖果：
每个孩子至少分配到 1 个糖果。
相邻的孩子中，评分高的孩子必须获得更多的糖果。
那么这样下来，老师至少需要准备多少颗糖果呢？
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/candy

### 示例1

输入: [1,0,2]
输出: 5
解释: 你可以分别给这三个孩子分发 2、1、2 颗糖果。

### 示例2

输入: [1,2,2]
输出: 4
解释: 你可以分别给这三个孩子分发 1、2、1 颗糖果。
第三个孩子只得到 1 颗糖果，这已满足上述两个条件

### 解题思路

两个数组left和right分别记录从左向右规则和从右向左规则时，每个孩子应该分的糖果数。
题目要求即为同时满足两个方向的规则，即两个数组按位取max。
最终结果要加上每个孩子至少一个糖果。（或者left，right数组初始化为1）
```js 
class Solution {
    public int candy(int[] ratings) {
        int len = ratings.length;
    	int sum = len;
    	int[] left = new int[len];
    	int[] right = new int[len];
    	for (int i = 1; i < len; i++) {
            if(ratings[i] > ratings[i-1]) {
                left[i] = left[i-1]+1;
            }
        }
    	for (int i = len-2; i>=0; i--) {
            if(ratings[i] > ratings[i+1]) {
                right[i] = right[i+1]+1;	
            }
        }
    	
    	for (int i = 0; i < len; i++) {
            sum += Math.max(left[i], right[i]);
        }
    	return sum;
    }
}
```