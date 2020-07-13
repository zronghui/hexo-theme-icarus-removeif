---
title: 剑指 Offer 57 - II 和为s的连续正数序列
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:24/"剑指 Offer 57 - II 和为s的连续正数序列".html'
date: 2020-06-30 21:56:24
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
    def findContinuousSequence(self, target: int) -> List[List[int]]:
        # 3 4 5 6  (3+6)*4/2=18
        # x = (l+(l+h-1))*h/2= (2l+h-1)*h/2 -> 2x = (2l+h-1)*h (h>1)
        # 求 2x 的因数
        res = []
        x = 2*target
        h = 2
        # 比如求 18 的因数，能被 2 整除, h 的上界不超过 r=9
        r = x
        while h<=r:
            if x%h==0:
                r = x//h
                # 此时 r=2l+h-1 : r、 h 奇偶相反，即相加为奇数
                if (r+h)&1:
                    l = (r+1-h)//2
                    if l>0:
                        res.append([l+i for i in range(h)])
            h += 1
        return res[::-1]
```

