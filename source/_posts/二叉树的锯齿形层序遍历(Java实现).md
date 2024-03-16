---
title: 二叉树的锯齿形层序遍历(Java实现)
author: 
tags: 
       - leetcode

category: 
       - 算法

date: 2021-04-06 09:15:05
---
### 题目

给定一个二叉树，返回其节点值的锯齿形层序遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。
/#/#/#示例
给定二叉树 [3,9,20,null,null,15,7],
![](https://gitee.com/fuyingyou/picgo/raw/master/img_algorithm/202403151952749.png)

返回锯齿形层序遍历如下：

```js 
[
  [3],
  [20,9],
  [15,7]
]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/

### Java代码实现

```js 
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
 
public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {	
    	List<List<Integer>> res = new ArrayList<List<Integer>>();
    	if(root == null) {			//二叉树为空，直接返回空结果
    		return res;
    	}
    	ArrayDeque<TreeNode> deque = new ArrayDeque<TreeNode>();
    	deque.add(root);
    	leverOrder(res, deque, 1);
    	return res;
    }
    
    /*
     * 参数列表说明：
     * res 结果集
     * deque 双向队列存储当前层次遍历的结点
     * flag 记录当前遍历的层数，用来控制层序遍历的方向
     */
    void leverOrder(List<List<Integer>> res,ArrayDeque<TreeNode> deque,int flag) {
    	int size = deque.size();
    	if(size == 0) {
    		return;
    	}
    	ArrayList<Integer> arrayList = new ArrayList<Integer>(); 
    	if(flag == 1) {
    		while(size-- != 0) {
    			TreeNode root = deque.pollFirst();
    			arrayList.add(root.val);
    			if(root.left != null) {
    				deque.offer(root.left);
    			}
    			if(root.right != null) {
    				deque.offer(root.right);
    			}
    		}
    		//加入结果集，递归下一层
    		res.add(arrayList);
    		leverOrder(res, deque, 2);
    	}else {
    		while(size-- != 0) {
    			TreeNode root = deque.pollLast();
    			arrayList.add(root.val);
    			if(root.right != null) {
    				deque.offerFirst(root.right);
    			}
    			if(root.left != null) {
    				deque.offerFirst(root.left);
    			}
    		}
    		//加入结果集，//递归下一层
    		res.add(arrayList);
    		leverOrder(res, deque, 1);
		}
    	
    }
}
```