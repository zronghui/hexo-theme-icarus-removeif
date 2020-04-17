---
title: leetcode 300. Longest Increasing Subsequence
date: 2019-11-30 21:24:42
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Binary Search
- Dynamic Programming
---
### 难度：Middle

<a href="https://leetcode.com/problems/longest-increasing-subsequence/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/longest-increasing-subsequence/">九章</a>
## 题目描述
Given an unsorted array of integers, find the length of longest increasing
subsequence.

**Example:**
        
    Input: [10,9,2,5,3,7,101,18]
    Output: 4 
    Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 

**Note:**

  * There may be more than one LIS combination, it is only necessary for you to return the length.
  * Your algorithm should run in O( _n 2_) complexity.

**Follow up:** Could you improve it to O( _n_ log _n_ ) time complexity?


**Tags:** Binary Search, Dynamic Programming

**Difficulty:** Medium
## 答案
<!--more-->
```java
// Dp[i] 表示以第i个数字为结尾的最长上升子序列的长度。
// 对于每个数字，枚举前面所有小于自己的数字 j，Dp[i] = max{Dp[j]} + 1. 如果没有比自己小的，Dp[i] = 1;
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums==null || nums.length==0) return 0;
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);
        int res = 1;
        for(int i=1; i<n; i++) {
            for(int j=i-1; j>=0; j--) {
                if(nums[i]>nums[j]){
                    dp[i] = Math.max(dp[j]+1, dp[i]);
                }
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```
