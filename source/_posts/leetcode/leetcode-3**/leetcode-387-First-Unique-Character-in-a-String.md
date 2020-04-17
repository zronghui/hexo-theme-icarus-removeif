---
title: leetcode 387. First Unique Character in a String
date: 2019-11-28 16:38:34
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Hash Table
- String
---
### 难度：Easy

<a href="https://leetcode.com/problems/first-unique-character-in-a-string/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/first-unique-character-in-a-string/">九章</a>
## 题目描述
Given a string, find the first non-repeating character in it and return it's
index. If it doesn't exist, return -1.

**Examples:**
        
    s = "leetcode"
    return 0.
    
    s = "loveleetcode",
    return 2.
    

**Note:** You may assume the string contain only lowercase letters.


**Tags:** Hash Table, String

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public int firstUniqChar(String s) {
        // map记录char出现次数
        int[] map = new int[256];
        // start记录char起始的index
        int[] start = new int[256];
        Arrays.fill(start, -1);
        char[] cs = s.toCharArray();
        for(int i=0; i<cs.length; i++) {
            char c = cs[i];
            map[c] += 1;
            if(start[c]==-1){
                start[c] = i;
            }
        }
        int res = Integer.MAX_VALUE;
        for(int i=0; i<256; i++){
            if(map[i]==1){
                res = Math.min(res, start[i]);
            }
        }
        return res==Integer.MAX_VALUE?-1:res;
    }
}
```
