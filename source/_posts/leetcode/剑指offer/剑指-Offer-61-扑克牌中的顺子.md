---
title: 剑指 Offer 61 扑克牌中的顺子
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:35/"剑指 Offer 61 扑克牌中的顺子".html'
date: 2020-06-30 21:56:35
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[面试题61. 扑克牌中的顺子（集合 Set / 排序，清晰图解） - 扑克牌中的顺子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/solution/mian-shi-ti-61-bu-ke-pai-zhong-de-shun-zi-ji-he-se/)

![image-20200714110159558](https://i.loli.net/2020/07/14/EZnm4cUkoxQJlKI.png)



改进版：

```python
class Solution:
    def isStraight(self, nums: List[int]) -> bool:
        # 1. 排序
        nums.sort(reverse=True)
        # 2. 去除所有 0
        while nums and nums[-1]==0:
            nums.pop()
        # 3. 判断是否没有重复数字，且最大最小值差小于 5
        return nums[0]-nums[-1]<5 and len(nums)==len(set(nums))

```



别人的代码：

```python
class Solution:
    def isStraight(self, nums: List[int]) -> bool:
        joker = 0
        nums.sort() # 数组排序
        for i in range(4):
            if nums[i] == 0: joker += 1 # 统计大小王数量
            elif nums[i] == nums[i + 1]: return False # 若有重复，提前返回 false
        return nums[4] - nums[joker] < 5 # 最大牌 - 最小牌 < 5 则可构成顺子
```

