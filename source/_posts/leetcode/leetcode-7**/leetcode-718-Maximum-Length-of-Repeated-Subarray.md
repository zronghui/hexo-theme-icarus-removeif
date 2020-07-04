---
thumbnail:
title: leetcode 718. Maximum Length of Repeated Subarray
date: 2020-07-01 09:33:32
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- Array
- Hash Table
- Binary Search
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-01 09:33:32/"leetcode 718. Maximum Length of Repeated Subarray".html'
---

<a href="https://leetcode.com/problems/maximum-length-of-repeated-subarray/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/maximum-length-of-repeated-subarray/">九章</a>
## 题目描述
Given two integer arrays `A` and `B`, return the maximum length of an subarray
that appears in both arrays.

**Example 1:**
        
    Input:
    A: [1,2,3,2,1]
    B: [3,2,1,4,7]
    Output: 3
    Explanation: 
    The repeated subarray with maximum length is [3, 2, 1].




**Note:**

  1. 1 <= len(A), len(B) <= 1000
  2. 0 <= A[i], B[i] < 100




**Tags:** Array, Hash Table, Binary Search, Dynamic Programming

**Difficulty:** Medium

## 答案
<!--more-->

[370，最长公共子串和子序列](https://mp.weixin.qq.com/s/XJyujBI5nofVE9CUbStemA)



```Python
class Solution:
    def findLength(self, A: List[int], B: List[int]) -> int:
        n, m = len(A), len(B)
        res = 0
        dp = [[0]*(m+1) for _ in range(n+1)]
        # n = 3 m=5 A:[3, 2, 1]  B:[4, 7, 3, 2, 1]
        #   [1, 7, 3, 2, 1]
        # 3: 0  0  1  0  0
        # 2: 0  0  0  2  0
        # 1: 1  0  0  0  3
        for i in range(1, n+1):
            for j in range(1, m+1):
                if A[i-1]==B[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                    res = max(res, dp[i][j])
                else:
                    dp[i][j] = 0
        return res

```



```python
class Solution:
    def findLength(self, A: List[int], B: List[int]) -> int:
        n, m = len(A), len(B)
        res = 0
        dp = [0]*(m+1)
        # n = 3 m=5 A:[3, 2, 1]  B:[4, 7, 3, 2, 1]
        #   [1, 7, 3, 2, 1]
        # 3: 0  0  1  0  0
        # 2: 0  0  0  2  0
        # 1: 1  0  0  0  3
        for i in range(1, n+1):
            for j in reversed(range(1, m+1)):
                if A[i-1]==B[j-1]:
                    dp[j] = dp[j-1]+1
                    res = max(res, dp[j])
                else:
                    dp[j] = 0
        return res
```

