---
title: leetcode 96. Unique Binary Search Trees
date: 2019-11-22 11:52:23
categories:
- leetcode
- leetcode-9**
toc: true
tags:
- Dynamic Programming
- Tree
---
### 难度：Middle

<a href="https://leetcode.com/problems/unique-binary-search-trees/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/unique-binary-search-trees/">九章</a>
## 题目描述
Given _n_ , how many structurally unique **BST 's** (binary search trees) that
store values 1 ...  _n_?

**Example:**
        
    Input: 3
    Output: 5
    Explanation: Given _n_ = 3, there are a total of 5 unique BST's:
    
       1         3     3      2      1
        \       /     /      / \      \
         3     2     1      1   3      2
        /     /       \                 \
       2     1         2                 3
    


**Tags:** Dynamic Programming, Tree

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int numTrees(int n) {
        //         1为root, 左子树有0个    。。。。   n为root，右子树有0个
        // dp[n] = dp[0]*dp[n-1]+dp[1]*dp[n-2]+...+dp[n-1]*dp[0]
        int[] dp = new int[n+1];
        dp[0] = 1;
        for(int i=1; i<=n; i++){
            for(int j=0; j<i; j++){
                dp[i] += dp[j]*dp[i-j-1];
            }
        }
        return dp[n];
    }
}
```
