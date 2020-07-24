---
thumbnail:
title: leetcode 1025. Divisor Game
date: 2020-07-24 10:42:10
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Math
- Dynamic Programming
recommend: 1
keywords:
uniqueId: '2020-07-24 10:42:10/"leetcode 1025. Divisor Game".html'
---

<a href="https://leetcode.com/problems/divisor-game/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/divisor-game/">九章</a>
## 题目描述
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there is a number `N` on the chalkboard.  On each player's turn,
that player makes a _move_  consisting of:

  * Choosing any `x` with `0 < x < N` and `N % x == 0`.
  * Replacing the number `N` on the chalkboard with `N - x`.

Also, if a player cannot make a move, they lose the game.

Return `True` if and only if Alice wins the game, assuming both players play
optimally.



**Example 1:**
        
    Input: 2
    Output: true
    Explanation: Alice chooses 1, and Bob has no more moves.


**Example 2:**
        
    Input: 3
    Output: false
    Explanation: Alice chooses 1, Bob chooses 1, and Alice has no more moves.




**Note:**

  1. `1 <= N <= 1000`


**Tags:** Math, Dynamic Programming

**Difficulty:** Easy

## 答案
<!--more-->
```python
class Solution:
    def __init__(self):
        self.dp = [False]*1000
        for i in range(2, 1001):
            for factor in self.getFactor(i):
                if not self.dp[i-factor-1]:
                    self.dp[i-1] = True
        # print(self.dp)

    def getFactor(self, n):
        yield 1
        for i in range(2, math.ceil(math.sqrt(n))):
            if n%i==0:
                yield i
                yield n//i

    def divisorGame(self, N: int) -> bool:
        return self.dp[N-1]
```



官方 数学解法，淦！

可发现规律：`N`是`2`的倍数就是`True`，其他情况：`False`（`not N%2`）

```python
class Solution:
    def divisorGame(self, N: int) -> bool:
        return not N%2

```

