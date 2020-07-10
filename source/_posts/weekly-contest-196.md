---
title: weekly-contest-196
toc: true
recommend: 1
uniqueId: '2020-07-08 12:29:59/"weekly-contest-196".html'
date: 2020-07-08 20:29:59
thumbnail:
categories:
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

```


# 3

```python

```


# 4

```python

```

