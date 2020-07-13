---
title: 剑指 Offer 59 - I 滑动窗口的最大值
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:29/"剑指 Offer 59 - I 滑动窗口的最大值".html'
date: 2020-06-30 21:56:29
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->



```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums: return []
        stack = collections.deque()
        n = len(nums)
        stack.append([nums[0], 0])
        res = [nums[0]]
        for i in range(1, n):
            # 去除队尾比当前小的数
            while stack and stack[-1][0]<nums[i]: stack.pop()
            stack.append([nums[i], i])
            if stack[0][1]<i-k+1: stack.popleft()
            res.append(stack[0][0])
        return res[k-1:]
        # s = [(nums[0], 0)]
        # n = len(nums)
        # res = []
        # for i in range(1, n):
        #     res.append(s[0][0])
        #     while s and nums[i]>s[-1][0]:
        #         s.pop()
        #     s.append((nums[i], i))
        #     if s[0][1]<=i-k:
        #         s.pop(0)
        # res.append(s[0][0])
        # return res[k-1:]
```

