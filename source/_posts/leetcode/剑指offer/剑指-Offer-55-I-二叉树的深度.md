---
title: 剑指 Offer 55 - I 二叉树的深度
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:14/"剑指 Offer 55 - I 二叉树的深度".html'
date: 2020-06-30 21:56:14
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

class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        res = 0
        stack = collections.deque()
        if root: stack.append(root)
        while stack:
            res += 1
            for _ in range(len(stack)):
                node = stack.popleft()
                if node.left: stack.append(node.left)
                if node.right: stack.append(node.right)
        return res
```

