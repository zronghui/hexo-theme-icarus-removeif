---
title: Vue.js基础入门Vue实战黑马程序员VUE
date: 2019-12-21 15:27:47
categories:
- frontEnd
toc: true
tags:
- vue
---
## 开发工具
vscode + live server

##  在挂载点及其子节点中生效

```js
el: "#app" // id
el: ".app" // class
el: "app" // 标签
```

可以使用其他双标签，但是不能使用html body
<!--more-->
## v-text 
设置标签的文本值
v-text="message+'|'"
""中可以使用字符串的拼接等表达式

## v-html
设置元素的innerHtml

## v-on
为元素绑定事件
v-on:click
v-on:mouseenter
v-en:dbclick
@简写
@click
@mouseenter
@dbclick

## v-bind
设置元素的属性 src title class
v-bind:src 简写为 :src
```html
<img :src="imgsrc">
<img :title="imgtitle">
// 增删class
<img :class="{active:isActive}">
```

## v-on 补充
回车出发事件：
@keyup.enter="sayHi"

## v-model 双向数据绑定

## axios
功能强大的网络请求库

```js
<script src="https://unpkg.com/axios/dist/axios.min.js">
</script>
axios.get(url).then(function(response){}, function(err){})
axios.post(url, {key1:value,key2:value}).
then(function(response){}, function(err){})
```

axios回调函数中的this已经改变，无法访问到data中的数据
把this保存起来，回调函数中直接使用保存的this

