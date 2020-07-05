---
thumbnail:
title: leetcode 583. Delete Operation for Two Strings
date: 2020-07-05 22:17:14
categories:
- leetcode
- leetcode-5**
toc: true
tags:
- String
recommend: 1
keywords:
uniqueId: '2020-07-05 22:17:14/"leetcode 583. Delete Operation for Two Strings".html'
---

<a href="https://leetcode.com/problems/delete-operation-for-two-strings/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/delete-operation-for-two-strings/">九章</a>
## 题目描述
Given two words _word1_ and _word2_ , find the minimum number of steps
required to make _word1_ and _word2_ the same, where in each step you can
delete one character in either string.

**Example 1:**  
        
    Input: "sea", "eat"
    Output: 2
    Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".


**Note:**  

  1. The length of given words won't exceed 500.
  2. Characters in given words can only be lower-case letters.


**Tags:** String

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    def minDistance(self, s1: str, s2: str) -> int:
        #     s e a
        #   0 1 2 3
        # e 1 2 1 2
        # a 2 3 2 1
        # t 3 4 3 2
        # s1[i]==s2[j]: dp[i][j] = dp[i-1][j-1]
        # else: dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1]+2)
        n, m = len(s1), len(s2)
        dp = [[0]*(m+1) for _ in range(n+1)]
        # 初始化第一行、第一列
        dp[0] = list(range(m+1))
        for i in range(n+1):
            dp[i][0] = i
        for i in range(1, n+1):
            for j in range(1, m+1):
                if s1[i-1]==s2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1]+2)
                print(s1[i-1], s2[j-1], dp[i][j], '')
            print()
        print(dp)
        return dp[-1][-1]
```
