---
title: weekly-contest-187
toc: true
recommend: 1
uniqueId: '2020-05-04 12:52:25/"weekly-contest-187".html'
date: 2020-05-04 20:52:25
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [旅行终点站](https://leetcode-cn.com/problems/destination-city/)**3**
- [x] [是否所有 1 都至少相隔 k 个元素](https://leetcode-cn.com/problems/check-if-all-1s-are-at-least-length-k-places-away/)**4**
- [ ] [绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)**5**
- [x] [有序矩阵中的第 k 个最小数组和](https://leetcode-cn.com/problems/find-the-kth-smallest-sum-of-a-matrix-with-sorted-rows/)**7**

<!--more-->



# 1

```python
class Solution:
    def destCity(self, paths: List[List[str]]) -> str:
        res = paths[0][1]
        m = {}
        for s, e in paths[1:]:
            if s==res:
                res = e
                while res in m:
                    res = m.get(res)
            else:
                m[s] = e
        return res
```

# 2

```python
class Solution:
    def kLengthApart(self, nums: List[int], k: int) -> bool:
        try:
            last = nums.index(1)
        except:
            return True
        n = len(nums)
        for i in range(last+1, n):
            if nums[i]==1:
                if i-last<k+1:
                    return False
                last = i
        return True
```


# 3

```python

```


# 4

```python
class Solution:
    def kthSmallest(self, mat: List[List[int]], k: int) -> int:
        l = []
        def helper(mat):
            if len(mat)==1:
                return mat[0]
            la = mat.pop()
            lb = mat.pop()
            res = []
            for a in la:
                for b in lb:
                    res.append(a+b)
            res.sort()
            mat.append(res[:k])
            return helper(mat)
        
        mat = helper(mat)
        mat.sort()
        return mat[k-1]
```

