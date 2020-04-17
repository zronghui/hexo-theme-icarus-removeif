---
title: leetcode 189. Rotate Array
date: 2019-12-03 22:46:03
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Array
---
### 难度：Easy

<a href="https://leetcode.com/problems/rotate-array/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/rotate-array/">九章</a>
## 题目描述
Given an array, rotate the array to the right by _k_ steps, where  _k_  is
non-negative.

**Example 1:**
        
    Input: [1,2,3,4,5,6,7] and _k_ = 3
    Output: [5,6,7,1,2,3,4]
    Explanation:
    rotate 1 steps to the right: [7,1,2,3,4,5,6]
    rotate 2 steps to the right: [6,7,1,2,3,4,5]
    rotate 3 steps to the right: [5,6,7,1,2,3,4]
    

**Example 2:**
        
    Input: [-1,-100,3,99] and _k_ = 2
    Output: [3,99,-1,-100]
    Explanation: 
    rotate 1 steps to the right: [99,-1,-100,3]
    rotate 2 steps to the right: [3,99,-1,-100]
    

**Note:**

  * Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
  * Could you do it in-place with O(1) extra space?


**Tags:** Array

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public void rotate(int[] nums, int k) {
        // [1,2,3,4,5,6,7] k = 3
        // 1. reverse整个数组 [7654321]
        // 2. reverse 123 , reverse 4567
        // [5,6,7,1,2,3,4]
        int n = nums.length;
        k %= n;
        reverse(nums, 0, n-1);
        reverse(nums, 0, k-1);
        reverse(nums, k, n-1);
    }
    private void reverse(int[] nums, int start, int end) {
        while(start<end){
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```
