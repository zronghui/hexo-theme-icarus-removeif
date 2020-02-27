---
title: leetcode 230. Kth Smallest Element in a BST
date: 2019-11-26 17:36:16
categories:
- leetcode
toc: true
tags:
- Binary Search
- Tree
---
### 难度：Middle

<a href="https://leetcode.com/problems/kth-smallest-element-in-a-bst/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/kth-smallest-element-in-a-bst/">九章</a>
## 题目描述
Given a binary search tree, write a function `kthSmallest` to find the **k**
th smallest element in it.

**Note:**  
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Example 1:**
        
    Input: root = [3,1,4,null,2], k = 1
       3
      / \
     1   4
      \
       2
    Output: 1

**Example 2:**
        
    Input: root = [5,3,6,2,4,null,null,1], k = 3
           5
          / \
         3   6
        / \
       2   4
      /
     1
    Output: 3
    

**Follow up:**  
What if the BST is modified (insert/delete operations) often and you need to
find the kth smallest frequently? How would you optimize the kthSmallest
routine?


**Tags:** Binary Search, Tree

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        // 中序遍历第k个元素
        // 中序遍历：将左节点压栈，直到为空，弹栈，转向右节点
        // 弹栈的顺序就是中序遍历
        Stack<TreeNode> stack = new Stack<>();
        while(root!=null || !stack.isEmpty()){
            while(root!=null){
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            k--;
            if(k==0) return root.val;
            root = root.right;
        }
        return 0;
    }
}
```
