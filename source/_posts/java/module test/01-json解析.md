---
title: 01 json解析
date: 2020-02-07 16:47:53
categories:
- java
- module test
toc: true
tags:
---

[toc]

<!--more-->

# fastjson

## maven

```xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>fastjson</artifactId>
  <version>1.2.62</version>
</dependency>
```



## code

```java
package json;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;

import java.util.ArrayList;
import java.util.List;

// [Java中那些常用的json库性能比较，常见Json库用法示例代码 - 51CTO.COM](https://developer.51cto.com/art/201907/599631.htm)
public class JsonTest {

    public static void main(String[] args) {
        Person person = new Person();
        person.setId(1111);
        person.setUsername("zronghui");
        person.setAddress("广东");
        person.setAge(100);

        List<Person> list = new ArrayList<>();
        list.add(person);

        // fastjson
        // Java对象转化成为json字符串
        System.out.println(JSON.toJSONString(person));
        // 集合对象转化成为json字符串
        System.out.println(JSON.toJSONString(list));
        // 字符串转化成java对象
        Person person1 = JSON.parseObject(JSON.toJSONString(person), Person.class);
        // 字符串转化成json对象
        JSONObject jsonObject = JSON.parseObject(JSON.toJSONString(person));
        // 字符串转化成为java集合
        List<Person> people = JSON.parseArray(JSON.toJSONString(list), Person.class);

    }
}
```

Person.java

```java
package json;

import lombok.Data;
import lombok.Getter;
import lombok.Setter;

@Data
@Getter
@Setter
public class Person {
    private Integer id;
    private String username;
    private Integer age;
    private String address;
} 
```



# 问题

## maven helloworld

8、运行程序

在运行之前需要先进行相关的配置。点击Run->Edit Configurations...

<img src="https://i.loli.net/2020/02/07/Ju3Ec5bvPNfGq7e.png" alt="Ju3Ec5bvPNfGq7e" style="zoom: 33%;" />

<img src="https://i.loli.net/2020/02/07/78eY2PUXsBvbtVM.png" alt="78eY2PUXsBvbtVM" style="zoom:33%;" />

2 种运行方法

<img src="https://i.loli.net/2020/02/07/bnUOJMNKmqfSVYZ.png" alt="bnUOJMNKmqfSVYZ" style="zoom:33%;" />

## Maven项目中出现“不再支持目标选项 1.5。请使用 1.6 或更高版本。”的解决方法


在pom.xml中添加：

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```


## IDEA自动补全当前语句的分号

command shift + enter



## IDEA 中快速导入 maven 依赖

command + n 调出 generator 窗口，选 dependency

<img src="https://i.loli.net/2020/02/07/bsA7EJiP4SLRHlG.png" alt="bsA7EJiP4SLRHlG" style="zoom:33%;" />



输入关键词搜索，搜索的速度比较慢

<img src="https://i.loli.net/2020/02/07/tb5RYlQiDGU1qZx.png" alt="tb5RYlQiDGU1qZx" style="zoom:33%;" />

## [如何使用分页插件](https://pagehelper.github.io/docs/howtouse/)
