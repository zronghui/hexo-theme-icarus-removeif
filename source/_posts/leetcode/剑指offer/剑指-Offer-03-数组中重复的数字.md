---
title: 剑指 Offer 03. 数组中重复的数字
toc: true
recommend: 1
uniqueId: '2020-06-30 13:04:46/"剑指 Offer 03. 数组中重复的数字".html'
date: 2020-06-30 21:04:46
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 03. 数组中重复的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)



```python
class Solution:
    def findRepeatNumber(self, nums: List[int]) -> int:
        # 0 1 2 3 3 5
        def swap(l, a, b):
            l[a], l[b] = l[b], l[a]
        i = 0
        for i in range(len(nums)):
            while i!=nums[i]:
                if nums[nums[i]]==nums[i]:
                    return nums[i]
                swap(nums, i, nums[i])
```

