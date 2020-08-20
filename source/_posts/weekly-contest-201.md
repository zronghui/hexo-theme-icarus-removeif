---
title: weekly-contest-201
toc: true
recommend: 1
uniqueId: '2020-08-09 08:13:35/"weekly-contest-201".html'
date: 2020-08-09 16:13:35
thumbnail:
categories:
tags:
keywords:
---

[TOC]

- [x] [整理字符串](https://leetcode-cn.com/problems/make-the-string-great/)**3**
- [x] [找出第 N 个二进制字符串中的第 K 位](https://leetcode-cn.com/problems/find-kth-bit-in-nth-binary-string/)**4**
- [ ] [和为目标值的最大数目不重叠非空子数组数目](https://leetcode-cn.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/)**6**
- [ ] [切棍子的最小成本](https://leetcode-cn.com/problems/minimum-cost-to-cut-a-stick/)**7**

公开处刑

![image-20200809191851010](https://i.loli.net/2020/08/09/X9RbtwhnNxVi86q.png)

<!--more-->

[[LeetCode] Weekly Contest 201 (rank 50)[1544,1545,1546,1547][OTTFF]_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/av754126490)

解释的很好

# 1

```python
class Solution:
    def makeGood(self, s: str) -> str:
        stack = []
        t = abs(ord('a')-ord('A'))
        for c in s:
            if stack and abs(ord(c)-ord(stack[-1]))==t: stack.pop()
            else: stack.append(c)
        return ''.join(stack)
```

# 2

```python
class Solution:
    def __init__(self):
        self.d = {1:1}
        for i in range(2, 21):
            self.d[i] = self.d[i-1]*2+1

    def findKthBit(self, n: int, k: int) -> str:
        if n==k==1: return '0'
        a = self.d[n]//2 # length = 2a+1
        if k==a+1: return '1'
        if k>a: return '10'[int(self.findKthBit(n-1, 2*a-k+2))]
        if k<=a: return self.findKthBit(n-1, k)
```


# 3

```python
class Solution:
    def maxNonOverlapping(self, nums: List[int], target: int) -> int:
        # -1 3 5 1 4 2 -9
        # -1 2 7 8 12 14 5
        n = len(nums)
        dp = [0 for i in range(n+2)] # 前 i 个数最多有多少个符合条件的
        presum = {0: -2} # presum: i  0~i 的和为 presum ，i 保留较大的
        s = 0        
        for i in range(1, n+1):
            s += nums[i-1]
            if s-target in presum:
                p = presum[s-target]
                dp[i] = max(dp[i-1], dp[p+1]+1)
            else:
                dp[i] = dp[i-1]
            presum[s] = i-1
        # print(presum, dp)
        return dp[-2]
```


# 4

```python
class Solution:
    def minCost(self, n: int, cuts: List[int]) -> int:
        cuts.extend([0, n])
        cuts.sort()
        m = len(cuts)
        # dp[i][j] cut[i]~cut[j] 间最低成本
        # dp[i][j] = min(dp[i][k]+dp[k][j]+(cuts[j]-cuts[i]))  i<k<j
        # 要求 dp[i][j] 需要 dp[i][k] dp[k][j] 特点： 长度比 j-i 短
        # 所以遍历顺序 从短到长 j-i<2 时 dp 为 0  如 0 1 2 3 中 dp[1][1]=dp[1][2]=0
        dp = [[sys.maxsize]*m for i in range(m)]
        for i in range(m):
            dp[i][i] = 0
            if i>0: dp[i][i-1] = 0
            if i<m-1: dp[i][i+1] = 0

        for gap in range(2, m):
            for i in range(m-gap):
                j = i+gap
                lenij = cuts[j]-cuts[i]
                for k in range(i+1, j):
                    dp[i][j] = min(dp[i][j], dp[i][k]+dp[k][j]+lenij)
        return dp[0][m-1]
            
```

