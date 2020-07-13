---
title: 剑指 Offer 53 - II 0～n-1中缺失的数字
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:11/"剑指 Offer 53 - II 0～n-1中缺失的数字".html'
date: 2020-06-30 21:56:11
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
    def missingNumber(self, nums: List[int]) -> int:
        l, r = 0, len(nums)-1
        while l<=r:
            mid = (l+r)//2
            if nums[mid]!=mid: r = mid-1
            else: l = mid+1 
        return l
```

