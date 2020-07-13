---
title: 剑指 Offer 64 求1+2+…+n
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:40/"剑指 Offer 64 求1+2+…+n".html'
date: 2020-06-30 21:56:40
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

思路参考：

[面试题64. 求 1 + 2 + … + n（逻辑符短路，清晰图解） - 求1+2+…+n - 力扣（LeetCode）](https://leetcode-cn.com/problems/qiu-12n-lcof/solution/mian-shi-ti-64-qiu-1-2-nluo-ji-fu-duan-lu-qing-xi-/)

```python
class Solution:
    def sumNums(self, n: int) -> int:
        self.res = 0
        # 用 and 替代 if
        def helper(n):
            n>1 and helper(n-1)
            self.res += n
            return self.res
        return helper(n)
```

