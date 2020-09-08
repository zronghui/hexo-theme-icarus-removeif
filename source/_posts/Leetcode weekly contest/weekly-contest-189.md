---
title: weekly-contest-189
toc: true
recommend: 1
uniqueId: '2020-08-29 03:43:43/"weekly-contest-189".html'
date: 2020-08-29 11:43:43
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

<!--more-->



# 1

```python
class Solution:
    def busyStudent(self, A: List[int], B: List[int], k: int) -> int:
        return sum(1 if a<=k<=b else 0 for a, b in zip(A, B))
            
```

# 2

```python
class Solution:
    def arrangeWords(self, text: str) -> str:
        text = text[0].lower()+text[1:]
        words = text.split()
        words.sort(key=len)
        res = ' '.join(words)
        return res[0].upper()+res[1:]
```


# 3

```python
class Solution:
    def peopleIndexes(self, fcs: List[List[str]]) -> List[int]:
        # 数据规模不大，意味着暴力
        # 稍微优化一下
        # fcs 内每个元素按照 长度+字典序 排列
        # 然后判断一个列表是不是另一个列表的子列表时，用双指针
        for i in fcs:
            i.sort(key=lambda i: (-len(i), i))
        # 添加原序号
        for i in range(len(fcs)):
            fcs[i].append(i)
        # fcs 按列表元素长度排序
        fcs.sort(key=len, reverse=True)
        print(fcs)
        res = [fcs[0][-1]]
        def isSubList(i, j):
            # j issublist of i
            p1, p2 = 0, 0
            while p1<len(i)-1 and p2<len(j)-1:
                if i[p1]==j[p2]:
                    p1 += 1
                    p2 += 1
                else:
                    if len(i[p1])<len(j[p2]):
                        return False
                    p1 += 1
            return p2==len(j)-1
        
        def helper(i):
            for j in fcs:
                if len(j)<=len(i):
                    return False
                if isSubList(j, i):
                    return True
            return False

        for i in fcs[1:]:
            if not helper(i):
                res.append(i[-1])
        res.sort()
        return res
        
```

人家就直接用 set 做。。。

```python
class Solution:
    def peopleIndexes(self, favoriteCompanies):
        if len(favoriteCompanies) == 0:
            return
        res = []
        for i in range(0, len(favoriteCompanies)):
            # 判断 列表i 是否为其他列表的子集，非则记录下标，是则无操作
            isFlag = False
            for j in range(0, len(favoriteCompanies)):
                if i == j:
                    continue
                if set(favoriteCompanies[i]).issubset(favoriteCompanies[j]):
                    # 如果为其他人子集，则停止
                    isFlag = True
                    break
            if not isFlag:
                res.append(i)
        return res
```



# 4

```python

```

