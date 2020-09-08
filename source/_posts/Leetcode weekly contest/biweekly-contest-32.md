---
title: biweekly-contest-32
toc: true
recommend: 1
uniqueId: '2020-08-09 00:48:03/"biweekly-contest-32".html'
date: 2020-08-09 08:48:03
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [第 k 个缺失的正整数](https://leetcode-cn.com/problems/kth-missing-positive-number/)**3**
- [x] [K 次操作转变字符串](https://leetcode-cn.com/problems/can-convert-string-in-k-moves/)**4**
- [ ] [平衡括号字符串的最少插入次数](https://leetcode-cn.com/problems/minimum-insertions-to-balance-a-parentheses-string/)**5**
- [ ] [找出最长的超赞子字符串](https://leetcode-cn.com/problems/find-longest-awesome-substring/)**6**

菜！

![image-20200809084943831](https://i.loli.net/2020/08/09/Bf1OwNyg4ojLp9z.png)

<!--more-->



# 1

```python
class Solution:
    def findKthPositive(self, arr: List[int], k: int) -> int:
        cnt = 0
        s = set(arr)
        for i in range(1, 3000):
            if i not in s:
                cnt += 1
                if cnt==k: return i
```

# 2

```python
class Solution:
    def canConvertString(self, s: str, t: str, k: int) -> bool:
        n = len(s)
        if n!=len(t): return False
        d = collections.defaultdict(int)
        for i in range(n):
            diffn = (ord(t[i])-ord(s[i]))%26
            if not diffn: continue
            d[diffn] += 1
            if (d[diffn]-1)*26+diffn>k: 
                return False
        return True
```


# 3

```python

```


# 4

```python

```

