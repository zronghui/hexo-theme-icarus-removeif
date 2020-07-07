---
title: 剑指 Offer 33 二叉搜索树的后序遍历序列
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:29/"剑指 Offer 33 二叉搜索树的后序遍历序列".html'
date: 2020-06-30 21:55:29
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 33. 二叉搜索树的后序遍历序列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)



```python
class Solution:
    def verifyPostorder(self, l: List[int]) -> bool:
        # 1 3 2 6 5
        # start end
        # 0, 4 -> (0, 2) (3, 3)
        # 解法1. 递归
        def helper(l, start, end):
            # [start, end] 左闭右闭
            if end-start<2:
                return True
            # print(start, end)
            mid = start
            while l[mid]<l[end] and mid<end:
                mid += 1
            if not all(i>l[end] for i in l[mid:end]):
                return False
            return helper(l, start, mid-1) and helper(l, mid, end-1)
        return helper(l, 0, len(l)-1)
        # 解法2.(略) 
        # l = [] 存放子树的 (start, end)
        # 不断 pop, push 子树的子树
        # 如果 有一个子树不符合要求，return False
        # 最后 return True
```

