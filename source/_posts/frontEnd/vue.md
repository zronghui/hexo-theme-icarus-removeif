---
title: vue
date: 2019-12-31 19:08:03
categories:
- frontEnd
toc: true
tags:
---
## 引入json文件
import data from 'static/h5Static.json'
此时，data已经被解析为字典

## Vue对list的增删改查
### 删除

```js
delete(id) { 
    for (const v of this.list) {
      if (v.id === id) {
        const index = this.list.indexOf(v)
        this.list.splice(index, 1, this.temp)
        break
      }
    }
}	
```
### 新增

```js
this.list.push(car)
```
