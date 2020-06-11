---
thumbnail:
title: leetcode 983. Minimum Cost For Tickets
date: 2020-06-10 21:35:32
categories:
toc: true
tags:
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-06-10 21:35:32/"leetcode 983. Minimum Cost For Tickets".html'
---

<a href="https://leetcode.com/problems/minimum-cost-for-tickets/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/minimum-cost-for-tickets/">九章</a>
## 题目描述
In a country popular for train travel, you have planned some train travelling
one year in advance.  The days of the year that you will travel is given as an
array `days`.  Each day is an integer from `1` to `365`.

Train tickets are sold in 3 different ways:

  * a 1-day pass is sold for `costs[0]` dollars;
  * a 7-day pass is sold for `costs[1]` dollars;
  * a 30-day pass is sold for `costs[2]` dollars.

The passes allow that many days of consecutive travel.  For example, if we get
a 7-day pass on day 2, then we can travel for 7 days: day 2, 3, 4, 5, 6, 7,
and 8.

Return the minimum number of dollars you need to travel every day in the given
list of `days`.



**Example 1:**
        
    Input: days = [1,4,6,7,8,20], costs = [2,7,15]
    Output: 11
    Explanation:
    For example, here is one way to buy passes that lets you travel your travel plan:
    On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
    On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
    On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
    In total you spent $11 and covered all the days of your travel.


**Example 2:**
        
    Input: days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
    Output: 17
    Explanation:
    For example, here is one way to buy passes that lets you travel your travel plan:
    On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
    On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
    In total you spent $17 and covered all the days of your travel.




**Note:**

  1. `1 <= days.length <= 365`
  2. `1 <= days[i] <= 365`
  3. `days` is in strictly increasing order.
  4. `costs.length == 3`
  5. `1 <= costs[i] <= 1000`


**Tags:** Dynamic Programming

**Difficulty:** Medium

## 答案

遇到一道不算简单的 DP，好不容易吭哧吭哧写出来，结果：

<img src="https://i.loli.net/2020/06/11/tpRq6cuehr2OJ9Q.png" alt="image-20200610213723057" style="zoom:50%;" />

<!--more-->
```python
import sys

class Solution:
    def mincostTickets(self, days: List[int], costs: List[int]) -> int:
        # dp[n][4]  n: 最多n天，4: -1(不买) 1 7 30 这 4 种选择
        # 主要考虑：n 在不在 days 中； -1 买没买
        # n 不在 days 中，一定不用买，其他三种情况存 sys.maxsize
        
        n = days[-1]
        # start 和 nxt 都存放 index
        start = days.pop(0) - 1
        if days:
            nxt = days.pop(0) - 1

        dp = [[sys.maxsize for _ in range(4)] for _ in range(n)]
        dp[start] = [sys.maxsize, costs[0], costs[1], costs[2]]
        
        m = {1: 1, 2: 7, 3: 30}
        for i in range(start+1, n):
            if nxt!=i:
                dp[i][0] = min(dp[i-1])
                continue
            if days:
                nxt = days.pop(0) - 1
            dp[i][1] = min(dp[i-1]) + costs[0]
            dp[i][2] = min(dp[i-1]) + costs[1]
            dp[i][3] = min(dp[i-1]) + costs[2]
            # 要求：i-j >= start -> j <= i-start
            mj = i-start
            for j in range(1, min(29, mj)+1):
                dp[i][0] = min(dp[i][0], dp[i-j][3])
            for j in range(1, min(6, mj)+1):
                dp[i][0] = min(dp[i][0], dp[i-j][2])

        return min(dp[-1])


```
