---
thumbnail:
title: leetcode 63. Unique Paths II
date: 2020-07-06 08:55:42
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Array
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-06 08:55:42/"leetcode 63. Unique Paths II".html'
---

<a href="https://leetcode.com/problems/unique-paths-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/unique-paths-ii/">九章</a>
## 题目描述
A robot is located at the top-left corner of a _m_ x _n_ grid (marked 'Start'
in the diagram below).

The robot can only move either down or right at any point in time. The robot
is trying to reach the bottom-right corner of the grid (marked 'Finish' in the
diagram below).

Now consider if some obstacles are added to the grids. How many unique paths
would there be?

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

An obstacle and empty space is marked as `1` and `0` respectively in the grid.

**Note:** _m_ and _n_ will be at most 100.

**Example 1:**
        
    Input: [
      [0,0,0],
      [0,1,0],
      [0,0,0]
    ]
    Output: 2
    Explanation:
    There is one obstacle in the middle of the 3x3 grid above.
    There are two ways to reach the bottom-right corner:
    1. Right -> Right -> Down -> Down
    2. Down -> Down -> Right -> Right



**Tags:** Array, Dynamic Programming

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    def uniquePathsWithObstacles(self, matrix: List[List[int]]) -> int:
        # dp
        #          0 0 0 0
        # 0 0 0    0 1 1 1
        # 0 1 0 -> 0 1 0 1
        # 0 0 0    0 1 1 2
        n, m = len(matrix), len(matrix[0])
        # 初始化第一行，第一列，若当前为 1，则往后都不可达
        if matrix[0][0] == 1:
            return 0
        matrix[0][0] = 1
        for i in range(1, n):
            if matrix[i][0]==0:
                matrix[i][0] = 1
            else:
                for j in range(i, n):
                    matrix[j][0] = 0
                break
        for i in range(1, m):
            if matrix[0][i]==0:
                matrix[0][i] = 1
            else:
                for j in range(i, m):
                    matrix[0][j] = 0
                break
        # print(matrix)
        for i in range(1, n):
            for j in range(1, m):
                if matrix[i][j] == 1:
                    matrix[i][j] = 0
                else:
                    matrix[i][j] = matrix[i-1][j]+matrix[i][j-1]
        # print(matrix)
        return matrix[-1][-1]
```
