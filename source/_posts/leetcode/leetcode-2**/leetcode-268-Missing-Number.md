---
title: leetcode 268. Missing Number
date: 2019-11-29 11:19:06
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Array
- Math
- Bit Manipulation
---
### 难度：Easy

<a href="https://leetcode.com/problems/missing-number/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/missing-number/">九章</a>
## 题目描述
Given an array containing _n_ distinct numbers taken from `0, 1, 2, ..., n`,
find the one that is missing from the array.

**Example 1:**
        
    Input: [3,0,1]
    Output: 2
    

**Example 2:**
        
    Input: [9,6,4,2,3,5,7,0,1]
    Output: 8
    

**Note** :  
Your algorithm should run in linear runtime complexity. Could you implement it
using only constant extra space complexity?


**Tags:** Array, Math, Bit Manipulation

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public int missingNumber(int[] nums) {
        for(int i=0; i<nums.length; i++){
            while(i!=nums[i]){
                if(nums[i]>=nums.length || nums[i]<0) break;
                swap(nums, i, nums[i]);
            }
        }
        for(int i=0; i<nums.length; i++){
            if(i!=nums[i]){
                return i;
            }
        }
        return nums.length;
    }
    private void swap(int[] nums, int a, int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```
