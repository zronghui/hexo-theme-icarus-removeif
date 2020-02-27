---
title: leetcode 204. Count Primes
date: 2019-11-21 12:21:31
categories:
- leetcode
toc: true
tags:
- Hash Table
- Math
---
## 题目描述
Count the number of prime numbers less than a non-negative number, **_n_**.

**Example:**
        
    Input: 10
    Output: 4
    Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
    


**Tags:** Hash Table, Math

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public int countPrimes(int n) {
        if(n<=2) return 0;
        // 加上 n+1
        boolean[] b = new boolean[n];
        int res = 0;
        for(int i=2; i<n; i++){
            // 是默认值false <-> 是质数
            if(!b[i]) {
                res++;
                // 将质数的整数标记为非质数
                for(int j=2; i*j<n; j++){
                    b[i*j] = true;
                }
            }
        }
        return res;
    }
}
```
