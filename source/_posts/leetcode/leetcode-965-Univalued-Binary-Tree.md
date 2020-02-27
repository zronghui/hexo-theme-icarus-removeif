---
title: leetcode 965. Univalued Binary Tree
date: 2019-12-06 22:47:42
categories:
- leetcode
toc: true
tags:
- Tree
---
### 难度：Easy

<a href="https://leetcode.com/problems/univalued-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/univalued-binary-tree/">九章</a>
## 题目描述
A binary tree is _univalued_ if every node in the tree has the same value.

Return `true` if and only if the given tree is univalued.



**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_1.png)
        
    Input: [1,1,1,1,1,null,1]
    Output: true
    

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_2.png)
        
    Input: [2,2,2,5,2]
    Output: false
    



**Note:**

  1. The number of nodes in the given tree will be in the range `[1, 100]`.
  2. Each node's value will be an integer in the range `[0, 99]`.


**Tags:** Tree

**Difficulty:** Easy
## 答案
<!--more-->
```java

class Solution {
    public boolean isUnivalTree(TreeNode root) {
        if(root==null) return true;
        if(root.left!=null && root.left.val!=root.val) return false;
        if(root.right!=null && root.right.val!=root.val) return false;
        return isUnivalTree(root.left) && isUnivalTree(root.right);
    }
}
```
