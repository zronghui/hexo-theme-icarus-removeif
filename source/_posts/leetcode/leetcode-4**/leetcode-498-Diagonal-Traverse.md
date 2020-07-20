---
thumbnail:
title: leetcode 498. Diagonal Traverse
date: 2020-07-20 23:12:44
categories:
- leetcode
- leetcode-4**
toc: true
tags:
- 
recommend: 1
keywords:
uniqueId: '2020-07-20 23:12:44/"leetcode 498. Diagonal Traverse".html'
---

<a href="https://leetcode.com/problems/diagonal-traverse/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/diagonal-traverse/">九章</a>
## 题目描述
Given a matrix of M x N elements (M rows, N columns), return all elements of
the matrix in diagonal order as shown in the below image.



**Example:**
        
    Input:
    [
     [ 1, 2, 3 ],
     [ 4, 5, 6 ],
     [ 7, 8, 9 ]
    ]
    
    Output:  [1,2,4,7,5,3,6,8,9]
    
    Explanation:
    ![](https://assets.leetcode.com/uploads/2018/10/12/diagonal_traverse.png)




**Note:**

The total number of elements of the given matrix will not exceed 10,000.


**Tags:** 

**Difficulty:** Medium

## 答案
<!--more-->

[498. 对角线遍历 - 力扣（LeetCode）](https://leetcode-cn.com/problems/diagonal-traverse/)
[对象线横列坐标之和相等 - 对角线遍历 - 力扣（LeetCode）](https://leetcode-cn.com/problems/diagonal-traverse/solution/dui-xiang-xian-heng-lie-zuo-biao-zhi-he-xiang-deng/)

```python
class Solution:
    def findDiagonalOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix: return []
        n, m = len(matrix), len(matrix[0])
        if n==1: return matrix[0]
        d = collections.defaultdict(list)
        for i in range(n):
            for j in range(m):
                d[i+j].append(matrix[i][j])
        res = []
        for i in range(m+n-1):
            if i&1: res.extend(d[i])
            else: res.extend(d[i][::-1])
        return res
```
