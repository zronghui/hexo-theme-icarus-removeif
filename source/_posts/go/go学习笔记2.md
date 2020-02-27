---
title: go学习笔记2
date: 2019-11-25 17:53:25
categories:
- go
toc: true
tags:
---
# 笨鸟学Go——最简单的Go教程 
https://www.slowbirdgogogo.com/syntax

### 阅读笔记
https://note.youdao.com/ynoteshare1/index.html?id=f58aa20a530e9e08a71250d411ff6831&type=note
<!--more-->
## 接口
接口的理解：
struct实现interface的所有方法后，方法返回struct可以将类型写成实现的那个interface名。
如：
error是个接口，有Error方法
```go
type error interface {
    Error() string
}
```
```go
type MyError struct {
	When time.Time
	What string
}
func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}
func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}
func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
```
