---
title: jquery复习
date: 2019-11-27 10:20:40
categories:
- frontEnd
toc: true
tags:
---
# 样式篇
<!--more-->
## 第1章 初识jQuery
```html
<script type="text/javascript" src="./static/lib/jquery.js">
```

```javascript
// $(document).ready() 的作用是等文档的节点都加载完毕后再执行后续的代码
$(document).ready(function(){
    $("div").html("hello jquery");
});
```

### 1-4 jQuery对象与DOM对象

```javascript
// 标准js处理
var p = document.getElementById('imooc');
p.innerHTML = 'adsfas';
p.style.color = 'red';
// jquery处理
var $p = $('#imooc');
$p.html('adsfaf').css('color', 'red');
```

### 1-5 jQuery对象转化成DOM对象
```javascript
var $div = $('div')
var div = $div[0] // jquery对象转dom对象
// 或者 var div = $div.get(0)
div.style.color = 'red'

// 1-6 DOM对象转化成jQuery对象
var $div = $(div); // $(dom)
var $first = $div.first()
```

## 第2章 jQuery选择器
[jQuery 参考手册 - 选择器](https://www.w3school.com.cn/jquery/jquery_ref_selectors.asp)

### 2-1 id选择器

```js
$("#id")
```

### 2-2 类选择器

```js
$(".class")
```

### 2-3 元素选择器

```js
$("p")
document.getElementsByTagName('div')
```

### 2-4 全选择器（*选择器）

```js
$("*")
```
###  2-5 层级选择器

```js
// 子元素选择器
$("parent > child")
// 后代元素选择器
$("ancestor descendant")
// 相邻兄弟元素选择器
$("prev + next")
// 一般兄弟元素选择器
$("prev ~ siblings")

```
### 2-7 基本筛选选择器

```js
$(".div:first")

$(":first") // 匹配第一个元素
$(":last") // 匹配最后一个元素
$(":not(selector)") //一个用来过滤的选择器,选择所有元素去除不匹配给定的选择器元素
$(":eq(index)")// 在匹配的集合中选择索引值为 index的元素
$(":gt(index)") // 选择匹配集合中所有大于给定 index(索引值)的元素
$(":even") // 选择索引值为偶数的元素,从0开始计数
$(":odd") // 选择索引值为奇数的元素,从O开始计数
$(":lt(index")// 选择匹配集合中所有索引值小于给定 index参数的元素
$(":header")// 选择所有标题元素,像h1h2,h3等
$(":lang(language)")// 选择指定语言的所有元素
$(":root") // 选择该文档的根元素
$(":animated") // 选择所有正在执行动画效果的元素

```
###  2-8 内容筛选选择器

```js
$(":contains(text)") //选择所有包含指定 文本 的元素
$(":parent") // 选择所有含有子元素或者文本的元素(即非空)
$(":empty") // 选择所有没有子元素的元素(包含文本节点)
$(":has(selector")) // 选择元素中至少包含指定 选择器 的元素
// 1) :contains与:has都有查找的意思，但是contains查找包含“指定文本”的元素，has查找包含“指定元素”的元素
// 2) 如果:contains匹配的文本包含在元素的子元素中，同样认为是符合条件的。
// 3):parent与:empty是相反的，两者所涉及的子元素，包括文本节点
```

### 2-9 可见性筛选选择器

```js
$(":visible") // 选择所有现实的元素
$(":hidden") // 选择所有隐藏的元素
```

### 2-10 属性筛选选择器

```js
$("[attribute]") // 选择所有具有指定属性的元素
$("[attribute='value']") // 选择指定属性是给定值的元素
$("[attribute^='value']") // 选择指定属性是以给定字符串开始的元素
$("[attribute$='value']") //选择指定属性是以给定值结尾的元素,这个比较是区分大小写的
$("[attribute!='value']") // 选择不存在指定属性,或者指定的属性值不等于给定值的元素

$("[attribute|=value]") // 选择指定属性值等于给定字符串或以该文字串为前缀(该字符串后跟一个连字符"-")的元素
$("[attribute*='value']") // 选择指定属性具有包含一个给定的子字符串的元素(选择给定的属性是以包含某些值的元素)
$("[attribute-='value']") // 选择指定属性用空格分隔的值中包含一个给定值的元素
$("[attributeFilter1][attributeFilterN]") //选择匹配所有指定的属性筛选器的元素

```
### 2-11 子元素筛选选择器

```js
// 选择所有父级元素的第一个子元素
$(":first-child")
$(":last-child")
$(":only-child")
$(":nth-child(n)")
$(":nth-last-child(n)")
// :first只匹配一个单独的元素，但是:first-child选择器可以匹配多个：即为每个父级元素匹配第一个子元素。这相当于:nth-child(1)
```

### 2-12 表单元素选择器

```js
$(":input") // 选择所有 input, textarea, select和 button元素
$(":text") // 匹配所有文本框
$(":password") // 匹配所有密码框
$(":radio") // 匹配所有单选按钮
$(":checkbox") // 匹配所有复选框
$(":submit") // 匹配所有提交按钮
$(":image")// 匹配所有图像域
$(":reset") // 匹配所有重置按钮
$(":button") // 匹配所有按钮
$(":file") // 匹配所有文件域
```

### 2-13 表单对象属性筛选选择器

```js
$(":enabled") // 选取可用的表单元素
$(":disabled") // 选取不可用的表单元素
$(":checked") // 选取被选中的<input>元素
$(":selected") // 选取被选中的<option>元素
```

## 第3章 jQuery的属性与样式

```js
// 3-1 .attr()与.remov...
attr(key) 获取key属性值
attr(key, value) 设置
attr(attributes) // attrs: {key1: val1, key2:val2}

.removeAttr(attributeName) 

// 3-2 html()及.text()
.html() 获取
.html(htmlString) 设置
.text()
.text(textString)

// 3-3 .val()
// 获取、设置表单元素的值
.val()
.val(value)

// 3-4 增加样式.addClass()
.addClass(className) // 为每个匹配元素增加一个或多个样式名

// 3-5 删除样式.removeClass()
.removeClass([className]) // 每个匹配元素移除一个或多个用空格隔开的样式名

// 3-6 切换样式.toggleCla...
.toggleClass(className) //如果存在（不存在）就删除（添加）一个类

// 3-7 样式操作.css()
// 获取
.css(propertyName) // 获取匹配元素集合中的第一个元素的样式属性的计算值
.css(propertyNames) // 传递一组数组，返回一个对象结果
// 设置
.css(propertyName, value ) // 设置CSS
.css(properties) // 可以传一个对象，同时设置多个样式
```

# DOM篇
## 第2章 DOM节点的创建
###  2-1 DOM创建节点及节点属性
创建元素：document.createElement
设置属性：setAttribute
添加文本：innerHTML
加入文档：appendChild
###  2-2 jQuery节点创建与属性的处理

```js
$("<div class='right'><div class='aaron'>动态创建DIV元素节点</div></div>")
```
##  第3章 DOM节点的插入
###  3-1 DOM内部插入append()与appendTo()
b加到a中:

```js
a.append(b) 
b.appendTo(a) 
```
###  3-2 DOM外部插入after()与before()
###  3-3 DOM内部插入prepend()与prependTo()
###  3-4 DOM外部插入insertAfter()与insertBefore()
## 第4章 DOM节点的删除
###  4-1 DOM节点删除之empty()的基本用法

```js
$('.hello').empty()
```
###  4-2 DOM节点删除之remove()的有参用法和无参用法

```js
$('.hello').remove()
$("p").filter(":contains('3')").remove()
```
###  4-3 DOM节点删除之empty和remove区别
* empty()方法并不是删除节点，而是清空节点，它能清空元素中的所有后代节点
* 该节点与该节点所包含的所有后代节点将同时被删除; 提供传递一个筛选的表达式，删除指定合集中的元素

###  4-4 DOM节点删除之保留数据的删除操作detach()
detach()不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。与remove()不同的是，所有绑定的事件、附加的数据等都会保留下来。
$("div").detach()这一句会移除对象，仅仅是显示效果没有了。但是内存中还是存在的。当你append之后，又重新回到了文档流中。就又显示出来了。

## 第5章 DOM节点的复制与替换
###  5-1 DOM拷贝clone()
.clone()方法深度 复制所有匹配的元素集合，包括所有匹配元素、匹配元素的下级元素、文字节点。
clone(ture) 不仅仅只是克隆单纯的节点结构，还要把附带的事件与数据给一并克隆了
###  5-2 DOM替换replaceWith()和replaceAll()
二者方向相反

```js
$("p:eq(1)").replaceWith('<a style="color:red">替换第二段的内容</a>')
$('<a style="color:red">替换第二段的内容</a>').replaceAll('p:eq(1)')
```
###  5-3 DOM包裹wrap()方法
.wrap( wrappingElement )：在集合中匹配的每个元素周围包裹一个HTML结构

```js
$('p').wrap('<div></div>')
```
###  5-4 DOM包裹unwrap()方法
去除1层父级元素
```js
$('p').unwrap();
```
###  5-5 DOM包裹wrapAll()方法
给所有p元素增加一个div包裹
```js
$('p').wrapAll('<div></div>')
```
###  5-6 DOM包裹wrapInner()方法
将合集中的元素内部所有的子元素用其他元素包裹起来

```js
$('div').wrapInner('<p></p>')
```
替换前
```html
<div>p元素</div>
<div>p元素</div>
```
替换后

```HTML
<div>
    <p>p元素</p>
</div>
<div>
    <p>p元素</p>
</div>
```

## 第6章 jQuery遍历
###  6-1 children()方法
$("div").children(".selected")
###  6-2 find()方法
.find()和.children()方法是相似的
1.children只查找第一级的子节点
2.find查找范围包括子节点的所有后代节点
###  6-3 parent()方法
因为是父元素，这个方法只会向上查找一级;parent()无参数
###  6-4 parents()方法
类似find与children的区别，parent只会查找一级，parents则会往上一直查到查找到祖先节点
###  6-5 closest()方法
从元素本身开始，在DOM 树上逐级向上级元素匹配，并返回最先匹配的祖先元素
$("div").closet("li')
注意：jQuery是一个合集对象，所以通过closest是匹配合集中每一个元素的祖先元素
###  6-6 next()方法
next()无参数
允许我们找遍元素集合中紧跟着这些元素的直接兄弟元素，并根据匹配的元素创建一个新的 jQuery 对象
###  6-7 prev()方法

###  6-8 siblings()
siblings()无参数
取得一个包含匹配的元素集合中每一个元素的同辈元素的元素集合
注意：jQuery是一个合集对象，所以通过siblings是匹配合集中每一个元素的同辈元素
###  6-9 add()方法

```js
$('li').add('p')
$('li').add(document.getElementsByTagName('p')[0])
$('li').add('<p>新的p元素</p>').appendTo(目标位置)
```
###  6-10 each()

```js
$("li").each(function(index, element) {
     index 索引 0,1
     element是对应的li节点 li,li
     this 指向的是li
})
```

# 更多
[jQuery基础(五)一Ajax应用与常用插件-慕课网](https://www.imooc.com/learn/762)

[jQuery基础修炼圣典—动画篇](https://www.imooc.com/learn/430)



```js
1. 
// $div是一个变量名？不加$?
var $div = $('div')
var div = $div[0] // jquery对象转dom对象
```
