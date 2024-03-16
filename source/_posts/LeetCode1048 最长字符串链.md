---
title: LeetCode1048 最长字符串链
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2023-08-19 13:04:04
---
### 思路

* 从短到长，增加字母的话，有多个位置，并且每个位置都有26种选择，正难则反。选择从长到短，删除某个字母。
* 删除字母得到的新字符串可能已经计算过，所以将计算的结果都记录一下。
* 记忆化搜索： 先查表再计算，先存表再返回。

### 代码

```js 
class Solution {
    
	//记忆化
    HashMap<String, Integer> hashMap = new HashMap<>();
    public int longestStrChain(String[] words) {
    
        for (String str: words) {
    
            hashMap.put(str, 0);
        }
        int ans = 0;
        for (String str : words) {
    
            ans = Math.max(ans, dfs(str));
        }
        return ans;
    }

    private int dfs(String str) {
    
        int res = hashMap.get(str);
        //大于0代表曾经计算过
        if(res > 0) {
    
            return res;
        }
        for (int i = 0; i < str.length(); i++) {
    
            String tmp = str.substring(0, i) + str.substring(i+1);
            if (hashMap.containsKey(tmp)) {
    
                res = Math.max(res, dfs(tmp));
            }
        }
        hashMap.put(str, res + 1);
        return res + 1;
    }
}
```