---
title: 剑指 Offer 05 替换空格
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:35/"剑指 Offer 05 替换空格".html'
date: 2020-06-30 21:54:35
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 05. 替换空格 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
        # return s.replace(' ', '%20')
        l = []
        for i in s:
            if i==' ':
                l.append('%20')
            else:
                l.append(i)
        return ''.join(l)
```

