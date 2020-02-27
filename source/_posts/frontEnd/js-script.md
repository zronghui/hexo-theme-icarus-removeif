---
title: js script
date: 2020-01-10 17:24:41
categories:
- frontEnd
toc: true
tags:
- js
---
# js script
<!--more-->
## 回文十进制数
求用十进制、二进制、八进制表示都是回 文数的所有数字中， 大于十进制数 10 的最 小值。

```js
/* 为字符串类型添加返回逆序字符串的方法 */ 
String.prototype.reverse = function (){ 
    return this.split("").reverse().join(""); 
}

/* 从 11 开始搜索 */ 
var num = 11; 

while (true){
if (
    (num.toString() == num.toString().reverse()) 
    && (num.toString(8) == num.toString(8).reverse()) 
    && (num.toString(2) == num.toString(2).reverse())){ 
    console.log(num); break; 
} /* 只搜索奇数，每次加 2 */ 
    num += 2;
}
```



