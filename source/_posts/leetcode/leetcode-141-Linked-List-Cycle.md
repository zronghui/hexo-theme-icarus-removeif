---
title: leetcode 141. Linked List Cycle
date: 2019-11-28 11:54:56
categories:
- leetcode
toc: true
tags:
- Linked List
- Two Pointers
---
### 难度：Easy

<a href="https://leetcode.com/problems/linked-list-cycle/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/linked-list-cycle/">九章</a>
## 题目描述
Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which
represents the position (0-indexed) in the linked list where tail connects to.
If `pos` is `-1`, then there is no cycle in the linked list.



**Example 1:**
        
    Input: head = [3,2,0,-4], pos = 1
    Output: true
    Explanation: There is a cycle in the linked list, where tail connects to the second node.
    

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Example 2:**
        
    Input: head = [1,2], pos = 0
    Output: true
    Explanation: There is a cycle in the linked list, where tail connects to the first node.
    

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Example 3:**
        
    Input: head = [1], pos = -1
    Output: false
    Explanation: There is no cycle in the linked list.
    

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)



**Follow up:**

Can you solve it using _O(1)_ (i.e. constant) memory?


**Tags:** Linked List, Two Pointers

**Difficulty:** Easy
## 答案
<!--more-->
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null || head.next==null){
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while(fast!=slow){
            if(fast==null || fast.next==null) return false;
            fast = fast.next.next;
            slow = slow.next;
        }
        return true;
    }
}
```
