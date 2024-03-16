---
title: LeetCode 901. 股票价格跨度
author: 
tags: 
       - 算法

category: 
       - 算法

date: 2022-10-21 13:24:58
---
忘了队列咋用了

##### 题目来源

LeetCode 901. 股票价格跨度https://leetcode.cn/problems/online-stock-span/

##### 题目

编写一个 StockSpanner 类，它收集某些股票的每日报价，并返回该股票当日价格的跨度。

今天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。

例如，如果未来7天股票的价格是 [100, 80, 60, 70, 60, 75, 85]，那么股票跨度将是 [1, 1, 1, 2, 1, 4, 6]。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/online-stock-span
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 个人思路

![在这里插入图片描述](https://gitee.com/fuyingyou/picgo/raw/master/img_algorithm/202403151953885.png)

* 设A为 B之前 最近的 比B大 的元素。
* 即A与B之间的任一元素X满足 A>X && X<= B
* 若此时在B后添加一元素C
* 若C小于B则返回1
* 否则，C>=B，又因为A与B之间任一元素X<=B, 即X<=B<=C
* 所以我们获取A后一共多少个元素就行。即A与B之间的元素在添加B时就可以remove了。
* 为了避免没有元素充当A这种情况，在初始化list时添加一个Integer.MAX_VALUE元素。

##### 代码

```js 
class StockSpanner {
    

    private List<int[]> list;
    private int index = -1;

    public StockSpanner() {
    
        list = new ArrayList<int[]>();
        list.add(new int[]{
    index,Integer.MAX_VALUE});
    }
    
    public int next(int price) {
    
        index++;
        for(int i=list.size()-1; i>=0; i--){
    
            if(list.get(i)[1] <= price){
    
                list.remove(i);
            }else{
    
                continue;
            }
        }

        int res = index - list.get(list.size()-1)[0];

        list.add(new int[]{
    index,price});
        return res;
    }
}

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner obj = new StockSpanner();
 * int param_1 = obj.next(price);
 */
```