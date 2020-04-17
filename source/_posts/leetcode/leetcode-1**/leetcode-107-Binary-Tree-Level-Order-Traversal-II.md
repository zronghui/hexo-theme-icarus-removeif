---
title: leetcode 107. Binary Tree Level Order Traversal II
date: 2019-11-23 14:10:42
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Tree
- Breadth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/binary-tree-level-order-traversal-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/binary-tree-level-order-traversal-ii/">九章</a>
## 题目描述
Given a binary tree, return the _bottom-up level order_ traversal of its
nodes' values. (ie, from left to right, level by level from leaf to root).

For example:  
Given binary tree `[3,9,20,null,null,15,7]`,  
        
        3
       / \
      9  20
        /  \
       15   7
    

return its bottom-up level order traversal as:  
        
    [
      [15,7],
      [9,20],
      [3]
    ]
    


**Tags:** Tree, Breadth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root==null) return res;
        Deque<TreeNode> deque = new ArrayDeque<>();
        deque.add(root);
        while(!deque.isEmpty()){
            List<Integer> l = new ArrayList<>();
            int size = deque.size();
            for(int i=0; i<size; i++){
                TreeNode cur = deque.removeFirst();
                l.add(cur.val);
                if(cur.left!=null) deque.add(cur.left);
                if(cur.right!=null) deque.add(cur.right);
            }
            res.add(l);
        }
        Collections.reverse(res);
        return res;
    }
}
```
