---
title: 剑指 Offer 41 数据流中的中位数
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:44/"剑指 Offer 41 数据流中的中位数".html'
date: 2020-06-30 21:55:44
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
class MedianFinder:

    def __init__(self):
        self.l = []


    def addNum(self, num: int) -> None:
        bisect.insort(self.l, num)


    def findMedian(self) -> float:
        n = len(self.l)
        if n&1:
            return self.l[n//2]
        return (self.l[n//2]+self.l[n//2-1])/2

```

