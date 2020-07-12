---
title: weekly-contest-192
toc: true
recommend: 1
uniqueId: '2020-06-07 03:38:18/"weekly-contest-192".html'
date: 2020-06-07 11:38:18
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

我好菜

![image-20200607113911523](https://i.loli.net/2020/06/07/tPdg3qjHvmWRhJz.png)

<!--more-->

- [x] [重新排列数组](https://leetcode-cn.com/contest/weekly-contest-192/problems/shuffle-the-array/)**3**
- [x] [数组中的 k 个最强值](https://leetcode-cn.com/contest/weekly-contest-192/problems/the-k-strongest-values-in-an-array/)**4**
- [x] [设计浏览器历史记录](https://leetcode-cn.com/contest/weekly-contest-192/problems/design-browser-history/)**5**
- [ ] [给房子涂色 III](https://leetcode-cn.com/contest/weekly-contest-192/problems/paint-house-iii/)**6**

# 1

```python
class Solution:
    def shuffle(self, nums: List[int], n: int) -> List[int]:
        ans = []
        for i in range(n):
            ans.append(nums[i])
            ans.append(nums[n+i])
        return ans
```

# 2

```python
class Solution:
    def getStrongest(self, arr: List[int], k: int) -> List[int]:
        arr.sort()
        n = len(arr)
        m = arr[int((n-1)/2)]
        print(m)
        
        @functools.lru_cache(maxsize=None)
        def sortkey(a, b):
            x = abs(a-m)-abs(b-m)
            if x==0:
                return a-b
            return x
        # 选择排序
        # for i in range(k):
        #     mi = i
        #     for j in range(i+1, n):
        #         if sortkey(arr[j], arr[mi])>0:
        #             mi = j
        #     arr[mi], arr[i] = arr[i], arr[mi]
        
        # 双指针
        left = 0
        right = n-1
        ans = []
        # while left<=right:
        for i in range(k):
            if sortkey(arr[left], arr[right])>=0:
                ans.append(arr[left])
                left += 1
            else:
                ans.append(arr[right])
                right -= 1
                
        return ans
```


# 3

```python
class BrowserHistory:

    def __init__(self, homepage: str):
        self.his = [homepage]
        self.cur = 1 # 当前位置到开头 URL 的个数


    def visit(self, url: str) -> None:
        self.his = self.his[:self.cur]
        self.his.append(url)
        self.cur = self.cur+1
        # print(self.cur, self.his)


    def back(self, steps: int) -> str:
        self.cur = max(1, self.cur-steps)
        # print(self.cur, self.his)
        return self.his[self.cur-1]


    def forward(self, steps: int) -> str:
        self.cur = min(len(self.his), self.cur+steps)
        # print(self.cur, self.his)
        return self.his[self.cur-1]



# Your BrowserHistory object will be instantiated and called as such:
# obj = BrowserHistory(homepage)
# obj.visit(url)
# param_2 = obj.back(steps)
# param_3 = obj.forward(steps)
```


# 4

```python

```

