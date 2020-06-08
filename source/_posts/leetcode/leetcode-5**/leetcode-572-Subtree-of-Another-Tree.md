---
thumbnail:
title: leetcode 572. Subtree of Another Tree
date: 2020-06-08 20:23:46
categories:
- leetcode
- leetcode-5**
toc: true
tags:
- Tree
recommend: 1
keywords:
uniqueId: '2020-06-08 20:23:46/"leetcode 572. Subtree of Another Tree".html'
---

<a href="https://leetcode.com/problems/subtree-of-another-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/subtree-of-another-tree/">九章</a>
## 题目描述
Given two non-empty binary trees **s** and **t** , check whether tree **t**
has exactly the same structure and node values with a subtree of **s**. A
subtree of **s** is a tree consists of a node in **s** and all of this node's
descendants. The tree **s** could also be considered as a subtree of itself.

**Example 1:**  
Given tree s:
        
         3
        / \
       4   5
      / \
     1   2


Given tree t:
        
       4 
      / \
     1   2


Return **true** , because t has the same structure and node values with a
subtree of s.

**Example 2:**  
Given tree s:
        
         3
        / \
       4   5
      / \
     1   2
        /
       0


Given tree t:
        
       4
      / \
     1   2


Return **false**.


**Tags:** Tree

**Difficulty:** Easy

## 答案
<!--more-->
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, s: TreeNode, t: TreeNode) -> bool:
        if s is None:
            return False
        return self.isSubtree(s.left, t) or self.isSubtree(s.right, t) or self.isSameTree(s, t)

    def isSameTree(self, s: TreeNode, t: TreeNode) -> bool:
        if s is None and t is None:
            return True
        if any([s is None, t is None]):
            return False
        return s.val==t.val and self.isSameTree(s.left, t.left) and self.isSameTree(s.right, t.right)
```

