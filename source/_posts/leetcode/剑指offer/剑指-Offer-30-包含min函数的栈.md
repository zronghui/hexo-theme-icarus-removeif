---
title: 剑指 Offer 30 包含min函数的栈
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:20/"剑指 Offer 30 包含min函数的栈".html'
date: 2020-06-30 21:55:20
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

我的解法：

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.l = []
        self.minl = []

    def push(self, x: int) -> None:
        self.l.append(x)
        curmin = x
        if self.minl:
           curmin = min(x, self.minl[-1]) 
        self.minl.append(curmin)

    def pop(self) -> None:
        self.minl.pop()
        return self.l.pop()

    def top(self) -> int:
        return self.l[-1]

    def min(self) -> int:
        return self.minl[-1]

```

更省空间的解法：

[面试题30. 包含 min 函数的栈（辅助栈，清晰图解） - 包含min函数的栈 - 力扣（LeetCode）](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/solution/mian-shi-ti-30-bao-han-minhan-shu-de-zhan-fu-zhu-z/)

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.l, self.minl = [], []

    def push(self, x: int) -> None:
        self.l.append(x)
        if not self.minl or x<=self.minl[-1]:
            self.minl.append(x)

    def pop(self) -> None:
        if self.l.pop()==self.minl[-1]:
            self.minl.pop()

    def top(self) -> int:
        return self.l[-1]

    def getMin(self) -> int:
        return self.minl[-1]

```

