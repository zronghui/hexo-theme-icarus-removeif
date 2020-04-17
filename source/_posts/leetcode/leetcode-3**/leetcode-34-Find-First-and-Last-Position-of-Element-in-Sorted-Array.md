---
title: leetcode 34. Find First and Last Position of Element in Sorted Array
date: 2019-11-24 14:13:35
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Array
- Binary Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/find-first-and-last-position-of-element-in-sorted-array/">九章</a>
## 题目描述
Given an array of integers `nums` sorted in ascending order, find the starting
and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of _O_ (log _n_ ).

If the target is not found in the array, return `[-1, -1]`.

**Example 1:**
        
    Input: nums = [5,7,7,8,8,10], target = 8
    Output: [3,4]

**Example 2:**
        
    Input: nums = [5,7,7,8,8,10], target = 6
    Output: [-1,-1]


**Tags:** Array, Binary Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int[] res = new int[]{-1, -1};
        if(nums==null || nums.length==0) return res;
        // end 为nums.length 为了确保能进while, 如[0]的情况不会进入while
        int start = 0, end = nums.length, mid = start+(end-start)/2;
        // 第一次二分的过程，确定第二次二分的end
        int max = end;
        while(start < end){
            mid = start+(end-start)/2;
            if(nums[mid]==target) {
                if(mid==0 || nums[mid-1]!=target) {
                    res[0] = mid;
                    break;
                }else {
                    end = mid;
                }
            }else if(nums[mid]>target){
                max = mid;
                end = mid;
            }else {
                // 没有target的情况下，直接返回
                if(mid+1>=nums.length || nums[mid+1]>target){
                    return res;
                }
                start = mid;
            }
        }
        end = max;
        while(start < end){
            mid = start+(end-start)/2;
            if(nums[mid]==target) {
                if(mid==nums.length-1 || nums[mid+1]!=target) {
                    res[1] = mid;
                    break;
                }else {
                    start = mid;
                }
            }else if(nums[mid]>target){
                end = mid;
            }else {
                start = mid;
            }
        }
        
        return res;
    }
}
```
