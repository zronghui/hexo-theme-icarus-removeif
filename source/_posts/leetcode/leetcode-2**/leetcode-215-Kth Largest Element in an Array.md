---
title: leetcode 215. Kth Largest Element in an Array
date: 2019-11-21 12:21:31
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Divide and Conquer
- Heap
---
## 题目描述
Find the **k** th largest element in an unsorted array. Note that it is the
kth largest element in the sorted order, not the kth distinct element.

**Example 1:**
        
    Input: [3,2,1,5,6,4] and k = 2
    Output: 5
    

**Example 2:**
        
    Input: [3,2,3,1,2,4,5,5,6] and k = 4
    Output: 4

**Note:**  
You may assume k is always valid, 1 ≤ k ≤ array's length.


**Tags:** Divide and Conquer, Heap

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(k, Collections.reverseOrder());
        for(int num: nums){
            priorityQueue.add(num);
        }
        for(int i=0; i<k-1; i++){
                priorityQueue.poll();
        }
        return priorityQueue.poll();
    }
}
```
