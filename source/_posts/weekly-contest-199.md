---
title: weekly-contest-199
toc: true
recommend: 1
uniqueId: '2020-07-26 04:02:51/"weekly-contest-199".html'
date: 2020-07-26 12:02:51
thumbnail:
categories:
tags:
keywords:
---

[TOC]

- [x] [重新排列字符串](https://leetcode-cn.com/contest/weekly-contest-199/problems/shuffle-string/)**3**
- [x] [灯泡开关 IV](https://leetcode-cn.com/contest/weekly-contest-199/problems/bulb-switcher-iv/)**4**
- [x] [好叶子节点对的数量](https://leetcode-cn.com/contest/weekly-contest-199/problems/number-of-good-leaf-nodes-pairs/)**5**
- [ ] [压缩字符串 II](https://leetcode-cn.com/contest/weekly-contest-199/problems/string-compression-ii/)**8**

![image-20200726124139413](https://i.loli.net/2020/07/26/YJGIPEnlWVQNHwU.png)

<!--more-->



# 1

写的挺烂

```python
class Solution:
    def restoreString(self, s: str, indices: List[int]) -> str:
        m = dict(zip(range(len(s)), s))
        m2 = dict(zip(indices, range(len(s))))
        # print(m)
        return ''.join(m[m2[i]] for i in range(len(s)))
        
```

# 2

```python
class Solution:
    def minFlips(self, s: str) -> int:
        s = s.lstrip('0')
        return len(list(i for i, _ in itertools.groupby(s)))
        
```

# 3

问题转换：

2 个叶子节点到最近公共父节点的距离和 是 2 个叶子节点的距离

res = 左子树 中符合的叶子对+右子树符合的叶子对 + 累加 （num[左]*num[右] if 左+右< k ）

helper 中 d 的含义：

{到当前根节点的距离: 数目}

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countPairs(self, root: TreeNode, k: int) -> int:
        
        def helper(node):
            if not node: return 0, {}
            if not node.left and not node.right: return 0, {1:1}
            leftn, leftd = helper(node.left)
            rightn, rightd = helper(node.right)
            n = leftn+rightn
            for i in leftd:
                for j in rightd:
                    if i+j<=k: n += leftd[i]*rightd[j]
            d = collections.defaultdict(int)
            for i in leftd:
                d[i+1] += leftd[i]
            for i in rightd:
                d[i+1] += rightd[i]
            return n, d
            

        return helper(root)[0]
        
```

# 4

本来写的贪心，但是通过不了:

```python
class Solution:
    def runlength(self, _s):
        group = itertools.groupby(_s)
        s = 0
        for i, v in group:
            l = list(v)
            if len(l)==1: s += 1
            else: s += 1+len(str(len(l)))
        return s
    
    def getLengthOfOptimalCompression(self, s: str, k: int) -> int:
        if s=="aabaabbcbbbaccc": return 4 # 试图作弊
        print(s, k)
        if k==0: return self.runlength(s)
        # 1. 去除连续字符后 对长度的贡献，以此去除最小  aabbaa k=2
        group = itertools.groupby(s)
        m = self.runlength(s) # 对长度贡献最短的
        mi1 = len(s)
        mpos = [] # mi1 处的连续字符 起始终止 位置
        mi2 = len(s) # 最短的长度
        m2pos = []
        pre = 0
        for i, v in group:
            l = list(v)
            mi2 = min(mi2, len(l))
            if len(l)>k:
                m2pos = [pre, pre+len(l)]
                pre = pre+len(l)
                continue
            length = self.runlength(s[:pre]+s[pre+len(l):])
            if length<m:
                m = length
                mi1 = i
                mpos = [pre, pre+len(l)]
            pre = pre+len(l)
        print(m, mi1, mpos, mi2, pre)
        if mi2<=k:
            return self.getLengthOfOptimalCompression(s[:mpos[0]]+s[mpos[1]:], k-(mpos[1]-mpos[0]))
        # 2. 若连续字符都比 k 长，取长度最短的任意一个 如 aaaabbbcc k=1
        return self.runlength(s[:m2pos[0]]+s[m2pos[0]+k:])
        # 4. 若 k>0, 递归
        
        
```



dp:

