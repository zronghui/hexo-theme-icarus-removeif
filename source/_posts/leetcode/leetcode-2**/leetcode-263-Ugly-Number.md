---
thumbnail:
title: leetcode 263. Ugly Number
date: 2020-07-13 21:45:45
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Math
recommend: 1
keywords:
uniqueId: '2020-07-13 21:45:45/"leetcode 263. Ugly Number".html'
---

<a href="https://leetcode.com/problems/ugly-number/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/ugly-number/">九章</a>
## 题目描述
Write a program to check whether a given number is an ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3,
5`.

**Example 1:**
        
    Input: 6
    Output: true
    Explanation: 6 = 2 × 3

**Example 2:**
        
    Input: 8
    Output: true
    Explanation: 8 = 2 × 2 × 2


**Example 3:**
        
    Input: 14
    Output: false 
    Explanation:14 is not ugly since it includes another prime factor 7.


**Note:**

  1. `1` is typically treated as an ugly number.
  2. Input is within the 32-bit signed integer range: [−231,  231 − 1].


**Tags:** Math

**Difficulty:** Easy

## 答案
<!--more-->
```python
class Solution:
    def isUgly(self, n: int) -> bool:
        if n<=0: return False
        while not n%5: n = n//5
        while not n%3: n = n//3
        while not n%2: n = n//2
        return n==1
```
