---
thumbnail:
title: leetcode 25. Reverse Nodes in k-Group
date: 2020-06-06 21:39:13
categories:
- leetcode
- leetcode-2**
toc: true
tags:
- Linked List
recommend: 1
keywords:
uniqueId: '2020-06-06 21:39:13/"leetcode 25. Reverse Nodes in k-Group".html'
---

<a href="https://leetcode.com/problems/reverse-nodes-in-k-group/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/reverse-nodes-in-k-group/">九章</a>
## 题目描述
Given a linked list, reverse the nodes of a linked list _k_ at a time and
return its modified list.

_k_ is a positive integer and is less than or equal to the length of the
linked list. If the number of nodes is not a multiple of _k_ then left-out
nodes in the end should remain as it is.

**Example:**

Given this linked list: `1->2->3->4->5`

For _k_ = 2, you should return: `2->1->4->3->5`

For _k_ = 3, you should return: `3->2->1->4->5`

**Note:**

  * Only constant extra memory is allowed.
  * You may not alter the values in the list's nodes, only nodes itself may be changed.


**Tags:** Linked List

**Difficulty:** Hard

## 答案

虽说效率有点低，但是图很直观

[图解k个一组翻转链表 - K 个一组翻转链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/solution/tu-jie-kge-yi-zu-fan-zhuan-lian-biao-by-user7208t/)

<!--more-->
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        pre = dummy
        cur = dummy
        while cur.next:
            for i in range(k):
                cur = cur.next
                if not cur:
                    break
            if not cur:
                break
            
            start = pre.next
            next = cur.next
            cur.next = None
            pre.next = self.reverse(start)
            start.next = next
            pre = start
            cur = start
        return dummy.next

    
    def reverse(self, head: ListNode) -> ListNode:
        cur = head
        pre = None
        while cur:
            next = cur.next
            cur.next = pre
            pre = cur
            cur = next
        return pre
```
