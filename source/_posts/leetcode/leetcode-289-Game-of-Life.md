---
title: leetcode 289. Game of Life
date: 2019-11-29 19:30:51
categories:
- leetcode
toc: true
tags:
- Array
---
### 难度：Middle

<a href="https://leetcode.com/problems/game-of-life/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/game-of-life/">九章</a>
## 题目描述
According to the [Wikipedia's
article](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life): "The **Game
of Life** , also known simply as **Life** , is a cellular automaton devised by
the British mathematician John Horton Conway in 1970."

Given a _board_ with _m_ by _n_ cells, each cell has an initial state _live_
(1) or _dead_ (0). Each cell interacts with its [eight
neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood) (horizontal,
vertical, diagonal) using the following four rules (taken from the above
Wikipedia article):

  1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
  2. Any live cell with two or three live neighbors lives on to the next generation.
  3. Any live cell with more than three live neighbors dies, as if by over-population..
  4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board
given its current state. The next state is created by applying the above rules
simultaneously to every cell in the current state, where births and deaths
occur simultaneously.

**Example:**
        
    Input: [
      [0,1,0],
      [0,0,1],
      [1,1,1],
      [0,0,0]
    ]
    Output: [
      [0,0,0],
      [1,0,1],
      [0,1,1],
      [0,1,0]
    ]
    

**Follow up** :

  1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
  2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?


**Tags:** Array

**Difficulty:** Medium
## 答案
<!--more-->
```java
// 二位二进制数表示状态变化， 下一状态_当前状态
// 00: 0 die to die
// 01: 1 live to die
// 10: 2 die to live
// 11: 3 live to live
class Solution {
    private int[][] directions = {{-1, -1}, {-1, 0}, {-1, 1},
                                            {0, -1}, {0, 1}, 
                                            {1, -1}, {1, 0}, {1, 1}
                                           };
    public void gameOfLife(int[][] b) {
        if(b==null || b.length==0) return;
        int n = b.length, m = b[0].length;
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                update(b, n, m, i, j);
            }
        }
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                b[i][j] = b[i][j]>>1;
            }
        }
    }
    private void update(int[][] b, int n, int m, int i, int j){
        int count = 0;
        for(int[] direction: directions){
            int ii = i+direction[0], jj = j+direction[1];
            if(ii<0 || ii>= n || jj<0 || jj>=m) continue;
            count += b[ii][jj]%2==1?1:0;
        }
        if(b[i][j]==1){
            if(count<2 || count>3) b[i][j] = 1;
            else b[i][j] = 3;
            
        }else{
            if(count==3){
                b[i][j] = 2;
            }
        }
    }
}
```
