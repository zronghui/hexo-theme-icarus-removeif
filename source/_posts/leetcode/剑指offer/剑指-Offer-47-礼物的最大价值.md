---
title: 剑指 Offer 47 礼物的最大价值
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:56/"剑指 Offer 47 礼物的最大价值".html'
date: 2020-06-30 21:55:56
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
    def maxValue(self, grid: List[List[int]]) -> int:
        n, m = len(grid), len(grid[0])
        dp = [[0]*(m+1) for _ in range(n+1)]
        for i in range(1, n+1):
            for j in range(1, m+1):
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])+grid[i-1][j-1]
        return dp[-1][-1]
```

