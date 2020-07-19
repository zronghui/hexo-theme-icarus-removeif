---
title: biweekly-contest-28
toc: true
recommend: 1
uniqueId: '2020-07-16 02:19:15/"biweekly-contest-28".html'
date: 2020-07-16 10:19:15
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [商品折扣后的最终价格](https://leetcode-cn.com/problems/final-prices-with-a-special-discount-in-a-shop/)**3**
- [x] [子矩形查询](https://leetcode-cn.com/problems/subrectangle-queries/)**4**
- [ ] [找两个和为目标值且不重叠的子数组](https://leetcode-cn.com/problems/find-two-non-overlapping-sub-arrays-each-with-target-sum/)**5**
- [ ] [安排邮筒](https://leetcode-cn.com/problems/allocate-mailboxes/) 7

<!--more-->



# 1

```python
class Solution:
    def finalPrices(self, prices: List[int]) -> List[int]:
        n = len(prices)
        res = [i for i in prices]
        queue = [] # [(i, prices[i]),,,]
        for i in range(n):
            while queue and prices[i]<=queue[-1][1]:
                j, price = queue.pop()
                res[j] = price-prices[i]
            queue.append([i, prices[i]])
        return res
```

# 2

```python
class SubrectangleQueries:

    def __init__(self, rectangle: List[List[int]]):
        self.rec = rectangle


    def updateSubrectangle(self, row1: int, col1: int, row2: int, col2: int, newValue: int) -> None:
        for i in range(row1, row2+1):
            for j in range(col1, col2+1):
                self.rec[i][j] = newValue

    def getValue(self, i: int, j: int) -> int:
        return self.rec[i][j]

```

# 3

滑动窗口，超时（dp 题解看不懂）

```python
class Solution:
    def minSumOfLengths(self, arr: List[int], target: int) -> int:
        _sum, l, r = 0, 0, 0
        ls = []
        while r<len(arr):
            _sum += arr[r]
            r += 1
            if _sum<target: continue
            while _sum>target:
                _sum -= arr[l]
                l += 1
            if _sum==target: ls.append([l, r-1, r-l])
        
        # print(ls)
        def cross(l1, l2):
            if l1[0]<l2[0]:
                return l2[0]<=l1[1]
            else:
                return l1[0]<=l2[1]
        
        res = sys.maxsize
        ls.sort(key=lambda i: i[2])
        for i in range(len(ls)-1):
            if 2*ls[i][2]>res: break
            for j in range(i+1, len(ls)):
                # 有重叠的情况：ls[i]=[1, 3] ls[j]=[2, 4]
                if not cross(ls[i], ls[j]):
                    res = min(res, ls[j][1]-ls[j][0]+ls[i][1]-ls[i][0]+2)
                    break
        return res if res!=sys.maxsize else -1

```


# 4

```python

```

