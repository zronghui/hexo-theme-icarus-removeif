---
title: 剑指 Offer 10- II 青蛙跳台阶问题
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:43/"剑指 Offer 10- II 青蛙跳台阶问题".html'
date: 2020-06-30 21:54:43
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
    def numWays(self, n: int) -> int:
        if n<2:
            return 1
        a = b = 1
        for _ in range(1, n):
            a, b = a+b, a
        return a%1000000007

```

