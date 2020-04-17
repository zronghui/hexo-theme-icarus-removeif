---
title: leetcode 344. Reverse String
date: 2019-11-25 09:36:57
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Two Pointers
- String
---
### 难度：Easy

<a href="https://leetcode.com/problems/reverse-string/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/reverse-string/">九章</a>
## 题目描述
Write a function that reverses a string. The input string is given as an array
of characters `char[]`.

Do not allocate extra space for another array, you must do this by **modifying
the input array  [in-place](https://en.wikipedia.org/wiki/In-
place_algorithm)** with O(1) extra memory.

You may assume all the characters consist of [printable ascii
characters](https://en.wikipedia.org/wiki/ASCII#Printable_characters).



**Example 1:**
        
    Input: ["h","e","l","l","o"]
    Output: ["o","l","l","e","h"]
    

**Example 2:**
        
    Input: ["H","a","n","n","a","h"]
    Output: ["h","a","n","n","a","H"]
    


**Tags:** Two Pointers, String

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public void reverseString(char[] s) {
        if(s==null || s.length==0) return;
        int i = 0, j = s.length-1;
        while(i<j){
            char t = s[i];
            s[i] = s[j];
            s[j] = t;
            i++;
            j--;
        }
    }
}
```
