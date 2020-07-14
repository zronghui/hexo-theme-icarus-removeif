---
title: 剑指 Offer 60 n个骰子的点数
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:33/"剑指 Offer 60 n个骰子的点数".html'
date: 2020-06-30 21:56:33
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[动态规划（扫了一圈，俺是最短的） - n个骰子的点数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/solution/java-dong-tai-gui-hua-by-zhi-xiong/)



```python
class Solution:
    def twoSum(self, n: int) -> List[float]:
        # n: n~6n 长度为 5*n+1
        pre = [1/6]*6
        for i in range(2, n+1):
            t = [0]*(5*i+1)
            for j in range(len(pre)):
                for k in range(6):
                    t[j+k] += pre[j]/6 # 实际是 * 1/6
            pre = t
        return pre
```

