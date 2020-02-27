---
title: leetcode 590. N-ary Tree Postorder Traversal
date: 2019-12-06 22:31:18
categories:
- leetcode
toc: true
tags:
- Tree
---
### 难度：Easy

<a href="https://leetcode.com/problems/n-ary-tree-postorder-traversal/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/n-ary-tree-postorder-traversal/">九章</a>
## 题目描述
Given an n-ary tree, return the _postorder_ traversal of its nodes' values.

_Nary-Tree input serialization  is represented in their level order traversal,
each group of children is separated by the null value (See examples)._



**Follow up:**

Recursive solution is trivial, could you do it iteratively?



**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)
            Input: root = [1,null,3,2,4,null,5,6]    Output: [5,6,3,2,4,1]    

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)
            Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]    Output: [2,6,14,11,7,3,12,8,4,13,9,10,5,1]    



**Constraints:**

  * The height of the n-ary tree is less than or equal to `1000`
  * The total number of nodes is between `[0, 10^4]`


**Tags:** Tree

**Difficulty:** Easy
## 答案
<!--more-->
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> postorder0(Node root) {
        if(root==null) return res;
        for(Node node: root.children) {
            postorder(node);
        }
        res.add(root.val);
        return res;
    }
}
```
