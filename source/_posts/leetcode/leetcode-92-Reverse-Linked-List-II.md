---
title: leetcode 92. Reverse Linked List II
date: 2019-11-24 18:48:47
categories:
- leetcode
toc: true
tags:
- Linked List
---
### 难度：Middle

<a href="https://leetcode.com/problems/reverse-linked-list-ii/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/reverse-linked-list-ii/">九章</a>
## 题目描述
Reverse a linked list from position _m_ to _n_. Do it in one-pass.

**Note:  **1 ≤ _m_ ≤ _n_ ≤ length of list.

**Example:**
        
    Input: 1->2->3->4->5->NULL, _m_ = 2, _n_ = 4
    Output: 1->4->3->2->5->NULL
    


**Tags:** Linked List

**Difficulty:** Medium
## 答案
<!--more-->
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode dummy = new ListNode(0), pre, cur, next;
        pre = dummy;
        dummy.next = head;
        cur = head;
        //    1    2    3 4 5  m=2, n=4
        // node1 node2
        // 开始翻转的前一个，以及头一个
        ListNode node1=head, node2=head;
        if(head==null) return null;
        for(int i=1; i<=n; i++){
            if(i<m) {
                pre = cur;
                cur = cur.next;
            }else{
                if(i==m){
                    node1 = pre;
                    node2 = cur;
                }
                next = cur.next;
                cur.next = pre;
                // next.next = cur;
                if(i==n){
                    node1.next = cur;
                    node2.next = next; 
                }
                pre = cur;
                cur = next;
            }
        }
        return dummy.next;
    }
}
```
