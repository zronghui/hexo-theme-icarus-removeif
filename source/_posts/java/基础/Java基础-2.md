---
title: Java基础-2
date: 2020-01-13 17:53:13
categories:
- java
- 基础
toc: true
tags:
- Java
- 多线程
---

[toc]

<!--more-->
## [还在用Synchronized？Atomic你了解不？](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247484416&idx=1&sn=540c0714263f8ee8b80ba90535162657&chksm=ebd74501dca0cc179e66c34cf3fa647f18860c670b47b0612fac0cb2c26b6cb17ad6824f0808&token=620000779&lang=zh_CN&scene=21###wechat_redirect)
### 一、基础铺垫
count++并不是原子操作。因为count++需要经过读取-修改-写入三个步骤
#### 1.2 CAS
compare and swap
旧值 期待的旧值 新值
前二者相同，执行赋值
否则，自旋（重试）或什么也不做
### 二、原子变量类简单介绍

<details>
  <summary>可以对其进行分类：</summary>
* 基本类型：

AtomicBoolean：布尔型

AtomicInteger：整型

AtomicLong：长整型

* 数组：

AtomicIntegerArray：数组里的整型

AtomicLongArray：数组里的长整型

AtomicReferenceArray：数组里的引用类型

* 引用类型：

AtomicReference：引用类型

AtomicStampedReference：带有版本号的引用类型

AtomicMarkableReference：带有标记位的引用类型

* 对象的属性：

AtomicIntegerFieldUpdater：对象的属性是整型

AtomicLongFieldUpdater：对象的属性是长整型

AtomicReferenceFieldUpdater：对象的属性是引用类型

JDK8新增DoubleAccumulator、LongAccumulator、DoubleAdder、LongAdder

是对AtomicLong等类的改进。比如LongAccumulator与LongAdder在高并发环境下比AtomicLong更高效。

</details>
#### 2.1 原子变量类使用

```Java
class Count{
    // 共享变量(使用AtomicInteger来替代Synchronized锁)
    private AtomicInteger count = new AtomicInteger(0);
    public Integer getCount() {
        return count.get();
    }
    public void increase() {
        count.incrementAndGet();
    }
}
```
#### 2.2 ABA问题
就是可能出现某线程以为旧值没变，其实变了的情况
a -> b -> a
2个a并不相同
<details>
  <summary>说明图</summary>
![WuOFTQBhkmiC7Mw](https://i.loli.net/2020/01/13/WuOFTQBhkmiC7Mw.jpg)

</details>
#### 2.3 解决ABA问题
使用JDK给我们提供的AtomicStampedReference和AtomicMarkableReference类
原理大概就是：维护了一个Pair对象，Pair对象存储我们的对象引用和一个stamp值。每次CAS比较的是两个Pair对象
#### 2.4 LongAdder性能比AtomicLong要好
<details>
  <summary>原因：</summary>
使用AtomicLong时，在高并发下大量线程会同时去竞争更新同一个原子变量，但是由于同时只有一个线程的CAS会成功，所以其他线程会不断尝试自旋尝试CAS操作，这会浪费不少的CPU资源。

而LongAdder可以概括成这样：内部核心数据value分离成一个数组(Cell)，每个线程访问时,通过哈希等算法映射到其中一个数字进行计数，而最终的计数结果，则为这个数组的求和累加。

简单来说就是将一个值分散成多个值，在并发的时候就可以分散压力，性能有所提高。
</details>

## [手把手带你体验Stream流](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247485861&idx=3&sn=9f4a83c8c75b43ead7b4ec187fe031ca&chksm=ebd748a4dca0c1b2d8a1b16656ada30c92bcb6e099c54e6b5b6eebbb29dc8914e1244e77c1e4&token=2052427710&lang=zh_CN#rd)
### 优点
代码简洁，支持并行计算
例如
```Java
int sum2 = IntStream.of(nums).sum();
int sum2 = IntStream.of(nums).parallel().sum();
```
### 使用Stream流分为三步：
创建Stream流
通过Stream流对象执行中间操作
执行最终操作，得到结果
#### 2.1 创建流
最常用的就是从集合中创建出流


```Java
/**
 * 返回的都是流对象
 * @param args
 */
public static void main(String[] args) {
    List<String> list = new ArrayList<>();
    // 从集合创建
    Stream<String> stream = list.stream();
    Stream<String> stream1 = list.parallelStream();
    // 从数组创建
    IntStream stream2 = Arrays.stream(new int[]{2, 3, 5});
    // 创建数字流
    IntStream intStream = IntStream.of(1, 2, 3);
    // 使用random创建
    IntStream limit = new Random().ints().limit(10);
}
```

#### 2.2 执行中间操作
常见的
limit - skip
![SyRq2dmsvr3FlgK](https://i.loli.net/2020/01/13/SyRq2dmsvr3FlgK.jpg)
其他的：
allMatch、anyMatch、noneMatch 返回boolean值
findFirst 和 findAny
#### 2.3 执行最终操作
常见的
![B5hOxvHn6mZASuk](https://i.loli.net/2020/01/13/B5hOxvHn6mZASuk.jpg)
```Java
String str = "my name is 007";
Stream.of(str.split(" ")).peek(System.out::println).forEach(System.out::println)
```
#### reduce的使用
##### sum

```Java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream().reduce(0, (a, b) -> a + b);
```

##### 最大 最小

```Java
// 最大值
Optional<Integer> max = numbers.stream().reduce(Integer::max);
// 最小值
Optional<Integer> max = numbers.stream().reduce(Integer::min);
```
