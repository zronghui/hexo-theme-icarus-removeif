---
title: leetcode 897. Increasing Order Search Tree
date: 2019-12-06 22:21:41
categories:
- leetcode
- leetcode-8**
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/increasing-order-search-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/increasing-order-search-tree/">九章</a>
## 题目描述
Given a binary search tree, rearrange the tree in **in-order** so that the
leftmost node in the tree is now the root of the tree, and every node has no
left child and only 1 right child.
        
    **Example 1:**
    Input: [5,3,6,2,4,null,8,1,null,null,null,7,9]
    
           5
          / \
        3    6
       / \    \
      2   4    8
     /        / \ 
    1        7   9
    
    Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
    
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
                \
                 7
                  \
                   8
                    \
                     9  

**Note:**

  1. The number of nodes in the given tree will be between 1 and 100.
  2. Each node will have a unique integer value from 0 to 1000.


**Tags:** Tree, Depth-first Search

**Difficulty:** Easy
## 答案
<!--more-->
```java

class Solution {
    public TreeNode increasingBST0(TreeNode root) {
        // Runtime: 5 ms, faster than 11.04%
        // Memory Usage: 44.5 MB, less than 40.63% 
        TreeNode res = new TreeNode(0);
        TreeNode cur = res;
        // inorder
        TreeNode temp = root;
        Stack<TreeNode> stack = new Stack<>();
        while(!stack.isEmpty() || temp!=null){
            while(temp!=null){
                stack.push(temp);
                temp = temp.left;
            }
            if(!stack.isEmpty()){
                TreeNode node = stack.pop();
                node.left = null;
                cur.right = node;
                cur = node;
                temp = node.right;
            }
        }
        return res.right;
    }
    TreeNode cur;
    public TreeNode increasingBST(TreeNode root) {
        // https://leetcode-cn.com/problems/increasing-order-search-tree/solution/di-zeng-shun-xu-cha-zhao-shu-by-leetcode/
        // 方法2
        // Runtime: 2 ms, faster than 100.00% 
        // Memory Usage: 38.2 MB, less than 100.00% 
        TreeNode dummy = new TreeNode(0);
        cur = dummy;
        helper(root);
        return dummy.right;
    }
    private void helper(TreeNode node){
        if(node==null) return;
        helper(node.left);
        node.left = null;
        cur.right = node;
        cur = node;
        helper(node.right);
    }
}
```
