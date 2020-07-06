---
title: 剑指 Offer 29 顺时针打印矩阵
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:18/"剑指 Offer 29 顺时针打印矩阵".html'
date: 2020-06-30 21:55:18
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[面试题29. 顺时针打印矩阵（模拟、设定边界，清晰图解） - 顺时针打印矩阵 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/solution/mian-shi-ti-29-shun-shi-zhen-da-yin-ju-zhen-she-di/)

超烂的代码：

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix or not matrix[0]:
            return []
        n, m = len(matrix), len(matrix[0])
        def helper(matrix, nstart, nend, mstart, mend):
            l = []
            if nend>nstart and mend>mstart:
                if nend-nstart==1:
                    return matrix[nstart][mstart:mend]
                if mend-mstart==1:
                    return [matrix[i][mstart] for i in range(nstart, nend)]
                l.extend(matrix[nstart][mstart:mend])
                for i in range(nstart+1, nend-1):
                    l.append(matrix[i][mend-1])
                print(matrix[nend-1][mstart:mend][::-1])
                l.extend(matrix[nend-1][mstart:mend][::-1])
                for i in reversed(range(nstart+1, nend-1)):
                    l.append(matrix[i][mstart])
                l.extend(helper(matrix, nstart+1, nend-1, mstart+1, mend-1))
            return l
         
        return helper(matrix, 0, n, 0, m)
```

改进后：

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        # top bottom left right
        if not matrix or not matrix[0]:
            return []
        res = []
        t, b, l, r = 0, len(matrix)-1, 0, len(matrix[0])-1
        while t<=b and l<=r:
            # 上
            res.extend(matrix[t][l:r+1])
            t += 1
            # 右
            if t<=b and l<=r:
                res.extend([matrix[i][r] for i in range(t, b+1)])
                r -= 1
            # 下
            if t<=b and l<=r:
                res.extend(matrix[b][l:r+1][::-1])
                b -= 1
            # 左
            if t<=b and l<=r:
                res.extend([matrix[i][l] for i in reversed(range(t, b+1))])
                l += 1
        return res
```

