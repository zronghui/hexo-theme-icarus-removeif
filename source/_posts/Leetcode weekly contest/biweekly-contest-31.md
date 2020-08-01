---
title: biweekly-contest-31
toc: true
recommend: 1
uniqueId: '2020-07-25 15:16:09/"biweekly-contest-31".html'
date: 2020-07-25 23:16:09
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]



- [x] [在区间范围内统计奇数数目](https://leetcode-cn.com/contest/biweekly-contest-31/problems/count-odd-numbers-in-an-interval-range/)**3**
- [x] [和为奇数的子数组数目](https://leetcode-cn.com/contest/biweekly-contest-31/problems/number-of-sub-arrays-with-odd-sum/)**4**
- [x] [字符串的好分割数目](https://leetcode-cn.com/contest/biweekly-contest-31/problems/number-of-good-ways-to-split-a-string/)**5**
- [x] [形成目标数组的子数组最少增加次数](https://leetcode-cn.com/contest/biweekly-contest-31/problems/minimum-number-of-increments-on-subarrays-to-form-a-target-array/)**7**

![image-20200725232234484](https://i.loli.net/2020/07/25/k5nfsqeoTUwzDrQ.png)

<!--more-->

5分多钟写完是不是太变态了

# 1

```python
class Solution:
    def countOdds(self, low: int, high: int) -> int:
        # 1234： 2  123:2  234:1 23: 1
        if low==high:
            if low&1: return 1
            return 0
        if not low&1: low += 1
        if not high&1: high -= 1
        return (high-low)//2+1
        
```

# 2

忘记取模，错了一次

```python
class Solution:
    def numOfSubarrays(self, arr: List[int]) -> int:
        #          1 3 5 7
        # presum 0 1 4 9 16
        # oddNum 0 1 1 2 2  oddnum 表示 presum 中到当前位置奇数的数目
        #evenNum 1 1 2 2 3
        # presum 是奇数，取前一个 evennum, 否则取 oddnum
        #          1+1+2+2
        # 遍历过程中只用到前一个遍历，可以简化数组
        presum = 0
        odd, even = 0, 1
        res = 0
        for i in arr:
            presum += i
            if presum&1:
                res += even
                odd += 1
            else:
                res += odd
                even += 1
        return res%(10**9+7)

```


# 3

```python
class Solution:
    def numSplits(self, s: str) -> int:
        c = collections.Counter(s)
        left, right = 0, len(set(s))
        lefts = set()
        res = 0
        for i in s:
            c[i] -= 1
            if i not in lefts:
                left += 1
                lefts.add(i)
            if c[i]==0:
                right -= 1
            if left==right: res += 1
        return res

```

# 4

if nums[i]<=nums[i-1]: dp[i] = dp[i-1]

else: dp[i] = dp[i-1]+nums[i]-nums[i-1]

简化 dp 数组后:

```python
class Solution:
    def minNumberOperations(self, target: List[int]) -> int:
        # dp
        prenum = 0
        dp = 0
        for num in target:
            if num>prenum: dp += num-prenum
            prenum = num
        return dp
```

