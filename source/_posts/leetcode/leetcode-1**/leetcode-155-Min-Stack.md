---
thumbnail:
title: leetcode 155. Min Stack
date: 2020-04-15 09:58:44
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Stack
- Design
recommend: 1
keywords:
uniqueId: '2020-04-15 09:58:44/"leetcode 155. Min Stack".html'
---

<a href="https://leetcode.com/problems/min-stack/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/min-stack/">九章</a>
## 题目描述
Design a stack that supports push, pop, top, and retrieving the minimum
element in constant time.

  * push(x) -- Push element x onto stack.
  * pop() -- Removes the element on top of the stack.
  * top() -- Get the top element.
  * getMin() -- Retrieve the minimum element in the stack.



**Example:**
        
    MinStack minStack = new MinStack();
    minStack.push(-2);
    minStack.push(0);
    minStack.push(-3);
    minStack.getMin();   --> Returns -3.
    minStack.pop();
    minStack.top();      --> Returns 0.
    minStack.getMin();   --> Returns -2.
    




**Tags:** Stack, Design

**Difficulty:** Easy

## 答案
<!--more-->
```java
import sys

class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.m = sys.maxsize
        self.minl = []
        self.l = []
        

    def push(self, x: int) -> None:
        self.l.append(x)
        self.m = min(x, self.m)
        self.minl.append(self.m)

    def pop(self) -> None:
        self.l.pop()
        self.minl.pop()
        if self.minl:
            self.m = self.minl[-1]
        else:
            self.m = sys.maxsize
        

    def top(self) -> int:
        return self.l[-1]
        

    def getMin(self) -> int:
        return self.minl[-1]
        


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```
