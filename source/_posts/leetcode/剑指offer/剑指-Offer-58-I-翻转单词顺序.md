---
title: 剑指 Offer 58 - I 翻转单词顺序
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:26/"剑指 Offer 58 - I 翻转单词顺序".html'
date: 2020-06-30 21:56:26
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
    def reverseWords(self, s: str) -> str:
        return ' '.join(s.split()[::-1])
```

