---
title: weekly-contest-203
toc: true
recommend: 1
uniqueId: '2020-08-23 03:53:15/"weekly-contest-203".html'
date: 2020-08-23 11:53:15
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [圆形赛道上经过次数最多的扇区](https://leetcode-cn.com/contest/weekly-contest-203/problems/most-visited-sector-in-a-circular-track/)**3**
- [x] [你可以获得的最大硬币数目](https://leetcode-cn.com/contest/weekly-contest-203/problems/maximum-number-of-coins-you-can-get/)**4**
- [ ] [查找大小为 M 的最新分组](https://leetcode-cn.com/contest/weekly-contest-203/problems/find-latest-group-of-size-m/)**6**
- [ ] [石子游戏 V](https://leetcode-cn.com/contest/weekly-contest-203/problems/stone-game-v/)**7**

打回原形，排名又要下降了

![image-20200823115729402](https://i.loli.net/2020/08/23/dsOEcbUFDyzvYaK.png)

<!--more-->



# 1

模拟，比较慢，不知道有没有快点的方法

```python
class Solution:
    def mostVisited(self, n: int, rounds: List[int]) -> List[int]:
        m = collections.defaultdict(int)
        m[rounds[0]] += 1
        # 后面不算起点
        cur = rounds[0]
        rounds.pop(0)
        while rounds:
            while cur!=rounds[0]:
                cur += 1
                if cur>n: cur = 1
                m[cur] += 1
            rounds.pop(0)
        # print(m, max(m.values()))
        res = []
        ma = max(m.values())
        for i in m:
            if m.get(i)==ma:
                res.append(i)
        res.sort()
        return res
```

# 2

每次扔个最大的，扔个最小的

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        # [2,4,1,2,7,8]
        # 1 2 2 4 7 8
        nums.sort()
        res = 0
        
        while nums:
            res += nums[-2]
            nums.pop()
            nums.pop()
            nums.pop(0)
        return res
        
```


# 3

```python

```

# 4

写的是对的，但是超时

复杂度: 500^3

哎，不知道哪里可以优化。遍历顺序还是什么地方

```python
class Solution:
    def stoneGameV(self, nums: List[int]) -> int:
        n = len(nums)
        # dp[i][j] i~j 石子的分数
        # return dp[0][n-1]
        #   dp[i][j] = max(min(_sum(), _sum() + dp[]) for k in range(i+1, j))
        dp = [[0]*n for i in range(n)]
        presum = list(itertools.accumulate(nums))
        
        @functools.lru_cache(None)
        def _sum(i, j):
            # i~j 的 sum
            return presum[j]-presum[i]+nums[i]
        
        @functools.lru_cache(None)
        def getCur(i, j, k):
            if _sum(i, k)>_sum(k+1, j):
                cur = _sum(k+1, j)+dp[k+1][j]
            elif _sum(i, k)<_sum(k+1, j):
                cur = _sum(i, k)+dp[i][k]
            else:
                cur = max(_sum(i, k)+dp[i][k], _sum(k+1, j)+dp[k+1][j])
            return cur

        for gap in range(1, n):
            for i in range(n-gap):
                j = i+gap
                if gap==1: dp[i][j] = min(nums[i:j+1])
                else:
                    # 划分为 [i, k] [k+1, j]-> i<=k<j
                    dp[i][j] = max(getCur(i, j, k) for k in range(i, j))
        for i in dp: print(i)
        return dp[0][n-1]
```





比赛完了，别人用的一样的思路，也是 O(n^3) 就是 LeetCode 对时间要求苛刻了一些，需要做一些优化:

从上往下的 记忆化递归 可以减少许多不必要的状态

比如上面代码运行完的 dp 数组：

```
[0, 2, 7, 10, 13, 18]
[0, 0, 2, 4, 7, 13]
[0, 0, 0, 3, 5, 10]
[0, 0, 0, 0, 4, 5]
[0, 0, 0, 0, 0, 5]
[0, 0, 0, 0, 0, 0]
```

记忆化递归 的 dp 数组

```
[0, 2, 7, 0, 0, 18]
[0, 0, 2, 0, 0, 0]
[0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 0]
[0, 0, 0, 0, 0, 5]
[0, 0, 0, 0, 0, 0]
```

presum 前面加个 0，方便计算前缀和 _sum(i, j) = presum[j+1]-presum[i]

```python
class Solution:
    def stoneGameV(self, nums: List[int]) -> int:
        n = len(nums)
        # dp[i][j] i~j 石子的分数
        # return dp[0][n-1]
        #   dp[i][j] = max(min(_sum(), _sum() + dp[]) for k in range(i+1, j))
        dp = [[0]*n for i in range(n)]
        presum = list(itertools.accumulate(nums))
        presum.insert(0, 0)
        
        @functools.lru_cache(None)
        def _sum(i, j):
            # i~j 的 sum
            return presum[j+1]-presum[i]
        
        def getCur(i, j, k):
            l, r = _sum(i, k), _sum(k+1, j)
            if l>r:
                cur = r+dfs(k+1, j)
            elif l<r:
                cur = l+dfs(i, k)
            else:
                cur = max(l+dfs(i, k), r+dfs(k+1, j))
            return cur
                    
        def dfs(i, j):
            if dp[i][j]: return dp[i][j]
            if j-i==0: return 0
            if j-i==1: 
                dp[i][j] = min(nums[i], nums[j])
                return dp[i][j]
            dp[i][j] = max(getCur(i, j, k) for k in range(i, j))
            return dp[i][j]
        
        dfs(0, n-1)
        # for i in dp: print(i)
        return dp[0][n-1]

```



此外，记忆化递归 在 Python 中可以不借助 map 或数组，可以用 lru_cache 来实现，代码更简短，而且速度上还更快

![image-20200824143033457](https://i.loli.net/2020/08/24/lk2uy8qNw37ZIAt.png)

```python
class Solution:
    def stoneGameV(self, nums: List[int]) -> int:
        n = len(nums)
        # dp[i][j] i~j 石子的分数
        # return dp[0][n-1]
        #   dp[i][j] = max(min(_sum(), _sum() + dp[]) for k in range(i+1, j))
        presum = list(itertools.accumulate(nums))
        presum.insert(0, 0)

        def _sum(i, j):
            # i~j 的 sum
            return presum[j+1]-presum[i]
        
        def getCur(i, j, k):
            l, r = _sum(i, k), _sum(k+1, j)
            if l>r:
                cur = r+dfs(k+1, j)
            elif l<r:
                cur = l+dfs(i, k)
            else:
                cur = max(l+dfs(i, k), r+dfs(k+1, j))
            return cur
        
        @functools.lru_cache(None)
        def dfs(i, j):
            if j-i==0: return 0
            if j-i==1:
                return min(nums[i], nums[j])
            return max(getCur(i, j, k) for k in range(i, j))
        
        return dfs(0, n-1)

```

































