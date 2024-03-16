---
title: LeetCode1218 最长定差子序列
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2023-08-19 13:03:02
---
### 思路

* 因为不能改变顺序，所以后面的元素，直接看它前面的元素就行。
* 长度为n的数组 = 长度为n - 1的数组 + 第 n 个数
* 从前向后遍历，对于每个元素，如果能找到它的前一个元素，就在前一个元素的基础上+1，否则就录入1。
* 同时记录录入的数据的最大值

### 代码

```js 
class Solution {
    
    public int longestSubsequence(int[] arr, int difference) {
    
        HashMap<Integer, Integer> hashMap = new HashMap<>();
        int res = 0;
        for (int i = 0; i < arr.length; i++) {
    
            int temp = hashMap.getOrDefault(arr[i] - difference, 0) + 1;
            hashMap.put(arr[i], temp);
            res = Math.max(res, temp);
        }
        return res;
    }
}
```