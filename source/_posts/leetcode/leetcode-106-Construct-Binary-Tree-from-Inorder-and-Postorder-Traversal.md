---
title: leetcode 106. Construct Binary Tree from Inorder and Postorder Traversal
date: 2019-11-22 23:51:49
categories:
- leetcode
toc: true
tags:
- Array
- Tree
- Depth-first Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/construct-binary-tree-from-inorder-and-postorder-traversal/">九章</a>
## 题目描述
Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**  
You may assume that duplicates do not exist in the tree.

For example, given
        
    inorder = [9,3,15,20,7]
    postorder = [9,15,7,20,3]

Return the following binary tree:
        
        3
       / \
      9  20
        /  \
       15   7
    


**Tags:** Array, Tree, Depth-first Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if(postorder.length<1) return null;
        return helper(inorder, postorder, 0, inorder.length-1, 0, postorder.length-1);
    }
    
    private TreeNode helper(int[] inorder, int[] postorder, int instart, int inend, int poststart, int postend){
        TreeNode root = new TreeNode(postorder[postend]);
        if(instart==inend){
            return root;
        }
        // root 在inorder的index
        int index = instart;
        for(; index<=inend; index++){
            if(inorder[index]==root.val){
                break;
            }
        }
        if(index==instart){
            // 没有左节点
            root.left = null;
            root.right = helper(inorder, postorder, instart+1, inend, poststart, postend-1);
        }else if(index==inend){
            root.right = null;
            root.left = helper(inorder, postorder, instart, inend-1, poststart, postend-1);
        }else{
            root.left = helper(inorder, postorder, instart, index-1, poststart, poststart+index-instart-1);
            root.right = helper(inorder, postorder, index+1, inend, poststart+index-instart, postend-1);
        }
        return root;
    }
}
```
