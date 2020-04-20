---
thumbnail:
title: leetcode 64. Minimum Path Sum
date: 2020-04-18 18:29:46
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Array
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-04-18 18:29:46/"leetcode 64. Minimum Path Sum".html'
---

<a href="https://leetcode.com/problems/minimum-path-sum/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/minimum-path-sum/">九章</a>
## 题目描述
Given a _m_ x _n_ grid filled with non-negative numbers, find a path from top
left to bottom right which _minimizes_ the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example:**
        
    Input:
    [
      [1,3,1],
      [1,5,1],
      [4,2,1]
    ]
    Output: 7
    Explanation: Because the path 1->3->1->1->1 minimizes the sum.



**Tags:** Array, Dynamic Programming

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    # 3. dp
    def minPathSum(self, grid: List[List[int]]) -> int:
        n = len(grid)
        m = len(grid[0])
        for i in range(n):
            for j in range(m):
                if i==0 and j==0:
                    continue
                elif i==0:
                    grid[i][j] += grid[i][j-1]
                elif j==0:
                    grid[i][j] += grid[i-1][j]
                else:
                    grid[i][j] += min(grid[i-1][j], grid[i][j-1])
        return grid[-1][-1]
    #  1. 简单的递归
    def minPathSum1(self, grid: List[List[int]]) -> int:
        def helper(grid, n, m, i, j):
            res = grid[i][j]
            if i==n-1 and j==m-1:
                return res
            elif i==n-1:
                return res + helper(grid, n, m, i, j+1)
            elif j==m-1:
                return res + helper(grid, n, m, i+1, j)
            return res + min(helper(grid, n, m, i, j+1), helper(grid, n, m, i+1, j))
                
        if not grid or not grid[0]:
            return 0
        return helper(grid, len(grid), len(grid[0]), 0, 0)
    
    # 2. 递归 + 存储 递归过程值
    def minPathSum2(self, grid: List[List[int]]) -> int:
        n = len(grid)
        m = len(grid[0])
        cache = [[0 for _ in range(m)]for _ in range(n)]
        def helper(grid, n, m, i, j):
            res = grid[i][j]
            if i==n-1 and j==m-1:
                return res
            elif i==n-1:
                if cache[i][j+1]:
                    return res+cache[i][j+1]
                else:
                    t = helper(grid, n, m, i, j+1)
                    cache[i][j+1] = t
                    return res + t
            elif j==m-1:
                if cache[i+1][j]:
                    return res+cache[i+1][j]
                else:
                    t = helper(grid, n, m, i+1, j)
                    cache[i+1][j] = t
                    return res + t
                
            if cache[i][j+1]:
                t1 = cache[i][j+1]
            else:
                t1 = helper(grid, n, m, i, j+1)
                cache[i][j+1] = t1
            if cache[i+1][j]:
                t2 = cache[i+1][j] 
            else:
                t2 = helper(grid, n, m, i+1, j)
                cache[i+1][j] = t2
            return res + min(t1, t2)
                
        if not grid or not grid[0]:
            return 0
        return helper(grid, n, m, 0, 0)
            
```
