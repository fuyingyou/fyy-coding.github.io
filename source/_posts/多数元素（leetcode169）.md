---
title: 多数元素（leetcode169）
author: 
tags: 
       - 数据结构

category: 
       - 算法

date: 2022-01-13 17:10:07
---
### 多数元素leetcode169

##### 1、题目

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/majority-element
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
点击跳转力扣169多数元素https://leetcode-cn.com/problems/majority-element/

##### 2、思路

摩尔投票算法。

个人理解：

记录第一个数为flag，初始化count为1，从第二个数开始遍历，当前元素与flag相等时，count++；
当前元素与flag不相等时，count–，当count为0时，记录下一个元素为flag，遍历到末尾则flag为最终结果。

原理

两两抵消，flag元素和非flag元素。当flag元素全部抵消（count==0）时，下一个元素为新的flag。最终剩下的即为多数元素。

##### 3、代码

```js 
class Solution {
    
public:
    int majorityElement(vector<int>& nums) {
    
        int flag = *nums.begin();
        int count=1;
        for(auto i=nums.begin()+1; i<nums.end(); i++){
    
            if(flag == *i){
    
                count++;
                if(count > nums.size()/2){
    
                    return flag;
                }
            }else{
    
                count--;
                if(count == 0){
    
                    flag = *(++i);
                    count = 1;
                }
            }
        }
        return flag;
    }
};
```