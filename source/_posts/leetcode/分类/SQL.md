---
title: SQL
toc: true
recommend: 1
uniqueId: '2020-08-08 10:45:31/"SQL".html'
date: 2020-08-08 18:45:31
thumbnail:
categories:
- leetcode
- 分类
tags:
keywords:
---

[TOC]

<!--more-->



### [176. 第二高的薪水 - 力扣（LeetCode）](https://leetcode-cn.com/problems/second-highest-salary/)

ifnull(x，y)，若x不为空则返回x，否则返回y，这道题y=null
limit x，y 

limit x offset y == limit x, y

```sql
select ifnull
    ((select distinct salary
     from Employee
     order by salary desc
     limit 1, 1), null)
as SecondHighestSalary

```

