---
title: leetcode 131. Palindrome Partitioning
date: 2019-12-01 11:31:57
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Backtracking
---
### 难度：Middle

<a href="https://leetcode.com/problems/palindrome-partitioning/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/palindrome-partitioning/">九章</a>
## 题目描述
Given a string _s_ , partition _s_ such that every substring of the partition
is a palindrome.

Return all possible palindrome partitioning of _s_.

**Example:**
        
    Input:  "aab"
    Output:
    [
      ["aa","b"],
      ["a","a","b"]
    ]
    


**Tags:** Backtracking

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res = new ArrayList<>();
        if(s==null || s.length()==0) return res;
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        for(int j=0; j<n; j++) {
            for(int i=0; i<=j; i++) {
                dp[i][j] = s.charAt(i)==s.charAt(j) && (j-i<=2 || dp[i+1][j-1]);
            }
        }
        System.out.println(Arrays.deepToString(dp));
        dfs(res, dp, 0, n, s, new ArrayList<String>());
        return res;
    }
    private void dfs(List<List<String>> res, boolean[][] dp, int i, int n, String s, ArrayList<String> tmp) {
        if(i==n) res.add(new ArrayList<>(tmp));
        for(int j=i; j<n; j++) {
            if(dp[i][j]){
                tmp.add(s.substring(i, j+1));
                dfs(res, dp, j+1, n, s, tmp);
                tmp.remove(tmp.size()-1);
            }
        }
    }
}
```
