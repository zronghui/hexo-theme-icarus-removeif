---
title: leetcode 42. Trapping Rain Water
date: 2019-11-29 21:34:58
categories:
- leetcode
- leetcode-4**
toc: true
tags:
- Array
- Two Pointers
- Stack
---
### 难度：Hard

<a href="https://leetcode.com/problems/trapping-rain-water/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/trapping-rain-water/">九章</a>
## 题目描述
Given _n_ non-negative integers representing an elevation map where the width
of each bar is 1, compute how much water it is able to trap after raining.

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)  
The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In
this case, 6 units of rain water (blue section) are being trapped. **Thanks
Marcos** for contributing this image!

**Example:**
        
    Input: [0,1,0,2,1,0,1,3,2,1,2,1]
    Output: 6


**Tags:** Array, Two Pointers, Stack

**Difficulty:** Hard
## 答案
<!--more-->
```java
// https://leetcode-cn.com/problems/trapping-rain-water/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-8/
// 解法3
class Solution {
    public int trap(int[] h) {
        int n = h.length;
        if(n<2) return 0;
        int[] left = new int[n];
        int[] right = new int[n];
        left[0] = 0;
        right[n-1] = 0;
        for(int i=1; i<n; i++){
            left[i] = Math.max(left[i-1], h[i-1]);
        }
        for(int i=n-2; i>=0; i--){
            right[i] = Math.max(right[i+1], h[i+1]);
        }
        int res = 0;
        for(int i=1; i<n; i++){
            int min = Math.min(left[i], right[i]);
            if(h[i]>=min) continue;
            res += min-h[i];
        }
        return res;
    }
}
```
