---
title: 去除重复字母（Java实现）
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2021-04-06 09:16:38
---
### 题目 去除重复字母

给你一个字符串 s ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

##### 示例1

输入：s = “bcabc”
输出：“abc”

##### 示例2

输入：s = “cbacdcbc”
输出：“acdb”

##### 数据范围

1 <= s.length <= 104
s 由小写英文字母组成
 
来源：力扣（LeetCode）
https://leetcode-cn.com/problems/remove-duplicate-letters/

##### 代码

```js 
/*
 * 算法思想
 * 当字典序最小时，即12341 12351 主要看4，5的位置
 * 用栈存储，当栈空时直接入栈
 * 栈不为空时，
 * 	若栈中包含当前要入栈的元素直接跳到下一次循环。（结果字符串每个字符只含有一次）
 * 	若当前要入栈的字母比栈顶字母大时，考虑是否栈顶元素出栈
 * 	若栈顶元素在剩余字符串中仍然存在，那么就可以出栈，出栈后继续判断新的栈顶元素是否出栈。
 */
import java.util.Stack;

public class RemoveDuplicateLetters {

    public static String removeDuplicateLetters(String s){
        Stack<Character> stack = new Stack<Character>();
            for (int i = 0; i < s.length(); i++) {
                char c=s.charAt(i);
                if(stack.contains(c))
                    continue;
                while(!stack.isEmpty() && stack.peek()>c && s.indexOf(stack.peek(),i)!=-1)
                    stack.pop();
                stack.push(c);
            }
            char chars[]=new char[stack.size()];
            for (int i = 0; i < stack.size(); i++) {
                chars[i]=stack.get(i);
            }
            return new String(chars);
    }

    public static void main(String[] args) {
        String string = "bbcaac";
        System.out.println(removeDuplicateLetters(string));
    }
}
```