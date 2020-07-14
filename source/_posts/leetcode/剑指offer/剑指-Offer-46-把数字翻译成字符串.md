---
title: 剑指 Offer 46 把数字翻译成字符串
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:54/"剑指 Offer 46 把数字翻译成字符串".html'
date: 2020-06-30 21:55:54
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
    def translateNum(self, num: int) -> int:
        #   1 2 2 5 8
        # 1 1 2 3 5 5
        s = str(num)
        n = len(s)
        if n<2: return 1
        dp = [1]*(n+1)
        for i in range(2, n+1):
            if 10<=int(s[i-2:i])<26:
                dp[i] = dp[i-1]+dp[i-2]
            else:
                dp[i] = dp[i-1]
        return dp[-1]

```

