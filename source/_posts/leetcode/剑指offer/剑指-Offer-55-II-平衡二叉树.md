---
title: 剑指 Offer 55 - II 平衡二叉树
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:16/"剑指 Offer 55 - II 平衡二叉树".html'
date: 2020-06-30 21:56:16
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
    def isBalanced(self, root: TreeNode) -> bool:
        def helper(node):
            if not node:
                return 0
            ldepth = helper(node.left)
            rdepth = helper(node.right)
            if -1 in [ldepth, rdepth] or abs(rdepth-ldepth)>1: return -1
            return max(ldepth, rdepth)+1
        return helper(root)!=-1
```

