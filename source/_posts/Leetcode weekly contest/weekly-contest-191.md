---
title: weekly-contest-191
toc: true
recommend: 1
uniqueId: '2020-06-05 13:15:03/"weekly-contest-191".html'
date: 2020-06-05 21:15:03
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

<!--more-->

- [x] [数组中两元素的最大乘积](https://leetcode-cn.com/problems/maximum-product-of-two-elements-in-an-array/)**3**
- [x] [切割后面积最大的*蛋糕*](https://leetcode-cn.com/problems/maximum-area-of-a-piece-of-cake-after-horizontal-and-vertical-cuts/)**4**
- [x] [重新规划路线](https://leetcode-cn.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/)**5**
- [ ] [两个盒子中球的颜色数相同的概率](https://leetcode-cn.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls/)**7**

# 1

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        m1 = m2 = 0
        for i in nums:
            if i>m1:
                m1, m2 = i, m1
                continue
            elif i>m2:
                m2 = i
        print(m1, m2)
        return (m1-1)*(m2-1)
```

# 2

```python
class Solution:
    def maxArea(self, h: int, w: int, hCuts: List[int], vCuts: List[int]) -> int:
        hCuts.append(0)
        hCuts.append(h)
        vCuts.append(0)
        vCuts.append(w)
        hCuts.sort()
        vCuts.sort()
        def maxInterval(cuts):
            m = 0
            for i in range(1, len(cuts)):
                m = max(m, cuts[i]-cuts[i-1])
            return m
        return (maxInterval(hCuts)*maxInterval(vCuts))%(1000000007)
```


# 3

```python
class Solution:
    def minReorder(self, n: int, connections: List[List[int]]) -> int:
        To, In = [set() for i in range(n)], [set() for i in range(n)]
        '''
        To[x] -> y
        In[y] -> x
        '''
        ans = 0
        for x, y in connections:
            To[x].add(y)
            In[y].add(x)
        
        queue = [0]
        while queue:
            # 不为空，添加所有与 cur 相连的节点，若反向，添加到 ans 里
            
            cur = queue.pop()
            # 如 0 -> 1 ，所以 To 里面都是需要纠正的
            # To 添加到 queue 里面后，discard In 里面对应的数据
            # In 添加到 queue 里面后，discard To 里面对应的数据
            ans += len(To[cur])

            for i in To[cur]:
                queue.append(i)
                In[i].discard(cur)
            for i in In[cur]:
                queue.append(i)
                To[i].discard(cur)
        return ans
```


# 4

```python

```

