---
title: 剑指 Offer 09. 用两个栈实现队列
toc: true
recommend: 1
uniqueId: '2020-06-30 11:18:05/"剑指 Offer 09. 用两个栈实现队列".html'
date: 2020-06-30 19:18:05
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

[ 剑指 Offer 09. 用两个栈实现队列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)
[ 面试题09. 用两个栈实现队列（清晰图解） - 用两个栈实现队列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/solution/mian-shi-ti-09-yong-liang-ge-zhan-shi-xian-dui-l-2/)

<!--more-->

双栈可实现列表倒序： 设有含三个元素的栈 A = [1,2,3]A=[1,2,3] 和空栈 B = []B=[]。若循环执行 AA 元素出栈并添加入栈 BB ，直到栈 AA 为空，则 A = []A=[] , B = [3,2,1]B=[3,2,1] ，即 栈 BB 元素实现栈 AA 元素倒序 。
利用栈 BB 删除队首元素： 倒序后，BB 执行出栈则相当于删除了 AA 的栈底元素，即对应队首元素。



加入队尾 appendTail()函数： 将数字 val 加入栈 A 即可。
删除队首deleteHead()函数： 有以下三种情况。
当栈 B 不为空： B中仍有已完成倒序的元素，因此直接返回 B 的栈顶元素。
否则，当 A 为空： 即两个栈都为空，无元素，因此返回 -1−1 。
否则： 将栈 A 元素全部转移至栈 B 中，实现元素倒序，并返回栈 B 的栈顶元素。

```python
class CQueue:

    def __init__(self):
        self.a, self.b = [], []


    def appendTail(self, value: int) -> None:
        self.a.append(value)


    def deleteHead(self) -> int:
        if self.b:
            return self.b.pop()
        if self.a:
            while self.a:
                self.b.append(self.a.pop())
            return self.b.pop()
        return -1



# Your CQueue object will be instantiated and called as such:
# obj = CQueue()
# obj.appendTail(value)
# param_2 = obj.deleteHead()
```

