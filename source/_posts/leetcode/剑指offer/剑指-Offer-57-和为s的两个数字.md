---
title: 剑指 Offer 57 和为s的两个数字
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:22/"剑指 Offer 57 和为s的两个数字".html'
date: 2020-06-30 21:56:22
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
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        l, r = 0, len(nums)-1
        while l<=r:
            _sum = nums[l]+nums[r]
            if _sum==target:
                return [nums[l], nums[r]]
            elif _sum>target:
                r -= 1
            else:
                l += 1
```

