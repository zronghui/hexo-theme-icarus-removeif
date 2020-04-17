---
title: leetcode 103. Binary Tree Zigzag Level Order Traversal
date: 2019-11-23 11:13:46
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Stack
- Tree
- Breadth-first Search
---
### 难度：Middle

<a href="https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/binary-tree-zigzag-level-order-traversal/">九章</a>
## 题目描述
Given a binary tree, return the _zigzag level order_ traversal of its nodes'
values. (ie, from left to right, then right to left for the next level and
alternate between).

For example:  
Given binary tree `[3,9,20,null,null,15,7]`,  
        
        3
       / \
      9  20
        /  \
       15   7
    

return its zigzag level order traversal as:  
        
    [
      [3],
      [20,9],
      [15,7]
    ]
    


**Tags:** Stack, Tree, Breadth-first Search

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root==null) return res;
        Deque<TreeNode> lr = new ArrayDeque<>();
        Deque<TreeNode> rl = new ArrayDeque<>();
        lr.addFirst(root);
        while(!(lr.isEmpty() && rl.isEmpty())){
            List<Integer> l = new ArrayList<>();
            while(!lr.isEmpty()){
                TreeNode cur = lr.removeFirst();
                l.add(cur.val);
                if(cur.left!=null) rl.addFirst(cur.left);
                if(cur.right!=null) rl.addFirst(cur.right);
            }
            if(!l.isEmpty())
                res.add(l);
            List<Integer> l1 = new ArrayList<>();
            while(!rl.isEmpty()){
                TreeNode cur = rl.removeFirst();
                l1.add(cur.val);
                if(cur.right!=null) lr.addFirst(cur.right);
                if(cur.left!=null) lr.addFirst(cur.left);
            }
            if(!l1.isEmpty())
                res.add(l1);
        }
        return res;
    }
}
```
