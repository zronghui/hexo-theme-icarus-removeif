---
thumbnail:
title: leetcode 125. Valid Palindrome
date: 2020-06-19 23:56:23
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Two Pointers
- String
recommend: 1
keywords:
uniqueId: '2020-06-19 23:56:23/"leetcode 125. Valid Palindrome".html'
---

<a href="https://leetcode.com/problems/valid-palindrome/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/valid-palindrome/">九章</a>
## 题目描述
Given a string, determine if it is a palindrome, considering only alphanumeric
characters and ignoring cases.

**Note:**  For the purpose of this problem, we define empty string as valid
palindrome.

**Example 1:**
        
    Input: "A man, a plan, a canal: Panama"
    Output: true


**Example 2:**
        
    Input: "race a car"
    Output: false



**Tags:** Two Pointers, String

**Difficulty:** Easy

## 答案
<!--more-->
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = ''.join(i.lower() for i in s if i.isalnum())
        n = len(s)
        l, r = 0, n-1
        while l<r:
            if s[l]!=s[r]:
                return False
            l += 1
            r -= 1
        return True

```
