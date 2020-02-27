---
title: leetcode 49. Group Anagrams
date: 2019-11-27 09:41:52
categories:
- leetcode
toc: true
tags:
- Hash Table
- String
---
### 难度：Middle

<a href="https://leetcode.com/problems/group-anagrams/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/group-anagrams/">九章</a>
## 题目描述
Given an array of strings, group anagrams together.

**Example:**
        
    Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
    Output:
    [
      ["ate","eat","tea"],
      ["nat","tan"],
      ["bat"]
    ]

**Note:**

  * All inputs will be in lowercase.
  * The order of your output does not matter.


**Tags:** Hash Table, String

**Difficulty:** Medium
## 答案
<!--more-->
```java
// map.putIfAbsent
// map.get(key).add(s)
// new ArrayList<>(map.values())
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<>();
        for(String s: strs){
            char[] cs = s.toCharArray();
            Arrays.sort(cs);
            String key = String.valueOf(cs);
            map.putIfAbsent(key, new ArrayList<String>());
            map.get(key).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
```
