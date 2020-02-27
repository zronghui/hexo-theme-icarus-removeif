---
title: leetcode 238. Product of Array Except Self
date: 2019-11-26 09:01:34
categories:
- leetcode
toc: true
tags:
- Array
---
### 难度：Middle

<a href="https://leetcode.com/problems/product-of-array-except-self/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/product-of-array-except-self/">九章</a>
## 题目描述
Given an array `nums` of _n_ integers where _n_ > 1,  return an array `output`
such that `output[i]` is equal to the product of all the elements of `nums`
except `nums[i]`.

**Example:**
        
    Input:  [1,2,3,4]
    Output: [24,12,8,6]
    

**Note:** Please solve it **without division** and in O( _n_ ).

**Follow up:**  
Could you solve it with constant space complexity? (The output array **does
not** count as extra space for the purpose of space complexity analysis.)


**Tags:** Array

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        //  1  2 3 4
        //  1  1 2 6  从左往右累乘前面的数
        // 24 12 8 6  从右往左累乘后面的数
        int n = nums.length;
        int[] res = new int[n];
        res[0] = 1;
        for(int i=1; i<n; i++){
            res[i] = res[i-1]*nums[i-1];
        }
        int right = nums[n-1];
        for(int i=n-2; i>=0; i--){
            res[i] = res[i] * right;
            right *= nums[i];
        }
        return res;
    }
}
```
