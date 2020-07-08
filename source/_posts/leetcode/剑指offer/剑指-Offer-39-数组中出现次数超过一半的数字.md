---
title: 剑指 Offer 39 数组中出现次数超过一半的数字
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:41/"剑指 Offer 39 数组中出现次数超过一半的数字".html'
date: 2020-06-30 21:55:41
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
    def majorityElement(self, nums: List[int]) -> int:
        major, cnt = '', 0
        for i in nums:
            if cnt:
                if major!=i: 
                    cnt -= 1
                else:
                    cnt += 1
            else:
                major = i
                cnt += 1
            # print(major, cnt)
        return major
```

