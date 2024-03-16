---
title: 回溯算法之装载问题（Java实现）
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2021-04-06 09:15:35
---
#### 问题描述

用回溯法编写一个递归程序解决如下装载问题：
有 n 个集装箱要装上 2 艘载重分别为 c1 和 c2的轮船，其中集装箱 i 的
重量为 wi（1≤ i ≤ n），且∑ 𝑤𝑖 ≤ 𝑐1 + 𝑐2 。
问是否有一个合理的装载方案可以将这 n 个集装箱装上这 2 艘轮船？如果有，请给出装载方案。

##### 示例

当 n=3，c1=c2=50，且 w=[10,40,40]时，可以将集装箱 1 和 2 装到第一艘轮船上，集装箱3装到第二艘轮船上；
如果 w=[20,40,40]时，无法将这 3 个集装箱都装上轮船。

Java代码

```js 
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Load {
	
    public static int weight1;  //记录第一艘船的载重能力
    public static int weight2;  //记录第二艘船的载重能力
	
    public static int sum1 = 0,sum2 = 0;        //分别代表此时第一艘船的载重和所有集装箱的总重量
    public static int[] arr;                    //记录集装箱的重量
    public static int n;                        //集装箱的个数
    public static List<Integer> list = new ArrayList<Integer>(); //第一艘船的集装箱的装载的重量
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        n = scanner.nextInt();
        arr = new int[n];
        weight1 = scanner.nextInt();
        weight2 = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            arr[i] = scanner.nextInt();
            sum2 += arr[i];
        }
        scanner.close();
        backtrack(0);
    }
    public static void backtrack(int i) {
        if(sum1 > weight1) {   // 如若超载，则回溯
            return;
        }
        if(i == n) {
            if(sum2 - sum1 < weight2) {      
                System.out.println(list);
            }
            return;
        }
        sum1 += arr[i];
        list.add(arr[i]);
        backtrack(i+1);
        sum1 -= arr[i];
        list.remove(list.size()-1);
        backtrack(i+1);
		
    }
}
```