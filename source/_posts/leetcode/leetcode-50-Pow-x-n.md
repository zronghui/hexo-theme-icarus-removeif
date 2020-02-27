---
title: leetcode 50. Pow(x, n)
date: 2019-12-06 14:41:33
categories:
- leetcode
toc: true
tags:
- Math
- Binary Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/powx-n/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/powx-n/">九章</a>
## 题目描述
Implement [pow( _x_ , _n_
)](http://www.cplusplus.com/reference/valarray/pow/), which calculates  _x_
raised to the power _n_ (xn).

**Example 1:**
        
    Input: 2.00000, 10
    Output: 1024.00000
    

**Example 2:**
        
    Input: 2.10000, 3
    Output: 9.26100
    

**Example 3:**
        
    Input: 2.00000, -2
    Output: 0.25000
    Explanation: 2-2 = 1/22 = 1/4 = 0.25
    

**Note:**

  * -100.0 < _x_ < 100.0
  * _n_ is a 32-bit signed integer, within the range [−231, 231 − 1]


**Tags:** Math, Binary Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public double myPow(double x, int n) {
        if(n==0) return 1;
        if(n==1) return x;
        if(n==-1) return 1/x;
        double half = myPow(x, n/2);
        return half*half*myPow(x, n%2);
    }
};
```
