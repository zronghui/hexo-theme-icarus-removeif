---
title: 剑指 Offer 07 重建二叉树
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:39/"剑指 Offer 07 重建二叉树".html'
date: 2020-06-30 21:54:39
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
    def rootIndex(self, preorder, inorder):
        for i in range(len(inorder)):
            if inorder[i]==preorder[0]:
                return i

    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not preorder:
            return None
        i = self.rootIndex(preorder, inorder)
        root = TreeNode(preorder[0])
        root.left = self.buildTree(preorder[1:i+1], inorder[:i])
        root.right = self.buildTree(preorder[i+1:], inorder[i+1:])
        return roots
```

