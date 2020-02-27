---
title: leetcode 328. Odd Even Linked List
date: 2019-11-28 18:01:19
categories:
- leetcode
toc: true
tags:
- Linked List
---
### 难度：Middle

<a href="https://leetcode.com/problems/odd-even-linked-list/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/odd-even-linked-list/">九章</a>
## 题目描述
Given a singly linked list, group all odd nodes together followed by the even
nodes. Please note here we are talking about the node number and not the value
in the nodes.

You should try to do it in place. The program should run in O(1) space
complexity and O(nodes) time complexity.

**Example 1:**
        
    Input:1->2->3->4->5->NULL
    Output:1->3->5->2->4->NULL
    

**Example 2:**
        
    Input: 2->1->3->5->6->4->7->NULL
    Output:2->3->6->7->1->5->4->NULL
    

**Note:**

  * The relative order inside both the even and odd groups should remain as it was in the input.
  * The first node is considered odd, the second node even and so on ...


**Tags:** Linked List

**Difficulty:** Medium
## 答案
<!--more-->
```java

class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head==null || head.next==null) return head;
        ListNode odd = head, even = head.next, evenHead = even;
        // odd even
        // 1    2   3
        // 
        while(even!=null && even.next!=null){
            odd.next = even.next;
            even.next = even.next.next;
            odd = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```
