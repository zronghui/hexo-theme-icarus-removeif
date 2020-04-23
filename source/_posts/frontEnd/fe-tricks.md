---
title: fe-tricks
toc: true
recommend: 1
uniqueId: '2020-04-23 04:31:25/"fe-tricks".html'
date: 2020-04-23 12:31:25
thumbnail:
categories:
- frontEnd
tags:
keywords:
---

[TOC]

<!--more-->

## 值得每日学习的 fe 资源

[haizlin/fe-interview: 前端面试每日 3+1，以面试题来驱动学习，提倡每日学习与思考，每天进步一点！每天早上5点纯手工发布面试题（死磕自己，愉悦大家）](https://github.com/haizlin/fe-interview)
[fe-interview/skill.md at master · haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/skill.md)



### 响应式和自适应

[响应式和自适应的区别 - 前端 - 掘金](https://juejin.im/entry/58b6414a128fe1006421bfa7)
[(12) 自适应页面的实现方式 - 前端开发 - SegmentFault 思否](https://segmentfault.com/a/1190000015759719)

```css
@media screen and (min-width: 800px) {
    .search_engines {
        position: fixed;
        right: 25%;
        top: 20%;
        display: inline;
    }
}
```



修改完 CSS 后，需要重命名css, 如 moon.m.0001.css  0001 是版本号，这样可以让浏览器强制加载新的 CSS
