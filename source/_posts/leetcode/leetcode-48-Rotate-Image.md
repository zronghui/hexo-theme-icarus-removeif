---
title: leetcode 48. Rotate Image
date: 2019-11-27 09:28:44
categories:
- leetcode
toc: true
tags:
- Array
---
### 难度：Middle

<a href="https://leetcode.com/problems/rotate-image/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/rotate-image/">九章</a>
## 题目描述
You are given an _n_ x _n_ 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

**Note:**

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-
place_algorithm), which means you have to modify the input 2D matrix directly.
**DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**
        
    Given **input matrix** = 
    [
      [1,2,3],
      [4,5,6],
      [7,8,9]
    ],
    
    rotate the input matrix **in-place** such that it becomes:
    [
      [7,4,1],
      [8,5,2],
      [9,6,3]
    ]
    

**Example 2:**
        
    Given **input matrix** =
    [
      [ 5, 1, 9,11],
      [ 2, 4, 8,10],
      [13, 3, 6, 7],
      [15,14,12,16]
    ], 
    
    rotate the input matrix **in-place** such that it becomes:
    [
      [15,13, 2, 5],
      [14, 3, 4, 1],
      [12, 6, 8, 9],
      [16, 7,10,11]
    ]
    


**Tags:** Array

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public void rotate(int[][] m) {
        // goal: (i, j) --> (j, n-1-i)
        // step 1: (i, j) --> (j, i)
        // step 2: (j, i) -- > (j, n-1-i)
        int n = m.length;
        // 下三角与上三角互换
        for(int i=0; i<n; i++) {
            for(int j=0; j<=i; j++) {
                int temp = m[i][j];
                m[i][j] = m[j][i];
                m[j][i] = temp;
            }
        }
        
        // 逆转每一行
        for(int r=0; r<n; r++) {
            for(int i=0, j=n-1; i<j; i++, j--){
                int temp = m[r][i];
                m[r][i] = m[r][j];
                m[r][j] = temp;
            }
        }
    }
}
```
