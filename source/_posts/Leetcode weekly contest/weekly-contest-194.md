---
title: weekly-contest-194
toc: true
recommend: 1
uniqueId: '2020-07-16 04:33:41/"weekly-contest-194".html'
date: 2020-07-16 12:33:41
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [数组异或操作](https://leetcode-cn.com/problems/xor-operation-in-an-array/)**3**
- [ ] [保证文件名唯一](https://leetcode-cn.com/problems/making-file-names-unique/)**5**
- [ ] [避免洪水泛滥](https://leetcode-cn.com/problems/avoid-flood-in-the-city/)**6**
- [ ] [找到最小生成树里的关键边和伪关键边](https://leetcode-cn.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/)

<!--more-->



# 1

```python
class Solution:
    def xorOperation(self, n: int, start: int) -> int:
        l = [start+i*2 for i in range(n)]
        return reduce(lambda a, b: a^b ,l)
            
```

# 2

题解里的代码，很厉害

d {name: nextIndex, }

```python
class Solution:
    def getFolderNames(self, names: List[str]) -> List[str]:
        d, ans = {}, []
        for name in names:
            s = name
            while s in d:
                s = f'{name}({d[name]})'
                d[name] += 1
            d[s] = 1
            ans.append(s)
        return ans

```


# 3

```python

```


# 4

```python

```

