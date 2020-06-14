---
thumbnail:
title: leetcode 15. 3Sum
date: 2020-06-12 15:25:11
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Array
- Two Pointers
recommend: 1
keywords:
uniqueId: '2020-06-12 15:25:11/"leetcode 15. 3Sum".html'
---

<a href="https://leetcode.com/problems/3sum/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/3sum/">九章</a>
## 题目描述
Given an array `nums` of _n_ integers, are there elements _a_ , _b_ , _c_ in
`nums` such that _a_ \+ _b_ \+ _c_ = 0? Find all unique triplets in the array
which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**
        
    Given array nums = [-1, 0, 1, 2, -1, -4],
    
    A solution set is:
    [
      [-1, 0, 1],
      [-1, -1, 2]
    ]



**Tags:** Array, Two Pointers

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        if len(nums)<3:
            return res
        nums.sort()
        # for i in range(len(nums)-2):
        i = 0
        while i<len(nums)-2:
            l = i+1
            r = len(nums)-1
            while l<r:
                _sum = nums[i]+nums[l]+nums[r]
                if _sum==0:
                    res.append([nums[i], nums[l], nums[r]])
                    while l<r-1 and nums[l]==nums[l+1]:
                        l += 1
                    l += 1
                elif _sum>0:
                    r -= 1
                else:
                    l += 1
            while i<len(nums)-2 and nums[i]==nums[i+1]:
                i += 1
            i += 1
        return res
```
