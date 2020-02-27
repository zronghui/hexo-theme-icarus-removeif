---
title: 06 joda time
description: 06 joda time
date: 2020-02-23 18:00:12
categories:
- java
- module test
toc: true
tags:
---

[toc]

[Introduction to Joda-Time | Baeldung](https://www.baeldung.com/joda-time)
[使用Joda-Time优雅的处理日期时间 - 简书](https://www.jianshu.com/p/efdeda608780)
[Joda-Time使用手册 - 后端 - 掘金](https://juejin.im/entry/5a30d9905188254a701f09fc)



## 核心类介绍

下面介绍5个最常用的date-time类：

- Instant - 不可变的类，用来表示时间轴上一个瞬时的点
- DateTime - 不可变的类，用来替换JDK的Calendar类
- LocalDate - 不可变的类，表示一个本地的日期，而不包含时间部分（没有时区信息）
- LocalTime - 不可变的类，表示一个本地的时间，而不包含日期部分（没有时区信息）
- LocalDateTime - 不可变的类，表示一个本地的日期－时间（没有时区信息）

## maven

```xml
<dependency>
  <groupId>joda-time</groupId>
  <artifactId>joda-time</artifactId>
  <version>2.10.5</version>
</dependency>
```



## code

```java
package jodaTime;

import org.joda.time.DateTime;

import java.util.Date;

public class JodaTime {
    public static void main(String[] args) {
        // 1. 构造一个DateTime实例
        // now
        DateTime dateTime1 = new DateTime();
        // 指定年月日
        DateTime dateTime2 = new DateTime(2020, 2, 23, 18, 12, 0);
        // long
        DateTime dateTime3 = new DateTime(12345678910L);
        // java.util.date
        DateTime dateTime4 = new DateTime(new Date());
        // ?
        DateTime dateTime5 = new DateTime("2016-02-15T00:00:00.000+08:00");

        // with 设定时间
        dateTime1.withYear(2019);
        // plus minus 加减时间
        dateTime1.plusDays(5);

        // 获取 property
        dateTime1.monthOfYear().getAsText();
        dateTime1.dayOfWeek().getAsText();

        // 因为当时那个地区执行夏令时的原因，在添加一个Period的时候会添加23个小时。
        // 而添加一个Duration，则会精确地添加24个小时，而不考虑历法。
        // 所以，Period和Duration的差别不但体现在精度上，也同样体现在语义上。
        // 因为，有时候按照有些地区的历法 1天 不等于 24小时。

        // 因此，用 Duration 不用 Period

        // 实例
        // 例一 计算上一个月的最后一天
        new DateTime().minusMonths(1).dayOfMonth().withMaximumValue();
        // 例二 获得任何一年中的第 11 月的第一个星期一的日期，而这天必须是这个月的第一个星期一之后
        new DateTime().monthOfYear().setCopy(11)
                .dayOfMonth().withMinimumValue()
                .plusDays(6)
                .dayOfWeek()
                .setCopy(1) // set to monday (it will round down)
                .plusDays(1);
        // 例三 计算五年后的第二个月的最后一天
        new DateTime().plusYears(5).monthOfYear().setCopy(2).dayOfMonth().withMaximumValue();

    }
}
```
