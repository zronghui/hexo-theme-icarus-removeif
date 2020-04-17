---
title: leetcode 674. Longest Continuous Increasing Subsequence
date: 2019-11-30 21:32:14
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Array
---
### 难度：Easy

<a href="https://leetcode.com/problems/longest-continuous-increasing-subsequence/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/longest-continuous-increasing-subsequence/">九章</a>
## 题目描述
Given an unsorted array of integers, find the length of longest `continuous`
increasing subsequence (subarray).

**Example 1:**  
        
    Input: [1,3,5,4,7]
    Output: 3
    Explanation: The longest continuous increasing subsequence is [1,3,5], its length is 3. 
    Even though [1,3,5,7] is also an increasing subsequence, it's not a continuous one where 5 and 7 are separated by 4. 
    

**Example 2:**  
        
    Input: [2,2,2,2,2]
    Output: 1
    Explanation: The longest continuous increasing subsequence is [2], its length is 1. 
    

**Note:** Length of the array will not exceed 10,000.


**Tags:** Array

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums==null || nums.length==0) return 0;
        if(nums.length==1) return 1;
        int res = 1;
        int cur = 1;
        for(int i=1; i<nums.length; i++) {
            if(nums[i]>nums[i-1]) {
                cur++;
            }else{
                res = Math.max(res, cur);
                cur = 1;
            }
        }
        return Math.max(res, cur);
    }
}
```
