---
title: leetcode 114. Flatten Binary Tree to Linked List
date: 2019-11-24 10:34:15
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/flatten-binary-tree-to-linked-list/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/flatten-binary-tree-to-linked-list/">九章</a>
## 题目描述
Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:
        
        1
       / \
      2   5
     / \   \
    3   4   6
    

The flattened tree should look like:
        
    1
     \
      2
       \
        3
         \
          4
           \
            5
             \
              6
    


**Tags:** Tree, Depth-first Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public void flatten(TreeNode root) {
        if(root==null || (root.left==null && root.right==null)) return;
        if(root.left!=null) {
            flatten(root.left);
            if(root.right==null){
                root.right = root.left;
            }
            else{
                TreeNode temp = root.right;
                root.right = root.left;
                // 左边的叶子节点
                TreeNode leftBottom = root.left;
                while(leftBottom.right!=null){
                    leftBottom = leftBottom.right;
                }
                leftBottom.right = temp;
                flatten(temp);
            }
            // 记得将左树清空
            root.left = null;
        }else{
            flatten(root.right);
        }
    }
}
```
