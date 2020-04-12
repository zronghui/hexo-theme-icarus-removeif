---
title: weekly-contest-181
toc: true
recommend: 1
uniqueId: '2020-04-11 13:14:52/"weekly-contest-181".html'
date: 2020-04-11 21:14:52
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [ ] [Create Target Array in the Given Order](https://leetcode.com/contest/weekly-contest-181/problems/create-target-array-in-the-given-order)**3**
- [ ] [Four Divisors](https://leetcode.com/contest/weekly-contest-181/problems/four-divisors)**4**
- [x] [Check if There is a Valid Path in a Grid](https://leetcode.com/contest/weekly-contest-181/problems/check-if-there-is-a-valid-path-in-a-grid)**5**
- [ ] [Longest Happy Prefix](https://leetcode.com/contest/weekly-contest-181/problems/longest-happy-prefix) **6**

<!--more-->

## 1

```python
class Solution:
    def createTargetArray(self, nums: List[int], index: List[int]) -> List[int]:
        result = []
        for num, index in zip(nums, index):
            result.insert(index, num)
        return result
```

## 2

为什么我就不会暴力呢

```python
class Solution:
    def isFourDivisors(self, n):
        # return 0 or sum of four divisors of n
        cnt = 2 # 1 以及 自身
        res = 1+n
        i = 2
        while True:
            if i*i>n:
                break
            if i*i==n:
                return 0
            if not n%i:
                cnt += 2
                res += i + n/i
            if cnt>4:
                return 0
            i += 1
        if cnt==4:
            return int(res)
        return 0
            
    def sumFourDivisors(self, nums: List[int]) -> int:
        return sum(self.isFourDivisors(i) for i in nums)
```

## 3

这题很恶心，也没说清楚，得自己试，浪费了很多时间

```python
class Solution:
    # direction 表示上一个格子到当前格子的流动方向
    
    def nextij(self, grid, i, j, direction):
        num = grid[i][j]
        # print([num, direction])
        if [num, direction] in [[1, 'right'], [4, 'up'], [6, 'down']]:
            return i, j+1, 'right'
        if [num, direction] in [[1, 'left'], [3, 'up'], [5, 'down']]:
            return i, j-1, 'left'
        if [num, direction] in [[2, 'up'], [5, 'right'], [6, 'left']]:
            return i-1, j, 'up'
        if [num, direction] in [[2, 'down'], [3, 'right'], [4, 'left']]:
            return i+1, j, 'down'
        return False
        
    def hasValidPath(self, grid: List[List[int]]) -> bool:
        m = len(grid)
        n = len(grid[0])
        if m==n==1:
            return True
        i = j = 0
        
        if grid[0][0] in [5]:
            return False
        elif grid[0][0] in [1, 3]:
            direction = 'right'
            return self.helper(grid, m, n, i, j, direction)
        elif grid[0][0] in [2, 6]:
            direction = 'down'
            return self.helper(grid, m, n, i, j, direction)
        elif grid[0][0] in [4]:
            return self.helper(grid, m, n, 0, 1, 'right') or self.helper(grid, m, n, 1, 0, 'down')


    def helper(self, grid, m, n, i, j, direction):
        walked = [[0 for j in range(n)] for i in range(m)]
        while True:
            nextijResult = self.nextij(grid, i, j, direction)
            # print('nextijResult', nextijResult)
            if not nextijResult:
                return False
            nexti, nextj, direction = nextijResult
            # print(nexti, nextj, direction)
            if not (0<=nexti<m and 0<=nextj<n):
                return False
            if walked[nexti][nextj]:
                return False
            if nexti==m-1 and nextj==n-1:
                return bool(self.nextij(grid, nexti, nextj, direction))
            walked[i][j] = 1
            i, j = nexti, nextj
```

## 4

kmp pi函数或者说 next fail 函数的性质题，输出匹配到最后位置的最长前缀即可

```python
class Solution:
    def longestPrefix(self, s: str) -> str:
        n = len(s)
        nxt = [0 for i in range(n+1)]
        # i 前缀
        i = 0
        # j 后缀
        j = nxt[0] = -1
        while(i<n):
            while(j!=-1 and s[i]!=s[j]):
                j = nxt[j]
            i += 1
            j += 1
            nxt[i] = j
        # print(nxt)
        return s[:nxt[-1]]
```


