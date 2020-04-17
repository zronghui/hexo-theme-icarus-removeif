---
title: leetcode 217. Contains Duplicate
date: 2019-11-21 12:21:31
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Array
- Hash Table
---
## 题目描述
Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the
array, and it should return false if every element is distinct.

**Example 1:**
        
    Input: [1,2,3,1]
    Output: true

**Example 2:**
        
    Input: [1,2,3,4]
    Output: false

**Example 3:**
        
    Input: [1,1,1,3,3,4,3,2,4,2]
    Output: true


**Tags:** Array, Hash Table

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for(int num: nums){
            if(!set.add(num)){
                return true;
            }
        }
        return false;
    }
}
```
