---
title: leetcode 1143. Longest Common Subsequence
date: 2019-12-01 00:21:48
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Dynamic Programming
---
### 难度：Middle

<a href="https://leetcode.com/problems/longest-common-subsequence/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/longest-common-subsequence/">九章</a>
## 题目描述
Given two strings `text1` and `text2`, return the length of their longest
common subsequence.

A _subsequence_ of a string is a new string generated from the original string
with some characters(can be none) deleted without changing the relative order
of the remaining characters. (eg, "ace" is a subsequence of "abcde" while
"aec" is not). A _common subsequence_  of two strings is a subsequence that is
common to both strings.



If there is no common subsequence, return 0.



**Example 1:**
            Input: text1 = "abcde", text2 = "ace"     Output: 3      Explanation: The longest common subsequence is "ace" and its length is 3.    

**Example 2:**
            Input: text1 = "abc", text2 = "abc"    Output: 3    Explanation: The longest common subsequence is "abc" and its length is 3.    

**Example 3:**
            Input: text1 = "abc", text2 = "def"    Output: 0    Explanation: There is no such common subsequence, so the result is 0.    



**Constraints:**

  * `1 <= text1.length <= 1000`
  * `1 <= text2.length <= 1000`
  * The input strings consist of lowercase English characters only.


**Tags:** Dynamic Programming

**Difficulty:** Medium
## 答案
[超详细，动态规划解法](https://leetcode-cn.com/problems/longest-common-subsequence/solution/chao-xiang-xi-dong-tai-gui-hua-jie-fa-by-shi-wei-h/)
<!--more-->
```java
class Solution {
    public int longestCommonSubsequence(String s1, String s2) {
        if(s1==null || s1.length()==0) return 0;
        if(s2==null || s2.length()==0) return 0;
        int n = s1.length();
        int m = s2.length();
        // i=0 j=0 是为了防止越界
        int[][] dp = new int[n+1][m+1];
        // 处理边界
        // i=j=1
        dp[1][1] = s1.charAt(0)==s2.charAt(0)?1:0;
        
        for(int i=1; i<=n; i++) {
            for(int j=1; j<=m; j++) {
                //如果末端相同
                if(s1.charAt(i-1)==s2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1]+1;
                }else{
                //如果末端不同
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        return dp[n][m];
    }
}
```
