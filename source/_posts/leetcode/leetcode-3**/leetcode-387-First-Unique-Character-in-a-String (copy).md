---
thumbnail:
title: leetcode 387. First Unique Character in a String
date: 2020-05-05 20:42:43
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Hash Table
- String
recommend: 1
keywords:
uniqueId: '2020-05-05 20:42:43/"leetcode 387. First Unique Character in a String".html'
---

<a href="https://leetcode.com/problems/first-unique-character-in-a-string/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/first-unique-character-in-a-string/">九章</a>
## 题目描述
Given a string, find the first non-repeating character in it and return it's
index. If it doesn't exist, return -1.

**Examples:**
        
    s = "leetcode"
    return 0.
    
    s = "loveleetcode",
    return 2.
    

**Note:** You may assume the string contain only lowercase letters.


**Tags:** Hash Table, String

**Difficulty:** Easy

## 答案
<!--more-->
```java
class Solution:
    def firstUniqChar(self, ss: str) -> int:
        # 添加未重复的 字母：index
        m = {}
        # 重复的字母
        s = set()
        for i, c in enumerate(ss):
            if c in s:
                continue
            if c in m:
                del m[c]
                s.add(c)
            else:
                m[c] = i
        return min(m.items(), key=lambda i:i[1])[1] if m else -1
```
