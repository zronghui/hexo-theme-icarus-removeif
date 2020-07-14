---
title: 剑指 Offer 65 不用加减乘除做加法
toc: true
recommend: 1
uniqueId: '2020-06-30 13:56:42/"剑指 Offer 65 不用加减乘除做加法".html'
date: 2020-06-30 21:56:42
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[面试题65. 不用加减乘除做加法（位运算，清晰图解） - 不用加减乘除做加法 - 力扣（LeetCode）](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/solution/mian-shi-ti-65-bu-yong-jia-jian-cheng-chu-zuo-ji-7/)

```python
class Solution {
    public int add(int a, int b) {
        // 用 Python 此解法会有问题
        while(b != 0) { // 当进位为 0 时跳出
            int c = (a & b) << 1;  // c = 进位
            a ^= b; // a = 非进位和
            b = c; // b = 进位
        }
        return a;
    }
}
```

