---
title: weekly-contest-197
toc: true
recommend: 1
uniqueId: '2020-07-12 05:53:52/"weekly-contest-197".html'
date: 2020-07-12 13:53:52
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [好数对的数目](https://leetcode-cn.com/problems/number-of-good-pairs/)**3**
- [x] [仅含 1 的子串数](https://leetcode-cn.com/problems/number-of-substrings-with-only-1s/)**4**
- [ ] [概率最大的路径](https://leetcode-cn.com/problems/path-with-maximum-probability/)**5**
- [ ] [服务中心的最佳位置](https://leetcode-cn.com/problems/best-position-for-a-service-centre/)**7**

<!--more-->



# 1

```python
class Solution:
    def numIdenticalPairs(self, nums: List[int]) -> int:
        res = 0
        n = len(nums)
        if n==1:
            return 0
        for i in range(n-1):
            for j in range(i+1, n):
                if nums[i]==nums[j]:
                    res += 1
        return res
```

# 2

```python
class Solution:
    def numSub(self, s: str) -> int:
        # 0 1 1 0 1 1 1
        # 0 1 2 0 1 2 3 dp: 以当前位置结尾的 1 的长度
        # 1+2+1+2+3
        n = len(s)
        dp = [0 for _ in range(n+1)]
        for i in range(1, n+1):
            if s[i-1]=='1':
                dp[i] = dp[i-1]+1
        return sum(dp[1:])%(10**9+7)
```

# 3

[bfs遍历求解 - 概率最大的路径 - 力扣（LeetCode）](https://leetcode-cn.com/problems/path-with-maximum-probability/solution/bfsbian-li-qiu-jie-by-jie-dong/)

```python
class Solution:
    def maxProbability(self, n: int, e: List[List[int]], succProb: List[float], start: int, end: int) -> float:
        # 1. defaultdict 构造 graph
        graph = collections.defaultdict(dict)
        for i in range(len(e)):
            p1, p2 = e[i]
            graph[p1][p2] = succProb[i]
            graph[p2][p1] = succProb[i]
        stack = collections.deque()
        stack.append([start, 1, 1<<start]) # start 到 当前点 的 概率 ，遍历过哪些点
        ans = 0
        while stack:
            cur, prob, passed = stack.popleft()
            if cur==end:
                ans = max(ans, prob)
            elif prob>ans:
                for point, _prob in graph[cur].items():
                    if not passed&(1<<point) and _prob*prob>ans:
                        stack.append([point, _prob*prob, passed^1<<point])
            
        return ans
        
```

优化：

```python
class Solution:
    def maxProbability(self, n: int, e: List[List[int]], succProb: List[float], start: int, end: int) -> float:
        # defaultdict 构造 graph
        graph = collections.defaultdict(dict)
        # 因为这个题目比较特殊，权重是乘性的并且0-1之间，所以可以用一个dic记录从start开始到每个节点的最大权重。 然后每次你只需要判断pop出的节点的权重就可以。如果比曾经记录的dic中的权重小那就没必要继续（这里面包含了环，回头），如果比曾经记录的权重大，那么继续进行。
        d = collections.defaultdict(float)
        for i in range(len(e)):
            p1, p2 = e[i]
            graph[p1][p2] = succProb[i]
            graph[p2][p1] = succProb[i]
        stack = collections.deque()
        stack.append([start, 1]) # start 到 当前点 的 概率
        ans = 0
        while stack:
            cur, prob = stack.popleft()
            if cur==end:
                ans = max(ans, prob)
            elif prob>ans:
                for point, _prob in graph[cur].items():
                    if _prob*prob>ans and _prob*prob>d.get(point, 0):
                        d[point] = _prob*prob
                        stack.append([point, _prob*prob])
        return ans
```



# 4

```python

```

