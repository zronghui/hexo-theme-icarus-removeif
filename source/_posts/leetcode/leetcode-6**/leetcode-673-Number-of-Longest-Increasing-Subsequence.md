---
title: leetcode 673. Number of Longest Increasing Subsequence
date: 2019-11-30 23:18:46
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Dynamic Programming
---
### 难度：Middle

<a href="https://leetcode.com/problems/number-of-longest-increasing-subsequence/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/number-of-longest-increasing-subsequence/">九章</a>
## 题目描述
Given an unsorted array of integers, find the number of longest increasing
subsequence.

**Example 1:**  
        
    Input: [1,3,5,4,7]
    Output: 2
    Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
    

**Example 2:**  
        
    Input: [2,2,2,2,2]
    Output: 5
    Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
    

**Note:** Length of the given array will be not exceed 2000 and the answer is
guaranteed to be fit in 32-bit signed int.


**Tags:** Dynamic Programming

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        // keep 两个数组，一个记录当前的count，一个记录最长路径长度
        // if lis[i] == lis[j] + 1, count[i] += count[j]
        // 因为除了原有的到达当前lis的路径数量，还有count[i]种方法可以到达。
        // if lis[i] < lis[j] + 1，直接换为count[j]，
        // 因为首次发现了自己的最长路径
        // 最后keep一个max，然后loop找有几个是max，加起来count
        if(nums==null || nums.length==0) return 0;
        int n = nums.length;
        if(n==1) return 1;
        int[] len = new int[n];
        Arrays.fill(len, 1);
        int[] count = new int[n];
        Arrays.fill(count, 1);
        int max = 1;
        for(int i=1; i<n; i++) {
            for(int j=0; j<i; j++){
                if(nums[j]<nums[i]){
                    if(len[i]<len[j]+1) {
                        len[i] = len[j]+1;
                        count[i] = count[j];
                    }else if(len[i]==len[j]+1) {
                        count[i] += count[j];
                    }
                }
            }
            max = Math.max(max, len[i]);
        }
        int res = 0;
        for(int i=0; i<n; i++) {
            if(len[i]==max){
                res+=count[i];
            }
        }
        return res;
    }
}
```
