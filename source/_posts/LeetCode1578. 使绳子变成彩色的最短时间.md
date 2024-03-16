---
title: LeetCode1578. 使绳子变成彩色的最短时间
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2023-08-19 13:21:33
---
### 思路

* 拆除成本 = 全部拆除 - 最大的不拆除
* 在统计成本的同时，维持一个成本的最大值

### 代码

```js 
class Solution {
    
    public int minCost(String colors, int[] neededTime) {
    
        int res = 0;
        int i = 0;
        int len = colors.length();
        while (i < len) {
    
            int max = -1;
            int sum = 0;
            char ch = colors.charAt(i);
            while(i < len && colors.charAt(i) == ch) {
    
                sum += neededTime[i];
                max = Math.max(max, neededTime[i]);
                i++;
            }
            res += sum - max;
        }
        return res;
    }
}
```