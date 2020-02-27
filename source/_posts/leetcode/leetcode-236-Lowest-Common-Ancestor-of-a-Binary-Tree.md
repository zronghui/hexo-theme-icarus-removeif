---
title: leetcode 236. Lowest Common Ancestor of a Binary Tree
date: 2019-12-03 23:14:34
categories:
- leetcode
toc: true
tags:
- Tree
---
### 难度：Middle

<a href="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/lowest-common-ancestor-of-a-binary-tree/">九章</a>
## 题目描述
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes
in the tree.

According to the [definition of LCA on
Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): "The lowest
common ancestor is defined between two nodes p and q as the lowest node in T
that has both p and q as descendants (where we allow **a node to be a
descendant of itself** )."

Given the following binary tree:  root = [3,5,1,6,2,0,8,null,null,7,4]

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)



**Example 1:**
        
    Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
    Output: 3
    Explanation: The LCA of nodes 5 and 1 is 3.
    

**Example 2:**
        
    Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
    Output: 5
    Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
    



**Note:**

  * All of the nodes' values will be unique.
  * p and q are different and both values will exist in the binary tree.


**Tags:** Tree

**Difficulty:** Medium
## 答案
<!--more-->
```java

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //找 P 或 Q, 或者没找到
        if(root==null || root==q || root==p) return root;
        //左边查询结果
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        //右边查询结果
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //因为肯定有这两个节点，所以如果一边是空，另一边查到了 p 或 q 任意一个，那个就是目标节点
        //如果两边都查到了，肯定是一个 q,一个p ,所以直接返回顶部的就行。
        //两边都没查到的话 就不存在这种情况了
        if(left==null) return right;
        else if(right==null) return left;
        return root;
    }
}
```
