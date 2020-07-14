---
thumbnail:
title: leetcode 1041. Robot Bounded In Circle
date: 2020-07-14 07:34:57
categories:
- leetcode
- leetcode-1**
toc: true
tags:
- Math
recommend: 1
keywords:
uniqueId: '2020-07-14 07:34:57/"leetcode 1041. Robot Bounded In Circle".html'
---

<a href="https://leetcode.com/problems/robot-bounded-in-circle/">leetcode</a>
<a href="https://www.jiuzhang.com/solution/robot-bounded-in-circle/">九章</a>
## 题目描述
On an infinite plane, a robot initially stands at `(0, 0)` and faces north.
The robot can receive one of three instructions:

  * `"G"`: go straight 1 unit;
  * `"L"`: turn 90 degrees to the left;
  * `"R"`: turn 90 degress to the right.

The robot performs the `instructions` given in order, and repeats them
forever.

Return `true` if and only if there exists a circle in the plane such that the
robot never leaves the circle.



**Example 1:**
            Input: "GGLLGG"    Output: true    Explanation:    The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).    When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.    

**Example 2:**
            Input: "GG"    Output: false    Explanation:    The robot moves north indefinitely.    

**Example 3:**
            Input: "GL"    Output: true    Explanation:    The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...    



**Note:**

  1. `1 <= instructions.length <= 100`
  2. `instructions[i]` is in `{'G', 'L', 'R'}`


**Tags:** Math

**Difficulty:** Medium

## 答案
<!--more-->

只要走完一轮后，方向改变，即不是直走的话，最后无论再走多少轮总有一轮会走回终点

```python
class Solution:
    def isRobotBounded(self, instructions: str) -> bool:
        x = y = 0
        directions = [
            (0, 1), # 上北
            (1, 0), # 右东
            (0, -1), # 下南
            (-1, 0), # 左西
        ]
        di = 0 # 开始朝北，若 R，+1 %4
        for i in instructions:
            if i=='R': di = (di+1)%4
            elif i=='L': di = (di-1)%4
            elif i=='G': 
                x += directions[di][0]
                y += directions[di][1]
        return x==y==0 or di!=0
```
