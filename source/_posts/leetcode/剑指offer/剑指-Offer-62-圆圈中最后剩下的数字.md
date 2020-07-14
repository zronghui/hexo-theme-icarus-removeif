---
title: 剑指 Offer 62 圆圈中最后剩下的数字
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:37/"剑指 Offer 62 圆圈中最后剩下的数字".html'
date: 2020-06-30 21:56:37
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[Java解决约瑟夫环问题](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/javajie-jue-yue-se-fu-huan-wen-ti-gao-su-ni-wei-sh/)

```python
class Solution:
    def lastRemaining(self, n: int, m: int) -> int:
        # 数学公式，倒推
        res = 0
        for i in range(2, n+1):
            res = (res+m)%i
        return res
```

