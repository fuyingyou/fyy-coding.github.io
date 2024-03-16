---
title: 回溯算法之八皇后问题（Java实现）
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2021-04-06 09:16:06
---
#### 要求用回溯法求解 8-皇后问题

八皇后问题：使放置在 8/*8 棋盘上的 8 个皇后彼此不受攻击。
 
即：任何两个皇后都不在同一行、同一列或同一斜线上。
 
请输出 8 皇后问题的所有可行解的总数。
 
```js 
public class EightQueen {
	
    public static int[][] array = new int[8][8];
    public static int sum;
	
    public static void main(String[] args) {
		
        search(0);
        System.out.println(sum);
		
    }
	
    public static void search(int i){
        if(i > 7){
            sum++;
            return;
        }
        for(int j = 0; j < 8; j++){
            if(check(i, j)){
                array[i][j] = 1;
                search(i+1);
                array[i][j] = 0;
            }
        }
    }
	
    public static boolean check(int i, int j){
        for(int k = 0; k < i; k++){
            if(array[k][j] == 1){
                return false;
            }
        }
		
        for(int m = i-1, n = j-1; m>=0&& n>=0; m--, n--){
            if(array[m][n] == 1){
                return false;
            }
        }
		
        for(int m = i-1, n = j+1; m>=0&& n<=7; m--, n++){
            if(array[m][n] == 1){
                return false;
            }
        }
        return true;
    }
}
```