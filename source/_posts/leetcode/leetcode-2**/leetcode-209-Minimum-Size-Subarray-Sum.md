---
thumbnail:
title: leetcode 209. Minimum Size Subarray Sum
date: 2020-06-28 19:45:06
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Array
- Two Pointers
- Binary Search
recommend: 1
keywords:
uniqueId: '2020-06-28 19:45:06/"leetcode 209. Minimum Size Subarray Sum".html'
---

<a href="https://leetcode.com/problems/minimum-size-subarray-sum/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/minimum-size-subarray-sum/">九章</a>
## 题目描述
Given an array of **n** positive integers and a positive integer **s** , find
the minimal length of a **contiguous** subarray of which the sum ≥ **s**. If
there isn't one, return 0 instead.

**Example:  **
        
    Input: s = 7, nums = [2,3,1,2,4,3]
    Output: 2
    Explanation: the subarray [4,3] has the minimal length under the problem constraint.

**Follow up:**

If you have figured out the _O_ ( _n_ ) solution, try coding another solution
of which the time complexity is _O_ ( _n_ log _n_ ).


**Tags:** Array, Two Pointers, Binary Search

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        if not nums:
            return 0
        l = 0
        r = -1
        _sum = 0
        n = len(nums)
        res = n+1
        while r<n-1:
            r += 1
            _sum += nums[r]
            while _sum<s and r+1<n:
                r += 1
                _sum += nums[r]
            while l<=r and _sum-nums[l]>=s:
                _sum -= nums[l]
                l += 1
            if _sum>=s:
                res = min(res, r-l+1)
            print(l, r, _sum)
        return 0 if res==n+1 else res
```
