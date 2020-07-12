---
title: biweekly-contest-30
toc: true
recommend: 1
uniqueId: '2020-07-12 05:46:55/"biweekly-contest-30".html'
date: 2020-07-12 13:46:55
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [转变日期格式](https://leetcode-cn.com/contest/biweekly-contest-30/problems/reformat-date/)**3**
- [x] [子数组和排序后的区间和](https://leetcode-cn.com/contest/biweekly-contest-30/problems/range-sum-of-sorted-subarray-sums/)**4**
- [x] [三次操作后最大值与最小值的最小差](https://leetcode-cn.com/contest/biweekly-contest-30/problems/minimum-difference-between-largest-and-smallest-value-in-three-moves/)**5**
- [x] [石子游戏 IV](https://leetcode-cn.com/contest/biweekly-contest-30/problems/stone-game-iv/)**6**

<!--more-->



# 1

```python
class Solution:
    def reformatDate(self, date: str) -> str:
        day, month, year = date.split()
        day = day[:-2]
        month = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"].index(month)+1
        return '{}-{}-{}'.format(year, str(month).zfill(2), day.zfill(2))
```

# 2

```python
class Solution:
    def rangeSum(self, nums: List[int], n: int, left: int, right: int) -> int:
        _sum = [0 for _ in range(n+1)]
        _sum[0], _sum[1] = 0, nums[0]
        for i in range(1, n):
            _sum[i+1] += _sum[i]+nums[i]
        l = [_sum[j]-_sum[i] for i in range(n+1) for j in range(i+1, n+1)]
        l.sort()
        return sum(l[left-1:right]) % (10**9 + 7)
```


# 3

```python
class Solution:
    def minDifference(self, nums: List[int]) -> int:
        n = len(nums)
        if n<5:
            return 0
        # 思路: 取 4 个最小值，4 个最大值，最多 8 个元素时，可以暴力解法
        if n<9:
            nums.sort()
            # 1 2 3 4 5   -> 5-4(删除 123), 4-3(删除 125), 3-2(删除 145), 2-1(删除 345)
            n = len(nums)
            return min(nums[n-1]-nums[3], nums[n-2]-nums[2], nums[n-3]-nums[1], nums[n-4]-nums[0])
        else:
            l = []
            for i in range(4): # 取 4 个最小值
                mini = i
                for j in range(i, n):
                    if nums[j]<nums[mini]:
                        mini = j
                nums[mini], nums[i] = nums[i], nums[mini]
                l.append(nums[i])
            for i in range(4): # 4 个最大值
                mini = i
                for j in range(i, n):
                    if nums[j]>nums[mini]:
                        mini = j
                nums[mini], nums[i] = nums[i], nums[mini]
                l.append(nums[i])
            return self.minDifference(l)
            
        
```


# 4

```python
class Solution:
    def winnerSquareGame(self, n: int) -> bool:
        dp = [False for _ in range(n+1)]
        for i in range(1, n+1):
            j = 1
            while j*j<=i:
                if not dp[i-j*j]:
                    dp[i] = True
                    break
                j += 1
        return dp[-1]
```

