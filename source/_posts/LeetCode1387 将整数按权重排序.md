---
title: LeetCode1387 将整数按权重排序
author: 
tags: 
       - java

category: 
       - 算法

date: 2023-08-19 13:03:04
---
### 思路

* 首先是这种计算权重的方式很有可能出现重复，所以需要记忆化搜索
* 记忆化搜索：先查表再计算，先存表再返回。
* 将整数 x 和计算的权重分别存储数组的0和1的位置
* 重写compare将数组排序按规则排序
* 返回结果

### 代码

```js 
class Solution {
    
    private HashMap<Integer, Integer> me = new HashMap<>();
    public int getKth(int lo, int hi, int k) {
    
        int[][] arr = new int[hi - lo + 1][2];
        for (int i = lo; i <= hi; i++) {
    
            int tmp = dfs(i);
            me.put(i, tmp);
            arr[i - lo][0] = i;
            arr[i - lo][1] = tmp;
        }
        Arrays.sort(arr, new Comparator<int[]>() {
    
            @Override
            public int compare(int[] o1, int[] o2) {
    
                return o1[1] == o2[1] ? o1[0] - o2[0] : o1[1] - o2[1];
            }
        });

        return arr[k-1][0];
    }

    int dfs(int x) {
    
        int res = 0;
        if(me.get(x) != null){
    
            res = me.get(x);
            return res;
        }else if (x == 1) {
    
            res = 0;
        } else if (x % 2 == 1) {
    
            res = 1 + dfs(x * 3 + 1);
        } else {
    
            res = 1 + dfs(x / 2);
        }
        me.put(x, res);
        return res;
    }
}
```