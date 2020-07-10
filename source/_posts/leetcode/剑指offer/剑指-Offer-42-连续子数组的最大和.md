---
title: 剑指 Offer 42 连续子数组的最大和
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:46/"剑指 Offer 42 连续子数组的最大和".html'
date: 2020-06-30 21:55:46
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

![image-20200708202524576](https://i.loli.net/2020/07/08/wsP1blqSL5ANiR6.png)

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # -2 1 -3 4 -1 2 1 -5 4  nums
        # -2 1 -2 4  3 5 6  1 5  dp(表示以 i 结尾的最大和)
        # if dp[i-1] <= 0: dp[i] = nums[i]
        # else: dp[i] = dp[i-1]+nums[i]
        # 可以看到若要返回那个数组，范围是 [maxdp 前第一个非负的 dp : maxdp]
        dp = maxdp = nums[0]
        for num in nums[1:]:
            if dp<=0: dp = num
            else: dp += num
            maxdp = max(maxdp, dp)
        return maxdp

```

