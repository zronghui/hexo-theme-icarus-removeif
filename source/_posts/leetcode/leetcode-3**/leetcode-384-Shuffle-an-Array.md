---
title: leetcode 384. Shuffle an Array
date: 2019-11-28 16:19:40
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- 
---
### 难度：Middle

<a href="https://leetcode.com/problems/shuffle-an-array/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/shuffle-an-array/">九章</a>
## 题目描述
Shuffle a set of numbers without duplicates.

**Example:**
        
    // Init an array with set 1, 2, and 3.
    int[] nums = {1,2,3};
    Solution solution = new Solution(nums);
    
    // Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
    solution.shuffle();
    
    // Resets the array back to its original configuration [1,2,3].
    solution.reset();
    
    // Returns the random shuffling of array [1,2,3].
    solution.shuffle();
    


**Tags:** 

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    private int[] origin;
    private int[] array;
    private Random rand = new Random();
    
    public Solution(int[] nums) {
        array = nums;
        origin = array.clone();
    }
    
    
    public int[] reset() {
        array = origin.clone();
        return origin;
    }
    
    
    public int[] shuffle() {
        for(int i=0; i<array.length; i++){
            swap(array, i, rand.nextInt(array.length-i)+i);
        }
        return array;
    }
    private void swap(int[] nums, int a, int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}


```
