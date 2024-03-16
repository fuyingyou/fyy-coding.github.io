---
title: 岛屿的周长-LeetCode463
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2020-11-01 09:08:22
---
### 题目

给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

**来源：力扣（LeetCode）**
**输入:**
[[0,1,0,0],
[1,1,1,0],
[0,1,0,0],
[1,1,0,0]]

**输出: 16**

解释: 它的周长是下面图片中的 16 个黄色的边：

![在这里插入图片描述](https://gitee.com/fuyingyou/picgo/raw/master/img_algorithm/202403151945290.png)

### 题解

```js 
class Solution {
    
    public int islandPerimeter(int[][] grid) {
    
        int m = grid.length;
        int n = grid[0].length;
        int res = 0;
        for(int i=0; i<m; i++){
    
            for(int j=0; j<n;j++){
    
                if(grid[i][j] == 1){
    
                //如果是岛屿，判断其四个方向有没有岛屿，没有周长++
                    if(i-1<0){
    
                        res++;
                    }else{
    
                        if(grid[i-1][j] == 0){
    
                            res++;
                        }
                    }
                    if(j-1<0){
    
                        res++;
                    }else{
    
                        if(grid[i][j-1] == 0){
    
                            res++;
                        }
                    }
                    if(i+1>=m){
    
                        res++;
                    }
                    else{
    
                        if(grid[i+1][j] == 0){
    
                            res++;
                        }
                    }
                    if(j+1>=n){
    
                        res++;
                    }
                    else{
    
                        if(grid[i][j+1] == 0){
    
                            res++;
                        }
                    }
                }
            }
        }
        return res;       
    }
}
```

### 总结

写的时候直接暴力统计每一个岛屿的四个方向是否与其他岛屿相连，
使用了一堆的if…；
看到别人的题解是先统计周长为有几块岛屿/*4，再判断如果岛屿两两相连，那么周长-2；