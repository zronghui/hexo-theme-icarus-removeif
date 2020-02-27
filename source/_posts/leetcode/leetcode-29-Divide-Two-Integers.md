---
title: leetcode 29. Divide Two Integers
date: 2019-11-25 08:21:05
categories:
- leetcode
toc: true
tags:
- Math
- Binary Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/divide-two-integers/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/divide-two-integers/">九章</a>
## 题目描述
Given two integers `dividend` and `divisor`, divide two integers without using
multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero.

**Example 1:**
        
    Input: dividend = 10, divisor = 3
    Output: 3

**Example 2:**
        
    Input: dividend = 7, divisor = -3
    Output: -2

**Note:**

  * Both dividend and divisor will be 32-bit signed integers.
  * The divisor will never be 0.
  * Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.


**Tags:** Math, Binary Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int divide(int dividend, int divisor) {
        // a/b = c
        // c*b <= a <= (c+1)*b
        
        if(divisor==0) return dividend>=0?Integer.MAX_VALUE:Integer.MIN_VALUE;
        if(dividend==0) return 0;
        // -2147483648 / -1 
        if(dividend==Integer.MIN_VALUE && divisor==-1) return Integer.MAX_VALUE;
        if(dividend==Integer.MIN_VALUE && divisor==1) return Integer.MIN_VALUE;
        boolean sign = (dividend>0&&divisor>0) || (dividend<0&&divisor<0);
        long a = Math.abs((long)dividend);
        long b = Math.abs((long)divisor);
        long result = 0;
        while(a>=b){
            int shift = 0;
            while(a>(b<<shift)){
                shift++;
            }
            if(shift>1){
                a -= b<<(shift-1);
                result += 1<<(shift-1);
            }else{
                a -= b;
                result += 1;
            }
        }
        if(result>Integer.MAX_VALUE) return Integer.MAX_VALUE;
        return sign?(int)result:(int)-result;
    }
}
```
