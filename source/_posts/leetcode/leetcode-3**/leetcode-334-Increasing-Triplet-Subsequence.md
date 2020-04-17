---
title: leetcode 334. Increasing Triplet Subsequence
date: 2019-12-03 23:45:57
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- 
---
### 难度：Middle

<a href="https://leetcode.com/problems/increasing-triplet-subsequence/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/increasing-triplet-subsequence/">九章</a>
## 题目描述
Given an unsorted array return whether an increasing subsequence of length 3
exists or not in the array.

Formally the function should:

> Return true if there exists _i, j, k_  
>  such that _arr[i]_ < _arr[j]_ < _arr[k]_ given 0 ≤ _i_ < _j_ < _k_ ≤ _n_ -1
> else return false.

**Note:** Your algorithm should run in O( _n_ ) time complexity and O( _1_ )
space complexity.

**Example 1:**
        
    Input: [1,2,3,4,5]
    Output: true
    

**Example 2:**
        
    Input: [5,4,3,2,1]
    Output: false
    


**Tags:** 

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int one = Integer.MAX_VALUE, two = one;
        for(int n: nums){
            if(n<=one){
                one = n;
            }else if(n<=two) {
                two = n;
            }else{
                return true;
            }
        }
        return false;
    }
}
```
