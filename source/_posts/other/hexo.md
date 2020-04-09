---
title: hexo
date: 2019-08-29 19:05:31
categories:
- other
toc: true
tags:
---
<!--toc-->
<!--more-->
# hexo 安装、使用

## 安装

见

<a>https://wuzequn.com/2018/04/27/blog-build-and-ipv6-tools</a>

<a>http://www.dragonbaby308.com/hexo</a>

<!--more-->

## 初始化项目

hexo init blog

## 配置

修改 bog 项目下的\_config.yml 文件，D:\01 Code\hexo_config.yml
language: zh-Hans

## 使用

### 创建新文章

hexo new new-article

### 重新发布

hexo clean;hexo g;hexo s

hexo clean

hexo generate 或 hexo g

hexo server 或 hexo s

## 部署到 github

<blockquote>
服务器部署
（一） 本地 + github.io 白嫖部署

1. 生成 github.io 仓库
   首先注册并登录 GitHub，创建新 public 仓库，仓库名称一定要是：
   YourGitHubName.github.io（YourGitHubName 是你的 GitHub 昵称，大小写敏感！）

2. 本地安装 Hexo 的 git 部署插件
   在 Hexo 的目录下，输入 npm install --save hexo-deployer-git，会报一个 peerDependencies WARNING，可以忽略。

3. 本地修改\_config.yaml 文件
   在 Hexo 目录下，找到\_config.yaml 文件，在#Deployment 做如下修改：

<html>
    <title>Markdown</title>

    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
    type: git
    repo: https://github.com/DragonBaby308/DragonBaby308.github.io.git #你的 github.io 的网址
    branch: master #“type:”、“repo:”和“branch:”后都要带一个空格

</html>

4. 部署
   hexo d
   部署成功后，浏览器输入 YourGitHubName.github.io 即可访问，其中 YourGitHubName 是你的 GitHub 昵称，且大小写敏感
   见
   </blockquote>

详见

<a>http://www.dragonbaby308.com/hexo/</a>

## 发布

hexo clean;hexo g;hexo d

输入 github 账号名，密码

hexo d 即 hexo deploy

## 其他配置

Hexo+Next 个人博客主题优化 - 简书
https://www.jianshu.com/p/efbeddc5eb19

主要配置了：

![20190829215100.png](https://i.loli.net/2019/08/29/sE5TD1lwjCId7ue.png)

Hexo 之 next 主题设置首页不显示全文(只显示预览) - 简书
https://www.jianshu.com/p/393d067dba8d

Hexo+Next个人博客主题优化 - 简书
https://www.jianshu.com/p/efbeddc5eb19

Dragonstyle's Home – 记录、分享、交流
http://www.dragonstyle.win/

### 修改文章宽度

https://ihaoming.top/archives/9a935f57.html

### deploy 免账号密码

> 可将\_config.yml 中的 repo 修改为如下标准格式：
> repo: https://用户名:密码@github.com/用户名/用户名.github.io.git
> 这样做的好处就是每次 hexo deploy 提交时不需要输入账号密码。

### hexo的首页只显示文章的部分内容
[让hexo的首页只显示文章的部分内容而不是全部 | 朱启的个人博客](http://blog.smallerpig.com/set-hexo-show-more-button-on-indexpage.html)

``` 
第二种方法

在你写 md 文章的时候，可以在内容中加上 <!--more-->，这样首页和列表页展示的文章内容就是 <!--more--> 之前的文字，而之后的就不会显示了。

效果

上面两种方式展示出来的效果是不一样的。

第一种修改 _config.yml 文件的效果是会格式化你文章的样式，直接把文字挤在一起显示，最后会有 …。

而第二种加上 <!--more-->展示出来的就是你原本文章的样式，最后不会有…
```

## 换电脑

见

<a>https://feijunjie.github.io/2018/10/10/20181010-%E5%A6%82%E4%BD%95%E5%9C%A8%E5%8F%A6%E4%B8%80%E5%8F%B0%E7%94%B5%E8%84%91%E4%B8%8A%E7%BB%A7%E7%BB%ADhexo%E5%86%99%E5%8D%9A%E5%AE%A2/</a>

## 注意事项
在标题开头使用如下形式
[精]精华文章
会报错



在代码块中使用了双花括号，{{blank}}，使用这个会让markdown文件出错，在花括号中间随便写了一个单词{{massage}}就解决了

## 问题
### search一直加载
解决办法：
1. 查看search.xml的请求
2. 打开新窗口，直接访问xml网址，查看哪一行解析失败，回到请求页面，查看是哪个文件
3. 去vim查看异常字符

### 热度统计没生效
[Next 解决 Busuanzi 统计浏览失效 | G加菲](https://www.gmlyo.com/2018/12/02/Next-%E8%A7%A3%E5%86%B3-Busuanzi-%E7%BB%9F%E8%AE%A1%E6%B5%8F%E8%A7%88%E5%A4%B1%E6%95%88/)

### 文章置顶
[Hexo文章置顶的方法 - 简书](https://www.jianshu.com/p/bdd1239863de)
### 侧边栏添加自定义文件夹
[【Hexo + Next】侧边栏添加自定义文件夹（如友链）_Spr Chan的博客-CSDN博客](https://blog.csdn.net/weixin_43971764/article/details/97644190)
### Archive页面显示文章数量
npm install hexo-generator-archive --save

_config.yml中新增相关配置
archive_generator:
  per_page: 40  #值为0表示不分页，按需填写
  yearly: true  #是否按年生成归档
  monthly: true  #按月归档
index_generator:
  per_page: 40  #值为0表示不分页，按需填写
  yearly: true  #是否按年生成归档
  monthly: true  #按月归档

tag_generator:
  per_page: 40  #值为0表示不分页，按需填写
  yearly: true  #是否按年生成归档
  monthly: true  #按月归档

category_generator:
  per_page: 40  #值为0表示不分页，按需填写
  yearly: true  #是否按年生成归档
  monthly: true  #按月归档

### EIO: i/o error
➜  hexo hexo d
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Error: EIO: i/o error, stat '/Volumes/Data/01 Code/hexo/.deploy_git/2019'
➜  hexo open .
在finder中，删除文件夹
再hexo deploy

[hexo文章模板设置 | 拈花把酒偏折煞世人情狂](https://shmilybaozi.github.io/2018/11/05/hexo%E6%96%87%E7%AB%A0%E6%A8%A1%E6%9D%BF%E8%AE%BE%E7%BD%AE/)

[Hexo设置网站的图标Favicon | G加菲](https://www.gmlyo.com/2018/06/06/Hexo%E8%AE%BE%E7%BD%AE%E7%BD%91%E7%AB%99%E7%9A%84%E5%9B%BE%E6%A0%87Favicon/)
[在线免费将图片转换成ico图标格式-转换成ico](https://jinaconvert.com/cn/convert-to-ico.php)

# hexo中文版
<img src="https://i.loli.net/2020/01/14/WqyrkmoDMjSlHY7.png" alt="WqyrkmoDMjSlHY7" style="zoom: 33%;" />
<img src="https://i.loli.net/2020/01/14/uro1ynZdBiXEHKp.png" alt="uro1ynZdBiXEHKp" style="zoom: 33%;" />
<img src="https://i.loli.net/2020/01/14/8Wu9HGLymK3Cw4O.png" alt="8Wu9HGLymK3Cw4O" style="zoom:33%;" />
![fWXaVcbtCg8kMNo](https://i.loli.net/2020/01/14/fWXaVcbtCg8kMNo.png)

[jaredly/hexo-admin: An Admin Interface for Hexo](https://github.com/jaredly/hexo-admin)



## 图片和描述

```
photos:
- http://bruce.u.qiniudn.com/2013/11/27/reading/photos-0.jpg
description: 03 junit
```

## 多环境撰文

hexo与github pages共同搭建博客有一个麻烦的地方就在于多环境下要同步本地的博客内容很麻烦，唯一的方法就是你用要么一个硬盘云盘之类的要么就是用github之类的，每次有更新你就同时把本地的博客文件都同步上去，毫无疑问这非常麻烦。
之前想要自己写一个自动化工具来做这个事情，这几天发现有人已经写好了，可以直接用，这里直接贴出链接，里面也有详细的说明，按照步骤来做就好了。
[hexo-git-backup](https://github.com/coneycode/hexo-git-backup)



### SEO

当我们搭建一个网站之后，如果没有做一些相关的搜索引擎优化SEO，那么我们的网站是很难获取来自搜索引擎的流量的，用户很难在搜索引擎上搜索到我们网站的内容，所以在这里我们要为Hexo网站做一些简单的搜索优化。
上周刚搭建好博客的时候只有谷歌能搜索到自己的博客，百度直接搜域名都没有任何信息，主要原因是因为Github Pages屏蔽了百度爬虫，百度根据没办法知道我们博客的内容，所以我将博客同步到两个平台上，一个Github，一个国内的Gitcafe，目前搜索自己博客的相关信息基本都在第一条。

## SiteMap

首先安装hexo的sitemap网站地图生成插件：

```
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

在你的hexo站点的_config.yml添加下面的代码：

```
# hexo sitemap网站地图
sitemap:
path: sitemap.xml
baidusitemap:
path: baidusitemap.xml
```

配置成功后，hexo编译时会在hexo站点根目录生成sitemap.xml和baidusitemap.xml
其中sitemap.xml适合提交给谷歌搜素引擎，baidusitemap.xml适合提交百度搜索引擎。

## 蜘蛛协议robots.txt

在source目录下创建robots.txt文件，添加下面的一段代码：

```
Sitemap: http://www.linbinghe.com/sitemap.xml
Sitemap: http://www.linbinghe.com/baidusitemap.xml
```

请自行改成自己的网站。

完整robots.txt文件内容：

```
# hexo robots.txt
User-agent: *
Allow: /
Allow: /archives/

Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: http://www.arao.me/sitemap.xml
Sitemap: http://www.arao.me/baidusitemap.xml
```

## 谷歌Search Console与百度站长工具

这两个平台都是便于管理自己的网站，查看爬虫爬去频率等等，这两个的使用都不难，但是两者都需要通过验证，只要搜索这两个平台，到各自官网添加域名，按照文档说明通过验证即可。
谷歌可以通过提交站点地图提交我们的sitemap.xml，百度目前已经禁止了。



## 主动推送新链接

解决百度爬虫被禁止访问的问题，提升网站收录质量和速度。

```
npm install hexo-baidu-url-submit --save
```

在 站点配置文件 中添加如下代码：

```
baidu_url_submit:
  count: 5 ## 比如3，代表提交最新的三个链接
  host: blog.tangxiaozhu.com ## 在百度站长平台中注册的域名
  token:  ## 请注意这是您的秘钥， 请不要发布在公众仓库里!
  path: baidu_urls.txt ## 文本文档的地址， 新链接会保存在此文本文档里
```

为了主动推送链接，你还得在全局配置文件的deploy中添加配置：

```
deploy:
- type: baidu_url_submitter
```



## [Hexo: 给博客添加百度统计](https://postgres.fun/20181027203300.html)

当Hexo博客被百度、必应、谷歌搜索引擎收录以后，有件重要的工作是统计博客的访问情况，比如博客的历史访问量、搜索关键字、访问来源、访问地域等统计数据。

[**百度统计**](https://tongji.baidu.com/web/welcome/login) 能方便的完成网站访问量分析统计，本文简单演示下Hexo+Next博客配置百度统计功能。

**开通百度统计帐号**

在 [**百度统计**](https://tongji.baidu.com/web/welcome/login) 注册帐号。

帐号注册成功后，在网站列表中添加目标网站。

**获取跟踪代码**

网站添加之后在代码管理模块选择代码获取，可以看到如下代码:

```
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?____________________";
  var s = document.getElementsByTagName("script")[0];
  s.parentNode.insertBefore(hm, s);
})();
</script>
```

这段代码需要用户添加到网站全部页面的 `` 标签前，Next主题已对百度统计进行配置优化，只需要配置主题配置文件即可，下面会详细介绍。

其中 hm.js? 后面的字符串为用户的 key 值，将 key 值记录下来，后面会用到。

**配置主题配置文件**

配置主题配置文件 /d/hexo/themes/next/_config.yml ，配置 baidu_analytics 参数，如下:

```
# Baidu Analytics ID
baidu_analytics: 上面步骤中记录的百度统计里用户的key值。
```

修改完参数后执行 `hexo g` 和 `hexo d` 命令部署博客。

**验证百度统计**

之后仍然在代码管理模块的代码获取页面进行验证，如下图:

<img src="https://postgres.fun/images/baidu_tongji_check.png" alt="img" style="zoom:50%;" />

上图表示验证通过。

一般过20分钟左右就可以看到网站分析数据，过了几小时后，已经看到博客的访问统计分析数据，如下图:

<img src="https://postgres.fun/images/baidu_tongji_report1.png" alt="img" style="zoom:50%;" />



[网站列表 - 网站中心](https://tongji.baidu.com/sc-web/10000142524/home/site/index?from=3)
[网站概况 - 百度统计](https://tongji.baidu.com/web/10000142524/overview/index?siteId=14367916)

## 评论

[Hexo使用gitalk作为评论插件 | VoidKing](https://www.voidking.com/dev-hexo-gitalk-comment-plugin/)

[Hexo使用livere作为评论插件 | VoidKing](https://www.voidking.com/dev-hexo-livere-comment-plugin/)

gittalk 好看，livere 加载快

[(...) Gitalk评论插件使用教程 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000018072952)



## 邮件订阅

[Publicize :: Email Subscriptions](https://feedburner.google.com/fb/a/emailsyndicationSubmit)
[How To Set Up RSS Feed For WordPress Using Feedburner](https://www.shoutmeloud.com/how-to-burn-your-feed-using-feedburner.html)





[ico图标制作,在线Favicon.ico制作转换工具,实时预览ico生成效果,ico图标下载](http://cn.faviconico.org/favicon)
