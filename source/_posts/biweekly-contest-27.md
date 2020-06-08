---
title: biweekly-contest-27
toc: true
recommend: 1
uniqueId: '2020-06-06 14:02:30/"biweekly-contest-27".html'
date: 2020-06-06 22:02:30
thumbnail:
categories:
tags:
keywords:
---

[TOC]

<!--more-->

- [x] [通过翻转子数组使两个数组相等](https://leetcode-cn.com/problems/make-two-arrays-equal-by-reversing-sub-arrays/)**3**
- [ ] [检查一个字符串是否包含所有长度为 K 的二进制子串](https://leetcode-cn.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)**4**
- [ ] [课程安排 IV](https://leetcode-cn.com/problems/course-schedule-iv/)**5**
- [ ] [摘樱桃 II](https://leetcode-cn.com/problems/cherry-pickup-ii/)**6**

# 1

```python
class Solution:
    def canBeEqual(self, l1: List[int], l2: List[int]) -> bool:
        def getcount(l):
            m = {}
            for i in l:
                if i not in m:
                    m[i] = 1
                else:
                    m[i] += 1
            return m
        m1 = getcount(l1)
        m2 = getcount(l2)
        for i in m1:
            if m2.get(i)!=m1.get(i):
                return False
        return True

```

# 2

```python
class Solution:
    def hasAllCodes(self, s: str, k: int) -> bool:
        ss = set()
        if len(s)<=k:
            return False
        for i in range(k, len(s)+1):
            ss.add(s[i-k:i])
        print(ss, len(ss), 2**k)
        return len(ss)==2**k
```

# 3

## 法一：dfs  + functools.lru_cache

collections.defaultdict

@functools.lru_cache

执行用时 :692 ms, 在所有*Python*3 提交中击败了53.77%的用户

内存消耗 :31.8 MB, 在所有*Python*3 提交中击败了100.00%的用户

[DFS + 记忆化，化腐朽为神奇 - 课程安排 IV - 力扣（LeetCode）](https://leetcode-cn.com/problems/course-schedule-iv/solution/dfs-ji-yi-hua-hua-fu-xiu-wei-shen-qi-by-fuxuemingz/)

```python
class Solution:
    def checkIfPrerequisite(self, n: int, pres: List[List[int]], qs: List[List[int]]) -> List[bool]:
        self.graph = collections.defaultdict(list)
        for a, b in pres:
            self.graph[a].append(b)
        return [self.dfs(start, end) for start, end in qs]
    
    # 默认 maxsize 128 ，即存储最近 128 次调用
    @functools.lru_cache(maxsize=None)
    def dfs(self, start, end):
        if start==end:
            return True
        return any(self.dfs(nxt, end) for nxt in self.graph[start])
```

## 法二：Floyd

执行用时 :956 ms, 在所有*Python*3 提交中击败了48.57%的用户

内存消耗 :15.5 MB, 在所有*Python*3 提交中击败了100.00%的用户

[Python双百，不要DFS，不要BFS，只要最简单的打表法 - 课程安排 IV - 力扣（LeetCode）](https://leetcode-cn.com/problems/course-schedule-iv/solution/pythonbu-fu-za-de-da-biao-fa-by-bestfitting/)

[Floyd-Warshall算法 - 维基百科，自由的百科全书](https://zh.wikipedia.org/zh-hans/Floyd-Warshall%E7%AE%97%E6%B3%95)

```python
class Solution:
    def checkIfPrerequisite(self, n: int, pres: List[List[int]], qs: List[List[int]]) -> List[bool]:
        dp = [[False for _ in range(n)] for _ in range(n)]
        for a, b in pres:
            dp[a][b] = True
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    if dp[i][k] and dp[k][j]:
                        dp[i][j] = True
        return [dp[a][b] for a, b in qs]
```



# 4

```python

```

