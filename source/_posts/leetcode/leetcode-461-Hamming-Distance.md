---
title: leetcode 461. Hamming Distance
date: 2019-12-04 16:51:06
categories:
- leetcode
toc: true
tags:
- Bit Manipulation
---
### 难度：Easy

<a href="https://leetcode.com/problems/hamming-distance/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/hamming-distance/">九章</a>
## 题目描述
The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between
two integers is the number of positions at which the corresponding bits are
different.

Given two integers `x` and `y`, calculate the Hamming distance.

**Note:**  
0 ≤ `x`, `y` < 231.

**Example:**
        
    Input: x = 1, y = 4
    
    Output: 2
    
    Explanation:
    1   (0 0 0 1)
    4   (0 1 0 0)
           ↑   ↑
    
    The above arrows point to positions where the corresponding bits are different.
    


**Tags:** Bit Manipulation

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public int hammingDistance(int x, int y) {
        int res = 0;
        while(!(x==y || (x==0 && y==0))){
            int a = x%2, b = y%2;
            if(a!=b)
                res++;
            x>>=1;
            y>>=1;
        }
        return res;
    }
}
class Solution {
    public int hammingDistance(int x, int y) {
        int res = 0;
        int n = x^y;
        while(n!=0){
            res++;
            // 去除最后一个1
            n = n & (n-1);
        }
        return res;
    }
}
```
