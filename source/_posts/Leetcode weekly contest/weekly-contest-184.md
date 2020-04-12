---
title: weekly-contest-184
toc: true
recommend: 1
uniqueId: '2020-04-12 06:32:41/"weekly-contest-184".html'
date: 2020-04-12 14:32:41
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]



- [x] [String Matching in an Array](https://leetcode.com/contest/weekly-contest-184/problems/string-matching-in-an-array)**3**
- [x] [Queries on a Permutation With Key](https://leetcode.com/contest/weekly-contest-184/problems/queries-on-a-permutation-with-key)**4**
- [x] [HTML Entity Parser](https://leetcode.com/contest/weekly-contest-184/problems/html-entity-parser)**5**
- [x] [Number of Ways to Paint N × 3 Grid](https://leetcode.com/contest/weekly-contest-184/problems/number-of-ways-to-paint-n-3-grid)**7**

<!--more-->

![8socHYSt76UrE9D](https://i.loli.net/2020/04/12/8socHYSt76UrE9D.png)

值得纪念，第一次写完了，体验到一把做完题目的爽快感，虽然排名依旧很落后

## 1

本来打算按照长度排个序，却卡住了

`a.sort(key=lambda i: len(i))

```python
class Solution:
    def stringMatching(self, words: List[str]) -> List[str]:
        res = []
        for i in words:
            # 判断 i 是否为 j 的子串
            for j in words:
                if j in res or i==j or len(i)>=len(j):
                    continue
                if i in j and i not in res:
                    res.append(i)
        return res
```

## 2

逆序 range(index, 0, -1)

 p[1:index+1] = p[:index]  灵机一动，试了一下，还真可以

```python
class Solution:
    def processQueries(self, queries: List[int], m: int) -> List[int]:
        n = len(queries)
        p = list(range(1, m+1))
        res = []
        for i in queries:
            index = p.index(i)
            res.append(index)
            p[1:index+1] = p[:index]
            #for j in range(1, index+1):
            #    p[j] = p[j-1]
            p[0] = i
            # print(p)
        return res
```

## 3

```python
class Solution:
    def entityParser(self, text: str) -> str:
        m = {
            '&quot;': '"',
            '&apos;': "'",
            '&amp;': "&",
            '&gt;': ">",
            '&lt;': "<",
            '&frasl;': "/"
            }
        for key, value in m.items():
            text = text.replace(key, value)
        return text
```

## 4

一开始没想通，弄歪门邪道，拟合：

[在线曲线拟合神器_村美小站](http://www.qinms.com/webapp/curvefit/cf.aspx)

拟合效果还挺好

<img src="https://i.loli.net/2020/04/12/wUWsE6FYxhBa9Sc.png" alt="wUWsE6FYxhBa9Sc" style="zoom:50%;" />

### 解法

主要 2 种情况，1. 形如 1 2 3  2. 形如 1 2 1

设以上两种情况各 x y 个

1个 1 2 3 可衍生出 2 个 x，2 个 y

1个 1 2 1 可衍生出 2 个 x，3 个 y

因此**推导式**：

x' = 2x+2y

y' = 2x+3y

**起始状态**：

n = 1 时，x =  y = 6

```python
class Solution:
    def numOfWays(self, n: int) -> int:
        if n==1:
            return 12
        x = y = 6
        # 执行 n-1 次循环
        for i in range(n-1):
            tx, ty = x, y
            x = (2*tx+2*ty)%(10**9+7)
            y = (2*tx+3*ty)%(10**9+7)
        return (x+y)%(10**9+7)
```



## 心得

卡住了换一种思路，时间最重要，赛后再想

测试的时候可以 print，提交的时候一定要注释掉

不要想着走捷径，没有意义

