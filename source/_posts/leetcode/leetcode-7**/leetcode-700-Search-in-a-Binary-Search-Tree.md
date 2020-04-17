---
title: leetcode 700. Search in a Binary Search Tree
date: 2019-12-06 20:52:18
categories:
- leetcode
- leetcode-7**
toc: true
tags:
- Tree
---
### 难度：Easy

<a href="https://leetcode.com/problems/search-in-a-binary-search-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/search-in-a-binary-search-tree/">九章</a>
## 题目描述
Given the root node of a binary search tree (BST) and a value. You need to
find the node in the BST that the node's value equals the given value. Return
the subtree rooted with that node. If such node doesn't exist, you should
return NULL.

For example,
        
    Given the tree:
            4
           / \
          2   7
         / \
        1   3
    
    And the value to search: 2
    

You should return this subtree:
        
          2     
         / \   
        1   3
    

In the example above, if we want to search the value `5`, since there is no
node with value `5`, we should return `NULL`.

Note that an empty tree is represented by `NULL`, therefore you would see the
expected output (serialized tree format) as `[]`, not `null`.


**Tags:** Tree

**Difficulty:** Easy
## 答案
<!--more-->
```java

class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root==null || root.val==val) return root;
        if(root.val>val) return searchBST(root.left, val);
        return searchBST(root.right, val);
    }
}
```
