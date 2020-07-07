---
title: 剑指 Offer 34 二叉树中和为某一值的路径
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:31/"剑指 Offer 34 二叉树中和为某一值的路径".html'
date: 2020-06-30 21:55:31
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
    def pathSum(self, root: TreeNode, target: int) -> List[List[int]]:
        res, l = [], []
        if not root:
            return []
        def helper(root, cursum):
            cursum += root.val
            l.append(root.val)
            if not root.left and not root.right and cursum==target:
                res.append(l.copy())
            if root.left: helper(root.left, cursum)
            if root.right: helper(root.right, cursum)
            l.pop()
        helper(root, 0)
        return res
```

