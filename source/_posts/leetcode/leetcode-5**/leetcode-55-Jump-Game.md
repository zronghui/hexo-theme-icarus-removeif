---
thumbnail:
title: leetcode 55. Jump Game
date: 2020-04-17 12:40:04
categories:
- leetcode
- leetcode-5**
toc: true
tags:
- Array
- Greedy
recommend: 1
keywords:
uniqueId: '2020-04-17 12:40:04/"leetcode 55. Jump Game".html'
---

<a href="https://leetcode.com/problems/jump-game/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/jump-game/">九章</a>
## 题目描述
Given an array of non-negative integers, you are initially positioned at the
first index of the array.

Each element in the array represents your maximum jump length at that
position.

Determine if you are able to reach the last index.

**Example 1:**
        
    Input: [2,3,1,1,4]
    Output: true
    Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.


**Example 2:**
        
    Input: [3,2,1,0,4]
    Output: false
    Explanation: You will always arrive at index 3 no matter what. Its maximum
                 jump length is 0, which makes it impossible to reach the last index.



**Tags:** Array, Greedy

**Difficulty:** Medium

## 答案
<!--more-->

[跳跃游戏 - 跳跃游戏 - 力扣（LeetCode）](https://leetcode-cn.com/problems/jump-game/solution/tiao-yue-you-xi-by-leetcode-solution/)

```python
class Solution:
    def canJump1(self, nums: List[int]) -> bool:
        # 12:29-12:34  5min
        n = len(nums)
        dp = [0]*n
        dp[0] = 1
        for i in range(n-1):
            if dp[i]:
                # 0->3 1 2 3
                dp[i+1:min(n, i+1+nums[i])] = [1] * nums[i]
                if dp[-1]:
                    return True
        return bool(dp[-1])

    def canJump(self, nums: List[int]) -> bool:
        # 贪心
        # 12:37-12:39.5   2.5min
        m = 0
        n = len(nums)
        for i in range(n-1):
            if i>m:
                return False
            m = max(m, i+nums[i])
            if m>=n-1:
                return True
        return m>=n-1

```
