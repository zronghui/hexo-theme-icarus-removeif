---
title: weekly-contest-195
toc: true
recommend: 1
uniqueId: '2020-07-30 09:35:58/"weekly-contest-195".html'
date: 2020-07-30 17:35:58
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [判断路径是否相交](https://leetcode-cn.com/problems/path-crossing/)**3**
- [x] [检查数组对是否可以被 k 整除](https://leetcode-cn.com/problems/check-if-array-pairs-are-divisible-by-k/)**4**
- [x] [满足条件的子序列数目](https://leetcode-cn.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/)**6**
- [ ] [满足不等式的最大值](https://leetcode-cn.com/problems/max-value-of-equation/)**7**

<!--more-->



# 1

```python
class Solution:
    def isPathCrossing(self, path: str) -> bool:
        s = {(0, 0), }
        d = {
            'N': (0, 1),
            'S': (0, -1),
            'E': (1, 0),
            'W': (-1, 0),
        }
        cur = [0, 0]
        for i in path:
            cur[0] += d[i][0]
            cur[1] += d[i][1]
            if tuple(cur) in s: return True
            s.add(tuple(cur))
        return False
```

# 2

```python
class Solution:

    def canArrange(self, arr: List[int], k: int) -> bool:
        # 余数: 数量
        m = collections.defaultdict(int)
        for i in arr:
            mod = i%k
            if mod==0:
                if m.get(0, 0)==0: m[0] = 1
                else: m[0] = 0
            elif m.get(k-mod, 0)>0: m[k-mod] -= 1
            else: m[mod] += 1
        # print(m)
        for i in m:
            if m.get(i, 0)>0:
                return False
        return True
```


# 3

```python
class Solution:
    def numSubseq(self, nums: List[int], target: int) -> int:
        # 考数学的一道题
        # 0 1 2 3 4 5
        # 若最小为 0，target 为 5，则 1~5 都可有可无， 这样的子序列有 2**4 个
        # 用双指针确定最大最小值
        nums.sort()
        res = 0
        l, r = 0, len(nums)-1
        while l<=r:
            while nums[l]+nums[r]>target and l<=r:
                r -= 1
            if nums[l]+nums[r]<=target and l<=r:
                res += int(pow(2, r-l, 1000000007))
                res %= 1000000007
            l += 1
        return res%1000000007
        
```

# 4

仍是数学题

![image-20200730173812522](https://i.loli.net/2020/07/30/UHxVBjGlKQNOd9t.png)

```python
class Solution:
    def findMaxValueOfEquation(self, points: List[List[int]], k: int) -> int:
        # max(yi + yj + |xi - xj|)    i>j
        # = max(yi + yj + xj - xi) = max(xj + yj) - min(xi - yi)
        result = float('-inf')
        # (xi-yi, xi)
        stack = collections.deque()
        stack.append((points[0][0]-points[0][1], points[0][0]))
        for point in points[1:]:
            while stack and point[0]-stack[0][1]>k:
                stack.popleft()
            if stack:
                result = max(result, sum(point)-stack[0][0])
            while stack and stack[-1][0]>point[0]-point[1]:
                stack.pop()
            stack.append((point[0]-point[1], point[0]))
        return result

```

