---
title: weekly-contest-193
toc: true
recommend: 1
uniqueId: '2020-06-14 04:01:23/"weekly-contest-193".html'
date: 2020-06-14 12:01:23
thumbnail:
categories:
tags:
keywords:
---

[TOC]



380，最靠前的一次了吧

![image-20200614120137141](https://i.loli.net/2020/06/14/QGMqoUYwNjgmxkO.png)



- [x] [一维数组的动态和](https://leetcode-cn.com/contest/weekly-contest-193/problems/running-sum-of-1d-array/)**3**
- [x] [不同整数的最少数目](https://leetcode-cn.com/contest/weekly-contest-193/problems/least-number-of-unique-integers-after-k-removals/)**4**
- [x] [制作 m 束花所需的最少天数](https://leetcode-cn.com/contest/weekly-contest-193/problems/minimum-number-of-days-to-make-m-bouquets/)**5**
- [ ] [树节点的第 K 个祖先](https://leetcode-cn.com/contest/weekly-contest-193/problems/kth-ancestor-of-a-tree-node/)**6**

<!--more-->



# 1

```python
class Solution:
    def runningSum(self, nums: List[int]) -> List[int]:
        if len(nums)<2:
            return nums
        for i in range(1, len(nums)):
            nums[i] += nums[i-1]
        return nums
```

# 2

```python
class Solution:
    def findLeastNumOfUniqueInts(self, arr: List[int], k: int) -> int:
        m = collections.defaultdict(int)
        for i in arr:
            m[i] += 1
        print(m, m.values())
        l = [i for i in m.values()]
        l.sort(reverse=True)
        while k>0:
            i = l.pop()
            k -= i
        return len(l) if k==0 else len(l)+1
        
```


# 3

```python
import bisect

class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        # 将 l 并入排好序的 exclude 中
        def merge(exclude, l):
            # print(exclude, l)
            # 还能优化，l 好像也是排好序的
            for i in l:
                bisect.insort(exclude, i)
            # print(exclude)
                
        # 判断去除 exclude 后是否满足条件
        def satisfy(n, m, k, exclude):
            # 还能优化，可以在上次的结果里去除一部分不符合条件的
            if m*k>n-len(exclude):
                return False
            pre = -1
            # 能收多少束花 satisfied_m
            sm = 0
            
            # 0 1 2 3 4 5 6    n=7
            # x x x x _ x x
            exclude.append(n) # 考虑列表最后一部分
            # 若 exclude [4, 6]  n 10
            for i in exclude:
                sm += int((i-pre-1)/k)
                if sm>=m:
                    exclude.pop()
                    return True
                pre = i
            exclude.pop()
            return sm>=m
        
        # 计算最后一天可不可以，然后倒回去，直到有一天不行
        n = len(bloomDay)
        if m*k>n:
            return -1
        exclude = []
        
        dic = collections.defaultdict(list)
        # dic 存放哪一天开了哪些位置的花
        for i in range(n):
            dic[bloomDay[i]].append(i)
            
        days = [i for i in dic.keys()]
        days.sort(reverse=True)
        
        for i in range(len(days)):
            day = days[i]
            merge(exclude, dic[day])
            # print(n, m, k, exclude)
            # print(satisfy(n, m, k, exclude))
            if not satisfy(n, m, k, exclude):
                return days[i]
        
        
            # m 束花 k 朵花
            # 0 1 2 3 4 5 6 7 8 9  n=10 m=4 k=2
            # x _ x _ x x x x x x

```

# 4



```python
class TreeAncestor:

    def __init__(self, n: int, parent: List[int]):
        # 2**16 = 65536
        self.dp = [[-1]*16 for i in range(n)]
        for i in range(n):
            self.dp[i][0] = parent[i]
        for i in range(n):
            for j in range(1, 16):
                if self.dp[i][j-1]==-1:
                    break
                self.dp[i][j] = self.dp[self.dp[i][j-1]][j-1]


    def getKthAncestor(self, node: int, k: int) -> int:
        # 10 : 8+2 1010 将 10 分解，找到 1 所在的位置
        for i in reversed(range(16)):
            if k & (1<<i):# k 在 第 i 位是 1
                node = self.dp[node][i]
            if node==-1:
                break
        return node



# Your TreeAncestor object will be instantiated and called as such:
# obj = TreeAncestor(n, parent)
# param_1 = obj.getKthAncestor(node,k)
```

