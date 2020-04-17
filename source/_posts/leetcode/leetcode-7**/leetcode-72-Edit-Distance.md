---
thumbnail:
title: leetcode 72. Edit Distance
date: 2020-04-16 22:38:56
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- String
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-04-16 22:38:56/"leetcode 72. Edit Distance".html'
---

<a href="https://leetcode.com/problems/edit-distance/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/edit-distance/">九章</a>
## 题目描述
Given two words _word1_ and _word2_ , find the minimum number of operations
required to convert _word1_ to _word2_.

You have the following 3 operations permitted on a word:

  1. Insert a character
  2. Delete a character
  3. Replace a character

**Example 1:**
        
    Input: word1 = "horse", word2 = "ros"
    Output: 3
    Explanation: 
    horse -> rorse (replace 'h' with 'r')
    rorse -> rose (remove 'r')
    rose -> ros (remove 'e')


**Example 2:**
        
    Input: word1 = "intention", word2 = "execution"
    Output: 5
    Explanation: 
    intention -> inention (remove 't')
    inention -> enention (replace 'i' with 'e')
    enention -> exention (replace 'n' with 'x')
    exention -> exection (replace 'n' with 'c')
    exection -> execution (insert 'u')



**Tags:** String, Dynamic Programming

**Difficulty:** Hard

## 答案
<!--more-->

[(2) 【🐼熊猫刷题Python3】用视频讲DP, 一听就懂 - 编辑距离 - 力扣（LeetCode）](https://leetcode-cn.com/problems/edit-distance/solution/xiong-mao-shua-ti-python3-dong-tai-gui-hua-yi-dong/)

```python
class Solution:
    def minDistance(self, w1: str, w2: str) -> int:
        n, m = len(w1), len(w2)
        if  min(n, m)==0:
            return max(n, m)
        dp = [[0 for _ in range(m+1)]for _ in range(n+1)]
        # 边界值
        for i in range(n+1):
            dp[i][0] = i
        for j in range(m+1):
            dp[0][j] = j
        # 状态转移
        for i in range(1, n+1):
            for j in range(1, m+1):
                if w1[i-1]==w2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])+1
        return dp[-1][-1]
        
```
