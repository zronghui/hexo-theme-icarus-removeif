---
thumbnail:
title: leetcode 62. Unique Paths
date: 2020-07-06 09:05:57
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Array
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-06 09:05:57/"leetcode 62. Unique Paths".html'
---

<a href="https://leetcode.com/problems/unique-paths/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/unique-paths/">九章</a>
## 题目描述
A robot is located at the top-left corner of a _m_ x _n_ grid (marked 'Start'
in the diagram below).

The robot can only move either down or right at any point in time. The robot
is trying to reach the bottom-right corner of the grid (marked 'Finish' in the
diagram below).

How many possible unique paths are there?

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)  
Above is a 7 x 3 grid. How many possible unique paths are there?

**Note:** _m_ and _n_ will be at most 100.

**Example 1:**
        
    Input: m = 3, n = 2
    Output: 3
    Explanation:
    From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
    1. Right -> Right -> Down
    2. Right -> Down -> Right
    3. Down -> Right -> Right


**Example 2:**
        
    Input: m = 7, n = 3
    Output: 28


**Tags:** Array, Dynamic Programming

**Difficulty:** Medium

## 答案
<!--more-->
```java
/*
 * @lc app=leetcode id=62 lang=java
 *
 * [62] Unique Paths
 */
class Solution {
    public int uniquePaths(int m, int n) {
        // f[m, n] = f[m, n-1] + f[m-1, n]
        // f[0, 0] = 0 不重要，但可以设为1，为了方便
        // f[m=0, n] = 1
        // f[m, n=0] = 1
        // 求f[m-1, n-1]
        int [][] f = new int[m][n];
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(i==0 || j==0) {
                    f[i][j] = 1;
                }else{
                    f[i][j] = f[i-1][j]+f[i][j-1];
                }
            }
        }
        return f[m-1][n-1];
    }
}

```



```python

class Solution:
    @functools.lru_cache
    def uniquePaths(self, m: int, n: int) -> int:
        if 1 in [m, n]:
            return 1
        return self.uniquePaths(m-1, n)+self.uniquePaths(m, n-1)

```

