---
title: 《go语言核心编程》阅读笔记1
date: 2019-11-26 22:37:17
categories:
- go
toc: true
tags:
---
# 第一章 基础知识
1. 函数以func开头，{必须在函数头所在行尾部
2. 字符串用双引号
3. ->go build hello.go ->./hello
4. 标识符 包括fallthrough
5. go也有+= -= ... ++ --

## 字符串
* 字符串是常量 

```go
var a = "hello"
b:= a[0]
```
* 字符串转换成切片[]byte(s)要慎用，每转换一次都复制一次内容

```go
b := []byte(s)
```

* 字符串的切片 与原字符串指向同一底层数组，一样不能修改，字符串的切片操作返回的仍是string，而非slice
* 字符串可以转换为字节数组，也可以转换为字数组

```go
b := []byte(s)
c := []rune(s)
```
* 字符串的运算

```go
c := a+b
len(c)
```
## 指针 数组
* go不支持指针的运算
* 数组的初始化

```go
a := [3]int{1,2,3}
a := [...]int{1,2,3}
a := [3]int{1:1, 2:3} // 没有初始化的位置为零值
a := [...]int{1:1, 2:3} // 数组长度由提供的最大索引决定
```
* 数组创建完长度就固定了，不可再追加元素

## 切片
* 切片的创建

```go
// 1. 通过数组创建
array := [...]int{1,2,3,4,5,6,7,8,9}
slice := array[1:3] // 同Python，Java 左闭右开
// 2. 通过make创建
a := make(int[], 10) // len==cap==10
b := make(int[], 10, 20) // len=10, cap=20
// 直接声明切片的类型变量是没有意义的
var a []int
fmt.Printf("%v\n", a) // []
```
* 切片支持的操作：len() cap() append() copy()

```go
// 将c添加到b中 （返回新的slice，不改变b）
b = append(b, c...)
// 将d拷贝到c中 (不改变c)
copy(c, d) // copy 只会复制c d中较短的
```
## map
* map的创建

```go
// 1. 使用自变量创建
ma := map[string]int{"a":1, "b":2}
// 2. 使用make创建
mb := make(map[string]int)
// 删除
delete(ma, "b")
```
* go内置的map不是线程安全的，标准包sync中的map是
* 修改map中元素的值，最好整体替换

```go
//如, 不要使用：
ma[1].age = 19
// 改为
a := ma[1]
a.age = 19
ma[1] = a
```
## struct
初始化

```go
// 1. 按照类型声明顺序，逐个赋值
// 不推荐, 一旦struct增加字段，整个初始化语句会报错
a := Person{"Tom", 21}
// 推荐的初始化方式
// 2. 使用field名字初始化，没有指定字段的为零值
p := &Person{
    Name: "tata", 
    Age: 21,
}
```
## if
go没有a?b:c

```go
if [;] {
}
else if {
}else{
}
```
## switch
fallthough强制执行下一个case子句（不判断下一个case条件）
当所有case不符合时执行default， 且default语句可以放到任意位置。
## 标签和跳转

```go
L1:
for i:=0; ; i++{
    for :j=0; ; j++{
        if i>=5{
            break L1
        }
        if j>10{
            break
        }
    }
}
```

```go
L1:
for i:=0; ; i++{
    for j:=; ; j++{
        if i>=5{
            continue L1
        }
        if j>10{
            continue
        }
    }
}
```

# 第二章 函数
## 不定参数
不定参数必须是函数的最后一个参数
不定参数名在函数体内相当于切片
切片可以作为参数传给不定参数， func(slice...)
```go
func sum(a, arr ...int) (sum int){
    sum = a
    for _, v := range arr{
        sum += v
    }
    return 
}
func main(){
    slice := []int{1,2,3,4}
    array := [...]int{1,2,3,4}
    // 数组不可作为参数给不定参数
    sum(0, slice...)
}
```
## 函数签名
fmt.Printf 的%T 格式化参数打印函数类型
2个函数相同的条件是，拥有相同的形参列表和返回值列表，形参名可以不同

```go
fmt.Printf("%T\n", add) // func(int, int) int
```

```go
package main
import "fmt"
func add(a, b int) int {
    return a + b
}
func sub(a, b int) int {
    return a - b
}
type Op func(int, int)int   //定义一个函数类型,输入的是两个int类型
                            //返回值是一个int类型
func do (f Op, a, b int) int { //定义一个函数,第一个参数是函数类型Op
    return f(a,b)//函数类型变量可以直接用来进行函数调用
}
func main() {
    a := d(ad,1,2)//函数名a可以当作相同函数类型形参,不需要强制类型转换
    fmt.Println(a)//3
    s := do(sub, 1, 2)
    fmt.Println(s)//-1
}
```

```go
f := sum //函数可以直接赋值给变量
```
匿名函数作为返回值

```go
func wrap(op string) fun(int, int) int{
    switch op{
        case "add":
            return func(a, b int) int{
                return a+b
            }
        case "sub":
            return func(a, b int) int{
                return a-b
            }
        default:
            return nil
    }
}
```
## defer
defer注册多个延迟调用，先进先出
主动调用os.Exit(int) 退出进程时，defer将不再执行（即使defer已经提前注册）
一般defer语句放错误检查语句之后，不然有可能产生panic，如：

```go
dst, err := os.Create(dst)
if err != nil {
    return
}
defer dst.close()
```

## 闭包
闭包最初的目的是减少全局变量,在函数调用的过程中隐式地传递共享变量,有其有用的面;但是这种隐秘的共享变量的方式带来的坏处是不够直接,不够清晰,除非是非常有价值的地方,一般不建议使用闭包。
对象是附有行为的数据,而闭包是附有数据的行为,类在定义时已经显式地集中定义了行为,但是闭包中的数据没有显式地集中声明的地方,这种数据和行为耦合的模型不是一种推着的编程模型,**闭包仅仅是锦上添花的东西,不是不可缺少的**。
## panic recover
panic主动抛出错误，recover捕获panic抛出的错误
recover只有在defer后面的函数体内被直接调用才能捕获panic异常（？）

# 第四章 接口
接口类型查询语法：

```go
switch v:= i.(type) {
    case type1:
        xxxxx
    case type2:
        xxxxx
    default:
        xxxxx
}
```

