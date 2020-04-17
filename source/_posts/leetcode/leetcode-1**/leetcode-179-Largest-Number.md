---
title: leetcode 179. Largest Number
date: 2019-11-24 13:29:13
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Sort
---
### 难度：Middle

<a href="https://leetcode.com/problems/largest-number/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/largest-number/">九章</a>
## 题目描述
Given a list of non negative integers, arrange them such that they form the
largest number.

**Example 1:**
        
    Input: [10,2]
    Output: "210"

**Example 2:**
        
    Input: [3,30,34,5,9]
    Output: "9534330"
    

**Note:** The result may be very large, so you need to return a string instead
of an integer.


**Tags:** Sort

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public String largestNumber_0(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i=0; i<nums.length; i++){
            strs[i] = Integer.toString(nums[i]);
        }
        PriorityQueue<String> priorityQueue = new PriorityQueue<>(new Comparator<String>(){
            @Override
            public int compare(String s1, String s2){
                return (s2+s1).compareTo(s1+s2);
            }
        });
        for(int i=0; i<nums.length; i++) {
            priorityQueue.add(strs[i]);
        }
        StringBuilder sb = new StringBuilder();
        for(int i=0; i<nums.length; i++) {
            sb.append(priorityQueue.remove());
        }
        if(sb.toString().startsWith("0")) return "0";
        return sb.toString();
        
    }
    
    public String largestNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for(int i=0; i<nums.length; i++){
            strs[i] = Integer.toString(nums[i]);
        }
        Arrays.sort(strs, new Comparator<String>(){
            @Override
            public int compare(String s1, String s2){
                return (s2+s1).compareTo(s1+s2);
            }
        });
        StringBuilder sb = new StringBuilder();
        for(int i=0; i<nums.length; i++) {
            sb.append(strs[i]);
        }
        if(sb.toString().startsWith("0")) return "0";
        return sb.toString();
        
    }
}
```
