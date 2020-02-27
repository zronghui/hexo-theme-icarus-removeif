---
title: leetcode 105. Construct Binary Tree from Preorder and Inorder Traversal
date: 2019-11-22 23:32:41
categories:
- leetcode
toc: true
tags:
- Array
- Tree
- Depth-first Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/construct-binary-tree-from-preorder-and-inorder-traversal/">九章</a>
## 题目描述
Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**  
You may assume that duplicates do not exist in the tree.

For example, given
        
    preorder = [3,9,20,15,7]
    inorder = [9,3,15,20,7]

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length<1) return null;
        return helper(preorder, inorder, 0, preorder.length-1, 0, inorder.length-1);
    }
    
    private TreeNode helper(int[] preorder, int[] inorder, int prestart, int preend, int instart, int inend){
        TreeNode root = new TreeNode(preorder[prestart]);
        if(prestart==preend){
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
            root.right = helper(preorder, inorder, prestart+1, preend, instart+1, inend);
        }else if(index==inend){
            root.right = null;
            root.left = helper(preorder, inorder, prestart+1, preend, instart, inend-1);
        }else{
            root.left = helper(preorder, inorder, prestart+1, prestart+index-instart, instart, index-1);
            root.right = helper(preorder, inorder, prestart+index-instart+1, preend, index+1, inend);
        }
        return root;
    }
}
```
