---
title: leetcode 409. Longest Palindrome
date: 2019-12-01 11:47:10
categories:
- leetcode
toc: true
tags:
- Hash Table
---
### 难度：Easy

<a href="https://leetcode.com/problems/longest-palindrome/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/longest-palindrome/">九章</a>
## 题目描述
Given a string which consists of lowercase or uppercase letters, find the
length of the longest palindromes that can be built with those letters.

This is case sensitive, for example `"Aa"` is not considered a palindrome
here.

**Note:**  
Assume the length of given string will not exceed 1,010.

**Example:**
        
    Input:
    "abccccdd"
    
    Output:
    7
    
    Explanation:
    One longest palindrome that can be built is "dccaccd", whose length is 7.
    


**Tags:** Hash Table

**Difficulty:** Easy
## 答案
<!--more-->
```java
// map -> int[256]
// 6ms -> 1ms

// map iter
// for(Map.Entry<A, B> entry: map.entrySet())
// for(A key: map.keySet())
// for(B value: map.values())
class Solution {
    public int longestPalindrome(String s) {
        if(s==null || s.length()==0) return 0;
        // Map<Character, Integer> map = new HashMap<>();
        int[] map = new int[256];
        for(char c: s.toCharArray()) {
            // if(!map.containsKey(c)){
            //     map.put(c, 1);
            // }else{
            //     map.put(c, 1+map.get(c));
            // }
            map[c]++;
        }
        int res = 0;
        // map的value中是否有奇数
        boolean one = false;
        // for(Integer value: map.values()) {
        for(int value: map) {
            if(value%2==1){
                one = true;
                res += value-1;
            }else{
                res += value;
            }
            
        }
        return res+(one?1:0);
    }
}
```
