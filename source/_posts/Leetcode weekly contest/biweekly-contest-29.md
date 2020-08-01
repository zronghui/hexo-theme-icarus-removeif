---
title: biweekly-contest-29
toc: true
recommend: 1
uniqueId: '2020-07-30 01:51:56/"biweekly-contest-29".html'
date: 2020-07-30 09:51:56
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [去掉最低工资和最高工资后的工资平均值](https://leetcode-cn.com/contest/biweekly-contest-29/problems/average-salary-excluding-the-minimum-and-maximum-salary/)**3**
- [x] [n 的第 k 个因子](https://leetcode-cn.com/contest/biweekly-contest-29/problems/the-kth-factor-of-n/)**4**
- [x] [删掉一个元素以后全为 1 的最长子数组](https://leetcode-cn.com/contest/biweekly-contest-29/problems/longest-subarray-of-1s-after-deleting-one-element/)**5**
- [ ] [并行课程 II](https://leetcode-cn.com/contest/biweekly-contest-29/problems/parallel-courses-ii/)**6**

<!--more-->



# 1

```python
class Solution:
    def average(self, salary: List[int]) -> float:
        return (sum(salary)-min(salary)-max(salary))/(len(salary)-2)
        
```

# 2

```python
class Solution:
    def kthFactor(self, n: int, k: int) -> int:
        cnt = 0
        for i in range(1, n+1):
            # print(i)
            if n%i==0:
                cnt += 1
                if cnt==k: return i
        return -1
        
```


# 3

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        n = len(nums)
        left = [0]*n
        right = [0]*n
        cur = 0
        for i in range(n):
            left[i] = cur
            if nums[i]==1: cur += 1
            else: cur = 0
        # print(left)
        cur = 0
        for i in reversed(range(n)):
            right[i] = cur
            if nums[i]==1: cur += 1
            else: cur = 0
        # print(right)
        return max(left[i]+right[i] for i in range(n))
```


# 4

```python

```

