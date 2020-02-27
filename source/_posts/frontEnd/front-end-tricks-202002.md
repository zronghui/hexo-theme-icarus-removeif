---
title: front end tricks 202002
description: front end tricks 202002
date: 2020-02-24 17:17:38
categories:
- frontEnd
toc: true
tags:
---

[toc]

## [JavaScript 倒计时关闭页面-蚂蚁部落](https://www.softwhy.com/article-3402-1.html)

```html
<script type="text/javascript"> 
var otimer;
var second=10;
function timer(){
  otimer.innerHTML=second;
  if(second>0){
    second=second-1;
    return false;
  }
  window.close();
} 
window.onload=function(){
  otimer=document.getElementById("timer");
  setInterval(timer,1000);
}
```

以上代码实现了我们想要的功能，可以倒计时10秒之后关闭页面。

原理非常的简单，利用定时器setInterval()方法，不断调用timer()函数，每调用一次秒数减一，直到秒数变为零就执行window.close()，将页面关闭。同时每次调用函数都会将当前的剩余秒数写入div中，于是实现了倒计时效果。

**相关阅读：**

（1）.innerHTML属性参阅[JavaScript innerHTML](https://www.softwhy.com/article-9293-1.html)一章节。 

（2）.setInterval()方法参阅[JavaScript setInterval()](https://www.softwhy.com/article-9300-1.html)一章节。 

## [JavaScript 获取倒数第二个li元素-蚂蚁部落](https://www.softwhy.com/article-8133-1.html)

```html
<script type="text/javascript">
window.onload=function(){
  var obox=document.getElementById("box");
  var oshow=document.getElementById("show");
  var lis=obox.getElementsByTagName("li");
  oshow.innerHTML=lis[lis.length-2].innerHTML;
}
</script>
```

## [JavaScript 获取网页脚本代码内容-蚂蚁部落](https://www.softwhy.com/article-1154-1.html)

```js
function evalscript(s){
  if(s.indexOf('<script') == -1) return s;
  var p = /<script[^\>]*?>([^\x00]*?)<\/script>/ig;
  var arr = [];
  while(arr = p.exec(s)){
    var p1 = /<script[^\>]*?src=\"([^\>]*?)\"[^\>]*?(reload=\"1\")?(?:charset=\"([\w\-]+?)\")?><\/script>/i;
    var arr1 = [];
    arr1 = p1.exec(arr[0]);
    if(arr1){
      appendscript(arr1[1], '', arr1[2], arr1[3]);
    } 
    else{
      p1 = /<script(.*?)>([^\x00]+?)<\/script>/i;
      arr1 = p1.exec(arr[0]);
      appendscript('', arr1[2], arr1[1].indexOf('reload=') != -1);
    }
  }
  return s;
}
```

给出了核心代码，直接套用即可。

## [JavaScript 替换字符串全部指定内容-蚂蚁部落](https://www.softwhy.com/article-1153-1.html)

将字符串中所有的字符"n"替换为字符"b"

```js
String.prototype.replaceAll=function(str,repaceStr) {
  return this.replace(new RegExp(str,"gmi"),repaceStr) // "gmi" ? 
}
var str="antzone";
console.log(str.replaceAll("n","b"));
```


## [正则表达式删除字符串两端空格-蚂蚁部落](https://www.softwhy.com/article-1152-1.html)
```js
String.prototype.trim=function(){
  var reExtraSpace=/^\s*(.*?)\s+$/;
  return this.replace(reExtraSpace,"$1");
}
var str="  antzone ";
console.log(str.length);
var strNew=str.trim();
console.log(strNew.length);
```


## [CSS 紧邻下一个兄弟元素-蚂蚁部落](https://www.softwhy.com/article-8241-1.html)
```css
.antzone+li{
  color:red;
}
```

代码将class属性值为antzone的li元素的紧邻下一个兄弟li元素的字体颜色设置为红色。

+选择器可以参阅[CSS的相邻选择符(E+F)](https://www.softwhy.com/article-460-1.html)一章节。

## [CSS 获取所有紧邻兄弟元素-蚂蚁部落](https://www.softwhy.com/article-5201-1.html)

```css
.ant ~ li{
  color:red;
}
```

将class属性值为"ant"元素后面的li元素字体颜色设置为红色，具体参阅[CSS E~F 兄弟选择器](https://www.softwhy.com/article-9599-1.html)一章节。


