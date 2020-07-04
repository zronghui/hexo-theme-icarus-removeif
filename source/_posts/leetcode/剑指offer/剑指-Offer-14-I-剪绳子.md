---
title: 剑指 Offer 14- I 剪绳子
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:51/"剑指 Offer 14- I 剪绳子".html'
date: 2020-06-30 21:54:51
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 14- I. 剪绳子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/submissions/)
[面试题14- I. 剪绳子（数学推导 / 贪心思想，清晰图解） - 剪绳子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/)

省略数学推导

![image-20200702104512173](https://i.loli.net/2020/07/02/3wydZ78m5KRbSHj.png)

![image-20200702104215884](https://i.loli.net/2020/07/02/4RG5UluWo7S8Ftx.png)

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        if n<=3:
            return n-1
        a, b = divmod(n, 3)
        if b==0:
            return int(math.pow(3, a))
        elif b==1:
            return int(math.pow(3, a-1)*4)
        else:
            return int(math.pow(3, a)*2)
```

