---
thumbnail:
title: leetcode 120. Triangle
date: 2020-07-14 08:04:19
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Array
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-14 08:04:19/"leetcode 120. Triangle".html'
---

<a href="https://leetcode.com/problems/triangle/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/triangle/">九章</a>
## 题目描述
Given a triangle, find the minimum path sum from top to bottom. Each step you
may move to adjacent numbers on the row below.

For example, given the following triangle
        
    [
         [ **2** ],
        [ **3** ,4],
       [6, **5** ,7],
      [4, **1** ,8,3]
    ]


The minimum path sum from top to bottom is `11` (i.e., **2** \+ **3** \+ **5**
\+ **1** = 11).

**Note:**

Bonus point if you are able to do this using only _O_ ( _n_ ) extra space,
where _n_ is the total number of rows in the triangle.


**Tags:** Array, Dynamic Programming

**Difficulty:** Medium

## 答案
<!--more-->

[自底向上动态规划，类似于从起点到目的地之间的最短路径 - 三角形最小路径和 - 力扣（LeetCode）](https://leetcode-cn.com/problems/triangle/solution/zi-di-xiang-shang-dong-tai-gui-hua-lei-si-yu-cong-/)

```python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        if len(triangle)==1:
            return triangle[0][0]
        for i in reversed(range(len(triangle)-1)):
            for j in range(len(triangle[i])):
                triangle[i][j] += min(triangle[i+1][j:j+2])
        return triangle[0][0]
```
