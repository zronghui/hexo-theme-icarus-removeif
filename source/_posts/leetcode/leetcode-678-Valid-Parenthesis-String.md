---
thumbnail:
title: leetcode 678. Valid Parenthesis String
date: 2020-04-16 19:17:33
categories:
- leetcode
toc: true
tags:
- String
recommend: 1
keywords:
uniqueId: '2020-04-16 19:17:33/"leetcode 678. Valid Parenthesis String".html'
---

<a href="https://leetcode.com/problems/valid-parenthesis-string/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/valid-parenthesis-string/">九章</a>
## 题目描述
Given a string containing only three types of characters: '(', ')' and '*',
write a function to check whether this string is valid. We define the validity
of a string by these rules:

  1. Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
  2. Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
  3. Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
  4. `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string.
  5. An empty string is also valid.

**Example 1:**  
        
    Input: "()"
    Output: True


**Example 2:**  
        
    Input: "(*)"
    Output: True


**Example 3:**  
        
    Input: "(*))"
    Output: True


**Note:**  

  1. The string size will be in the range [1, 100].


**Tags:** String

**Difficulty:** Medium

## 答案
<!--more-->

[有效的括号字符串 - 有效的括号字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/valid-parenthesis-string/solution/you-xiao-de-gua-hao-zi-fu-chuan-by-leetcode/)

<img src="https://i.loli.net/2020/04/16/cftrJKkDhgmVRal.png" alt="cftrJKkDhgmVRal" style="zoom:50%;" />

```java
class Solution:
    def checkValidString(self, s: str) -> bool:
        lo = hi = 0
        for i in s:
            if i=='(':
                lo += 1
                hi += 1
            elif i==')':
                lo = max(0, lo-1)
                hi -= 1
            elif i=='*':
                lo = max(0, lo-1)
                hi += 1
            if hi<0:
                return False
        print(lo, hi)
        return lo==0
```
