---
title: 剑指 Offer 38 字符串的排列
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:39/"剑指 Offer 38 字符串的排列".html'
date: 2020-06-30 21:55:39
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 38. 字符串的排列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/submissions/)

```python
class Solution:
    def permutation(self, s: str) -> List[str]:
        n = len(s)
        if n==0: return []
        if n==1: return [s]
        c = s[0]
        l = self.permutation(s[1:])
        res = set()
        for si in l:
            for i in range(n):
                res.add(si[:i]+c+si[i:])
        return list(res)

```

