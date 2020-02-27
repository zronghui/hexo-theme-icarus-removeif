---
title: leetcode 297. Serialize and Deserialize Binary Tree
date: 2019-12-02 10:51:06
categories:
- leetcode
toc: true
tags:
- Tree
- Design
---
### 难度：Hard

<a href="https://leetcode.com/problems/serialize-and-deserialize-binary-tree/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/serialize-and-deserialize-binary-tree/">九章</a>
## 题目描述
Serialization is the process of converting a data structure or object into a
sequence of bits so that it can be stored in a file or memory buffer, or
transmitted across a network connection link to be reconstructed later in the
same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no
restriction on how your serialization/deserialization algorithm should work.
You just need to ensure that a binary tree can be serialized to a string and
this string can be deserialized to the original tree structure.

**Example:  **
        
    You may serialize the following tree:
    
        1
       / \
      2   3
         / \
        4   5
    
    as "[1,2,3,null,null,4,5]"
    

**Clarification:** The above format is the same as [how LeetCode serializes a
binary tree](/faq/#binary-tree). You do not necessarily need to follow this
format, so please be creative and come up with different approaches yourself.

**Note:  **Do not use class member/global/static variables to store states.
Your serialize and deserialize algorithms should be stateless.


**Tags:** Tree, Design

**Difficulty:** Hard
## 答案
<!--more-->
```java

public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        // 前序遍历
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        System.out.println(sb.toString());
        return sb.substring(1, sb.length()).toString();
    }
    private void serializeHelper(TreeNode root, StringBuilder sb){
        if(root==null) {
            sb.append(" #");
            return;
        }
        sb.append(" ");
        sb.append(String.valueOf(root.val));
        serializeHelper(root.left, sb);
        serializeHelper(root.right, sb);
    }
    

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data==null || data.length()==0) return null;
        String[] temp = data.split(" ");
        List<String> strs = new ArrayList<>();
        for(String s: temp){
            strs.add(s);
        }
        return deserializeHelper(strs);
    }
    private TreeNode deserializeHelper(List<String> strs){
        if(strs==null || strs.size()==0) return null;
        String str = strs.get(0);
        strs.remove(str);
        if(str.equals("#")) return null;
        TreeNode node = new TreeNode(Integer.parseInt(str));
        node.left = deserializeHelper(strs);
        node.right = deserializeHelper(strs);
        return node;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```
