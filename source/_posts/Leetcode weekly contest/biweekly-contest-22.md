---
title: biweekly-contest-22
toc: true
recommend: 1
uniqueId: '2020-04-13 05:17:54/"biweekly-contest-22".html'
date: 2020-04-13 13:17:54
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [Find the Distance Value Between Two Arrays](https://leetcode.com/contest/biweekly-contest-22/problems/find-the-distance-value-between-two-arrays)**3**
- [x] [Cinema Seat Allocation](https://leetcode.com/contest/biweekly-contest-22/problems/cinema-seat-allocation)**4**
- [x] [Sort Integers by The Power Value](https://leetcode.com/contest/biweekly-contest-22/problems/sort-integers-by-the-power-value)**5**
- [ ] [Pizza With 3n Slices](https://leetcode.com/contest/biweekly-contest-22/problems/pizza-with-3n-slices)**7**

<!--more-->



## 1

```python
class Solution:
    def satisfy(self, arr2, i, d):
        for j in arr2:
            if abs(j-i)<=d:
                return False
        return True
    
    def findTheDistanceValue(self, arr1: List[int], arr2: List[int], d: int) -> int:
        n = 0
        for i in arr1:
            if self.satisfy(arr2, i, d):
                n += 1
        return n
```

## 2

```python
class Solution:
    def maxNumberOfFamiliesCurrentRow(self, l):
        a1 = 2 not in l and 3 not in l
        a2 = 4 not in l and 5 not in l
        a3 = 6 not in l and 7 not in l
        a4 = 8 not in l and 9 not in l
        n = 0
        if a1:
            n += 1
        if a2:
            n += 1
        if a3:
            n += 1
        if a4:
            n += 1
        if n==4:
            return 2
        if n==3:
            return 1
        if n==1:
            return 0
        if (a1 and a2) or (a2 and a3) or (a3 and a4):
            return 1
        return 0
        
        
    def maxNumberOfFamilies1(self, n: int, reservedSeats: List[List[int]]) -> int:
        # 超时
        d = {i:[] for i in range(1, n+1)}
        for i, j in reservedSeats:
            d[i].append(j)
        n = 0
        for i in d.values():
            n += self.maxNumberOfFamiliesCurrentRow(i)
        return n
    
    def maxNumberOfFamilies(self, n: int, reservedSeats: List[List[int]]) -> int:
        d = {}
        for i, j in reservedSeats:
            if i not in d:
                d[i] = [j]
            else:
                d[i].append(j)
        n = 2*(n-len(d))
        for i in d.values():
            n += self.maxNumberOfFamiliesCurrentRow(i)
        return n
        
```


## 3

```python
class Solution:
    def getPower(self, n):
        res = 0
        while n!=1:
            if n%2 == 0:
                n /= 2
            else:
                n = n*3+1
            res += 1
        return res
        
    def getKth(self, lo: int, hi: int, k: int) -> int:
        d = {}
        for i in range(lo, hi+1):
            d[i] = self.getPower(i)
        l = list(d.items())
        l.sort(key=lambda i: i[1])
        #print(l)
        return l[k-1][0]
```

## 4

[LeetCode 1388. Pizza With 3n Slices - AcWing](https://www.acwing.com/solution/LeetCode/content/10196/)

<img src="https://i.loli.net/2020/04/13/5StfrDRZJ7W3mE2.png" alt="5StfrDRZJ7W3mE2" style="zoom:50%;" />

```python
class Solution:
    def helper(self, slices, n):
        m = len(slices)
        dp = [[0 for _ in range(n)] for _ in range(m)]
        dp[0][0] = slices[0]
        dp[1][0] = max(slices[0], slices[1])
        for i in range(2, m):
            for j in range(n):
                if j==0:
                    dp[i][j] = max(dp[i-1][j], slices[i])
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i-2][j-1]+slices[i])
        return dp[-1][n-1]
        
    def maxSizeSlices(self, slices: List[int]) -> int:
        n = int(len(slices)/3)
        return max(self.helper(slices[1:], n), self.helper(slices[:-1], n))
```


