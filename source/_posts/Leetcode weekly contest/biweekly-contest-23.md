---
title: biweekly-contest-23
toc: true
recommend: 1
uniqueId: '2020-04-05 03:58:36/"biweekly-contest-23".html'
date: 2020-04-05 11:58:36
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

## 

- [x] [Count Largest Group](https://leetcode.com/contest/biweekly-contest-23/problems/count-largest-group)**3**
- [x] [Construct K Palindrome Strings](https://leetcode.com/contest/biweekly-contest-23/problems/construct-k-palindrome-strings)**5**
- [ ] [Circle and Rectangle Overlapping](https://leetcode.com/contest/biweekly-contest-23/problems/circle-and-rectangle-overlapping)**5**
- [x] [Reducing Dishes](https://leetcode.com/contest/biweekly-contest-23/problems/reducing-dishes)**6**

<!--more-->

## 1

```python
class Solution:
    def countLargestGroup(self, n: int) -> int:
        # {sum: [n1, n2], }
        d = {}
        for i in range(1, n+1):
            _sum = 0
            t = i
            while t:
                _sum += t%10
                t = int(t/10)
            if _sum not in d:
                d[_sum] = [i]
            else:
                d[_sum].append(i)
        result = 0
        m = 0
        for i in d.values():
            if len(i)==m:
                result += 1
            elif len(i) > m:
                result = 1
                m = len(i)
        return result
```



## 2

```python
class Solution:
    def canConstruct(self, s: str, k: int) -> bool:
        if len(s)<k:
            return False
        if len(s)==k:
            return True
        # 单数，双数 个数 x, y
        # 前提：len(s)=x+2y>k
        # 要求：x<=k
        l = []
        for i in s:
            if i in l:
                l.remove(i)
            else:
                l.append(i)
        if len(l) <= k:
            return True
        return False
```

## 3

事后看一两三四五的解法，做出来的

https://www.bilibili.com/video/BV1Ae411x7fa/?p=1

LeetCode 考的是智商吧

<img src="/Users/zhangronghui/Library/Application Support/typora-user-images/image-20200405151432618.png" alt="image-20200405151432618" style="zoom:50%;" />



求距离时不要用 根号，反之把半径平方即可比较，否则会很慢

```python
class Solution:
    def d(self, x1, y1, x2, y2):
        return (x1-x2)**2+(y1-y2)**2
    
    def checkOverlap(self, r: int, x: int, y: int, x1: int, y1: int, x2: int, y2: int) -> bool:
        # 圆心在矩形内
        if x1 <= x <= x2 and y1 <= y <= y2:
            return True
        # 圆心到上下两边的距离
        if x1 <= x <= x2 and min(abs(y1-y), abs(y2-y))<=r:
            return True
        # 圆心到左右两边的距离
        if y1 <= y <= y2 and min(abs(x1-x), abs(x2-x))<=r:
            return True
        # # 圆心到四点的距离
        if min(self.d(x, y, x1, y1), self.d(x, y, x1, y2), self.d(x, y, x2, y1), self.d(x, y, x2, y2)) <= r*r:
            return True
        return False
        
```

另一种思路：

将矩形扩大后判断圆心是否在里面，接着判断圆心到四个顶点的距离是否不大于 r

其实这就是上面解法的另一种解释

<img src="https://i.loli.net/2020/04/05/5DRCiebSaBYI8Gu.png" alt="5DRCiebSaBYI8Gu" style="zoom:50%;" />





## 4

```python
class Solution:
    def maxSatisfaction(self, satisfaction: List[int]) -> int:
        # 1. sort
        # 2. 找到第一个让 sum 为负数的位置, 移动中将 sum 加到 result 中
        satisfaction.sort(reverse=True)
        _sum  = 0
        result = 0
        for i in satisfaction:
            _sum += i
            if _sum<0:
                break
            result += _sum
        return result
```

