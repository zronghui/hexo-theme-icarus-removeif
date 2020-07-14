---
title: 剑指 Offer 66 构建乘积数组
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:44/"剑指 Offer 66 构建乘积数组".html'
date: 2020-06-30 21:56:44
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
    def constructArr(self, a: List[int]) -> List[int]:
        # a [1,2,3,4,5]
        # l [1,1,2,6,24]
        # r [120,60,20,5,1]
        n = len(a)
        l, r = [1]*n, [1]*n
        for i in range(1, n):
            l[i] = l[i-1]*a[i-1]
        for i in reversed(range(n-1)):
            r[i] = r[i+1]*a[i+1]
        return [l[i]*r[i] for i in range(n)]
```

