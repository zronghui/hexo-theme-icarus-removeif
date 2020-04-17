---
title: leetcode 118. Pascal's Triangle
date: 2019-11-29 13:03:20
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Hash Table
- Two Pointers
- Binary Search
- Sort
---
### 难度：Easy

<a href="https://leetcode.com/problems/pascals-triangle/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/pascals-triangle/">九章</a>
## 题目描述
Given two arrays, write a function to compute their intersection.

**Example 1:**
        
    Input: nums1 = [1,2,2,1], nums2 = [2,2]
    Output: [2,2]
    

**Example 2:**
        
    Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
    Output: [4,9]

**Note:**

  * Each element in the result should appear as many times as it shows in both arrays.
  * The result can be in any order.

**Follow up:**

  * What if the given array is already sorted? How would you optimize your algorithm?
  * What if _nums1_ 's size is small compared to _nums2_ 's size? Which algorithm is better?
  * What if elements of _nums2_ are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?


**Tags:** Hash Table, Two Pointers, Binary Search, Sort

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public List<List<Integer>> generate(int n) {
        List<List<Integer>> res = new ArrayList<>();
        if(n==0) return res;
        List<Integer> l = new ArrayList<>();
        l.add(1);
        res.add(new ArrayList<>(l));
        if(n==1) {
            return res;
        }
        l.add(1);
        res.add(new ArrayList<>(l));
        if(n==2) {
            return res;
        }
        for(int i=3; i<=n; i++) {
            List<Integer> last = res.get(res.size()-1);
            l.clear();
            l.add(1);
            // i=3->[1]     
            for(int j=1; j<=i-2; j++){
                l.add(last.get(j-1)+last.get(j));
            }
            l.add(1);
            res.add(new ArrayList<>(l));
        }
        return res;
    }
}
```
