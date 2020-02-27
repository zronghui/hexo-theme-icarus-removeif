---
title: leetcode 278. First Bad Version
date: 2019-12-04 15:37:24
categories:
- leetcode
toc: true
tags:
- Binary Search
---
### 难度：Easy

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
<!--more-->
```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */
// https://leetcode-cn.com/problems/first-bad-version/solution/di-yi-ge-cuo-wu-de-ban-ben-by-leetcode/
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int start = 0, end = n;
        while(start<end) {
            int mid = start+(end-start)/2;
            if(isBadVersion(mid))
                end = mid;
            else
                start = mid+1;
        }
        return start;
    }
}
```
