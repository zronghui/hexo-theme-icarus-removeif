---
thumbnail:
title: leetcode 154. Find Minimum in Rotated Sorted Array II
date: 2020-07-02 08:24:16
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Array
- Binary Search
recommend: 1
keywords:
uniqueId: '2020-07-02 08:24:16/"leetcode 154. Find Minimum in Rotated Sorted Array II".html'
---

<a href="https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/find-minimum-in-rotated-sorted-array-ii/">九章</a>
## 题目描述
Suppose an array sorted in ascending order is rotated at some pivot unknown to
you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

The array may contain duplicates.

**Example 1:**
        
    Input: [1,3,5]
    Output: 1

**Example 2:**
        
    Input: [2,2,2,0,1]
    Output: 0

**Note:**

  * This is a follow up problem to [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/).
  * Would allow duplicates affect the run-time complexity? How and why?


**Tags:** Array, Binary Search

**Difficulty:** Hard

## 答案
<!--more-->
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
