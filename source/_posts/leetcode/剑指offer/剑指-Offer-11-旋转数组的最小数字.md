---
title: 剑指 Offer 11 旋转数组的最小数字
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:45/"剑指 Offer 11 旋转数组的最小数字".html'
date: 2020-06-30 21:54:45
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
    def minArray(self, nums: List[int]) -> int:
        i, j = 0, len(nums)-1
        while i<j:
            m = i+(j-i)//2
            if nums[m]>nums[j]:
                i = m+1
            elif nums[m]<nums[j]:
                j = m
            else:
                j -= 1
        return nums[i]

```

