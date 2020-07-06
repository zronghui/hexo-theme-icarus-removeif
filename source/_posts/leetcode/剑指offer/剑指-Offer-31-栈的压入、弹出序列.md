---
title: 剑指 Offer 31 栈的压入、弹出序列
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:22/"剑指 Offer 31 栈的压入、弹出序列".html'
date: 2020-06-30 21:55:22
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 31. 栈的压入、弹出序列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/submissions/)



```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        nxti = 0 # pushed 中下一个入栈的序号
        l = [] # 栈
        for i in popped:
            # 若栈为空，或栈顶元素非 i，入栈
            if not l or l[-1]!=i:
                while nxti < len(pushed):
                    l.append(pushed[nxti])
                    nxti += 1
                    if l[-1]==i:
                        break
            if l.pop()!=i:
                return False
        return True

```

