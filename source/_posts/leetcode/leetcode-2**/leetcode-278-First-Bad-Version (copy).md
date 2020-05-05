---
thumbnail:
title: leetcode 278. First Bad Version
date: 2020-05-05 10:26:02
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Binary Search
recommend: 1
keywords:
uniqueId: '2020-05-05 10:26:02/"leetcode 278. First Bad Version".html'
---

<a href="https://leetcode.com/problems/first-bad-version/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/first-bad-version/">九章</a>
## 题目描述
You are a product manager and currently leading a team to develop a new
product. Unfortunately, the latest version of your product fails the quality
check. Since each version is developed based on the previous version, all the
versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the
first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether
`version` is bad. Implement a function to find the first bad version. You
should minimize the number of calls to the API.

**Example:**
        
    Given n = 5, and version = 4 is the first bad version.
    
    call isBadVersion(3) -> false
    call isBadVersion(5) -> true
    call isBadVersion(4) -> true
    
    Then 4 is the first bad version. 



**Tags:** Binary Search

**Difficulty:** Easy

## 答案
<!--more-->
```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        left, right = 1, n
        while left<right:
            mid = left+(right-left)//2
            # 最后 left right 都指向第一个 badVersion
            if isBadVersion(mid):
                right = mid
            else:
                left = mid + 1
        return left

```
