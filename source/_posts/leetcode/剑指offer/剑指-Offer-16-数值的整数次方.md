---
title: 剑指 Offer 16 数值的整数次方
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:56/"剑指 Offer 16 数值的整数次方".html'
date: 2020-06-30 21:54:56
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
    def myPow(self, x: float, n: int) -> float:
        if x==0:
            return 0
        if n==0:
            return 1
        if x<0 and n%2==1:
            sign = -1
        else:
            sign = 1
        if n<0:
            reverse = True
        else:
            reverse = False
        x, n = abs(x), abs(n)
        i, v = 0, x # 当前位置 i, 当前 x**(2**i)
        res = 1
        # 10 1010 4*256
        while n!=1:
            print(n, v)
            if n&1:
                print(i)
                print('res *', v)
                res *= v
            v *= v
            i += 1
            n = n//2
        print('res *', v)
        res *= v # 最高位
        if sign==-1:
            res *= -1
        if reverse:
            res = 1/res
        return res

```

