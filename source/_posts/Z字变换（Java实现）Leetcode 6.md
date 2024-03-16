---
title: Z字变换（Java实现）Leetcode 6
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2022-03-28 15:36:03
---
### 题目

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
 
比如输入字符串为 “LEETCODEISHIRING” 行数为 3 时，排列如下：
 
L C I R
E T O E S I I G
E D H N
 
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：“LCIRETOESIIGEDHN”。
 
请你实现这个将字符串进行指定行数变换的函数：
 
string convert(string s, int numRows);
 
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zigzag-conversionhttps://leetcode-cn.com/problems/zigzag-conversion

### 示例1

输入: s = “LEETCODEISHIRING”, numRows = 3
输出: “LCIRETOESIIGEDHN”

### 示例2

输入: s = “LEETCODEISHIRING”, numRows = 4
输出: “LDREOEIIECIHNTSG”
解释:
L D R
E O E I I
E C I H N
T S G

### 代码思路

1. 一行或两行可以直接返回
1. 其余情况先从上往下填满，然后向右上方向填满，重复此步骤
1. 加起来，toString()
```js 
class Solution {
    public String convert(String s, int numRows) {
        if(numRows <= 2) {
            return s;
    	}
        char[][] chs = new char[numRows][s.length()];
        int row = 0,col = 0;
        for(int i=0; i<s.length();){
            while(row < numRows && i<s.length())
            {
                chs[row][col] = s.charAt(i++);
                row++;
            }
            row--;
            while(row >= 1 && i<s.length()) {
                chs[--row][++col] = s.charAt(i++);
            }
            row++;
        }
        StringBuilder stringBuilder = new StringBuilder();
        for(int i=0; i<numRows; i++){
            for(int j=0; j< chs[0].length; j++){
            	if(chs[i][j] != 0) {
                    stringBuilder.append(chs[i][j]);
            	}
            }
        }
        return stringBuilder.toString();
    }
}
```