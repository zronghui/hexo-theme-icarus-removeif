---
thumbnail:
title: leetcode 32. Longest Valid Parentheses
date: 2020-07-04 08:36:37
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- String
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-04 08:36:37/"leetcode 32. Longest Valid Parentheses".html'
---

<a href="https://leetcode.com/problems/longest-valid-parentheses/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/longest-valid-parentheses/">九章</a>
## 题目描述
Given a string containing just the characters `'('` and `')'`, find the length
of the longest valid (well-formed) parentheses substring.

**Example 1:**
        
    Input: "(()"
    Output: 2
    Explanation: The longest valid parentheses substring is "()"


**Example 2:**
        
    Input: ")()())"
    Output: 4
    Explanation: The longest valid parentheses substring is "()()"



**Tags:** String, Dynamic Programming

**Difficulty:** Hard

## 答案
<!--more-->
```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        # )()()) 
        # [('', -1), (')', 0), ('(', 5), ('', 6)] 5-0-1
        l = [('', -1)]
        for i, c in enumerate(s):
            if c==')' and l[-1][0]=='(':
                l.pop()
            else:
                l.append((c, i))
        l.append(('', len(s)))
        res = 0
        for i, v in enumerate(l[1:], start=1):
            res = max(res, v[1]-l[i-1][1]-1)
        return res
        
```
