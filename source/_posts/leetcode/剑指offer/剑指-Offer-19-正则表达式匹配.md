---
title: 剑指 Offer 19 正则表达式匹配
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:02/"剑指 Offer 19 正则表达式匹配".html'
date: 2020-06-30 21:55:02
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

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

