---
title: 剑指 Offer 36 二叉搜索树与双向链表
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:35/"剑指 Offer 36 二叉搜索树与双向链表".html'
date: 2020-06-30 21:55:35
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
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
"""
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        # 二叉搜索树 的中序遍历是有序的

        # 返回 node 的中序遍历链 的首尾节点
        def helper(node):
            if not node:
                return None, None
            lstart, lend = helper(node.left)
            rstart, rend = helper(node.right)
            if lstart:
                start = lstart
                lend.right = node
                node.left = lend
            else:
                start = node
            if rstart:
                end = rend
                node.right = rstart
                rstart.left = node
            else:
                end = node
            return start, end

        if not root:
            return None
        start, end = helper(root)
        start.left = end
        end.right = start
        return start

```

