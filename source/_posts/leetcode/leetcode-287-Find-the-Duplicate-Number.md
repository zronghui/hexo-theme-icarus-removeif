---
title: leetcode 287. Find the Duplicate Number
date: 2019-11-27 21:27:08
categories:
- leetcode
toc: true
tags:
- Array
- Two Pointers
- Binary Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/find-the-duplicate-number/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/find-the-duplicate-number/">九章</a>
## 题目描述
Given an array _nums_ containing _n_ \+ 1 integers where each integer is
between 1 and _n_ (inclusive), prove that at least one duplicate number must
exist. Assume that there is only one duplicate number, find the duplicate one.

**Example 1:**
        
    Input: [1,3,4,2,2]
    Output: 2
    

**Example 2:**
        
    Input: [3,1,3,4,2]
    Output: 3

**Note:**

  1. You **must not** modify the array (assume the array is read only).
  2. You must use only constant, _O_ (1) extra space.
  3. Your runtime complexity should be less than _O_ ( _n_ 2).
  4. There is only one duplicate number in the array, but it could be repeated more than once.


**Tags:** Array, Two Pointers, Binary Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int findDuplicate(int[] nums) {
        // 快慢指针，找环
        // 首先找到相遇的位置
        int slow = 0, fast = 0;
        do{
            fast = nums[nums[fast]];
            slow = nums[slow];
        }while(slow!=fast);
        // 再将一个指针移到队首，直至相遇，即为环的起点
        slow = 0;
        while(slow!=fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        return fast;
    }
}
```
