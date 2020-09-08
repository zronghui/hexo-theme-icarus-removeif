---
thumbnail:
title: leetcode 679. 24 Game
date: 2020-08-22 07:48:43
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Depth-first Search
recommend: 1
keywords:
uniqueId: '2020-08-22 07:48:43/"leetcode 679. 24 Game".html'
---

<a href="https://leetcode.com/problems/24-game/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/24-game/">九章</a>
## 题目描述
You have 4 cards each containing a number from 1 to 9. You need to judge
whether they could operated through `*`, `/`, `+`, `-`, `(`, `)` to get the
value of 24.

**Example 1:**  
        
    Input: [4, 1, 8, 7]
    Output: True
    Explanation: (8-4) * (7-1) = 24


**Example 2:**  
        
    Input: [1, 2, 1, 2]
    Output: False


**Note:**  

  1. The division operator `/` represents real division, not integer division. For example, 4 / (1 - 2/3) = 12.
  2. Every operation done is between two numbers. In particular, we cannot use `-` as a unary operator. For example, with `[1, 1, 1, 1]` as input, the expression `-1 - 1 - 1 - 1` is not allowed.
  3. You cannot concatenate numbers together. For example, if the input is `[1, 2, 1, 2]`, we cannot write this as 12 + 12.


**Tags:** Depth-first Search

**Difficulty:** Hard

## 答案
<!--more-->
```python
from operator import add, sub, mul, truediv

class Solution:
    def judgePoint24(self, nums: List[int]) -> bool:
        @functools.lru_cache(None)
        def dfs(nums):
            if len(nums)==1:
                print(nums)
                return math.isclose(nums[0], 24)
            # 选定前 2 个进行运算
            (a, b), nums = nums[:2], nums[2:]
            ops = [add, sub, mul, truediv]
            if not b: ops.pop()
            _abs = [op(a, b) for op in ops]
            # 把 ab 运算的结果插进 剩余数的可能位置
            return any(
                dfs(nums[:i]+(ab,)+nums[i:])
                for ab in _abs
                for i in range(len(nums)+1)
            )
        return any(dfs(permu) for permu in itertools.permutations(nums, 4))
```
