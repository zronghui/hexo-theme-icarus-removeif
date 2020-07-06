---
title: 剑指 Offer 28 对称的二叉树
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:16/"剑指 Offer 28 对称的二叉树".html'
date: 2020-06-30 21:55:16
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def helper(a, b):
            if not a and not b:
                return True
            if not a or not b:
                return False
            if a.val!=b.val:
                return False
            return helper(a.left, b.right) and helper(a.right, b.left)
        if not root:
            return True
        return helper(root.left, root.right)

```

