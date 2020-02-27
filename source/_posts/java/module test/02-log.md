---
title: 02 log
date: 2020-02-08 08:56:04
categories:
- java
- module test
toc: true
tags:
---

[toc]

<!--more-->

## log

**SLF4J**不同于其他日志类库，与其它有很大的不同。SLF4J(Simple logging Facade for Java)不是一个真正的日志实现，而是一个**抽象层**（ abstraction layer），它**允许你在后台使用任意一个日志类库**。

在代码中表示为**{}**的特性。占位符是一个非常类似于在String的format()方法中的%s，它会在运行时被某个提供的实际字符串所替换。

slf4j-log4j12-1.5.8.jar –- Log4J Adapter for SLF4J

### maven

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>1.7.30</version>
</dependency>
```



### code

```java
package log;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class log {
    private static Logger logger = LoggerFactory.getLogger(log.class);

    public static void main(String[] args) {
        logger.info("template: {} + {} = {}", 1, 2, 3);
        logger.error("something wrong");
    }
}

```



### properties

```properties
# Configure logging for testing: optionally with log file
log4j.rootLogger=INFO, stdout
#log4j.rootLogger=WARN, stdout
# log4j.rootLogger=WARN, stdout, logfile
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=target/spring.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```



