---
title: 剑指 Offer 59 - II 队列的最大值
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:31/"剑指 Offer 59 - II 队列的最大值".html'
date: 2020-06-30 21:56:31
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[如何解决 O(1) 复杂度的 API 设计题 - 队列的最大值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/solution/ru-he-jie-jue-o1-fu-za-du-de-api-she-ji-ti-by-z1m/)

![image-20200714093359463](https://i.loli.net/2020/07/14/t9mCYupQO4PFZED.png)

![fig3.gif](https://pic.leetcode-cn.com/9d038fc9bca6db656f81853d49caccae358a5630589df304fc24d8999777df98-fig3.gif)

```python
class MaxQueue:

    def __init__(self):
        self.q = collections.deque()
        self.maxq = collections.deque()

    def max_value(self) -> int:
        return self.maxq[0] if self.maxq else -1


    def push_back(self, value: int) -> None:
        self.q.append(value)
        while self.maxq and self.maxq[-1]<value:
            self.maxq.pop()
        self.maxq.append(value)


    def pop_front(self) -> int:
        if not self.q: return -1
        value = self.q.popleft()
        if self.maxq[0]==value: self.maxq.popleft()
        return value



# Your MaxQueue object will be instantiated and called as such:
# obj = MaxQueue()
# param_1 = obj.max_value()
# obj.push_back(value)
# param_3 = obj.pop_front()
```

