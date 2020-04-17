---
title: leetcode 73. Set Matrix Zeroes
date: 2019-12-02 10:34:20
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- Array
---
### 难度：Middle

<a href="https://leetcode.com/problems/set-matrix-zeroes/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/set-matrix-zeroes/">九章</a>
## 题目描述
Given a _m_ x _n_ matrix, if an element is 0, set its entire row and column to
0. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**
        
    Input: 
    [
      [1,1,1],
      [1,0,1],
      [1,1,1]
    ]
    Output: 
    [
      [1,0,1],
      [0,0,0],
      [1,0,1]
    ]
    

**Example 2:**
        
    Input: 
    [
      [0,1,2,0],
      [3,4,5,2],
      [1,3,1,5]
    ]
    Output: 
    [
      [0,0,0,0],
      [0,4,5,0],
      [0,3,1,0]
    ]
    

**Follow up:**

  * A straight forward solution using O( _m_ _n_ ) space is probably a bad idea.
  * A simple improvement uses O( _m_ \+ _n_ ) space, but still not the best solution.
  * Could you devise a constant space solution?


**Tags:** Array

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public void setZeroes(int[][] ma) {
        Set<Integer> row = new HashSet<>();
        Set<Integer> col = new HashSet<>();
        int n = ma.length;
        int m = ma[0].length;
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(ma[i][j]==0){
                    row.add(i);
                    col.add(j);
                }
            }
        }
        for(int i: row){
            for(int j=0; j<m; j++) {
                ma[i][j] = 0;
            }
        }
        for(int j: col){
            for(int i=0; i<n; i++) {
                ma[i][j] = 0;
            }
        }
    }
}
```
