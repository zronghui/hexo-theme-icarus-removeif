---
title: leetcode 347. Top K Frequent Elements
date: 2019-11-25 10:35:00
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Hash Table
- Heap
---
### 难度：Middle

<a href="https://leetcode.com/problems/top-k-frequent-elements/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/top-k-frequent-elements/">九章</a>
## 题目描述
Given a non-empty array of integers, return the **_k_** most frequent
elements.

**Example 1:**
        
    Input: nums = [1,1,1,2,2,3], k = 2
    Output: [1,2]
    

**Example 2:**
        
    Input: nums = [1], k = 1
    Output: [1]

**Note:**

  * You may assume _k_ is always valid, 1 ≤ _k_ ≤ number of unique elements.
  * Your algorithm's time complexity **must be** better than O( _n_ log _n_ ), where _n_ is the array's size.


**Tags:** Hash Table, Heap

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        if(nums==null || nums.length==0) return null;
        List<Integer> res = new ArrayList<>();
        
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int num: nums){
            if(!map.containsKey(num)){
                map.put(num, 1);
            }else{
                map.put(num, 1+map.get(num));
            }
        }
        PriorityQueue<Map.Entry<Integer, Integer>> priorityQueue = 
            new PriorityQueue<>(k, (j, i)->(i.getValue() - j.getValue()));
        for(Map.Entry<Integer, Integer> entry: map.entrySet()){
            priorityQueue.add(entry);
        }
        // 为什么取出了所有的值？
        // while(!priorityQueue.isEmpty()){
        //     res.add(priorityQueue.poll().getKey());
        // }
        for(int i=0; i<k; i++){
            res.add(priorityQueue.poll().getKey());
        }
        return res;
    }
}
```
