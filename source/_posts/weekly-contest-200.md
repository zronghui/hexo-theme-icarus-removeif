---
title: weekly-contest-200
toc: true
recommend: 1
uniqueId: '2020-08-02 02:14:24/"weekly-contest-200".html'
date: 2020-08-02 10:14:24
thumbnail:
categories:
tags:
keywords:
---

[TOC]



- [x] [统计好三元组](https://leetcode-cn.com/problems/count-good-triplets/)**3**
- [x] [找出数组游戏的赢家](https://leetcode-cn.com/problems/find-the-winner-of-an-array-game/)**4**
- [x] [排布二进制网格的最少交换次数](https://leetcode-cn.com/problems/minimum-swaps-to-arrange-a-binary-grid/)**5**
- [ ] [最大得分](https://leetcode-cn.com/problems/get-the-maximum-score/)**6**

误触 command + enter 2 次，淦！

前 10% 都没弄上

![image-20200802132822189](https://i.loli.net/2020/08/02/VpT8KPrOoAkqc2E.png)

<!--more-->



# 1

```python
class Solution:
    def countGoodTriplets(self, arr: List[int], a: int, b: int, c: int) -> int:
        def satisfy(i, j, k):
            if abs(arr[i] - arr[j])>a: return 0
            if abs(arr[j] - arr[k])>b: return 0
            if abs(arr[i] - arr[k])>c: return 0
            return 1
        n = len(arr)
        return sum(satisfy(i, j, k) for i in range(n-2) for j in range(i+1, n-1) for k in range(j+1, n))
        
```

# 2

```python
class Solution:
    def getWinner(self, arr: List[int], k: int) -> int:
        n = len(arr)
        # 比较 n 次后，arr 结构: max, n1, n2..... 
        # 但是不用真实模拟移动过程，只需要记录下标即可
        if len(arr)==2: return max(arr)
        m = collections.defaultdict(int)
        cur = 0
        for i in range(1, n):
            if arr[i]>arr[cur]:
                m[i] += 1
                cur = i
            else:
                m[cur] += 1
            if m[cur]==k: return arr[cur]
        return arr[cur]
        
```


# 3

```python
class Solution:
    def minSwaps(self, grid: List[List[int]]) -> int:
        # 记录每一行后面 0 的个数
        # 分配每一行应该贡献的 0 的个数
        # 如 0 2 2 实际算作 0 2 1
        #   0 2 2 4 4 5 算作 0 2 1 4 3 5
        # 就是说 遇到相同的数字，减一，加进去
        n = len(grid)
        def tailZeroNum(grid):
            s = set()
            l = []
            for i in range(n):
                cur = 0
                for j in reversed(range(n)):
                    if grid[i][j]==0: cur += 1
                    else: break
                cur = min(n-1, cur)
                while cur in s:
                    cur -= 1
                s.add(cur)
                l.append(cur)
            return l

        l = tailZeroNum(grid)
        # print(l)
        if set(range(n))!=set(l): return -1
        res = 0
        for i in range(n):
            for idx in range(len(l)):
                if l[idx]==i: break
            res += len(l)-1-idx
            l.remove(i)
        return res
```

# 4

没看清题，理解错了题意，实际上不算太难[C++双指针 分段统计最大和 相加即可 - 最大得分 - 力扣（LeetCode）](https://leetcode-cn.com/problems/get-the-maximum-score/solution/cshuang-zhi-zhen-fen-duan-tong-ji-zui-da-he-xiang-/)

<img src="https://i.loli.net/2020/08/02/TpVlkxoeftSFW6P.png" alt="image-20200802135845039" style="zoom:50%;" />

借鉴排行榜的代码



```python
class Solution:
    def maxSum(self, nums1: List[int], nums2: List[int]) -> int:
        s1, s2 = map(set, (nums1, nums2))
        a1 = a2 = 0
        for i in sorted(list(s1|s2)):
            if i in s1 and i in s2: a1 = a2 = max(a1, a2)+i
            elif i in s1: a1 += i
            else: a2 += i
        return max(a1, a2)%int(1e9+7)
```

