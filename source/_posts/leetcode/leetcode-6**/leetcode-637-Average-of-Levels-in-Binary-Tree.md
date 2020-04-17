---
title: leetcode 637. Average of Levels in Binary Tree
date: 2019-12-07 19:27:05
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Tree
---
### 难度：Easy

<a href="https://leetcode.com/problems/average-of-levels-in-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/average-of-levels-in-binary-tree/">九章</a>
## 题目描述
Given a non-empty binary tree, return the average value of the nodes on each
level in the form of an array.

**Example 1:**  
        
    Input:
        3
       / \
      9  20
        /  \
       15   7
    Output: [3, 14.5, 11]
    Explanation:
    The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
    

**Note:**  

  1. The range of node's value is in the range of 32-bit signed integer.


**Tags:** Tree

**Difficulty:** Easy
## 答案
<!--more-->
```java


// 为何注释部分失败
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<>();
        if(root==null) return res;
        Deque<TreeNode> deque = new ArrayDeque<>();
        deque.add(root);
        while(!deque.isEmpty()){
            int size = deque.size();
            double sum = 0;
            for(int i=0; i<size; i++) {
                TreeNode node = deque.removeFirst();
                sum += (double)node.val;
                // sum += (double)node.val/size;
                if(node.left!=null) deque.add(node.left);
                if(node.right!=null) deque.add(node.right);
            }
            res.add(sum/size);
            // res.add(sum);
        }
        return res;
    }
}
```
