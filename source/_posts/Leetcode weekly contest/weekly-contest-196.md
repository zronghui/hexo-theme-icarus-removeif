---
title: weekly-contest-196
toc: true
recommend: 1
uniqueId: '2020-07-08 12:29:59/"weekly-contest-196".html'
date: 2020-07-08 20:29:59
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

又是怀疑人生的一次比赛

<!--more-->



# 1

```python
class Solution:
    def canMakeArithmeticProgression(self, arr: List[int]) -> bool:
        n = len(arr)
        if n==2: return True
        arr.sort()
        gap = arr[1]-arr[0]
        return all(arr[i]-arr[i-1]==gap for i in range(1, n))
```

# 2

```python
class Solution:
    def getLastMoment(self, n: int, left: List[int], right: List[int]) -> int:
        if left and right:
            return max(max(left), n-min(right))
        if left:
            return max(left)
        if right:
            return n-min(right)
```


# 3

```python

```


# 4

```python

```

