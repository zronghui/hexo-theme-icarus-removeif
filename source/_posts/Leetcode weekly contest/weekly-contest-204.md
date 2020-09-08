---
title: weekly-contest-204
toc: true
recommend: 1
uniqueId: '2020-08-30 03:20:41/"weekly-contest-204".html'
date: 2020-08-30 11:20:41
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [重复至少 K 次且长度为 M 的模式](https://leetcode-cn.com/problems/detect-pattern-of-length-m-repeated-k-or-more-times/)**3**
- [x] [乘积为正数的最长子数组长度](https://leetcode-cn.com/problems/maximum-length-of-subarray-with-positive-product/)**4**
- [ ] [使陆地分离的最少天数](https://leetcode-cn.com/problems/minimum-number-of-days-to-disconnect-island/)**6**
- [ ] [将子数组重新排序得到同一个二叉查找树的方案数](https://leetcode-cn.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/)**7**

![image-20200830181438578](https://i.loli.net/2020/08/30/qUCh3Zd6VfxJsAj.png)



<!--more-->



# 1

```python
class Solution:
    def containsPattern(self, arr: List[int], m: int, k: int) -> bool:
        if len(arr)<m*k:
            return False
        def helper(i):
            for x in range(m):
                for y in range(k):
                    if arr[i+x+y*m]!=arr[i+x]:
                        return False
            return True
        for i in range(len(arr)-m*k+1):
            if helper(i):
                return True
        return False
```

# 2

超时：

```python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        for i in range(len(nums)):
            if nums[i]==0:
                return max(self.getMaxLen(nums[:i]), self.getMaxLen(nums[i+1:]))
        if not nums: return 0
        l = [] # 左侧的负数个数
        cur = 0
        for i in nums:
            if i<0:
                cur += 1
            l.append(cur)
        # 最后一位是偶数：数组本身乘积就是正数
        if not l[-1]&1:
            return len(l)
        # l 中 找到第一个 奇数（1）； 最后一个偶数（l[-1]-1）
        firstOdd = lastEven = -1
        # firstEven = -1
        for i in range(len(l)):
            # if l[i]==0:
            #     firstEven = i
            if l[i]==1:
                firstOdd = i
                break
        for i in reversed(range(len(l))):
            if l[i]==l[-1]-1:
                lastEven = i
                break
        res = max(lastEven+1, len(l)-1-firstOdd)
        # print(l, lastEven, firstOdd, lastEven-firstEven, len(l)-1-firstOdd)
        return res
```

优化1: 

仍然超时

```python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        for i in range(len(nums)):
            if nums[i]==0:
                return max(self.getMaxLen(nums[:i]), self.getMaxLen(nums[i+1:]))
        if not nums: return 0
        # l = [] # 左侧的负数个数
        cur = 0
        # l 中 找到第一个 奇数（1）； 最后一个偶数（l[-1]-1）
        firstOdd = lastEven = -1
        for index, i in enumerate(nums):
            if i<0:
                cur += 1
            if cur==1 and firstOdd==-1:
                firstOdd = index
            if not cur&1:
                lastEven = index
            # l.append(cur)
        if not cur&1:
            return len(nums)
        
        res = max(lastEven+1, len(nums)-1-firstOdd)
        return res
```

优化 2：

```python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        nums.append(0)
        cur = 0
        firstOdd = lastEven = -1
        lastZero = -1 # 上一个 0 的位置
        res = 0
        for index, i in enumerate(nums):
            if i==0:
                if not cur&1:
                    res = max(res, index-1-lastZero)
                else:
                    res = max(res, lastEven-lastZero, index-1-firstOdd)
                cur = 0
                lastZero = firstOdd = lastEven = index
                continue
            if i<0:
                cur += 1
            if cur==1 and firstOdd==lastZero:
                firstOdd = index
            if not cur&1:
                lastEven = index
        return res
```

# 3

```python

```


# 4

```python

```

