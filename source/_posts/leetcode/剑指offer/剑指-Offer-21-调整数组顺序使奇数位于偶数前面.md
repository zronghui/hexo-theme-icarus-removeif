---
title: 剑指 Offer 21 调整数组顺序使奇数位于偶数前面
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:05/"剑指 Offer 21 调整数组顺序使奇数位于偶数前面".html'
date: 2020-06-30 21:55:05
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
    def exchange(self, nums: List[int]) -> List[int]:
        i = j = 0
        while j<len(nums):
            if nums[j]%2:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
            j += 1
        return nums
```

