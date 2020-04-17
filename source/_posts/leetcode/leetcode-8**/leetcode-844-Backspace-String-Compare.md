---
thumbnail:
title: leetcode 844. Backspace String Compare
date: 2020-04-17 17:39:09
categories:
- leetcode
- leetcode-8**
toc: true
tags:
- Two Pointers
- Stack
recommend: 1
keywords:
uniqueId: '2020-04-17 17:39:09/"leetcode 844. Backspace String Compare".html'
---

<a href="https://leetcode.com/problems/backspace-string-compare/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/backspace-string-compare/">九章</a>
## 题目描述
Given two strings `S` and `T`, return if they are equal when both are typed
into empty text editors. `#` means a backspace character.

**Example 1:**
        
    Input: S = "ab#c", T = "ad#c"
    Output: true
    **Explanation** : Both S and T become "ac".


**Example 2:**
        
    Input: S = "ab

## 答案
<!--more-->

[比较含退格的字符串 - 比较含退格的字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/backspace-string-compare/solution/bi-jiao-han-tui-ge-de-zi-fu-chuan-by-leetcode/)

虽然第二种看起来厉害点，space O(1), 但是time memory 都不如第一种，不知为何

```java
class Solution:
    def backspace(self, S):
        res = ''
        for i in S:
            if i!='#':
                res += i
            else:
                res = res[:-1]
        return res
        
    def backspaceCompare1(self, S: str, T: str) -> bool:
        s = self.backspace(S)
        t = self.backspace(T)
        return s==t
    
    def backspaceCompare(self, S: str, T: str) -> bool:
        def skip(S):
            skip = 0
            for i in reversed(S):
                if i=='#':
                    skip += 1
                else:
                    if skip:
                        skip -= 1
                    else:
                        yield i
        return all(x==y for x, y in itertools.zip_longest(skip(S), skip(T)))
        
```
