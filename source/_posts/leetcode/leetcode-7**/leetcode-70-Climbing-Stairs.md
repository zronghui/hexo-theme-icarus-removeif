---
thumbnail:
title: leetcode 70. Climbing Stairs
date: 2020-06-13 14:59:48
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-06-13 14:59:48/"leetcode 70. Climbing Stairs".html'
---

<a href="https://leetcode.com/problems/climbing-stairs/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/climbing-stairs/">九章</a>
## 题目描述
You are climbing a stair case. It takes _n_ steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you
climb to the top?

**Note:** Given _n_ will be a positive integer.

**Example 1:**
        
    Input: 2
    Output: 2
    Explanation: There are two ways to climb to the top.
    1. 1 step + 1 step
    2. 2 steps


**Example 2:**
        
    Input: 3
    Output: 3
    Explanation: There are three ways to climb to the top.
    1. 1 step + 1 step + 1 step
    2. 1 step + 2 steps
    3. 2 steps + 1 step



**Tags:** Dynamic Programming

**Difficulty:** Easy

## 答案
<!--more-->
```python
class Solution:
    @functools.lru_cache(maxsize=None)
    def climbStairs(self, n: int) -> int:
        if n==1:
            return 1
        if n==2:
            return 2
        return self.climbStairs(n-1)+self.climbStairs(n-2)
```

