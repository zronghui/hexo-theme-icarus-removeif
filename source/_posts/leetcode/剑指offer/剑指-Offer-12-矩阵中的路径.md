---
title: 剑指 Offer 12 矩阵中的路径
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:47/"剑指 Offer 12 矩阵中的路径".html'
date: 2020-06-30 21:54:47
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        n, m = len(board), len(board[0])
        mark = [[False]*m for i in range(n)]
        lw = len(word)

        def helper(i, j, index):
            if not(0<=i<n and 0<=j<m) or mark[i][j] or board[i][j]!=word[index]:
                return False
            mark[i][j] = True
            if index==lw-1:
                return True
            for x, y in directions:
                if helper(i+x, j+y, index+1):
                    return True
            mark[i][j] = False
            return False

        for i in range(n):
            for j in range(m):
                if helper(i, j, 0):
                    return True
        return False
```

