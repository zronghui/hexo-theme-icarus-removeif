---
title: leetcode模板
toc: true
recommend: 1
uniqueId: '2020-07-13 01:33:11/"leetcode模板".html'
date: 2020-07-13 09:33:11
thumbnail:
categories:
- leetcode
tags:
keywords:
---

[TOC]

<!--more-->

## 二分

helper 函数

l<=r  l=mid+1 r=mid-1 

根据求左边界还是右边界，l r 的更新不同

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # return bisect.bisect(nums, target)-bisect.bisect(nums, target-1)
        def helper(n):
            l, r = 0, len(nums)-1
            while l<=r:
                mid = (l+r)//2
                if nums[mid]<=n: l = mid+1
                else: r = mid-1
            return l

        return helper(target)-helper(target-1)
```

