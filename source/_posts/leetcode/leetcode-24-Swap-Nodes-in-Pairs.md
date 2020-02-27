---
title: leetcode 24. Swap Nodes in Pairs
date: 2019-11-24 15:15:31
categories:
- leetcode
toc: true
tags:
- Linked List
---
### 难度：Middle

<a href="https://leetcode.com/problems/swap-nodes-in-pairs/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/swap-nodes-in-pairs/">九章</a>
## 题目描述
Given a linked list, swap every two adjacent nodes and return its head.

You may **not** modify the values in the list's nodes, only nodes itself may
be changed.



**Example:**
        
    Given 1->2->3->4, you should return the list as 2->1->4->3.
    


**Tags:** Linked List

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head==null || head.next==null) return head;
        ListNode dummy = new ListNode(0);
        ListNode fast = head.next, slow = head, pre = dummy;
        swap(fast, slow, pre);
        head = slow;
        while(slow.next!=null && slow.next.next!=null){
            pre = slow;
            fast = slow.next.next;
            slow = slow.next;
            swap(fast, slow, pre);
        }
        return dummy.next;
    }
    private void swap(ListNode fast, ListNode slow, ListNode pre){
        ListNode next = fast.next;
        fast.next = slow;
        slow.next = next;
        pre.next = fast;
    }
}
```
