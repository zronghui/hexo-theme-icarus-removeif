---
title: weekly-contest-205
toc: true
recommend: 1
uniqueId: '2020-09-06 08:14:11/"weekly-contest-205".html'
date: 2020-09-06 16:14:11
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [替换所有的问号](https://leetcode-cn.com/problems/replace-all-s-to-avoid-consecutive-repeating-characters/)**3**
- [x] [数的平方等于两数乘积的方法数](https://leetcode-cn.com/problems/number-of-ways-where-square-of-number-is-equal-to-product-of-two-numbers/)**5**
- [x] [避免重复字母的最小删除成本](https://leetcode-cn.com/problems/minimum-deletion-cost-to-avoid-repeating-letters/)**5**
- [ ] [保证图可完全遍历](https://leetcode-cn.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/)**6**

![image-20200906161557664](https://i.loli.net/2020/09/06/hqcnGTL9xymlZt6.png)

<!--more-->

# 1

```python
class Solution:
    def modifyString(self, s: str) -> str:
        if s=='?': return 'a'
        if len(s)==1: return s
        if s.startswith('??'):
            s = 'a?'+s[2:]
        if s.startswith('?'):
            for x in 'ab':
                if x!=s[1]:
                    s = x+s[1:]
        if s.endswith('??'):
            s = s[:-2]+'?a'
        if s.endswith('?'):
            for x in 'ab':
                if x!=s[-2]:
                    s = s[:-1]+x
        res = s[0]
        for i in range(1, len(s)-1):
            if s[i]!='?':
                res += s[i]
            else:
                t = set('abc')-set([res[i-1], s[i+1]])
                for x in 'abc':
                    if x in t:
                        res += x
                        break
        res += s[-1]
        return res
```

# 2

```python
class Solution:
    def numTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        nums1.sort()
        nums2.sort()
        
        def helper(l1, l2):
            res = 0
            for i in l1:
                i2 = i*i
                l, r = 0, len(l2)-1
                while l<r:
                    mul = l2[l]*l2[r]
                    if l2[r]==l2[l]:
                        if mul==i2:
                            res += (r-l)*(r-l+1)//2
                        break
                    elif mul==i2:
                        newl = l
                        newr = r
                        while l2[newl+1]==l2[newl]:
                            newl += 1
                        while l2[newr-1]==l2[newr]:
                            newr -= 1
                        print(l, r, newl, newr)
                        res += (newl-l+1)*(r-newr+1)
                        # print(res, (newl-l+1)*(r-newr+1))
                        l, r = newl, newr-1
                    elif mul>i2:
                        r -= 1
                    elif mul<i2:
                        l += 1
            return res
            
            
        return helper(nums1, nums2)+helper(nums2, nums1)
```


# 3

```python
class Solution:
    def minCost(self, s: str, cost: List[int]) -> int:
        res = 0
        n = len(s)
        i = 0
        for _, v in itertools.groupby(s):
            l = len(list(v))
            if l>1:
                res += sum(cost[i:i+l])-max(cost[i:i+l])
            i += l
        return res
```


# 4

```python

```

