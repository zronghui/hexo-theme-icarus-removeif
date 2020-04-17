---
title: leetcode 341. Flatten Nested List Iterator
date: 2019-11-28 19:17:24
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Stack
- Design
---
### 难度：Middle

<a href="https://leetcode.com/problems/flatten-nested-list-iterator/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/flatten-nested-list-iterator/">九章</a>
## 题目描述
Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be
integers or other lists.

**Example 1:**
        
    Input: [[1,1],2,[1,1]]
    Output: [1,1,2,1,1]
    Explanation: By calling _next_ repeatedly until _hasNext_ returns false, 
                 the order of elements returned by _next_ should be: [1,1,2,1,1].

**Example 2:**
        
    Input: [1,[4,[6]]]
    Output: [1,4,6]
    Explanation: By calling _next_ repeatedly until _hasNext_ returns false, 
                 the order of elements returned by _next_ should be: [1,4,6].
    


**Tags:** Stack, Design

**Difficulty:** Medium
## 答案
<!--more-->
```java

public class NestedIterator implements Iterator<Integer> {
    
    private Stack<NestedInteger> stack = new Stack<>();
    
    public NestedIterator(List<NestedInteger> l) {
        for(int i=l.size()-1; i>=0; i--){
            stack.push(l.get(i));
            // System.out.println("push");
        }
    }

    @Override
    public Integer next() {
        if (!hasNext()) {
            return null;
        }
        return stack.pop().getInteger();
    }

    @Override
    public boolean hasNext() {
        while(!stack.isEmpty() && !stack.peek().isInteger()){
            List<NestedInteger> l = stack.pop().getList();
            for(int i=l.size()-1; i>=0; i--){
                stack.push(l.get(i));
            }
        }
        return !stack.isEmpty();
    }
}


```
