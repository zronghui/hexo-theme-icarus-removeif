---
title: 剑指 Offer 04 二维数组中的查找
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:33/"剑指 Offer 04 二维数组中的查找".html'
date: 2020-06-30 21:54:33
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

[剑指 Offer 04. 二维数组中的查找 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

<!--more-->

1.找一个角落，使得 上下、左右 两个方向 一个正序 一个逆序

2.判断当前元素是否是 target，减少代码的编写（而不是判断下一行、列的元素）

```python
class Solution:
    def findNumberIn2DArray(self, matrix: List[List[int]], target: int) -> bool:
        # [n][m]
        if not matrix or not matrix[0]:
            return False
        n, m = len(matrix), len(matrix[0])
        i, j = 0, m-1
        
        while i<n and j>=0:
            if target==matrix[i][j]:
                return True
            if matrix[i][j]>target:
                j -= 1
            else:
                i += 1
        return False
```

