---
title: leetcode 171. Excel Sheet Column Number
date: 2019-11-27 09:12:17
categories:
- leetcode
toc: true
tags:
- Math
---
### 难度：Easy

<a href="https://leetcode.com/problems/excel-sheet-column-number/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/excel-sheet-column-number/">九章</a>
## 题目描述
Given a column title as appear in an Excel sheet, return its corresponding
column number.

For example:
        
        A -> 1
        B -> 2
        C -> 3
        ...
        Z -> 26
        AA -> 27
        AB -> 28 
        ...
    

**Example 1:**
        
    Input: "A"
    Output: 1
    

**Example 2:**
        
    Input: "AB"
    Output: 28
    

**Example 3:**
        
    Input: "ZY"
    Output: 701
    


**Tags:** Math

**Difficulty:** Easy
## 答案
<!--more-->
```java
// 用时 5 min
// char to int: int a = c;
class Solution {
    public int titleToNumber(String s) {
        int res = 0;
        char[] cs = s.toCharArray();
        int n = cs.length;
        for(int i=0; i<n; i++) {
            int ascii = cs[i];
            res += (ascii-64)*(int)Math.pow(26, n-i-1);
        }
        return res;
    }
}
```
