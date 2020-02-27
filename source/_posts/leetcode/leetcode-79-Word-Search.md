---
title: leetcode 79. Word Search
date: 2019-12-04 11:02:07
categories:
- leetcode
toc: true
tags:
- Array
- Backtracking
---
### 难度：Middle

<a href="https://leetcode.com/problems/word-search/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/word-search/">九章</a>
## 题目描述
Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where
"adjacent" cells are those horizontally or vertically neighboring. The same
letter cell may not be used more than once.

**Example:**
        
    board =
    [
      ['A','B','C','E'],
      ['S','F','C','S'],
      ['A','D','E','E']
    ]
    
    Given word = " **ABCCED** ", return **true**.
    Given word = " **SEE** ", return **true**.
    Given word = " **ABCB** ", return **false**.
    


**Tags:** Array, Backtracking

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    // dfs + 标记
    int[][] directions = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    public boolean exist(char[][] board, String word) {
        if(board==null || board.length==0 || board[0].length==0) return false;
        int n = board.length, m = board[0].length;
        boolean[][] mark = new boolean[n][m];
        for(int i=0; i<n; i++) {
            for(int j=0; j<m; j++) {
                if(helper(board, n, m, i, j, word, word.length(), 0, mark))
                    return true;
            }
        }
        return false;
    }
    
    private boolean helper(char[][] board, int n, int m, int i, int j, String word, int len, int start, boolean[][] mark) {
        if(i<0 || i>=n || j<0 || j>=m || mark[i][j]) return false;
        if(word.charAt(start)!=board[i][j]) 
            return false;
        if(start==len-1) return true;
        mark[i][j] = true;
        for(int[] direction: directions) {
            if (helper(board, n, m, i+direction[0], j+direction[1], word, len, start+1, mark))
                return true;
        }
        mark[i][j] = false;
        return false;
    }
}
```
