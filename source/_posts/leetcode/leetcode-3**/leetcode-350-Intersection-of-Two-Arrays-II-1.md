---
title: leetcode 350. Intersection of Two Arrays II
date: 2019-11-29 13:05:23
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

<a href="https://leetcode.com/problems/intersection-of-two-arrays-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/intersection-of-two-arrays-ii/">九章</a>
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
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> m = new HashMap<>();
        List<Integer> res;
        
        for(int n: nums1){
            if(!m.containsKey(n)){
                m.put(n, 1);
            }else{
                m.put(n, m.get(n)+1);
            }
        }
        res = new ArrayList<>();
        for(int n: nums2){
            if(m.containsKey(n)){
                res.add(n);
                int num = m.get(n);
                if(num==1){
                    m.remove(n);
                }else{
                    m.put(n, num-1);
                }
            }
            if(m.isEmpty()) break;
        }
        
        int[] ress = new int[res.size()];
        for(int i=0; i<res.size(); i++){
            ress[i] = res.get(i);
        }
        return ress;
    }
}
```
