---
thumbnail:
title: leetcode 200. Number of Islands
date: 2020-04-17 19:04:26
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Depth-first Search
- Breadth-first Search
- Union Find
recommend: 1
keywords:
uniqueId: '2020-04-17 19:04:26/"leetcode 200. Number of Islands".html'
---

<a href="https://leetcode.com/problems/number-of-islands/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/number-of-islands/">九章</a>
## 题目描述
Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of
islands. An island is surrounded by water and is formed by connecting adjacent
lands horizontally or vertically. You may assume all four edges of the grid
are all surrounded by water.

**Example 1:**
        
    Input:
    11110
    11010
    11000
    00000
    
    Output:  1


**Example 2:**
        
    Input:
    11000
    11000
    00100
    00011
    
    Output: 3



**Tags:** Depth-first Search, Breadth-first Search, Union Find

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    def removeIsland(self, grid, i, j, n, m):
        if i<0 or j<0 or i>=n or j>=m or grid[i][j]=='0':
            return
        grid[i][j] = '0'
        self.removeIsland(grid, i-1, j, n, m)
        self.removeIsland(grid, i+1, j, n, m)
        self.removeIsland(grid, i, j-1, n, m)
        self.removeIsland(grid, i, j+1, n, m)
        
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid or not grid[0]:
            return 0
        n, m = len(grid), len(grid[0])
        res = 0
        for i in range(n):
            for j in range(m):
                if grid[i][j]=='1':
                    self.removeIsland(grid, i, j, n, m)
                    res += 1
        return res
```
