---
title: weekly-contest-186
toc: true
recommend: 1
uniqueId: '2020-04-27 01:26:01/"weekly-contest-186".html'
date: 2020-04-27 09:26:01
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [分割字符串的最大得分](https://leetcode-cn.com/contest/weekly-contest-186/problems/maximum-score-after-splitting-a-string/)**3**
- [x] [可获得的最大点数](https://leetcode-cn.com/contest/weekly-contest-186/problems/maximum-points-you-can-obtain-from-cards/)**4**
- [ ] [对角线遍历 II](https://leetcode-cn.com/contest/weekly-contest-186/problems/diagonal-traverse-ii/)**5**
- [ ] [带限制的子序列和](https://leetcode-cn.com/contest/weekly-contest-186/problems/constrained-subset-sum/)**6**

<!--more-->



# 1

```python
class Solution:
    def maxScore(self, s: str) -> int:
        # 1 的总个数
        n1 = 0
        for i in s:
            if i=='1':
                n1 += 1
        # 最大值
        m = 0
        # 当前 0 的个数
        n0 = 0
        for i in range(len(s)-1):
            if s[i]=='0':
                n0 += 1
            else:
                n1 -= 1
            m = max(n0+n1, m)
        return m
        
```

# 2

```python
class Solution:
    def maxScore(self, nums: List[int], k: int) -> int:
        # 问题简化：nums 两边共 k 个数总共最大和是多少
        m = sum(nums[:k])
        n = len(nums)
        lastsum = m
        for i in reversed(range(k)):
            j = k-i
            # print('j', j, nums[n-j])
            # print('i', i, nums[i])
            lastsum = lastsum-nums[i]+nums[n-j]
            m = max(m, lastsum)
            print(m)
        return m
        
```

# 3

[每个数组元素的位置决定了它在最后结果的位置 - 对角线遍历 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/diagonal-traverse-ii/solution/mei-ge-shu-zu-yuan-su-de-wei-zhi-jue-ding-liao-ta-/)

优先从机器角度考虑

```python
class Solution:
    # 便于机器内存切换
    def findDiagonalOrder(self, nums: List[List[int]]) -> List[int]:
        res = []
        # 总共对角线数量
        m = 0
        n = len(nums)
        for i in range(n):
            m = max(m, i+len(nums[i]))
        res = [[] for _ in range(m)]
        for i in range(n):
            for j in range(len(nums[i])):
                res[i+j].append(nums[i][j])
        result = []
        for i in res:
            result.extend(i[::-1])
        return result
    
    # 超时：便于人类思考
    def findDiagonalOrder1(self, nums: List[List[int]]) -> List[int]:
        res = []
        # 总共对角线数量
        m = 0
        n = len(nums)
        length_map = {}
        for i in range(n):
            length_map[i] = len(nums[i])
            m = max(m, i+length_map[i])
        # 第 j 条对角线
        for j in range(m):
            # 第 i 个数字
            for i in range(j+1):
                _j = j-i
                if _j>=n or i>=length_map[_j]:
                    continue
                res.append(nums[_j][i])
                # print(res)
        return res
        
```

# 4

[DP+单调栈优化 详解 - 带限制的子序列和 - 力扣（LeetCode）](https://leetcode-cn.com/problems/constrained-subset-sum/solution/dpdan-diao-zhan-you-hua-xiang-jie-by-wangdh15/)

```python
class Solution:
    def constrainedSubsetSum(self, nums: List[int], k: int) -> int:
        # 1. 定义状态dp[i]为以i结尾的的最大子序和，那么当考虑第i+1个的时候，由于向量两个小标差距不大于k且非空，所以有以下状态转移方程
        # for j in range(k):
        #    dp[i+1] = max(dp[i+1], nums[i+1]+dp[i+1-j], nums[i+1])
        # 2. 优化：维护前 k 个 dp 的最大值
        # https://leetcode-cn.com/problems/sliding-window-maximum/
        # dp[i+1] = max( max(dp[i+1-j] for j in range(k)), 0 )+nums[i+1]
        n = len(nums)
        dp = [0 for _ in range(n)]
        dp[0] = nums[0]
        res = dp[0]
        s = [(dp[0], 0)]
        for i in range(1, n):
            dp[i] = max(nums[i], nums[i]+s[0][0])
            while s and s[-1][0]<=dp[i]:
                s.pop()
            s.append((dp[i], i))
            if s[0][1]<=i-k:
                s.pop(0)
            res = max(res, dp[i])
        return res
        
```

