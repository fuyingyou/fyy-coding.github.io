---
title: LeetCode 1700. 无法吃午餐的学生数量
author: 
tags: 
       - 职场和发展

category: 
       - 算法

date: 2022-10-21 13:39:26
---
##### 题目来源

LeetCode 1700. 无法吃午餐的学生数量https://leetcode.cn/problems/number-of-students-unable-to-eat-lunch/

##### 题目

学校的自助午餐提供圆形和方形的三明治，分别用数字 0 和 1 表示。所有学生站在一个队列里，每个学生要么喜欢圆形的要么喜欢方形的。
餐厅里三明治的数量与学生的数量相同。所有三明治都放在一个 栈 里，每一轮：

如果队列最前面的学生 喜欢 栈顶的三明治，那么会 拿走它 并离开队列。
否则，这名学生会 放弃这个三明治 并回到队列的尾部。
这个过程会一直持续到队列里所有学生都不喜欢栈顶的三明治为止。

给你两个整数数组 students 和 sandwiches ，其中 sandwiches[i] 是栈里面第 i 个三明治的类型（i = 0 是栈的顶部）， students[j] 是初始队列里第 j 名学生对三明治的喜好（j = 0 是队列的最开始位置）。请你返回无法吃午餐的学生数量。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/number-of-students-unable-to-eat-lunch
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 个人理解

* 总的来说就是，食物不能乱碰（厨师洁癖），每个人，只能拿栈顶的三明治。
* 如果栈顶是圆形的，你就需要一个喜欢吃圆的人把三明治拿走。
* 如果栈顶是方形的，你就需要一个喜欢吃方的人把三明治拿走。
* 如果大伙没有喜欢栈顶三明治的人，不好意思，都饿着吧。

##### 代码

```js 
class Solution {
    
    public int countStudents(int[] students, int[] sandwiches) {
    
       int x=0;
       int y=0;
       for(int i=0; i<students.length; i++){
    
           if(students[i] == 0){
    
               x++;
           }else{
    
               y++;
           }
       }

        for(int j=0; j<sandwiches.length; j++){
    
            if(sandwiches[j] == 0 && x>0){
    
                x--;
            }else if(sandwiches[j] == 1 && y>0){
    
                y--;
            }else{
    
                return x+y;
            }
        }
        return x+y;
    }
}
```