---
title: leetcode 150. Evaluate Reverse Polish Notation
date: 2019-12-06 19:35:17
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Stack
---
### 难度：Middle

<a href="https://leetcode.com/problems/evaluate-reverse-polish-notation/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/evaluate-reverse-polish-notation/">九章</a>
## 题目描述
Evaluate the value of an arithmetic expression in [Reverse Polish
Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or
another expression.

**Note:**

  * Division between two integers should truncate toward zero.
  * The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example 1:**
        
    Input: ["2", "1", "+", "3", "*"]
    Output: 9
    Explanation: ((2 + 1) * 3) = 9
    

**Example 2:**
        
    Input: ["4", "13", "5", "/", "+"]
    Output: 6
    Explanation: (4 + (13 / 5)) = 6
    

**Example 3:**
        
    Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
    Output: 22
    Explanation: 
      ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
    = ((10 * (6 / (12 * -11))) + 17) + 5
    = ((10 * (6 / -132)) + 17) + 5
    = ((10 * 0) + 17) + 5
    = (0 + 17) + 5
    = 17 + 5
    = 22
    


**Tags:** Stack

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        for(String s: tokens) {
            if(!isOperator(s)){
                stack.push(Integer.parseInt(s));
            }else{
                stack.push(operate(stack.pop(), stack.pop(), s));
            }
        }
        return stack.pop();
    }
    private boolean isOperator(String s){
        return s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/");
    }
    private int operate(int b, int a, String operator) {
        int res = 0;
        if(operator.equals("+")){
            res = a+b;
        }else if(operator.equals("-")) {
            res = a-b;
        }else if(operator.equals("*")) {
            res = a*b;
        }else if(operator.equals("/")) {
            res = a/b;
        }
        return res;
    }
}
```
