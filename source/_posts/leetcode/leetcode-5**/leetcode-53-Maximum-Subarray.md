---
thumbnail:
title: leetcode 53. Maximum Subarray
date: 2020-07-08 20:26:47
categories:
- leetcode
- leetcode-5**
toc: true
tags:
- Array
- Divide and Conquer
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-08 20:26:47/"leetcode 53. Maximum Subarray".html'
---

<a href="https://leetcode.com/problems/maximum-subarray/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/maximum-subarray/">九章</a>
## 题目描述
Given an integer array `nums`, find the contiguous subarray (containing at
least one number) which has the largest sum and return its sum.

**Example:**
        
    Input: [-2,1,-3,4,-1,2,1,-5,4],
    Output: 6
    Explanation:  [4,-1,2,1] has the largest sum = 6.


**Follow up:**

If you have figured out the O( _n_ ) solution, try coding another solution
using the divide and conquer approach, which is more subtle.


**Tags:** Array, Divide and Conquer, Dynamic Programming

**Difficulty:** Easy

## 答案
<!--more-->
```java
/*
 * @lc app=leetcode id=53 lang=java
 *
 * [53] Maximum Subarray
 * 
 */
// 动态规划
// 长度从0~nums.length
// solution(0~i) = max(solution(i-1)+nums[i],nums[i])
// solution = max(solutions)
// 下面用dp(dynamic programming) 表示 solution
class Solution {
    public int maxSubArray(int[] nums) {
        int dp = nums[0];
        int maxdp = nums[0];
        for (int i = 1; i < nums.length; i++) {
            dp = Math.max(dp+nums[i], nums[i]);
            maxdp = Math.max(dp, maxdp);
        }
        return maxdp;
    }
}


```



```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # -2 1 -3 4 -1 2 1 -5 4  nums
        # -2 1 -2 4  3 5 6  1 5  dp(表示以 i 结尾的最大和)
        # if dp[i-1] <= 0: dp[i] = nums[i]
        # else: dp[i] = dp[i-1]+nums[i]
        # 可以看到若要返回那个数组，范围是 [maxdp 前第一个非负的 dp : maxdp]
        dp = maxdp = nums[0]
        for num in nums[1:]:
            if dp<=0: dp = num
            else: dp += num
            maxdp = max(maxdp, dp)
        return maxdp
```

