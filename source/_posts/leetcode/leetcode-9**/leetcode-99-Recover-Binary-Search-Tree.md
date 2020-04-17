---
title: leetcode 99. Recover Binary Search Tree
date: 2019-11-22 16:45:46
categories:
- leetcode
- leetcode-9**
toc: true
tags:
- Tree
- Depth-first Search
---
### 难度：Hard

<a href="https://leetcode.com/problems/recover-binary-search-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/recover-binary-search-tree/">九章</a>
## 题目描述
Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

**Example 1:**
        
    Input: [1,3,null,null,2]
    
       1
      /
     3
      \
       2
    
    Output: [3,1,null,null,2]
    
       3
      /
     1
      \
       2
    

**Example 2:**
        
    Input: [3,1,4,null,null,2]
    
      3
     / \
    1   4
       /
      2
    
    Output: [2,1,4,null,null,3]
    
      2
     / \
    1   4
       /
      3
    

**Follow up:**

  * A solution using O( _n_ ) space is pretty straight forward.
  * Could you devise a constant space solution?


**Tags:** Tree, Depth-first Search

**Difficulty:** Hard
## 答案
<!--more-->
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    TreeNode first, second, pre;
    // 1 2 3 4 5
    // 2种情况：替换的值相邻（pre>cur 发生一次）
    // 替换的值不相邻 (pre>cur 发生2次)
    // 1 3 2 4 5
    // 1 4 3 2 5
    public void recoverTree(TreeNode root) {
        helper(root);
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }
    private void helper(TreeNode root){
        if(root==null) return;
        helper(root.left);
        if(pre!=null && pre.val>root.val){
            if(first==null) {
                first = pre;
                second = root;
            }
            else {
                second = root; 
                return;
            }
        }
        pre = root;
        helper(root.right);
    }
}
```
