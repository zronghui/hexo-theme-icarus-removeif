---
title: weekly-contest-183
toc: true
recommend: 1
uniqueId: '2020-04-05 03:58:19/"weekly-contest-183".html'
date: 2020-04-05 11:58:19
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

<!--more-->



- [x] [Minimum Subsequence in Non-Increasing Order](https://leetcode.com/contest/weekly-contest-183/problems/minimum-subsequence-in-non-increasing-order)**4**
- [x] [Number of Steps to Reduce a Number in Binary Representation to One](https://leetcode.com/contest/weekly-contest-183/problems/number-of-steps-to-reduce-a-number-in-binary-representation-to-one)**4**
- [ ] [Longest Happy String](https://leetcode.com/contest/weekly-contest-183/problems/longest-happy-string)**6**
- [ ] [Stone Game III](https://leetcode.com/contest/weekly-contest-183/problems/stone-game-iii)**7**

<img src="/Users/zhangronghui/Library/Application Support/typora-user-images/image-20200405120055077.png" alt="image-20200405120055077" style="zoom:50%;" />



## 1

```python
class Solution:
    def minSubsequence(self, nums: List[int]) -> List[int]:
        if len(nums)<2:
            return nums
        nums.sort(reverse=True)
        _sum = sum(nums)
        t = 0
        for i, k in enumerate(nums):
            t += k
            if t>_sum/2:
                return nums[:i+1]
```

## 2

```python
class Solution:
    def add(self, s):
        if '0' not in s:
            return '1' + len(s)*'0'
        
        for i in list(range(len(s)))[::-1]:
            if s[i]=='0':
                return s[:i] + '1' + '0' * (len(s)-i-1)
        
        
    def numSteps(self, s: str) -> int:
        result = 0
        if len(s)==0:
            return 0
        while s!='1':
            result += 1
            if s[-1]=='0':
                s = s[:-1]
            else:
                s = self.add(s)
        return result
```

## 3

(贪心) O((a+b+c)2)O((a+b+c)2)
我们交替地往原字符串中插入 a，b 或 c，且保证每一次插入时都不会破坏规则。
由于是交替的插入，所以如果出现了某个字符不能插入的情况，则说明无论如何都没有办法插入这么多个该字符。

<img src="https://i.loli.net/2020/04/05/S3muIil68NCVzWt.png" alt="S3muIil68NCVzWt" style="zoom: 33%;" />



```python
class Solution:
    def insert(self, s, c):
        # 返回插入 i(a, b, c) 后的 s, isSuccess
        # 长度小于 2 or 第一个字母不同 or 第二位不同 直接放首位
        if len(s)<2 or s[0]!=c or s[1]!=c:
            return c + s, True
        # last 字母不同 or last-1 字母不同，直接放末位
        if s[-1]!=c or s[-2]!=c:
            return s + c, True
        # now len(s) >= 5  等于 5 的情况：ccbcc
        for i in range(2, len(s)-2):
            if s[i-1]==c and s[i-2]==c:
                continue
            if s[i-1]!=c and (s[i]!=c or s[i+1]!=c):
                return s[:i] + c + s[i:], True
        return s, False
    
    def longestDiverseString(self, a: int, b: int, c: int) -> str:
        # 逐个插入 a b c ，直至插入失败或者插完
        s = ''
        while a+b+c:
            if a:
                s, ok = self.insert(s, 'a')
                a -= 1
                if not ok:
                    return s
            if b:
                s, ok = self.insert(s, 'b')
                b -= 1
                if not ok:
                    return s
            if c:
                s, ok = self.insert(s, 'c')
                c -= 1
                if not ok:
                    return s
            print(s)
        return s
```

## 4

(动态规划) O(n)O(n)
设状态 f(i)f(i) 表示从第 ii 堆开始往后取，在最优策略下先手比后手能多赢的分数。假设下标从 0 开始到 n - 1。
初始值，f(n)=0f(n)=0，其余待定。
转移时，对于 f(i)f(i) 有三种决策，取第 ii 堆，取第 ii 和 i+1i+1 堆，以及取第 ii，i+1i+1 和 i+2i+2 堆，对应的转移分别为 max(s(i)−f(i+1),s(i)+s(i+1)−f(i+2),s(i)+s(i+1)+s(i+2)−f(i+3)max(s(i)−f(i+1),s(i)+s(i+1)−f(i+2),s(i)+s(i+1)+s(i+2)−f(i+3)。
最终，如果 f(0)=0f(0)=0，则平局；如果 f(0)>0f(0)>0 则 Alice 胜，否则 Bob 胜。

```python

```


