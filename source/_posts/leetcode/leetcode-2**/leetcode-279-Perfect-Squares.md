---
title: leetcode 279. Perfect Squares
date: 2019-11-30 20:09:05
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Math
- Dynamic Programming
- Breadth-first Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/perfect-squares/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/perfect-squares/">九章</a>
## 题目描述
Given a positive integer _n_ , find the least number of perfect square numbers
(for example, `1, 4, 9, 16, ...`) which sum to _n_.

**Example 1:**
        
    Input: _n_ = 12
    Output: 3 
    Explanation:12 = 4 + 4 + 4.

**Example 2:**
        
    Input: _n_ = 13
    Output: 2
    Explanation:13 = 4 + 9.


**Tags:** Math, Dynamic Programming, Breadth-first Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int numSquares(int n) {
        // 四平方和定理
        // 一个数如果含有因子4，那么我们可以把4都去掉，并不影响结果
        // 一个数除以8余7，那么肯定是由4个完全平方数组成
        while(n%4==0) n/=4;
        if(n%8==7) return 4;
        // dp[i] = min(dp[i], dp[i-j*j]+1) (j*j<i)
        int[] dp = new int[n+1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        for(int i=0; i*i<=n; i++){
            dp[i*i] = 1;
        }
        for(int i=0; i<=n; i++) {
            for(int j=0; i+j*j<=n; j++){
                dp[i+j*j] = Math.min(dp[i]+1, dp[i+j*j]);
            }
        }
        return dp[n];
    }
}
```
