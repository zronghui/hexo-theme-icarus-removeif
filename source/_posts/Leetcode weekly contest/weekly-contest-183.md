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

<img src="https://i.loli.net/2020/04/08/oQypV49rS5KL32k.png" alt="oQypV49rS5KL32k" style="zoom:50%;" />

```python
class Solution:
    def stoneGameIII(self, l: List[int]) -> str:
        dp = [0 for i in range(len(l))]
        dp[-1] = l[-1]
        if len(l)>1:
            dp[-2] = max(l[-2]-dp[-1], sum(l[-2:]))
        if len(l)>2:
            dp[-3] = max(l[-3]-dp[-2], l[-3]+l[-2]-dp[-1], sum(l[-3:]))
        for i in range(len(l)-4, -1, -1):
            dp[i] = max(l[i]-dp[i+1], l[i]+l[i+1]-dp[i+2], l[i]+l[i+1]+l[i+2]-dp[i+3])
        return 'Alice' if dp[0]>0 else 'Bob' if dp[0]<0 else 'Tie'
        
```


