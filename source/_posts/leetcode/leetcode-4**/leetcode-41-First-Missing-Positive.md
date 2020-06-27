---
thumbnail:
title: leetcode 41. First Missing Positive
date: 2020-06-27 19:40:37
categories:
- leetcode
- leetcode-4**
toc: true
tags:
- Array
recommend: 1
keywords:
uniqueId: '2020-06-27 19:40:37/"leetcode 41. First Missing Positive".html'
---

<a href="https://leetcode.com/problems/first-missing-positive/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/first-missing-positive/">九章</a>
## 题目描述
Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**
        
    Input: [1,2,0]
    Output: 3


**Example 2:**
        
    Input: [3,4,-1,1]
    Output: 2


**Example 3:**
        
    Input: [7,8,9,11,12]
    Output: 1


**Note:**

Your algorithm should run in _O_ ( _n_ ) time and uses constant extra space.


**Tags:** Array

**Difficulty:** Hard

## 答案
<!--more-->
```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        def swap(nums, i, j):
            nums[i], nums[j] = nums[j], nums[i]

        n = len(nums)
        for i in range(n):
            while nums[i]>0 and nums[i]!=i+1 and nums[i]-1<n and nums[nums[i]-1]!=nums[i]:
                swap(nums, i, nums[i]-1)
        for i in range(n):
            if nums[i]!=i+1:
                return i+1
        return n+1
```
