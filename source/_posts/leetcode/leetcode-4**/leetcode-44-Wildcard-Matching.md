---
thumbnail:
title: leetcode 44. Wildcard Matching
date: 2020-07-05 18:13:46
categories:
- leetcode
- leetcode-4**
toc: true
tags:
- String
- Dynamic Programming
- Backtracking
- Greedy
recommend: 1
keywords:
uniqueId: '2020-07-05 18:13:46/"leetcode 44. Wildcard Matching".html'
---

<a href="https://leetcode.com/problems/wildcard-matching/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/wildcard-matching/">九章</a>
## 题目描述
Given an input string (`s`) and a pattern (`p`), implement wildcard pattern
matching with support for `'?'` and `'*'`.
        
    '?' Matches any single character.
    '*' Matches any sequence of characters (including the empty sequence).


The matching should cover the **entire** input string (not partial).

**Note:**

  * `s` could be empty and contains only lowercase letters `a-z`.
  * `p` could be empty and contains only lowercase letters `a-z`, and characters like `?` or `*`.

**Example 1:**
        
    Input:
    s = "aa"
    p = "a"
    Output: false
    Explanation: "a" does not match the entire string "aa".


**Example 2:**
        
    Input:
    s = "aa"
    p = "*"
    Output: true
    Explanation:  '*' matches any sequence.


**Example 3:**
        
    Input:
    s = "cb"
    p = "?a"
    Output: false
    Explanation:  '?' matches 'c', but the second letter is 'a', which does not match 'b'.


**Example 4:**
        
    Input:
    s = "adceb"
    p = "*a*b"
    Output: true
    Explanation:  The first '*' matches the empty sequence, while the second '*' matches the substring "dce".


**Example 5:**
        
    Input:
    s = "acdcb"
    p = "a*c?b"
    Output: false



**Tags:** String, Dynamic Programming, Backtracking, Greedy

**Difficulty:** Hard

## 答案
<!--more-->

[字符串动态规划，🤷‍♀️ 必须秒懂！ - 通配符匹配 - 力扣（LeetCode）](https://leetcode-cn.com/problems/wildcard-matching/solution/zi-fu-chuan-dong-tai-gui-hua-bi-xu-miao-dong-by-sw/)
[44. 通配符匹配 - 力扣（LeetCode）](https://leetcode-cn.com/problems/wildcard-matching/submissions/)

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        # 思路参考：
        # [字符串动态规划，🤷‍♀️ 必须秒懂！ - 通配符匹配 - 力扣（LeetCode）](https://leetcode-cn.com/problems/wildcard-matching/solution/zi-fu-chuan-dong-tai-gui-hua-bi-xu-miao-dong-by-sw/)
        m, n = len(s), len(p)
        # dp[n][m]: 表示 p 的前 i 个字符和 s 的前 j 个字符是否匹配
        # 一个很好的例子解释代码：(0表示 false，1 表示 true)
        #     a d c e b
        #   1 0 0 0 0 0 
        # * 1 1 1 1 1 1
        # a 0 1 0 0 0 0
        # * 0 1 1 1 1 1 ( *a 匹配所以后面都是 1)
        # b 0 0 0 0 0 1
        dp = [[False]*(m+1) for _ in range(n+1)]
        dp[0][0] = True # 空串匹配空串
        # p 的前若干个* 匹配空串
        for i in range(1, n+1):
            if p[i-1]!='*':
                break
            dp[i][0] = True
        for i in range(1, n+1):
            for j in range(1, m+1):
                if p[i-1]=='*':
                    dp[i][j] = dp[i-1][j]|dp[i][j-1]
                elif p[i-1]=='?' or p[i-1]==s[j-1]:
                    dp[i][j] = dp[i-1][j-1]
        return dp[-1][-1]
        
```
