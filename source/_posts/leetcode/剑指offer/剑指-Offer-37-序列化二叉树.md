---
title: 剑指 Offer 37 序列化二叉树
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:37/"剑指 Offer 37 序列化二叉树".html'
date: 2020-06-30 21:55:37
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
class Codec:
    def serialize(self, root):
        l = []
        def helper(node):
            if node:
                l.append(str(node.val))
                helper(node.left)
                helper(node.right)
            else:
                l.append('X')
        helper(root)
        return ','.join(l)

    def deserialize(self, data):
        l = collections.deque(data.split(','))
        def helper():
            val = l.popleft()
            if val=='X':
                return None
            node = TreeNode(val)
            node.left = helper()
            node.right = helper()
            return node
        return helper()

```

