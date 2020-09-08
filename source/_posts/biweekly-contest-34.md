---
title: biweekly-contest-34
toc: true
recommend: 1
uniqueId: '2020-09-05 15:56:45/"biweekly-contest-34".html'
date: 2020-09-05 23:56:45
thumbnail:
categories:
tags:
keywords:
---

[TOC]

- [x] [矩阵对角线元素的和](https://leetcode-cn.com/contest/biweekly-contest-34/problems/matrix-diagonal-sum/)**3**
- [x] [分割字符串的方案数](https://leetcode-cn.com/contest/biweekly-contest-34/problems/number-of-ways-to-split-a-string/)**4**
- [x] [删除最短的子数组使剩余数组有序](https://leetcode-cn.com/contest/biweekly-contest-34/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)**5**
- [x] [统计所有可行路径](https://leetcode-cn.com/contest/biweekly-contest-34/problems/count-all-possible-routes/)**6**



<!--more-->



# 1

```python
class Solution:
    def diagonalSum(self, mat: List[List[int]]) -> int:
        n = len(mat)
        return sum(mat[i][i] if n-1-i==i else mat[i][i]+mat[i][n-1-i] for i in range(n))
```

# 2

```python
class Solution:
    def numWays(self, s: str) -> int:
        n = sum(1 if c=='1' else 0 for c in s)
        if n%3: return 0
        if n==0:
            return math.comb(len(s)-1, 2)%(10**9+7)
        t = n//3
        # 找到 t t+1 的 1 的位置
        cur = 0
        for i in range(len(s)):
            if s[i]=='1':
                cur += 1
                if cur==t:
                    it = i
                elif cur==t+1:
                    it1 = i
                    break
        cur = 0
        # 倒着数 t t+1 1 的位置
        for i in reversed(range(len(s))):
            if s[i]=='1':
                cur += 1
                if cur==t:
                    rit = i
                elif cur==t+1:
                    rit1 = i
                    break
        return ((it1-it)*(rit-rit1))%(10**9+7)
        
```

# 3



```python
from bisect import bisect_left, insort
class Solution:
    def findLengthOfShortestSubarray(self, nums: List[int]) -> int:
        n = len(nums)
        if n<2: return 0
        l, r = 0, n-1
        
        # 1. 确定首部递增序列的尾部 l
        while l+1<n and nums[l+1]>=nums[l]:
            l += 1
        if l==n-1:
            return 0
        # print(l)
        # 2. 确定尾部递增序列的首部 minr
        minr = r
        while minr-1>=0 and nums[minr]>=nums[minr-1]:
            minr -= 1
        
        res = min(n-l-1, minr)
        while l>=0 and r>=0:
            if nums[l]>nums[r]:
                res = min(res, (r-l))
            else:
                while r-1>=minr and nums[r-1]>=nums[l]:
                    r -= 1
                res = min(res, (r-l-1))
            # print(l, r)
            l -= 1
        return res
        
         
```

# 4



```python
class Solution:
    def countRoutes(self, nums: List[int], start: int, end: int, fuel: int) -> int:
        n = len(nums)
        @functools.lru_cache(None)
        def helper(s, e, f):
            if f < 0: return 0
            return (sum(helper(i, end, f-abs(nums[s]-nums[i])) if i!=s else 0 for i in range(n))%(10**9+7) + (1 if s==end else 0))%(10**9+7)
        
        return helper(start, end, fuel)%(10**9+7)
```

