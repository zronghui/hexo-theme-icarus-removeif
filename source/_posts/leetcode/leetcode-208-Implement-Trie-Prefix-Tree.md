---
title: leetcode 208. Implement Trie (Prefix Tree)
date: 2019-12-02 11:33:21
categories:
- leetcode
toc: true
tags:
- Design
- Trie
---
### 难度：Middle

<a href="https://leetcode.com/problems/implement-trie-prefix-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/implement-trie-prefix-tree/">九章</a>
## 题目描述
Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**
        
    Trie trie = new Trie();
    
    trie.insert("apple");
    trie.search("apple");   // returns true
    trie.search("app");     // returns false
    trie.startsWith("app"); // returns true
    trie.insert("app");   
    trie.search("app");     // returns true
    

**Note:**

  * You may assume that all inputs are consist of lowercase letters `a-z`.
  * All inputs are guaranteed to be non-empty strings.


**Tags:** Design, Trie

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Trie {
    
    class Node {
        boolean hasWord = false;
        char c;
        Map<Character, Node> children = new HashMap<>();
        public Node(){}
        public Node(char c){
            this.c = c;
        }
    }
    
    Node root;
    
    
    public Trie() {
        root = new Node();
    }
    
    
    public void insert(String word) {
        Node cur = root;
        Node temp;
        for(char c: word.toCharArray()) {
            if(!cur.children.containsKey(c)) {
                temp = new Node(c);
                cur.children.put(c, temp);
            }
            cur = cur.children.get(c);
        }
        cur.hasWord = true;
    }
    
    
    public boolean search(String word) {
        Node cur = root;
        for(char c: word.toCharArray()) {
            if(!cur.children.containsKey(c)) {
                return false;
            }else{
                cur = cur.children.get(c);
            }
        }
        return cur.hasWord;
    }
    
    
    public boolean startsWith(String prefix) {
        Node cur = root;
        for(char c: prefix.toCharArray()) {
            if(!cur.children.containsKey(c)) {
                return false;
            }else{
                cur = cur.children.get(c);
            }
        }
        return true;
    }
}


```
