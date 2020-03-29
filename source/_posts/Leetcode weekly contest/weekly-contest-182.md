---
title: weekly-contest-182
toc: true
recommend: 1
uniqueId: '2020-03-29 05:24:35/"weekly-contest-182".html'
date: 2020-03-29 13:24:35
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

<!--more-->



[Contest - LeetCode](https://leetcode.com/contest/weekly-contest-182)
[Contest - LeetCode](https://leetcode.com/contest/weekly-contest-182/ranking/)

![W3V4YouLTnPEXjO](https://i.loli.net/2020/03/29/W3V4YouLTnPEXjO.png)

太丢人了



- [x] [Find Lucky Integer in an Array](https://leetcode.com/contest/weekly-contest-182/problems/find-lucky-integer-in-an-array)**3**
- [x] [Count Number of Teams](https://leetcode.com/contest/weekly-contest-182/problems/count-number-of-teams)**4**
- [x] [Design Underground System](https://leetcode.com/contest/weekly-contest-182/problems/design-underground-system)**5**
- [ ] [Find All Good Strings](https://leetcode.com/contest/weekly-contest-182/problems/find-all-good-strings) **8**



## Find Lucky Integer in an Array

Given an array of integers arr, a lucky integer is an integer which has a frequency in the array equal to its value.

Return a lucky integer in the array. If there are multiple lucky integers return the largest of them. If there is no lucky integer return -1.

```python
class Solution:
    def findLucky(self, arr: List[int]) -> int:
        result = -1
        d = {i:0 for i in arr}
        for i in arr:
            d[i] += 1
        for i in arr:
            if i==d[i]:
                result = max(result, i)
        return result
```



## Count Number of Teams

长度为 3 的单调上升或下降子序列的个数

相似的题目：[leetcode 300. Longest Increasing Subsequence - zronghui的博客](https://zronghui.github.io/leetcode/leetcode-300-Longest-Increasing-Subsequence.html)

比赛最后才想明白，可惜没做完

### 思路

长度为 3，选定中间那个数，然后 前面比它小的数*后面比它大的数=以这个数为第二个数的上升子序列

举例

Input: rating = [1,2,3,4]
Output: 4

​                           1 2 3 4

前面比 i 小的个数   0 1 2 3 

后面比 i 大的个数   3 2 1 0

```
0*3 + 1*2 + 2*1 + 3*0 = 4
```

再把 1234 颠倒过来，再算一遍为 0，最后二者相加4+0=4



```python
class Solution:
    def numTeams(self, rating: List[int]) -> int:
        return self.helper(rating) + self.helper(rating[::-1])
    
    def helper(self, l):
        dp1 = [0 for i in l]
        dp2 = [0 for i in l]
        for i in range(len(l)):
            for j in range(i-1, -1, -1):
                if l[j] < l[i]:
                    dp1[i] += 1
        for i in range(len(l)-1, -1, -1):
            for j in range(i+1, len(l)):
                if l[j] > l[i]:
                    dp2[i] += 1
        print(dp1, dp2)
        return sum(i*j for i, j in zip(dp1, dp2))
```



## Design Underground System

Implement the class UndergroundSystem that supports three methods:

1. checkIn(int id, string stationName, int t)

A customer with id card equal to id, gets in the station stationName at time t.
A customer can only be checked into one place at a time.
2. checkOut(int id, string stationName, int t)

A customer with id card equal to id, gets out from the station stationName at time t.
3. getAverageTime(string startStation, string endStation) 

Returns the average time to travel between the startStation and the endStation.
The average time is computed from all the previous traveling from startStation to endStation that happened directly.
Call to getAverageTime is always valid.
You can assume all calls to checkIn and checkOut methods are consistent. That is, if a customer gets in at time t1 at some station, then it gets out at time t2 with t2 > t1. All events happen in chronological order.

这题简单，构造 2 个字典就完事

```python
class UndergroundSystem:

    def __init__(self):
        # {id: [t, src]}
        self.checkInDict = {}
        # {(src, dest): [sumTime, n]}
        self.timeDict = {}
        

    def checkIn(self, id: int, stationName: str, t: int) -> None:
        self.checkInDict[id] = [t, stationName]

    def checkOut(self, id: int, stationName: str, t: int) -> None:
        inTime, inStation = self.checkInDict[id]
        del self.checkInDict[id]
        if (inStation, stationName) not in self.timeDict:
            self.timeDict[(inStation, stationName)] = [t-inTime, 1]
        else:
            self.timeDict[(inStation, stationName)][0] += t-inTime
            self.timeDict[(inStation, stationName)][1] += 1
        

    def getAverageTime(self, startStation: str, endStation: str) -> float:
        sum, n = self.timeDict[(startStation, endStation)]
        return sum/n


# Your UndergroundSystem object will be instantiated and called as such:
# obj = UndergroundSystem()
# obj.checkIn(id,stationName,t)
# obj.checkOut(id,stationName,t)
# param_3 = obj.getAverageTime(startStation,endStation)
```



## Find All Good Strings

Given the strings s1 and s2 of size n, and the string evil. Return the number of good strings.

A good string has size n, it is alphabetically greater than or equal to s1, it is alphabetically smaller than or equal to s2, and it does not contain the string evil as a substring. Since the answer can be a huge number, return this modulo 10^9 + 7.



这道题真的 hard。。。

