---
title: leetcode 202. Happy Number
date: 2019-11-21 12:21:31
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Hash Table
- Math
---
## 题目描述
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any
positive integer, replace the number by the sum of the squares of its digits,
and repeat the process until the number equals 1 (where it will stay), or it
loops endlessly in a cycle which does not include 1. Those numbers for which
this process ends in 1 are happy numbers.

**Example:  **
        
    Input: 19
    Output: true
    Explanation: 12 + 92 = 82
    82 + 22 = 68
    62 + 82 = 100
    12 + 02 + 02 = 1
    


**Tags:** Hash Table, Math

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet<>();
        while(n!=1){
            if(!set.add(n)){
                return false;
            }
            n = trans(n);
        }
        return true;
    }
    private int trans(int n){
        int res = 0;
        while(n!=0){
            res += (int)Math.pow(n%10, 2);
            n = n/10;
        }
        return res;
    }
    
}
```
