---
title: 剑指 Offer 50 第一个只出现一次的字符
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:03/"剑指 Offer 50 第一个只出现一次的字符".html'
date: 2020-06-30 21:56:03
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
    def firstUniqChar(self, s: str) -> str:
        once = []
        more = []
        for i in s:
            if i not in more:
                if i not in once:
                    once.append(i)
                else:
                    once.remove(i)
                    more.append(i)
        once.append(' ')
        return once[0]

```

优化的解法：用字典可以快速查询元素是否在里面，且 Python3.6 之后 dict 是有序的了

[面试题50. 第一个只出现一次的字符（哈希表 / 有序哈希表，清晰图解） - 第一个只出现一次的字符 - 力扣（LeetCode）](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/solution/mian-shi-ti-50-di-yi-ge-zhi-chu-xian-yi-ci-de-zi-3/)



```python
class Solution:
    def firstUniqChar(self, s: str) -> str:
        d = {}
        for i in s:
            d[i] = not (i in d)
        for i, v in d.items():
            if v: return i
        return ' '

```

