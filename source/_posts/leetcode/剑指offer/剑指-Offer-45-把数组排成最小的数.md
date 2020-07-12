---
title: 剑指 Offer 45 把数组排成最小的数
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:52/"剑指 Offer 45 把数组排成最小的数".html'
date: 2020-06-30 21:55:52
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[面试题45. 把数组排成最小的数（自定义排序，清晰图解） - 把数组排成最小的数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/solution/mian-shi-ti-45-ba-shu-zu-pai-cheng-zui-xiao-de-s-4/)

绝了

strs.sort(key = functools.cmp_to_key(sort_rule))

```python
class Solution:
    def minNumber(self, nums: List[int]) -> str:
        def sort_rule(x, y):
            a, b = x + y, y + x
            if a > b: return 1
            elif a < b: return -1
            else: return 0
        
        strs = [str(num) for num in nums]
        strs.sort(key = functools.cmp_to_key(sort_rule))
        return ''.join(strs)
        
```

