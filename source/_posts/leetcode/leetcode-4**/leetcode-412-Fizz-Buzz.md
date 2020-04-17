---
title: leetcode 412. Fizz Buzz
date: 2019-11-22 12:13:01
categories:
- leetcode
- leetcode-4**
toc: true
tags:
- 
---
### 难度：Easy

<a href="https://leetcode.com/problems/fizz-buzz/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/fizz-buzz/">九章</a>
## 题目描述
Write a program that outputs the string representation of numbers from 1 to
_n_.

But for multiples of three it should output “Fizz” instead of the number and
for the multiples of five output “Buzz”. For numbers which are multiples of
both three and five output “FizzBuzz”.

**Example:**
        
    n = 15,
    
    Return:
    [
        "1",
        "2",
        "Fizz",
        "4",
        "Buzz",
        "Fizz",
        "7",
        "8",
        "Fizz",
        "Buzz",
        "11",
        "Fizz",
        "13",
        "14",
        "FizzBuzz"
    ]
    


**Tags:** 

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>();
        for(int i=0; i<n; i++){
            if((i+1)%15==0) res.add("FizzBuzz");
            else if((i+1)%5==0) res.add("Buzz");
            else if((i+1)%3==0) res.add("Fizz");
            else res.add(String.valueOf(i+1));
        }
        return res;
    }
}
```
