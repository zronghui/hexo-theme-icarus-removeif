---
thumbnail:
title: leetcode 312. Burst Balloons
date: 2020-07-19 21:13:39
categories:
toc: true
tags:
- Divide and Conquer
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-19 21:13:39/"leetcode 312. Burst Balloons".html'
---

<a href="https://leetcode.com/problems/burst-balloons/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/burst-balloons/">九章</a>
## 题目描述
Given `n` balloons, indexed from `0` to `n-1`. Each balloon is painted with a
number on it represented by array `nums`. You are asked to burst all the
balloons. If the you burst balloon `i` you will get `nums[left] * nums[i] *
nums[right]` coins. Here `left` and `right` are adjacent indices of `i`. After
the burst, the `left` and `right` then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Note:**

  * You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them.
  * 0 ≤ `n` ≤ 500, 0 ≤ `nums[i]` ≤ 100

**Example:**
        
    Input: [3,1,5,8]
    Output: 167 
    Explanation:nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
                 coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167



**Tags:** Divide and Conquer, Dynamic Programming

**Difficulty:** Hard

## 答案
<!--more-->

参考： [动态规划套路解决戳气球问题 - 戳气球 - 力扣（LeetCode）](https://leetcode-cn.com/problems/burst-balloons/solution/dong-tai-gui-hua-tao-lu-jie-jue-chuo-qi-qiu-wen-ti/)

```java
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1, *nums, 1]
        n = len(nums)
        dp = [[0]*n for _ in range(n)]
        for i in reversed(range(n-1)):
            for j in range(i+1, n):
                for k in range(i+1, j):
                    dp[i][j] = max(dp[i][j], dp[i][k]+dp[k][j]+nums[i]*nums[j]*nums[k])
        return dp[0][-1]

```
