---
title: leetcode 229. Majority Element II
date: 2019-12-06 20:13:25
categories:
- leetcode
toc: true
tags:
- Array
---
### 难度：Middle

<a href="https://leetcode.com/problems/majority-element-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/majority-element-ii/">九章</a>
## 题目描述
Given an integer array of size _n_ , find all elements that appear more than
`⌊ n/3 ⌋` times.

**Note:** The algorithm should run in linear time and in O(1) space.

**Example 1:**
        
    Input: [3,2,3]
    Output: [3]

**Example 2:**
        
    Input: [1,1,1,3,3,2,2,2]
    Output: [1,2]


**Tags:** Array

**Difficulty:** Medium
## 答案
<!--more-->
```java
// https://leetcode-cn.com/problems/majority-element-ii/solution/duo-shu-tou-piao-de-sheng-ji-ban-hao-li-jie-java-b/
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> res = new ArrayList<>();
        if(nums==null || nums.length==0){
            return res;
        }
        int major1=nums[0], major2=nums[0], count1=0, count2=0;
        for(int n: nums) {
            if(major1==n){
                count1++;
            }else if(major2==n){
                count2++;
            }else if(count1==0){
                major1 = n;
                count1++;
            }else if(count2==0){
                major2 = n;
                count2++;
            }else{
                count1--;
                count2--;
            }
        }
        count1 = 0;
        count2 = 0;
        for(int n: nums) {
            if(n==major1) count1++;
            else if(n==major2) count2++;
        }
        if(count1>nums.length/3) {
            res.add(major1);
        }
        if(count2>nums.length/3){
            res.add(major2);
        }
        return res;
    }
}
```
