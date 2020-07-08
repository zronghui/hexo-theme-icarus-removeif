---
thumbnail:
title: leetcode 295. Find Median from Data Stream
date: 2020-07-08 09:26:19
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Heap
- Design
recommend: 1
keywords:
uniqueId: '2020-07-08 09:26:19/"leetcode 295. Find Median from Data Stream".html'
---

<a href="https://leetcode.com/problems/find-median-from-data-stream/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/find-median-from-data-stream/">九章</a>
## 题目描述
Median is the middle value in an ordered integer list. If the size of the list
is even, there is no middle value. So the median is the mean of the two middle
value.

For example,

`[2,3,4]`, the median is `3`

`[2,3]`, the median is `(2 + 3) / 2 = 2.5`

Design a data structure that supports the following two operations:

  * void addNum(int num) - Add a integer number from the data stream to the data structure.
  * double findMedian() - Return the median of all elements so far.



**Example:**
        
    addNum(1)
    addNum(2)
    findMedian() -> 1.5
    addNum(3) 
    findMedian() -> 2




**Follow up:**

  1. If all integer numbers from the stream are between 0 and 100, how would you optimize it?
  2. If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?


**Tags:** Heap, Design

**Difficulty:** Hard

## 答案
<!--more-->
```python
class MedianFinder:

    def __init__(self):
        self.l = []


    def addNum(self, num: int) -> None:
        bisect.insort(self.l, num)


    def findMedian(self) -> float:
        n = len(self.l)
        if n&1:
            return self.l[n//2]
        return (self.l[n//2]+self.l[n//2-1])/22
```
