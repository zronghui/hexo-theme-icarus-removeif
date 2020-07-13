---
thumbnail:
title: leetcode 264. Ugly Number II
date: 2020-07-13 22:30:50
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Math
- Dynamic Programming
- Heap
recommend: 1
keywords:
uniqueId: '2020-07-13 22:30:50/"leetcode 264. Ugly Number II".html'
---

<a href="https://leetcode.com/problems/ugly-number-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/ugly-number-ii/">九章</a>
## 题目描述
Write a program to find the `n`-th ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3,
5`.

**Example:**
        
    Input: n = 10
    Output: 12
    Explanation:1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.

**Note:**  

  1. `1` is typically treated as an ugly number.
  2. `n` **does not exceed 1690**.


**Tags:** Math, Dynamic Programming, Heap

**Difficulty:** Medium

## 答案
<!--more-->

[丑数 II - 丑数 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/ugly-number-ii/solution/chou-shu-ii-by-leetcode/)
[面试题49. 丑数（动态规划，清晰图解） - 丑数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/chou-shu-lcof/solution/mian-shi-ti-49-chou-shu-dong-tai-gui-hua-qing-xi-t/)

```python
class Ugly:
    def __init__(self):
        self.nums = [1]
        a = b = c = 0
        for _ in range(1690):
            n2, n3, n5 = self.nums[a]*2, self.nums[b]*3, self.nums[c]*5
            m = min(n2, n3, n5)
            self.nums.append(m)
            if m==n2: a += 1
            if m==n3: b += 1
            if m==n5: c += 1


class Solution:
    u = Ugly()
    def nthUglyNumber(self, n: int) -> int:
        return self.u.nums[n-1]
        
```
