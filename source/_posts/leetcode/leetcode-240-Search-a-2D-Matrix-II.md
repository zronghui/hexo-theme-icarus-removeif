---
title: leetcode 240. Search a 2D Matrix II
date: 2019-12-02 10:16:42
categories:
- leetcode
toc: true
tags:
- Binary Search
- Divide and Conquer
---
### 难度：Middle

<a href="https://leetcode.com/problems/search-a-2d-matrix-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/search-a-2d-matrix-ii/">九章</a>
## 题目描述
Write an efficient algorithm that searches for a value in an _m_ x _n_ matrix.
This matrix has the following properties:

  * Integers in each row are sorted in ascending from left to right.
  * Integers in each column are sorted in ascending from top to bottom.

**Example:**

Consider the following matrix:
        
    [
      [1,   4,  7, 11, 15],
      [2,   5,  8, 12, 19],
      [3,   6,  9, 16, 22],
      [10, 13, 14, 17, 24],
      [18, 21, 23, 26, 30]
    ]
    

Given target = `5`, return `true`.

Given target = `20`, return `false`.


**Tags:** Binary Search, Divide and Conquer

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public boolean searchMatrix(int[][] ma, int t) {
        if(ma==null || ma.length==0 || ma[0].length==0) return false;
        int n = ma.length;
        int m = ma[0].length;
        int i=n-1, j=0;
        while(i>=0 && j<m) {
            if(ma[i][j]==t){
                return true;
            }else if(ma[i][j]>t){
                i--;
            }else{
                j++;
            }
        }
        return false;
    }
}
```
