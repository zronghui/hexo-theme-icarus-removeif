---
title: biweekly-contest-33
toc: true
recommend: 1
uniqueId: '2020-08-22 15:23:44/"biweekly-contest-33".html'
date: 2020-08-22 23:23:44
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [千位分隔数](https://leetcode-cn.com/contest/biweekly-contest-33/problems/thousand-separator/)**3**
- [x] [可以到达所有点的最少点数目](https://leetcode-cn.com/contest/biweekly-contest-33/problems/minimum-number-of-vertices-to-reach-all-nodes/)**4**
- [x] [得到目标数组的最少函数调用次数](https://leetcode-cn.com/contest/biweekly-contest-33/problems/minimum-numbers-of-function-calls-to-make-target-array/)**5**
- [x] [二维网格图中探测环](https://leetcode-cn.com/contest/biweekly-contest-33/problems/detect-cycles-in-2d-grid/)**6**

还可以，都不算太难，速度有些慢了

![image-20200822232459723](https://i.loli.net/2020/08/22/hUQrkCzwIqeYGa1.png)

<!--more-->



# 1

```python
class Solution:
    def thousandSeparator(self, n: int) -> str:
        l = []
        while n>1000:
            n, cur = divmod(n, 1000)
            l.append(str(cur).zfill(3))
        l.append(str(n))
        return '.'.join(l[::-1])
```

# 2

脑筋急转弯

```python
class Solution:
    def findSmallestSetOfVertices(self, n: int, edges: List[List[int]]) -> List[int]:
        # 所有入度为 0 的点
        indegrees = collections.defaultdict(int)
        points = set()
        for a, b in edges:
            points.add(a)
            points.add(b)
            indegrees[b] += 1
        res = []
        for p in points:
            if indegrees.get(p, 0)==0:
                res.append(p)
        return res
        
```


# 3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        # 问题简化：nums 的每个元素都最少经过 a 次 +1,  b 次 *2
        # 结果等同于 max(*2次数)+sum(+1 次数)
        #   3 2 2 4
        # a 2 1 1 1 (+1)
        # b 1 1 1 2 (*2)
        # max(b) + sum(a) = 2+5
        a, b = [], []
        
        def getab(n):
            x, y = 0, 0
            while n:
                if n&1:
                    n -= 1
                    x += 1
                else:
                    n //=2
                    y += 1
            return x, y
        
        for i in nums:
            x, y = getab(i)
            a.append(x)
            b.append(y)
        # print(a, b, max(b), sum(a))
        return max(b) + sum(a)
```


# 4

```python
class Solution:
    def containsCycle(self, grid: List[List[str]]) -> bool:
        # bfs
        n, m = len(grid), len(grid[0])
        # 0: 没访问过 其他数字表示同一轮的搜索轮次
        visited = [[0]*m for i in range(n)]
        self.cur = 1
        
        def bfs(i0, j0):
            # bfs 遍历上下左右
            visited[i0][j0] = self.cur
            queue = [[i0, j0, 0, 0]] # 后 2 位记录方向
            while queue:
                _i, _j, x, y = queue.pop()
                for a, b in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
                    i, j = _i+a, _j+b
                    if (a==-x and b==-y) or not (0<=i<n) or not (0<=j<m): continue
                    # 如果已经访问过，并且格子内值相同，并且 cur 与 visited 值相同，表示有环
                    if visited[i][j]:
                        if visited[i][j]==self.cur:
                            # for i in visited: print(i)
                            # print()
                            return True
                    elif grid[i][j]==grid[i0][j0]:
                        visited[i][j] = self.cur
                        queue.append([i, j, a, b])
            self.cur += 1
            # for i in visited: print(i)
            # print()
            return False
        return any(bfs(i, j) for i in range(n) for j in range(m))
```

