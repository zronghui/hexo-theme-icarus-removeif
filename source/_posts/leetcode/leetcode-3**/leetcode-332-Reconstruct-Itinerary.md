---
thumbnail:
title: leetcode 332. Reconstruct Itinerary
date: 2020-08-27 20:00:07
categories:
- leetcode
- leetcode-3**
toc: true
tags:
- Depth-first Search
- Graph
recommend: 1
keywords:
uniqueId: '2020-08-27 20:00:07/"leetcode 332. Reconstruct Itinerary".html'
---

<a href="https://leetcode.com/problems/reconstruct-itinerary/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/reconstruct-itinerary/">九章</a>
## 题目描述
Given a list of airline tickets represented by pairs of departure and arrival
airports `[from, to]`, reconstruct the itinerary in order. All of the tickets
belong to a man who departs from `JFK`. Thus, the itinerary must begin with
`JFK`.

**Note:**

  1. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.
  2. All airports are represented by three capital letters (IATA code).
  3. You may assume all tickets form at least one valid itinerary.

**Example 1:**
        
    Input:[["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
    Output:["JFK", "MUC", "LHR", "SFO", "SJC"]


**Example 2:**
        
    Input:[["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
    Output:["JFK","ATL","JFK","SFO","ATL","SFO"]
    Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"].
                 But it is larger in lexical order.



**Tags:** Depth-first Search, Graph

**Difficulty:** Medium

## 答案
<!--more-->
```python
# 从起点出发，进行深度优先搜索。
# 每次沿着某条边从某个顶点移动到另外一个顶点的时候，都需要删除这条边。
# 如果没有可移动的路径，则将所在节点加入到栈中，并返回。
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph = collections.defaultdict(list)
        for a, b in tickets:
            graph[a].append(b)
        for i in graph:
            graph[i].sort(reverse=True)
        res = []
        def search(i):
            while graph[i]:
                search(graph[i].pop())
            res.append(i)
        search('JFK')
        return res[::-1]
```
