---
thumbnail:
title: leetcode 231. Power of Two
date: 2020-07-03 19:38:39
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Math
- Bit Manipulation
recommend: 1
keywords:
uniqueId: '2020-07-03 19:38:39/"leetcode 231. Power of Two".html'
---

<a href="https://leetcode.com/problems/power-of-two/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/power-of-two/">九章</a>
## 题目描述
Given an integer, write a function to determine if it is a power of two.

**Example 1:**
        
    Input: 1
    Output: true 
    Explanation: 20 = 1


**Example 2:**
        
    Input: 16
    Output: true
    Explanation: 24 = 16

**Example 3:**
        
    Input: 218
    Output: false


**Tags:** Math, Bit Manipulation

**Difficulty:** Easy

## 答案
<!--more-->
```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        if n<=0:pyt
            return False
        cnt = 0
        while n:
            n &= n-1
            cnt += 1
        return True if cnt==1 else False
```
