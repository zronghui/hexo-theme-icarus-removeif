---
title: 剑指 Offer 14- II 剪绳子 II
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:52/"剑指 Offer 14- II 剪绳子 II".html'
date: 2020-06-30 21:54:52
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

更正规的解法：

> todo 大数求余

[面试题14- II. 剪绳子 II（数学推导 / 贪心思想 + 快速幂求余，清晰图解） - 剪绳子 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/solution/mian-shi-ti-14-ii-jian-sheng-zi-iitan-xin-er-fen-f/)

![image-20200702105826709](https://i.loli.net/2020/07/02/xoWbpsCFD1rNLAB.png)

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        if n<=3:
            return n-1
        a, b = divmod(n, 3)

        def powmod(x):
            res = 1
            for _ in range(x):
                res = 3*res % 1000000007
            return res

        if b==0:
            return powmod(a)
        elif b==1:
            return powmod(a-1)*4 % 1000000007
        else:
            return powmod(a)*2 % 1000000007
    

```

