---
thumbnail:
title: leetcode 10. Regular Expression Matching
date: 2020-07-05 21:55:46
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- String
- Dynamic Programming
- Backtracking
recommend: 1
keywords:
uniqueId: '2020-07-05 21:55:46/"leetcode 10. Regular Expression Matching".html'
---

<a href="https://leetcode.com/problems/regular-expression-matching/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/regular-expression-matching/">九章</a>
## 题目描述
Given an input string (`s`) and a pattern (`p`), implement regular expression
matching with support for `'.'` and `'*'`.
            '.' Matches any single character.    '*' Matches zero or more of the preceding element.    

The matching should cover the **entire** input string (not partial).

**Note:**

  * `s` could be empty and contains only lowercase letters `a-z`.
  * `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

**Example 1:**
            Input:    s = "aa"    p = "a"    Output: false    Explanation: "a" does not match the entire string "aa".    

**Example 2:**
            Input:    s = "aa"    p = "a*"    Output: true    Explanation:  '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".    

**Example 3:**
            Input:    s = "ab"    p = ".*"    Output: true    Explanation:  ".*" means "zero or more (*) of any character (.)".    

**Example 4:**
            Input:    s = "aab"    p = "c*a*b"    Output: true    Explanation:  c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".    

**Example 5:**
            Input:    s = "mississippi"    p = "mis*is*p*."    Output: false    


**Tags:** String, Dynamic Programming, Backtracking

**Difficulty:** Hard

## 答案
<!--more-->



```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        #     a a b
        #   1 0 0 0
        # c 0 0 0 0
        # * 1 0 0 0
        # a 0 1 0 0
        # * 1 1 1 0
        # b 0 0 0 1
        # 更复杂的情况："mississippi"   "mis*is*p*."
        n, m = len(p), len(s)
        dp = [[False]*(m+1) for _ in range(n+1)] 
        dp[0][0] = True
        # 更新 dp 第一列的值
        nums = 0 # 当前字母个数，遇到 * -1, 大于 1 break
        for i in range(1, n+1):
            if p[i-1]=='*':
                nums -= 1
                dp[i][0] = True
            else:
                nums += 1
                if nums>1:
                    break

        for i in range(1, n+1):
            # 开头一个*, 或连续 2 个* ,格式错误
            if p[i-1]=='*' and (i==1 or p[i-2]=='*'):
                return False
            for j in range(1, m+1):
                if p[i-1]=='.' or p[i-1]==s[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                elif p[i-1]=='*':
                    # dp[i-2][j] 当 * 表示 0 次
                    # dp[i-1][j] 当 * 表示 1 次
                    # 当 * 表示 多 次: 
                    #  条件 1. p[i-2] in [s[j-1], '.'] (当前字符匹配*前的字符)
                    dp[i][j] = dp[i-2][j] or (dp[i-1][j]) or (p[i-2] in [s[j-1], '.'] and dp[i][j-1])
        # print(dp)
        return dp[-1][-1]

```
