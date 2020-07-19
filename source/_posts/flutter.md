---
title: flutter
toc: true
recommend: 1
uniqueId: '2020-07-15 08:11:32/"flutter".html'
date: 2020-07-15 16:11:32
thumbnail:
categories:
tags:
keywords:
---



[TOC]

<!--more-->

# 学习



# 实践

## 安装

[入门: 在macOS上搭建Flutter开发环境 - Flutter中文网](https://flutterchina.club/setup-macos/#%E4%B8%8B%E4%B8%80%E6%AD%A5)

vscode -> flutter 插件

Android studio

flutter 下载后，解压，移动到 application 中，修改环境变量:

```shell
# flutter
# 中国代理
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
export PATH=/Applications/flutter/bin:$PATH
```

用 finder 找到，/Applications/flutter/packages/flutter_tools/gradle/flutter.gradle

替换为：

```python
# 第一处
buildscript {
    repositories {
        // google()
        // jcenter()
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.5.0'
    }
}
allprojects {
     repositories {
         maven { url 'https://maven.aliyun.com/repository/google' }
         maven { url 'https://maven.aliyun.com/repository/jcenter' }
         maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
     }
 }
# 第二处
// private static final String MAVEN_REPO      = "https://storage.googleapis.com/download.flutter.io";
    private static final String MAVEN_REPO      = "https://storage.flutter-io.cn/download.flutter.io";
```

为了能用 USB 调试米 6，vscode ，shift cmd p, flutter emulator , new（不是真的创建，因为太耗费存储、内存了）

## 创建新项目

vscode ，shift cmd p, flutter new project

# 工具



# 其他

## 有用的链接

['Tutorial/Flutter with App' 카테고리의 글 목록 (6 Page)](https://here4you.tistory.com/category/Tutorial/Flutter%20with%20App?page=6)
[Search results for time.](https://pub.dev/packages?q=time)
[time | Dart Package](https://pub.dev/packages/time#-readme-tab-)
[Page 1 | Flutter Favorite packages](https://pub.dev/flutter/favorites)
[json_serializable | Dart Package](https://pub.dev/packages/json_serializable)
[flutter-go/demo.dart at master · alibaba/flutter-go](https://github.com/alibaba/flutter-go/blob/master/lib/widgets/components/Pick/DayPicker/demo.dart)
[alibaba/flutter-go: flutter 开发者帮助 APP，包含 flutter 常用 140+ 组件的demo 演示与中文文档](https://github.com/alibaba/flutter-go)

## 遇到的问题

[Flutter Permissions Handler Not Working Properly - Stack Overflow](https://stackoverflow.com/questions/59515755/flutter-permissions-handler-not-working-properly)

在 android/app/src/main/AndroidManifest.xml 中声明需要的权限

