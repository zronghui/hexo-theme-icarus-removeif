---
title: 03 junit
date: 2020-02-08 10:48:14
categories:
- java
- module test
toc: true
tags:
description: 03 junit
---

[toc]

<!--more-->



# Junit

## maven

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.13</version>
  <scope>test</scope>
</dependency>
```



## code

```java
import log.log;
import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;

/**
 * log Tester.
 *
 * @author <Authors name>
 * @version 1.0
 * @since <pre>2月 8, 2020</pre>
 */
public class logTest {

    @Before
    public void before() throws Exception {
    }

    @After
    public void after() throws Exception {
    }

    /**
     * Method: add(int a, int b)
     */
    @Test
    public void testAdd() throws Exception {
        //TODO: Test goes here...
        log log = new log();
        assert log.add(1, 3) == 4;
        Assert.assertEquals(1, 2);
        Assert.assertTrue(true);
        Assert.assertNotNull(1);
    }

    /**
     * Method: main(String[] args)
     */
    @Test
    public void testMain() throws Exception {
        //TODO: Test goes here...
        testAdd();

    }
} 
```



### Assert

例如，Assert.assertEquals(3, result);

主要有如下几个断言方法：

- assertTrue/False（）：判断一个条件是 true 还是 false。
- fail（）：失败，可以有消息，也可以没有消息。
- assertEquals（）：判断是否想等，可以指定输出错误信息。注意不同数据类型所使用的 assertEquals 方法参数有所不同。
- assertNotNull/Null（）：判断一个对象是否为空。
- assertSame/NotSame（）：判断两个对象是否指向同一个对象。
- failNotSame/failNotEquals（）：当不指向同一个内存地址或者不相等的时候，输出错误信息。错误信息为指定的格式。
- setUp
  每次测试方法执行之前，都会执行 setUp 方法，此方法用于进行一些固定的准备工作，比如，实例化对象，打开网络连接等。
- tearDown
  每次测试方法执行之后，都会执行 tearDown 方法，此方法用于进行一些固定的善后工作，比如，关闭网络连接等。



## Failure与Error区别

**单元测试的失败（Failure）与测试出现了错误（Error）**
JUnit 将测试失败的情况分为两种：Failure 和 Error 。 Failure 一般是由单元测试使用的断言方法判断失败引起的，它表示在测试点发现了问题（程序中的 bug）；而 Error 则是有代码异常引起的，这是测试目的之外的发现，它可能产生于测试代码本身的错误（也就是说，编写的测试代码有问题），也可能是被测试代码中的一个隐藏 bug 。不过，一般情况下是第一种情况。

## 常用注解

- @Before
  初始化方法，在任何一个测试方法执行之前，必须执行的代码。对比 JUnit 3 ，和 setUp（）方法具有相同的功能。在该注解的方法中，可以进行一些准备工作，比如初始化对象，打开网络连接等。
- @After
  释放资源，在任何一个测试方法执行之后，需要进行的收尾工作。对比 JUnit 3 ，和 tearDown（）方法具有相同的功能。
- @Test
  测试方法，表明这是一个测试方法。在 JUnit 中将会自动被执行。对与方法的声明也有如下要求：名字可以随便取，没有任何限制，但是返回值必须为 void ，而且不能有任何参数。如果违反这些规定，会在运行时抛出一个异常。不过，为了培养一个好的编程习惯，我们一般在测试的方法名上加 test ，比如：testAdd（）。
  同时，该 Annotation（@Test） 还可以测试期望异常和超时时间，如 @Test（timeout=100），我们给测试函数设定一个执行时间，超过这个时间（100毫秒），他们就会被系统强行终止，并且系统还会向你汇报该函数结束的原因是因为超时，这样你就可以发现这些 bug 了。而且，它还可以测试期望的异常，例如，我们刚刚的那个空指针异常就可以这样：@Test(expected=NullPointerException.class)，此时如果出现空指针异常，反正会认为测试通过

- @Ignore
  忽略的测试方法，标注的含义就是“某些方法尚未完成，咱不参与此次测试”；这样的话测试结果就会提示你有几个测试被忽略，而不是失败。一旦你完成了相应的函数，只需要把 @Ignore 注解删除即可，就可以进行正常测试了。
- @BeforeClass
  针对所有测试，也就是整个测试类中，在所有测试方法执行前，都会先执行由它注解的方法，而且只执行一次。当然，需要注意的是，修饰符必须是 public static void xxxx ；此 Annotation 是 JUnit 4 新增的功能。
- @AfterClass
  针对所有测试，也就是整个测试类中，在所有测试方法都执行完之后，才会执行由它注解的方法，而且只执行一次。当然，需要注意的是，修饰符也必须是 public static void xxxx ；此 Annotation 也是 JUnit 4 新增的功能，与 @BeforeClass 是一对。
