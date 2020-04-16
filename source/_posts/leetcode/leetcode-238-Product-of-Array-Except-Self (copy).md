---
thumbnail:
title: leetcode 238. Product of Array Except Self
date: 2020-04-16 13:14:28
categories:
- leetcode
toc: true
tags:
- Array
recommend: 1
keywords:
uniqueId: '2020-04-16 13:14:28/"leetcode 238. Product of Array Except Self".html'
---

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
<!--more-->

[除自身以外数组的乘积 - 除自身以外数组的乘积 - 力扣（LeetCode）](https://leetcode-cn.com/problems/product-of-array-except-self/solution/chu-zi-shen-yi-wai-shu-zu-de-cheng-ji-by-leetcode/)

方法一：左右乘积列表
我们不必将所有数字的乘积除以给定索引处的数字得到相应的答案，而是可以利用索引处左侧的所有数字乘积和右侧所有数字的乘积相乘得到答案。

对于给定索引 ii，我们将使用它左边所有数字的乘积乘以右边所有数字的乘积。

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        productBefore = [1 for i in nums]
        productAfter = [1 for i in nums]
        for i in range(1, n):
            productBefore[i] = productBefore[i-1]*nums[i-1]
        for i in range(n-2, -1, -1):
            productAfter[i] = productAfter[i+1]*nums[i+1]
        print(productBefore, productAfter)
        return list(i*j for i, j in zip(productBefore, productAfter))
```



```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        length = len(nums)
        L, R, answer = [0]*length, [0]*length, [0]*length
        L[0] = 1
        for i in range(1, length):
            L[i] = nums[i - 1] * L[i - 1]
        R[length - 1] = 1
        for i in reversed(range(length - 1)):
            R[i] = nums[i + 1] * R[i + 1]
```



方法二：空间复杂度 O(1) 的方法
尽管上面的方法已经能够很好的解决这个问题，但是不是常数的空间复杂度。

**由于输出数组不算在空间复杂度内，那么我们可以将 L 或 R 数组在用输出数组来计算，然后再动态构造另一个**



```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [1]*n
        for i in range(1, n):
            res[i] = res[i-1]*nums[i-1]
        R = 1
        for i in reversed(range(n-1)):
            R *= nums[i+1]
            res[i] *= R
        return res
```

