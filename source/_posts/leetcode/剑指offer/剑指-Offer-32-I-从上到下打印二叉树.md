---
title: 剑指 Offer 32 - I 从上到下打印二叉树
toc: true
recommend: 1
uniqueId: '2020-06-30 13:55:24/"剑指 Offer 32 - I 从上到下打印二叉树".html'
date: 2020-06-30 21:55:24
thumbnail:
categories:
- leetcode
- 剑指offer
tags:
keywords:
---

[TOC]

<!--more-->

![image-20200706213752707](/Users/zhangronghui/Library/Application Support/typora-user-images/image-20200706213752707.png)

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[int]:
        if not root: return []
        res, queue = [], collections.deque()
        queue.append(root)
        while queue:
            node = queue.popleft()
            res.append(node.val)
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
        return res

# 作者：jyd
# 链接：https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/solution/mian-shi-ti-32-i-cong-shang-dao-xia-da-yin-er-ch-4/
# 来源：力扣（LeetCode）
# 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

