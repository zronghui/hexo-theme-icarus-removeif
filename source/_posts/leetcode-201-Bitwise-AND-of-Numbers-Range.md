---
thumbnail:
title: leetcode 201. Bitwise AND of Numbers Range
date: 2020-08-23 09:07:07
categories:
toc: true
tags:
- Bit Manipulation
recommend: 1
keywords:
uniqueId: '2020-08-23 09:07:07/"leetcode 201. Bitwise AND of Numbers Range".html'
---

<a href="https://leetcode.com/problems/bitwise-and-of-numbers-range/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/bitwise-and-of-numbers-range/">九章</a>
## 题目描述
Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND
of all numbers in this range, inclusive.

**Example 1:**
        
    Input: [5,7]
    Output: 4


**Example 2:**
        
    Input: [0,1]
    Output: 0


**Tags:** Bit Manipulation

**Difficulty:** Medium

## 答案
<!--more-->

bin(45)

int('12', base=10)

```python
class Solution:
    def rangeBitwiseAnd(self, m: int, n: int) -> int:
        shift = 0
        while n>m:
            n = n>>1
            m = m>>1
            shift += 1
        return n<<shift

    def rangeBitwiseAnd1(self, m: int, n: int) -> int:
        # 5   7
        # 101 111 -> 100
        # m n 的二进制的共同前缀，不同比特填充 0
        a = bin(m)[2:]
        b = bin(n)[2:]
        if len(b)>len(a):
            # 1 10101 -> 0
            # 长度不同，return 0
            return 0
        pre = ''
        for i, j in zip(a, b):
            if i==j:
                pre += i
            else:
                return int('0b'+pre+'0'*(len(b)-len(pre)), 2)
        # 到这里，表示 m==n
        return m
```
