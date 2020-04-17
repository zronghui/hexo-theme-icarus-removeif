---
title: leetcode 128. Longest Consecutive Sequence
date: 2019-11-30 20:48:11
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Array
- Union Find
---
### 难度：Hard

<a href="https://leetcode.com/problems/longest-consecutive-sequence/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/longest-consecutive-sequence/">九章</a>
## 题目描述
Given an unsorted array of integers, find the length of the longest
consecutive elements sequence.

Your algorithm should run in O( _n_ ) complexity.

**Example:**
        
    Input:  [100, 4, 200, 1, 3, 2]
    Output: 4
    Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
    


**Tags:** Array, Union Find

**Difficulty:** Hard
## 答案
<!--more-->
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int num: nums){
            set.add(num);
        }
        int res = 0;
        for(int num: nums){
            // 只考虑连续序列起始数字
            if(set.contains(num-1)){
                continue;
            }
            int count = 0;
            int cur = num;
            while(set.contains(cur++)) {
                count++;
            }
            res = Math.max(res, count);
        }
        return res;
    }
}
```
