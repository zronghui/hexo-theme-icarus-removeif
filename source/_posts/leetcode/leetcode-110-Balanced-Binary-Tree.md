---
title: leetcode 110. Balanced Binary Tree
date: 2019-11-23 15:12:48
categories:
- leetcode
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/balanced-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/balanced-binary-tree/">九章</a>
## 题目描述
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of _every_ node differ in
> height by no more than 1.



**Example 1:**

Given the following tree `[3,9,20,null,null,15,7]`:
                3       / \      9  20        /  \       15   7

Return true.  
  
**Example 2:**

Given the following tree `[1,2,2,3,3,null,null,4,4]`:
                   1          / \         2   2        / \       3   3      / \     4   4    

Return false.


**Tags:** Tree, Depth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return maxDepth(root)!=-1;
    }
    private int maxDepth(TreeNode root){
        if(root==null) return 0;
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        if(Math.abs(left-right)>1 || left==-1 || right==-1) return -1;
        return Math.max(left, right)+1;
    }
}
```
