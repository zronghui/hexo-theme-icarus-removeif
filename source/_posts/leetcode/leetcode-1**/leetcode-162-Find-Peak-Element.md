---
title: leetcode 162. Find Peak Element
date: 2019-12-02 10:01:52
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Array
- Binary Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/find-peak-element/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/find-peak-element/">九章</a>
## 题目描述
A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element
and return its index.

The array may contain multiple peaks, in that case return the index to any one
of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

**Example 1:**
        
    Input: **nums** = [1,2,3,1]
    Output: 2
    Explanation: 3 is a peak element and your function should return the index number 2.

**Example 2:**
        
    Input: **nums** = [1,2,1,3,5,6,4]
    Output: 1 or 5 
    Explanation: Your function can return either index number 1 where the peak element is 2, 
                 or index number 5 where the peak element is 6.
    

**Note:**

Your solution should be in logarithmic complexity.


**Tags:** Array, Binary Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int n = nums.length;
        if(n==1) return 0;
        // 找到第一个开始下降的数字
        for(int i=0; i<n-1; i++) {
            if(nums[i]>nums[i+1]) {
                return i;
            }
        }
        // 没找到返回最后一个
        return n-1;
    }
}
```
