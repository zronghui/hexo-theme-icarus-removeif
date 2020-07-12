---
title: biweekly-contest-24
toc: true
recommend: 1
uniqueId: '2020-04-19 01:56:34/"biweekly-contest-24".html'
date: 2020-04-19 09:56:34
thumbnail:
categories:
- Leetcode weekly contest
tags:
keywords:
---

[TOC]

- [x] [逐步求和得到正数的最小值](https://leetcode-cn.com/problems/minimum-value-to-get-positive-step-by-step-sum/)**3**
- [ ] [和为 K 的最少斐波那契数字数目](https://leetcode-cn.com/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/)**4**
- [ ] [长度为 n 的开心字符串中字典序第 k 小的字符串](https://leetcode-cn.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n/)**5**
- [ ] [恢复数组](https://leetcode-cn.com/problems/restore-the-array/)**6**

<!--more-->

## 1

```python
class Solution:
    def minStartValue(self, nums: List[int]) -> int:
        m = nums[0]
        s = 0
        for i in nums:
            s += i
            m = min(m, s)
        if m<=0:
            return 1-m
        return 1
        
```

## 2

[和为 K 的最少斐波那契数字数目 - 和为 K 的最少斐波那契数字数目 - 力扣（LeetCode）](https://leetcode-cn.com/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/solution/he-wei-k-de-zui-shao-fei-bo-na-qi-shu-zi-shu-mu-by/)

```python
class Solution:
    def findMinFibonacciNumbers(self, k: int) -> int:
        # 想不出来，抄答案
        # 先构造小于 k 的 fib 数组 l
        # 逆序遍历 l ，遇到小于 K 的i，k -= i, ans +=1
        a = b = 1
        fib = [a, b]
        while b<k:
            c = a+b
            fib.append(c)
            a, b = b, c
        ans = 0
        for i in reversed(fib):
            if i<=k:
                k -= i
                ans += 1
        return ans

```

## 3

[getStart和getNext, O(NK) - 长度为 n 的开心字符串中字典序第 k 小的字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n/solution/getstarthe-getnext-onk-by-suibianfahui/)

```python
class Solution:
    def getHappyString(self, n: int, k: int) -> str:
        posible = {'a': 'bc', 'b': 'c', 'c': ''}

        def getStart(k):
            return ''.join('a' if i%2==0 else 'b' for i in range(k))
        
        def getNext(cur):
            for i in reversed(range(len(cur))):
                for po in posible[cur[i]]:
                    if i==0 or cur[i-1]!=po:
                        return cur[:i] + po + getStart(n-i-1)
            return ''

        cur = getStart(n)
        for i in range(k-1):
            cur = getNext(cur)
            if not cur:
                return cur
        return cur

```

## 4

[动态规划，逐字符解析判断有多少种可能 - 恢复数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/restore-the-array/solution/dong-tai-gui-hua-zhu-zi-fu-jie-xi-pan-duan-you-duo/)

[恢复数组 - 恢复数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/restore-the-array/solution/hui-fu-shu-zu-by-leetcode-solution/)

```python
class Solution:
    def numberOfArrays(self, s: str, k: int) -> int:
        n = len(s)
        dp = [0] * (n + 1) # numberOfArrays(s[:i])
        #    1 3 1 7 : s
        #  1 1 2 4 8 : dp  
        # 解释:  8 = numberOfArrays(131)+numberOfArrays(13)+numberOfArrays(1)+numberOfArrays('')
        dp[0] = 1
        mod = 10 ** 9 + 7
        for i in range(1, n+1):
            # 优化，k <= 10^9，故最后一个空格最多隔开 10 个数字
            # for j in reversed(range(i)):
            for j in reversed(range(max(0, i-10), i)):
                # print(i, j, s[j:i])
                if s[j]=='0':
                    continue
                if int(s[j:i])>k:
                    break
                dp[i] += dp[j]
        # print(dp)
        return dp[-1] % mod
```



