---
thumbnail:
title: leetcode 525. Contiguous Array
date: 2020-04-13 17:34:46
categories:
- leetcode
- leetcode-5**
toc: true
tags:
- Hash Table
recommend: 1
keywords:
uniqueId: '2020-04-13 17:34:46/"leetcode 525. Contiguous Array".html'
---

<a href="https://leetcode.com/problems/contiguous-array/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/contiguous-array/">九章</a>
## 题目描述
Given a binary array, find the maximum length of a contiguous subarray with
equal number of 0 and 1.

**Example 1:**  
        
    Input: [0,1]
    Output: 2
    Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.


**Example 2:**  
        
    Input: [0,1,0]
    Output: 2
    Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.


**Note:** The length of the given binary array will not exceed 50,000.


**Tags:** Hash Table

**Difficulty:** Medium

## 答案
<!--more-->
```python
class Solution:
    def findMaxLength(self, nums: List[int]) -> int:
        res = 0
        count = 0
        m = {0: -1}
        # 遇到 1 的时候， count 变量加 1 ，遇到 0 的时候 count 变量减 1
        # 如果我们在遍历数组的过程汇中遇到了相同的 count 2 次，这意味着这两个位置之间 0 和 1 的数目一样多
        # 因此我们只需要使用一个 HashMap来保存所有的 (index, count)对。对于一个 count ，我们只记录它第一次出现的位置，后面遇到相同的 count我们都用这个第一次记录的 index 来计算子数组的长度
        for i, v in enumerate(nums):
            if not v:
                count -= 1
            else:
                count += 1
            if count in m:
                res = max(res, i-m[count])
            else:
                m[count] = i
        return res
```
