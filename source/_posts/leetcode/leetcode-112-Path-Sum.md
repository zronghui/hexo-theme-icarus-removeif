---
title: leetcode 112. Path Sum
date: 2019-11-23 15:47:36
categories:
- leetcode
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/path-sum/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/path-sum/">九章</a>
## 题目描述
Given a binary tree and a sum, determine if the tree has a root-to-leaf path
such that adding up all the values along the path equals the given sum.

**Note:**  A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,
        
          **5**
         **/** \
        **4**   8
       **/**   / \
      **11**  13  4
     /  **\**      \
    7    **2**      1
    

return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.


**Tags:** Tree, Depth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root==null) return false;
        if(root.left==null && root.right==null && root.val==sum)
            return true;
        return hasPathSum(root.left, sum-root.val) || hasPathSum(root.right, sum-root.val);
    }
}
```
