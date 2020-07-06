---
title: 剑指 Offer 32 - III 从上到下打印二叉树 III
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:27/"剑指 Offer 32 - III 从上到下打印二叉树 III".html'
date: 2020-06-30 21:55:27
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

层次遍历的同时，记录层次

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        l, res, stack = [], [], collections.deque()
        if not root:
            return res
        stack.append(root)
        level = 0
        while stack:
            t = []
            slen = len(stack)
            for _ in range(slen):
                node = stack.popleft()
                t.append(node.val)
                if node.left: stack.append(node.left)
                if node.right: stack.append(node.right)
            if level&1:
                res.append(t[::-1])
            else:
                res.append(t)
            level += 1
        return res
```

