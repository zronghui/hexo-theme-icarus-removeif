---
title: leetcode 559. Maximum Depth of N-ary Tree
date: 2019-12-06 22:42:12
categories:
- leetcode
toc: true
tags:
- Tree
- Depth-first Search
- Breadth-first Search
---
### 难度：Easy

<a href="https://leetcode.com/problems/maximum-depth-of-n-ary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/maximum-depth-of-n-ary-tree/">九章</a>
## 题目描述
Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root
node down to the farthest leaf node.

_Nary-Tree input serialization  is represented in their level order traversal,
each group of children is separated by the null value (See examples)._



**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)
            Input: root = [1,null,3,2,4,null,5,6]    Output: 3    

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)
            Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]    Output: 5    



**Constraints:**

  * The depth of the n-ary tree is less than or equal to `1000`.
  * The total number of nodes is between `[0, 10^4]`.


**Tags:** Tree, Depth-first Search, Breadth-first Search

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
    public int maxDepth(Node root) {
        Deque<Node> deque = new ArrayDeque<>();
        if(root==null) return 0;
        int res = 0;
        deque.add(root);
        while(!deque.isEmpty()) {
            res++;
            int size = deque.size();
            for(int i=0; i<size; i++){
                Node node = deque.removeFirst();
                for(Node n: node.children) {
                    deque.add(n);
                }
            }
        }
        return res;
    }
}
```
