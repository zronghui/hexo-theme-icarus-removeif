---
title: leetcode 589. N-ary Tree Preorder Traversal
date: 2019-12-06 21:15:10
categories:
- leetcode
toc: true
tags:
- Tree
---
### 难度：Easy

<a href="https://leetcode.com/problems/n-ary-tree-preorder-traversal/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/n-ary-tree-preorder-traversal/">九章</a>
## 题目描述
Given an n-ary tree, return the _preorder_ traversal of its nodes' values.

_Nary-Tree input serialization  is represented in their level order traversal,
each group of children is separated by the null value (See examples)._



**Follow up:**

Recursive solution is trivial, could you do it iteratively?



**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)
            Input: root = [1,null,3,2,4,null,5,6]    Output: [1,3,5,6,2,4]    

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)
            Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]    Output: [1,2,3,6,7,11,14,4,8,12,5,9,13,10]    



**Constraints:**

  * The height of the n-ary tree is less than or equal to `1000`
  * The total number of nodes is between `[0, 10^4]`


**Tags:** Tree

**Difficulty:** Easy
## 答案
<!--more-->
```java
// class Node {
//     public int val;
//     public List<Node> children;

//     public Node() {}

//     public Node(int _val) {
//         val = _val;
//     }

//     public Node(int _val, List<Node> _children) {
//         val = _val;
//         children = _children;
//     }
// };

// Recursive solution is trivial, could you do it iteratively?
// trival 微不足道的
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> preorder0(Node root) {
        // 1 ms
        if(root==null) return res;
        res.add(root.val);
        for(Node child: root.children){
            preorder(child);
        }
        return res;
    }
    public List<Integer> preorder(Node root) {
        // 3 ms?
        if(root==null) return res;
        // stack逆序存储children
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            Node node = stack.pop();
            res.add(node.val);
            for(int i=node.children.size()-1; i>=0; i--) {
                stack.push(node.children.get(i));
            }
        }
        return res;
    }
}
```
