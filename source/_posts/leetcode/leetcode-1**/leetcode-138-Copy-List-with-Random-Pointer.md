---
thumbnail:
title: leetcode 138. Copy List with Random Pointer
date: 2020-07-07 11:32:03
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Hash Table
- Linked List
recommend: 1
keywords:
uniqueId: '2020-07-07 11:32:03/"leetcode 138. Copy List with Random Pointer".html'
---

<a href="https://leetcode.com/problems/copy-list-with-random-pointer/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/copy-list-with-random-pointer/">九章</a>
## 题目描述
A linked list is given such that each node contains an additional random
pointer which could point to any node in the list or null.

Return a [**deep
copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list.

The Linked List is represented in the input/output as a list of `n` nodes.
Each node is represented as a pair of `[val, random_index]` where:

  * `val`: an integer representing `Node.val`
  * `random_index`: the index of the node (range from `0` to `n-1`) where random pointer points to, or `null` if it does not point to any node.



**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/18/e1.png)
            Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]    Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]    

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/18/e2.png)
            Input: head = [[1,1],[2,1]]    Output: [[1,1],[2,1]]    

**Example 3:**

**![](https://assets.leetcode.com/uploads/2019/12/18/e3.png)**
            Input: head = [[3,null],[3,0],[3,null]]    Output: [[3,null],[3,0],[3,null]]    

**Example 4:**
            Input: head = []    Output: []    Explanation: Given linked list is empty (null pointer), so return null.    



**Constraints:**

  * `-10000 <= Node.val <= 10000`
  * `Node.random` is null or pointing to a node in the linked list.
  * Number of Nodes will not exceed 1000.


**Tags:** Hash Table, Linked List

**Difficulty:** Medium

## 答案
<!--more-->
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        idx = 0
        d = {} # node -> idx
        curcpnode = dummy = Node(0)
        cpl = [] # 存放 node
        # 遍历第一遍，复制 val, next
        cur = head
        while cur:
            d[cur] = idx
            newnode = Node(cur.val)
            curcpnode.next = newnode
            curcpnode = curcpnode.next
            cpl.append(curcpnode)
            cur = cur.next
            idx += 1
        # 遍历 head 第二遍，复制 random 指针
        cur = head
        curcpnode = dummy.next
        while cur:
            curcpnode.random = cpl[d[cur.random]] if cur.random else None
            curcpnode = curcpnode.next
            cur = cur.next
        return dummy.next
```
