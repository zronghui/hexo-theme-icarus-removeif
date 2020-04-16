---
title: weekly-contest-180
toc: true
recommend: 1
uniqueId: '2020-04-15 03:32:33/"weekly-contest-180".html'
date: 2020-04-15 11:32:33
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [Lucky Numbers in a Matrix](https://leetcode.com/contest/weekly-contest-180/problems/lucky-numbers-in-a-matrix)**3**
- [x] [Design a Stack With Increment Operation](https://leetcode.com/contest/weekly-contest-180/problems/design-a-stack-with-increment-operation)**4**
- [ ] [Balance a Binary Search Tree](https://leetcode.com/contest/weekly-contest-180/problems/balance-a-binary-search-tree)**4**
- [ ] [Maximum Performance of a Team](https://leetcode.com/contest/weekly-contest-180/problems/maximum-performance-of-a-team)**6**

<!--more-->

## 1

```python
class Solution:
    def addTo(self, d, i, m):
        if i in d:
            d[i].append(m)
        else:
            d[i] = [m]
            
    def luckyNumbers (self, matrix: List[List[int]]) -> List[int]:
        d = {}
        for l in matrix:
            m = min(l)
            for i, v in enumerate(l):
                if v==m:
                    self.addTo(d, i, m)
        result = []
        for k, v in d.items():
            m = max(i[k] for i in matrix)
            result.extend(i for i in v if i==m)
        return result
```

## 2

```python
class CustomStack:

    def __init__(self, maxSize: int):
        self.left = maxSize
        self.l = []

    def push(self, x: int) -> None:
        if self.left>0:
            self.l.append(x)
            self.left -= 1

    def pop(self) -> int:
        if self.l:
            self.left += 1
            result = self.l[-1]
            del self.l[-1]
            return result
        else:
            return -1

    def increment(self, k: int, val: int) -> None:
        for i in range(min(k, len(self.l))):
            self.l[i] +=  val
        


# Your CustomStack object will be instantiated and called as such:
# obj = CustomStack(maxSize)
# obj.push(x)
# param_2 = obj.pop()
# obj.increment(k,val)
```

## 3

[LeetCode 1382. Balance a Binary Search Tree - AcWing](https://www.acwing.com/solution/LeetCode/content/10001/)

<img src="https://i.loli.net/2020/04/15/OHumJwqtc6CyTNa.png" alt="OHumJwqtc6CyTNa" style="zoom:50%;" />

```python
class Solution:
    def getNums(self, root, l):
        if not root:
            return
        self.getNums(root.left, l)
        l.append(root.val)
        self.getNums(root.right, l)
    
    def build(self, l):
        n = len(l)
        if n==0:
            return None
        mid = int(n/2)
        root = TreeNode(l[mid])
        root.left = self.build(l[:mid])
        root.right = self.build(l[mid+1:])
        return root
        
    
    def balanceBST(self, root: TreeNode) -> TreeNode:
        l = []
        self.getNums(root, l)
        if len(l)<3:
            return root
        return self.build(l)
```

## 4

[排序+堆(Python) - 最大的团队表现值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/maximum-performance-of-a-team/solution/pai-xu-dui-python-by-elmer/)

思路：

按efficiency进行降序排列，
遍历排序后的speed和efficiency，在遍历过程中进行以下操作：
(1)将speed放入最小堆中，
(2)保持堆的大小为k，
(3)记录堆中speed之和，
(4)寻找最大团队表现值。

```python
from heapq import heappush as push
from heapq import heappop as pop

class Solution:
    def __init__(self):
        self.mod = 10**9+7
        
    def maxPerformance(self, n: int, speed: List[int], efficiency: List[int], k: int) -> int:
        l = sorted(zip(speed, efficiency), key=lambda i: -i[1])
        speed_sum = 0
        speedl = []
        res = 0
        for s, e in l:
            push(speedl, s)
            speed_sum += s
            if len(speedl)>k:
                speed_sum -= pop(speedl)
            res = max(res, speed_sum*e)
        return res%self.mod
```



