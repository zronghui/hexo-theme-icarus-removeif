---
title: 剑指 Offer 26 树的子结构
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:13/"剑指 Offer 26 树的子结构".html'
date: 2020-06-30 21:55:13
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
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        # 考虑  [10,12,6,8,3,11] [10,12,6,8]
        #      [-1,3,2,0] []
        if not A and not B:
            return True
        if not A or not B:
            return False
        # a b 有相同的结点
        def helper(A, B):
            if not B:
                return True
            if not A:
                return False
            if A.val==B.val:
                if helper(A.left, B.left) and helper(A.right, B.right):
                    return True
            return False
        return helper(A, B) or self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)


```

