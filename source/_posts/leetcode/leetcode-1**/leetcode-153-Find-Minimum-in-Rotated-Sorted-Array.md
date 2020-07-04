---
thumbnail:
title: leetcode 153. Find Minimum in Rotated Sorted Array
date: 2020-07-02 08:24:31
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Array
- Binary Search
recommend: 1
keywords:
uniqueId: '2020-07-02 08:24:31/"leetcode 153. Find Minimum in Rotated Sorted Array".html'
---

<a href="https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/find-minimum-in-rotated-sorted-array/">九章</a>
## 题目描述
Suppose an array sorted in ascending order is rotated at some pivot unknown to
you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

You may assume no duplicate exists in the array.

**Example 1:**
        
    Input: [3,4,5,1,2] 
    Output: 1


**Example 2:**
        
    Input: [4,5,6,7,0,1,2]
    Output: 0



**Tags:** Array, Binary Search

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        i, j = 0, len(nums)-1
        while i<j:
            m = i+(j-i)//2
            if nums[m]>nums[j]:
                i = m+1
            else:
                j = m
        return nums[i]
```
