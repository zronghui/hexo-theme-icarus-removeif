---
title: leetcode 653. Two Sum IV - Input is a BST
date: 2019-12-07 19:35:40
categories:
- leetcode
- leetcode-6**
toc: true
tags:
- Tree
---
### 难度：Easy

<a href="https://leetcode.com/problems/two-sum-iv-input-is-a-bst/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/two-sum-iv-input-is-a-bst/">九章</a>
## 题目描述
Given a Binary Search Tree and a target number, return true if there exist two
elements in the BST such that their sum is equal to the given target.

**Example 1:**
        
    Input: 
        5
       / \
      3   6
     / \   \
    2   4   7
    
    Target = 9
    
    Output: True
    



**Example 2:**
        
    Input: 
        5
       / \
      3   6
     / \   \
    2   4   7
    
    Target = 28
    
    Output: False
    




**Tags:** Tree

**Difficulty:** Easy
## 答案
<!--more-->
```java

class Solution {
    public boolean findTarget(TreeNode root, int k) {
        HashSet<Integer> set = new HashSet<>();
        return helper(root, k, set);
    }
    private boolean helper(TreeNode root, int k, HashSet<Integer> set){
        if(root==null) return false;
        if(set.contains(k-root.val)) {
            return true;
        }
        else{
            set.add(root.val);
        }
        return helper(root.left, k, set) || helper(root.right, k, set);
    }
}
```
