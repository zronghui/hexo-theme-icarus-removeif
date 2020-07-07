---
title: 剑指 Offer 35 复杂链表的复制
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:33/"剑指 Offer 35 复杂链表的复制".html'
date: 2020-06-30 21:55:33
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

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

