---
thumbnail:
title: leetcode 680. Valid Palindrome II
date: 2020-06-09 12:59:14
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- String
recommend: 1
keywords:
uniqueId: '2020-06-09 12:59:14/"leetcode 680. Valid Palindrome II".html'
---

<a href="https://leetcode.com/problems/valid-palindrome-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/valid-palindrome-ii/">九章</a>
## 题目描述
Given a non-empty string `s`, you may delete **at most** one character. Judge
whether you can make it a palindrome.

**Example 1:**  
        
    Input: "aba"
    Output: True


**Example 2:**  
        
    Input: "abca"
    Output: True
    Explanation: You could delete the character 'c'.


**Note:**  

  1. The string will only contain lowercase characters a-z. The maximum length of the string is 50000.


**Tags:** String

**Difficulty:** Easy

## 答案
<!--more-->
```python
class Solution:
    def isPalindrome(self, s):
        n = len(s)
        l = 0
        r = n-1
        while l<=r:
            if s[l]!=s[r]:
                return False
            l += 1
            r -= 1
        return True

    def validPalindrome(self, s: str) -> bool:
        n = len(s)
        l = 0
        r = n-1
        while l<=r:
            if s[l]!=s[r]:
                return self.isPalindrome(s[l:r]) or self.isPalindrome(s[l+1:r+1])
            l += 1
            r -= 1
        return True

```
