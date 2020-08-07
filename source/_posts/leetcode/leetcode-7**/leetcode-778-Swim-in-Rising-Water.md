---
thumbnail:
title: leetcode 778. Swim in Rising Water
date: 2020-08-06 19:27:31
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- Binary Search
- Heap
- Depth-first Search
- Union Find
recommend: 1
keywords:
uniqueId: '2020-08-06 19:27:31/"leetcode 778. Swim in Rising Water".html'
---

<a href="https://leetcode.com/problems/swim-in-rising-water/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/swim-in-rising-water/">九章</a>
## 题目描述
On an N x N `grid`, each square `grid[i][j]` represents the elevation at that
point `(i,j)`.

Now rain starts to fall. At time `t`, the depth of the water everywhere is
`t`. You can swim from a square to another 4-directionally adjacent square if
and only if the elevation of both squares individually are at most `t`. You
can swim infinite distance in zero time. Of course, you must stay within the
boundaries of the grid during your swim.

You start at the top left square `(0, 0)`. What is the least time until you
can reach the bottom right square `(N-1, N-1)`?

**Example 1:**
        
    Input: [[0,2],[1,3]]
    Output: 3
    Explanation:
    At time 0, you are in grid location (0, 0).
    You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
    
    You cannot reach point (1, 1) until time 3.
    When the depth of water is 3, we can swim anywhere inside the grid.


**Example 2:**
        
    Input: [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
    Output: 16
    Explanation:
    **0  1  2  3  4**
    24 23 22 21  **5**
    **12 13 14 15 16**
    **11** 17 18 19 20
    **10  9  8  7  6**
    
    The final route is marked in bold.
    We need to wait until time 16 so that (0, 0) and (4, 4) are connected.


**Note:**

  1. `2 <= N <= 50`.
  2. grid[i][j] is a permutation of [0, ..., N*N - 1].


**Tags:** Binary Search, Heap, Depth-first Search, Union Find

**Difficulty:** Hard

## 答案
<!--more-->

[优先队列法，以及为何会想到优先队列法 - 水位上升的泳池中游泳 - 力扣（LeetCode）](https://leetcode-cn.com/problems/swim-in-rising-water/solution/you-xian-dui-lie-fa-yi-ji-wei-he-hui-xiang-dao-you/)



```python
from collections import deque
from bisect import insort

class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        res = 0
        l = deque() # (value, i, j), ,
        l.append((grid[0][0], 0, 0))
        n = len(grid)
        visited = [[False]*n for _ in range(n)]
        while l:
            v, i, j = l.popleft()
            visited[i][j] = True
            # print(v)
            res = max(v, res)
            if i==j==n-1: return res
            for di, dj in [[0, 1],[0, -1],[1, 0],[-1, 0]]:
                ii, jj = i+di, j+dj
                if 0<=ii<n and 0<=jj<n and not visited[ii][jj]:
                    insort(l, [grid[ii][jj], ii, jj])
        return res
```
