---
title: leetcode 242. Valid Anagram
date: 2019-11-26 13:05:22
categories:
- leetcode
toc: true
tags:
- Hash Table
- Sort
---
### 难度：Easy

<a href="https://leetcode.com/problems/valid-anagram/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/valid-anagram/">九章</a>
## 题目描述
Given two strings _s_ and _t  _, write a function to determine if _t_ is an
anagram of _s_.

**Example 1:**
        
    Input: _s_ = "anagram", _t_ = "nagaram"
    Output: true
    

**Example 2:**
        
    Input: _s_ = "rat", _t_ = "car"
    Output: false
    

**Note:**  
You may assume the string contains only lowercase alphabets.

**Follow up:**  
What if the inputs contain unicode characters? How would you adapt your
solution to such case?


**Tags:** Hash Table, Sort

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public boolean isAnagram_0(String s, String t) {
        HashMap<Character, Integer> map = new HashMap<>();
        for(char c: s.toCharArray()){
            if(!map.containsKey(c)){
                map.put(c, 1);
            }else{
                map.put(c, map.get(c)+1);
            }
        }
        for(char c: t.toCharArray()){
            if(!map.containsKey(c)){
                return false;
            }else{
                if(map.get(c)==1){
                    map.remove(c);
                }else{
                    map.put(c, map.get(c)-1);
                }
                
            }
        }
        return map.isEmpty();
    }
    
    public boolean isAnagram(String s, String t) {
        if(s.length()!=t.length()) return false;
        int[] is = new int[256];
        int[] it = new int[256];
        for(char c: s.toCharArray()){
            is[c]++;
        }
        for(char c: t.toCharArray()){
            it[c]++;
        }
        for(int i=0; i<256; i++){
            if(is[i]!=it[i]){
                return false;
            }
        }
        return true;
    }
}
```
