---
title: leetcode 3. Longest Substring Without Repeating Characters
date: 2019-12-04 17:39:21
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Hash Table
- Two Pointers
- String
- Sliding Window
---
### 难度：Middle

<a href="https://leetcode.com/problems/longest-substring-without-repeating-characters/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/longest-substring-without-repeating-characters/">九章</a>
## 题目描述
Given a string, find the length of the **longest substring** without repeating
characters.

**Example 1:**
        
    Input: "abcabcbb"
    Output: 3 
    Explanation: The answer is "abc", with the length of 3. 
    

**Example 2:**
        
    Input: "bbbbb"
    Output: 1
    Explanation: The answer is "b", with the length of 1.
    

**Example 3:**
        
    Input: "pwwkew"
    Output: 3
    Explanation: The answer is "wke", with the length of 3. 
                 Note that the answer must be a **substring** , "pwke" is a _subsequence_ and not a substring.
    


**Tags:** Hash Table, Two Pointers, String, Sliding Window

**Difficulty:** Medium
## 答案
<!--more-->
```java
// https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/wu-zhong-fu-zi-fu-de-zui-chang-zi-chuan-by-leetcod/
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int res=0;
        int[] map = new int[128]; // char: index+1
        for(int i=0, j=0; j<s.length(); j++) {
            char c = s.charAt(j);
            // 若c在map中, i是c的索引加一
            // 不能用map[c]+1,因为会比i=0大
            i = Math.max(map[c], i);
            map[c] = j+1;
            res = Math.max(res, j-i+1);
        }
        return res;
    }
}

```
