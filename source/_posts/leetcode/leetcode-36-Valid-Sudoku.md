---
title: leetcode 36. Valid Sudoku
date: 2019-11-25 08:49:49
categories:
- leetcode
toc: true
tags:
- Hash Table
---
### 难度：Middle

<a href="https://leetcode.com/problems/valid-sudoku/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/valid-sudoku/">九章</a>
## 题目描述
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be
validated  **according to the following rules** :

  1. Each row must contain the digits `1-9` without repetition.
  2. Each column must contain the digits `1-9` without repetition.
  3. Each of the 9 `3x3` sub-boxes of the grid must contain the digits `1-9` without repetition.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-
by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)  
A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with
the character `'.'`.

**Example 1:**
        
    Input:
    [
      ["5","3",".",".","7",".",".",".","."],
      ["6",".",".","1","9","5",".",".","."],
      [".","9","8",".",".",".",".","6","."],
      ["8",".",".",".","6",".",".",".","3"],
      ["4",".",".","8",".","3",".",".","1"],
      ["7",".",".",".","2",".",".",".","6"],
      [".","6",".",".",".",".","2","8","."],
      [".",".",".","4","1","9",".",".","5"],
      [".",".",".",".","8",".",".","7","9"]
    ]
    Output: true
    

**Example 2:**
        
    Input:
    [
      ["8","3",".",".","7",".",".",".","."],
      ["6",".",".","1","9","5",".",".","."],
      [".","9","8",".",".",".",".","6","."],
      ["8",".",".",".","6",".",".",".","3"],
      ["4",".",".","8",".","3",".",".","1"],
      ["7",".",".",".","2",".",".",".","6"],
      [".","6",".",".",".",".","2","8","."],
      [".",".",".","4","1","9",".",".","5"],
      [".",".",".",".","8",".",".","7","9"]
    ]
    Output: false
    Explanation: Same as Example 1, except with the **5** in the top left corner being 
        modified to **8**. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
    

**Note:**

  * A Sudoku board (partially filled) could be valid but is not necessarily solvable.
  * Only the filled cells need to be validated according to the mentioned rules.
  * The given board contain only digits `1-9` and the character `'.'`.
  * The given board size is always `9x9`.


**Tags:** Hash Table

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public boolean isValidSudoku(char[][] cs) {
        HashSet<Character> row = new HashSet<>();
        HashSet<Character> col = new HashSet<>();
        HashSet<Character> cube = new HashSet<>();
        for(int i=0; i<9; i++){
            row.clear();
            col.clear();
            cube.clear();
            for(int j=0; j<9; j++){
                if(cs[i][j]!='.' && !row.add(cs[i][j])) return false;
                if(cs[j][i]!='.' && !col.add(cs[j][i])) return false;
                // i 确定 是哪个cube
                // 0 1 2
                // 3 4 5
                // 6 7 8
                // rowIndex colIndex 起始为cube左上角坐标
                int rowIndex = 3*(i/3);
                int colIndex = 3*(i%3);
                // j 确定是cube的哪个点
                int rowi = rowIndex+j/3;
                int coli = colIndex+j%3;
                if(cs[rowi][coli]!='.' && !cube.add(cs[rowi][coli])) return false;
            }
        }
        return true;
    }
}
```
