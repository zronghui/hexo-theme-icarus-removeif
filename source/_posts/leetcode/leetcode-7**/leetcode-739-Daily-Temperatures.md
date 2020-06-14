---
thumbnail:
title: leetcode 739. Daily Temperatures
date: 2020-06-11 16:31:51
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- Hash Table
- Stack
recommend: 1
keywords:
uniqueId: '2020-06-11 16:31:51/"leetcode 739. Daily Temperatures".html'
---

<a href="https://leetcode.com/problems/daily-temperatures/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/daily-temperatures/">九章</a>

## 题目描述
Given a list of daily temperatures `T`, return a list such that, for each day
in the input, tells you how many days you would have to wait until a warmer
temperature. If there is no future day for which this is possible, put `0`
instead.

For example, given the list of temperatures `T = [73, 74, 75, 71, 69, 72, 76,
73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

**Note:** The length of `temperatures` will be in the range `[1, 30000]`. Each
temperature will be an integer in the range `[30, 100]`.


**Tags:** Hash Table, Stack

**Difficulty:** Medium

## 答案

[LeetCode 图解 | 739.每日温度 - 每日温度 - 力扣（LeetCode）](https://leetcode-cn.com/problems/daily-temperatures/solution/leetcode-tu-jie-739mei-ri-wen-du-by-misterbooo/)

<!--more-->
```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        n = len(T)
        res = [0] * n
        stack = []
        for i in range(n):
            while stack and stack[-1][1]<T[i]:
                res[stack[-1][0]] = i - stack[-1][0]
                stack.pop()
            stack.append((i, T[i]))

        return res
```
