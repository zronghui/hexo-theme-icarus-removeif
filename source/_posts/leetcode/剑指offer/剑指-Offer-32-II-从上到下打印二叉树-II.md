---
title: 剑指 Offer 32 - II 从上到下打印二叉树 II
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:26/"剑指 Offer 32 - II 从上到下打印二叉树 II".html'
date: 2020-06-30 21:55:26
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
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        l, stack, res = [], collections.deque(), []
        if not root:
            return res
        stack.append(root)
        while stack:
            t = []
            slen = len(stack)
            for _ in range(slen):
                node = stack.popleft()
                t.append(node.val)
                if node.left: stack.append(node.left)
                if node.right: stack.append(node.right)
            res.append(t)
        return res
```

