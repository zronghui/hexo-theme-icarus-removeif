---
title: weekly-contest-198
toc: true
recommend: 1
uniqueId: '2020-07-19 08:54:00/"weekly-contest-198".html'
date: 2020-07-19 16:54:00
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [换酒问题](https://leetcode-cn.com/contest/weekly-contest-198/problems/water-bottles/)**3**
- [x] [子树中标签相同的节点数](https://leetcode-cn.com/contest/weekly-contest-198/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/)**5**
- [ ] [最多的不重叠子字符串](https://leetcode-cn.com/contest/weekly-contest-198/problems/maximum-number-of-non-overlapping-substrings/)**6**
- [ ] [找到最接近目标值的函数值](https://leetcode-cn.com/contest/weekly-contest-198/problems/find-a-value-of-a-mysterious-function-closest-to-target/)

![image-20200719165557696](https://i.loli.net/2020/07/19/a5g9BFr7NnPzyqZ.png)

<!--more-->



# 1

```python
class Solution:
    def numWaterBottles(self, n: int, x: int) -> int:
        kong = 0
        res = 0
        while n>0 or kong>=x:
            res += n
            kong += n
            n = kong//x
            kong -= n*x
        return res
            
        
```

# 2

写的有点复杂，构造图 -> 构造树 -> 从叶子节点向 root 更新字典

```python
class Solution:
    def countSubTrees(self, n: int, edges: List[List[int]], labels: str) -> List[int]:
        res = [1]*n
        # 构造图
        d = collections.defaultdict(list)
        for a, b in edges:
            d[a].append(b)
            d[b].append(a)
        # 从0入手，构造树
        tree = [[0]] # tree 是层次遍历结果
        s = set()
        s.add(0)
        # print(d)
        while len(s)!=n:
            t = []
            for i in tree[-1]:
                for j in reversed(d[i]):
                    if j not in s:
                        t.append(j)
                        s.add(j)
                    else:
                        d[i].remove(j)
            tree.append(t)
        # 构造一个 i->{'x': cnt, }
        m = {i:{} for i in range(n)}        
        # print(d, tree, s)
        
        def update(l, m):
            if not l:
                return {}
            cur = m[l.pop()]
            while l:
                nxt = m[l.pop()]
                for i in nxt:
                    if i in cur:
                        cur[i] += nxt[i]
                    else: cur[i] = nxt[i]
            return cur
        
        # 先处理根节点
        for i in tree.pop():
            m[i] = {labels[i]:1}
        while tree:
            for i in tree.pop():
                m[i] = update(d[i], m)
                m[i][labels[i]] = m[i].get(labels[i], 0)+1
                res[i] = m[i][labels[i]]
        return res
    
    
    
```

根据别人的代码优化：

[DFS深度优先遍历-两种去重方式（Python） - 子树中标签相同的节点数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/solution/bfsyan-du-you-xian-bian-li-liang-chong-qu-zhong-fa/)

```python
from collections import defaultdict, Counter

class Solution:
    def countSubTrees(self, n: int, edges: List[List[int]], labels: str) -> List[int]:
        m = defaultdict(list)
        for a, b in edges:
            m[a].append(b)
            m[b].append(a)

        def dfs1(cur):
            data = Counter({labels[cur]: 1})
            vised[cur] = True
            for i in m[cur]:
                if vised[i]: continue
                data += dfs1(i)
            res[cur] = data[labels[cur]]
            return data
        
        def dfs2(cur, pre):
            data = Counter({labels[cur]: 1})
            for i in m[cur]:
                if i==pre: continue
                data += dfs2(i, cur)
            res[cur] = data[labels[cur]]
            return data

        res = [1]*n
        # 1. 用 visited 数组记录
        # vised = [False]*n
        # dfs1(0)
        # 记录父节点(pre)
        dfs2(0, 0)
        return res

```



# 3

```python

```

# 4

参考：[力扣周赛 198 - python 解答 | 我的博客](https://es2q.com/blog/2020/07/19/leetcode-weekly-198/)

```python
class Solution:
    def closestToTarget(self, arr: List[int], target: int) -> int:
        # & 的性质: 数越 & 越小；0 与所有数 & 都是 0; x&x=x
        # 利用 x&x=x 去除相邻的相同数字
        arr = [arr[0], *(v for i,v in enumerate(arr[1:]) if v!=arr[i-1])]
        res = sys.maxsize
        n = len(arr)
        for i in range(n):
            cur = arr[i]
            res = min(res, abs(cur-target)) # l==r 的情况
            for j in range(i+1, n):
                cur &= arr[j]
                res = min(res, abs(cur-target))
                if res==0: return 0
                if cur==0 or cur<target: break
                # 1. 0 与所有数 & 都是 0
                # 2. 数越 & 越小-> 若已经小于 target，再往后只能更小于 target, abs(x-target) 也更大
        return res
        
```

