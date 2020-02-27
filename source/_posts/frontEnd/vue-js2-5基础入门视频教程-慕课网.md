---
title: vue.js2.5基础入门视频教程-慕课网
date: 2019-12-07 23:20:56
categories:
- frontEnd
toc: true
tags:
- vue.js
---
## 1-1 课程介绍.mp4
[vue官方文档](https://cn.vuejs.org/v2/guide/installation.html)
## 2-1 创建第一个Vue实例.mp4

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="root">
        <h1>hello {{msg}}</h1>
    </div>
    <script>
        new Vue({
            //el确定挂载点
            el: "#root",
            data: {
                msg: "world"
            }
        })
    </script>
</body>
</html>
```
## 2-2 挂载点，模版与实例.mp4
挂载点、模板、实例之间的关系
vue中的el确定挂载点。
模板：挂载点里面的全部内容，可以直接写，也可以在Vue里面添加template

```html
<!--    <div id="root">-->
<!--        <h1>hello {{msg}}</h1>-->
<!--    </div>-->
<div id="root">
</div>
<script>
    new Vue({
        //el确定挂载点
        el: "#root",
        template: '<h1>hello {{msg}}</h1>',
        data: {
            msg: "world"
        }
    })
</script>
```
## 2-3 Vue实例中的数据,事件和方法.mp4
插值表达式
{{a}}
a在 new Vue({data: {a: 123}}) 中定义

```html
<h1>{{a}}</h1>
使用a-text也可实现同样的效果
<h1 a-text="a"></h1>
或
<h1 a-html="content"></h1>
```
```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
<!--添加点击事件-->
    <!--添加方法1-->
    <div id="root" @click="handleClick">
    <!--添加方法2-->
<!--    <div id="root" v-on:click="handleClick">-->
        {{msg}}
    </div>
    <script>
        new Vue({
            //el确定挂载点
            el: "#root",
            data: {
                msg: "world"
            },
            methods:{
                //定义方法
                handleClick: function(){
                    this.msg="hello";
                }
            }
        })
    </script>
</body>
</html>
```

## 2-4 Vue中的属性绑定和双向数据绑定.mp4

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="root">
        <div title="this is title1">hello world</div>
<!--属性绑定-->
<!--        方法1：-->
<!--        <div v-bind:title="title">hello world</div>-->
<!--        方法2（缩写，更常用）：-->
        <div :title="title">hello world</div>
<!-- 双向数据绑定-->
<!--        对比，单向：:value -->
<!--        <input :value="content">-->
<!--        对比，双向 v-model -->
        <input v-model="content">
        <div>{{content}}</div>
    </div>
    <script>
        new Vue({
            //el确定挂载点
            el: "#root",
            data: {
                msg: "world",
                title: "this is title2",
                content: "content"
            },
            methods:{

            }
        })
    </script>
</body>
</html>
```
## 2-5 Vue中的计算属性和侦听器.mp4

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="root">
        姓: <input v-model="firstName">
        名: <input v-model="lastName">
        <div>{{firstName}} {{lastName}}</div>
        <div>{{fullName}}</div>
        <div>{{count}}</div>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
                firstName: '',
                lastName: '',
                count: 0,
            },
            // 计算属性
            computed:{
                fullName: function () {
                    return this.firstName + ' ' + this.lastName
                }
            },
            // 监听器
            watch: {
                lastName: function () {
                    this.count ++;
                },
                firstName: function () {
                    this.count ++;
                }
            },
            methods:{
            }
        })
    </script>
</body>
</html>
```
## 2-6 v-if, v-show与v-for指令.mp4

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="root">
<!--        v-if 删除或添加元素-->
        <div v-if="show">hello world</div>
<!--        v-show 隐藏或显示元素-->
        <div v-show="show">hello world</div>
        <button @click="toggle">toggle</button>
<!--        v-for -->
        <ul>
            <li v-for="item of list">{{item}}</li>
            <li v-for="(item, index) of list" key="index">{{index}} {{item}}</li>
        </ul>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
                show: true,
                list: [1, 1, 3, 4, 5],
            },
            // 计算属性
            computed:{
            },
            // 监听器
            watch: {
            },
            methods:{
                toggle: function () {
                    this.show = !this.show
                }
            }
        })
    </script>
</body>
</html>
```
## 3-1 todolist功能开发.mp4

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="root">
        <div>
            <input v-model="thing">
            <button @click="submit">submit</button>
        </div>
        <ul>
            <li v-for="item of list">{{item}}</li>
        </ul>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
                thing: "",
                list: [],
            },
            // 计算属性
            computed:{
            },
            // 监听器
            watch: {
            },
            methods:{
                submit: function () {
                    if (this.thing.trim()==="")
                        return;
                    this.list.push(this.thing);
                    // append 失败
                    // this.list.append(this.thing);
                    this.thing = "";
                }
            }
        })
    </script>
</body>
</html>
```
## 3-2 todolist组件拆分-1.5.mp4

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="root">
        <div>
            <input v-model="thing">
            <button @click="submit">submit</button>
        </div>
        <ul>
            <!--  3. 属性传值  -->
            <todo-item
                    v-for="(item, index) of list"
                    :key="index"
                    :content="item">全局组件</todo-item>
<!--            <todo-item1>局部组件</todo-item1>-->
<!--            <li v-for="item of list">{{item}}</li>-->
        </ul>
    </div>
    <script>
        <!--全局组件-->
        // 注册后即可直接在HTML中使用
        Vue.component('todo-item', {
            // 1. 定义属性
            props: ['content'],
            // 2. 使用属性
            template: '<li>{{content}}</li>'
        });
        //局部组件
        // 声明后，在Vue中添加components
        // var todoItem = {
        //     template: '<li>item</li>'
        // };

        new Vue({
            el: "#root",
            // components: {
            //     'todo-item1': todoItem,
            // },
            data: {
                thing: "",
                list: [],
            },
            // 计算属性
            computed:{
            },
            // 监听器
            watch: {
            },
            methods:{
                submit: function () {
                    if (this.thing.trim()==="")
                        return;
                    this.list.push(this.thing);
                    // append 失败
                    // this.list.append(this.thing);
                    this.thing = "";
                }
            }
        })
    </script>
</body>
</html>
```
## 3-3 组件与实例的关系.mp4
组件也是实例, 也可以添加methods data等属性
new Vue(){} 实例中的template属性没有定义时，为挂载点下的HTML
```html
<!--全局组件-->
        // 注册后即可直接在HTML中使用
        Vue.component('todo-item', {
            // 1. 定义属性
            props: ['content'],
            // 2. 使用属性
            template: '<li @click="itemClicked">{{content}}</li>',
            methods: {
                itemClicked: function () {
                    alert('clicked');
                }
            }
        });
```
## 3-4 实现todolist的删除功能.mp4
组件之间通过属性传递值
```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Vue 入门</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="root">
        <div>
            <input v-model="thing">
            <button @click="submit">submit</button>
        </div>
        <ul>
<!--            @delete 或 v-on:delete 监听子组件-->
            <todo-item
                    v-for="(item, index) of list"
                    :key="index"
                    :content="item"
                    :index="index"
                    @delete="handleDelete"
            >全局组件</todo-item>
        </ul>
    </div>
    <script>
        Vue.component('todo-item', {
            props: ['content', 'index'],
            template: '<li @click="itemClicked">{{content}}</li>',
            methods: {
                itemClicked: function () {
                    // 子组件被点击，通知父组件删除
                    this.$emit("delete", this.index);
                }
            }
        });

        new Vue({
            el: "#root",
            // components: {
            //     'todo-item1': todoItem,
            // },
            data: {
                thing: "",
                list: [],
            },
            computed:{
            },
            watch: {
            },
            methods:{
                submit: function () {
                    if (this.thing.trim()==="")
                        return;
                    this.list.push(this.thing);
                    this.thing = "";
                },
                handleDelete: function (index) {
                    // delete 失败
                    // delete this.list[index]
                    // 从index开始删除1项
                    this.list.splice(index, 1);
                }
            }
        })
    </script>
</body>
</html>
```
## 4-1 vue-cli的简介与使用-1.5.mp4
https://cli.vuejs.org/zh/
npm install -g @vue/cli
npm install -g @vue/cli-init

```shell
➜  WebstormProjects vue init webpack todolist

? Project name todolist
? Project description A Vue.js project
? Author
? Vue build standalone
? Install vue-router? No
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Set up unit tests No
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recom
mended) npm
```
To get started:

  cd todolist
  npm run dev

components: {App} // {'App': App} 键值相等可简写
单文件组件
app.vue
template script style 3个标签
template 下只可有一个元素

网页自动刷新
## 4-2 使用vue-cli开发TodoList.mp4
npm run dev 执行的命令定义在package.json scripts中

```html

```
## 4-3 全局样式与局部样式.mp4

```html
scoped : 局部的样式
<style scoped>

</style>

```
## 4-4 课程总结-1.5.mp4

```html

```
