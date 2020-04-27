---
title: weekly-contest-185
toc: true
recommend: 1
uniqueId: '2020-04-27 07:16:00/"weekly-contest-185".html'
date: 2020-04-27 15:16:00
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
    def reformat(self, s: str) -> str:
        ss = []
        nn = []
        for i in s:
            if '0'<=i<='9':
                nn.append(i)
            else:
                ss.append(i)
        if abs(len(ss)-len(nn))>1:
            return ''
        s = []
        if len(ss) < len(nn):
            ss, nn = nn, ss
        while nn:
            s.append(ss.pop())
            s.append(nn.pop())
        if ss:
            s.append(ss[0]) 
        return ''.join(s)
        
```

# 2

```python
class Solution:
    def displayTable(self, orders: List[List[str]]) -> List[List[str]]:
        # 所有的菜，最后用来排序
        foodl = []
        # {table:{food: n, ...}}
        d = {}
        for _, table, food in orders:
            if food not in foodl:
                foodl.append(food)
            if table not in d:
                d[table] = {food: 1}
            else:
                d[table][food] = d[table].get(food, 0) + 1
        res = []
        foodl.sort()
        
        head = ["Table"]
        head.extend(foodl)
        res.append(head)
        
        t = []
        for table in sorted(d.keys(), key=lambda i: int(i)):
            t.append(str(table))
            for food in foodl:
                t.append(str(d[table].get(food, 0)))
            res.append(t)
            t = []
        return res
        
```

# 3

我的解法：

```python
class Solution:
    def minNumberOfFrogs(self, croakOfFrogs: str) -> int:
        d1 = {
            'r': 'c', 
            'o': 'cr', 
            'a': 'cro'
        }
        d2 = {
            'c': 0,
            'cr': 0,
            'cro': 0,
            'croa': 0
        }
        # 最多的青蛙
        m = 0
        # 当前的青蛙数
        n = 0
        for i in croakOfFrogs:
            key = d1.get(i, '')
            if i=='c':
                d2[i] += 1
                n += 1
            elif i=='k':
                if d2['croa']==0:
                    return -1
                else:
                    d2['croa'] -= 1
                    n -=1
            else:
                if d2[key]==0:
                    return -1
                d2[key] -= 1
                d2[key+i] += 1
            m = max(m, n)
            #print(m, n)
        return m if n==0 else -1
        
```

其他人的解法：

[记录当前青蛙个数的最大值 - 数青蛙 - 力扣（LeetCode）](https://leetcode-cn.com/problems/minimum-number-of-frogs-croaking/solution/ji-lu-dang-qian-qing-wa-ge-shu-de-zui-da-zhi-by-qi/)

c,r,o,a,k分别表示遍历到当前位置时这几个字母出现的次数。
每次遍历过程中当且仅当c>=r>=o>=a>=k时才符合要求。
now表示当前存在的青蛙个数，即遇到c时加一，叫完以后(遇到k)减一。
遍历完后now应为0表示每次叫声都有头有尾，记录now的最大值即为答案。

```python
class Solution:
    def minNumberOfFrogs(self, croakOfFrogs: str) -> int:
        c=r=o=a=k=0
        now=0
        res=0
        for i in croakOfFrogs:
            if i=='c':
                c+=1
                now+=1
                res=max(res,now)
            elif i=='r':
                r+=1
            elif i=='o':
                o+=1
            elif i=='a':
                a+=1
            elif i=='k':
                k+=1
                now-=1
            if not c>=r>=o>=a>=k:
                return -1
        return res if now==0 else -1

```

可以改进↓：

```python
class Solution:
    def minNumberOfFrogs(self, croakOfFrogs: str) -> int:
        c=r=o=a=k=0
        d = {
            'c': 0,
            'r': 0,
            'o': 0,
            'a': 0,
            'k': 0
        }
        now=0
        res=0
        for i in croakOfFrogs:
            d[i] += 1
            if i=='c':
                now+=1
                res=max(res,now)
            elif i=='k':
                now-=1
            if not d['c']>=d['r']>=d['o']>=d['a']>=d['k']:
                return -1
        return res if now==0 else -1
```



# 4

[超简单的（？）三维动态规划 - 生成数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons/solution/jian-dan-san-wei-dong-tai-gui-hua-by-coldme-2/)

学不来学不来

```python

```

