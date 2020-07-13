---
title: 剑指 Offer 68 - I 二叉搜索树的最近公共祖先
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:47/"剑指 Offer 68 - I 二叉搜索树的最近公共祖先".html'
date: 2020-06-30 21:56:47
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
    def lowestCommonAncestor(self, root, p, q):
        if p.val>root.val and q.val>root.val: 
            return self.lowestCommonAncestor(root.right, p, q)
        if p.val<root.val and q.val<root.val: 
            return self.lowestCommonAncestor(root.left, p, q)
        return root

```

