---
thumbnail:
title: leetcode 224. Basic Calculator
date: 2020-07-21 13:00:37
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Math
- Stack
recommend: 1
keywords:
uniqueId: '2020-07-21 13:00:37/"leetcode 224. Basic Calculator".html'

---

<a href="https://leetcode.com/problems/basic-calculator/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/basic-calculator/">九章</a>

## 题目描述

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the
plus `+` or minus sign `-`, **non-negative** integers and empty spaces ` `.

**Example 1:**
        

    Input: "1 + 1"
    Output: 2


**Example 2:**
        

    Input: " 2-1 + 2 "
    Output: 3

**Example 3:**
        

    Input: "(1+(4+5+2)-3)+(6+8)"
    Output: 23

**Note:**

  * You may assume that the given expression is always valid.
  * **Do not** use the `eval` built-in library function.

展开：

**Difficulty:** Hard

## 答案

<!--more-->

```python
class Solution:
    def calculate(self, _s: str) -> int:

        def helper(tl):
            if len(tl)==1: return int(tl[0])
            # print('tl:', tl)
            cur = int(tl.pop())
            while tl:
                op, num = tl.pop(), int(tl.pop())
                cur = cur-num if op=='-' else cur+num
            return cur

        stack = []
        s = ''
        for i, c in enumerate(_s):
            if c==' ': continue
            if c in '+-':
                if s: stack.append(s)
                stack.append(c)
                s = ''
            elif c=='(': stack.append('(')
            elif c==')':
                if s: stack.append(s)
                s = ''
                tl = []
                # 3-2-1 -> 1-2-3 -> 3-2-1
                while True:
                    cur = stack.pop()
                    if cur=='(': break
                    tl.append(cur)
                stack.append(helper(tl))
            else:
                s += c
            # print(c, stack)
        if s: stack.append(s)
        # 最外层可以没有括号
        return helper(stack[::-1])
        
```
