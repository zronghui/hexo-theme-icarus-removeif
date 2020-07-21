---
thumbnail:
title: leetcode 241. Different Ways to Add Parentheses
date: 2020-07-21 12:10:35
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Divide and Conquer
recommend: 1
keywords:
uniqueId: '2020-07-21 12:10:35/"leetcode 241. Different Ways to Add Parentheses".html'
---

<a href="https://leetcode.com/problems/different-ways-to-add-parentheses/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/different-ways-to-add-parentheses/">九章</a>
## 题目描述
Given a string of numbers and operators, return all possible results from
computing all the different possible ways to group numbers and operators. The
valid operators are `+`, `-` and `*`.

**Example 1:**
        
    Input: "2-1-1"
    Output: [0, 2]
    Explanation:
    ((2-1)-1) = 0 
    (2-(1-1)) = 2

**Example 2:**
        
    Input:"2*3-4*5"
    Output: [-34, -14, -10, -10, 10]
    Explanation: (2*(3-(4*5))) = -34 
    ((2*3)-(4*5)) = -14 
    ((2*(3-4))*5) = -10 
    (2*((3-4)*5)) = -10 
    (((2*3)-4)*5) = 10 ****


**Tags:** Divide and Conquer

**Difficulty:** Medium

## 答案
<!--more-->

精简版本：

```python
class Solution:
    def diffWaysToCompute(self, s: str) -> List[int]:
        if s.isdigit(): return [int(s)]
        res = []
        for i, c in enumerate(s):
            if c in '+-*':
                for left in self.diffWaysToCompute(s[:i]):
                    for right in self.diffWaysToCompute(s[i+1:]):
                        res.append(eval(f'{left}{c}{right}'))
        return res

```

展开：

```python
class Solution:
    def diffWaysToCompute(self, input: str) -> List[int]:
        # 如果只有数字，直接返回
        if input.isdigit():
            return [int(input)]

        res = []
        for i, char in enumerate(input):
            if char in ['+', '-', '*']:
                # 1.分解：遇到运算符，计算左右两侧的结果集
                # 2.解决：diffWaysToCompute 递归函数求出子问题的解
                left = self.diffWaysToCompute(input[:i])
                right = self.diffWaysToCompute(input[i+1:])
                # 3.合并：根据运算符合并子问题的解
                for l in left:
                    for r in right:
                        if char == '+':
                            res.append(l + r)
                        elif char == '-':
                            res.append(l - r)
                        else:
                            res.append(l * r)

        return res

```
