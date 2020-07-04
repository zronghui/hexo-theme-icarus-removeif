---
title: 剑指 Offer 13 机器人的运动范围
toc: true
recommend: 1
uniqueId: '2020-06-30 13:54:49/"剑指 Offer 13 机器人的运动范围".html'
date: 2020-06-30 21:54:49
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

[剑指 Offer 13. 机器人的运动范围 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/submissions/)
[面试题13. 机器人的运动范围（ DFS / BFS ，清晰图解） - 机器人的运动范围 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/solution/mian-shi-ti-13-ji-qi-ren-de-yun-dong-fan-wei-dfs-b/)

<img src="https://i.loli.net/2020/07/02/HoK3BkadOGPYlIf.png" alt="image-20200702101256400" style="zoom:50%;" />

```python
class Solution:
    def movingCount(self, m: int, n: int, k: int) -> int:
        def dfs(i, j, si, sj):
            if i>=m or j>=n or (i, j) in visited or si+sj>k:
                return 0
            visited.add((i, j))
            return 1+\
                    dfs(i+1, j, si+1 if (i+1)%10 else si-8, sj)+\
                    dfs(i, j+1, si, sj+1 if (j+1)%10 else sj-8)
            
        visited = set()
        return dfs(0, 0, 0, 0)
```

