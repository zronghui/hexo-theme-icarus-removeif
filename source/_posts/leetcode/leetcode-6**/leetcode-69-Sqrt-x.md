---
title: leetcode 69. Sqrt(x)
date: 2019-12-03 22:31:05
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Math
- Binary Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/sqrtx/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/sqrtx/">九章</a>
## 题目描述
Implement `int sqrt(int x)`.

Compute and return the square root of _x_ , where  _x_  is guaranteed to be a
non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only
the integer part of the result is returned.

**Example 1:**
        
    Input: 4
    Output: 2
    

**Example 2:**
        
    Input: 8
    Output: 2
    Explanation: The square root of 8 is 2.82842..., and since 
                 the decimal part is truncated, 2 is returned.
    


**Tags:** Math, Binary Search

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public int mySqrt(int x) {
        // 二分
        if(x<2) return x;
        int start = 1, end = x, mid = end/2;
        while(start < mid) {
            if(mid == x/mid) return mid;
            else if(mid < x/mid) start = mid;
            else end = mid;
            mid = start+(end-start)/2;
        }
        return mid;
    }
}
```
