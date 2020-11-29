---
title: spring
toc: true
recommend: 1
uniqueId: '2020-09-22 07:35:37/"spring".html'
date: 2020-09-22 15:35:37
thumbnail:
categories:
- java
tags:
keywords:
---

[TOC]

<!--more-->

## 

## Spring 面试题

[Spring 经典面试题汇总](https://mp.weixin.qq.com/s/IdjCxumDleLqdU8MgQnrLQ)

### Spring 的 2 种 IOC 容器：BeanFactory 和 ApplicationContext 的区别

ApplicationContext 继承自 BeanFactory, ApplicationContext 包含 BeanFactory 的所有特性，通常推荐使用前者。

只有对内存占用要求高的程序使用 BeanFactory，以利用其懒加载的特性

### Spring 常见的依赖注入方法及区别

spring 中，常使用构造函数和 setter 注入。其中 setter 方法注入因优雅被广泛使用

构造函数：

```xml
<bean id="person" class="cn.xh.dao.Person">
 <constructor-arg name="pid" value="1"></constructor-arg>
 <constructor-arg name="pname" value="张三"></constructor-arg>
 <constructor-arg name="age" value="18"></constructor-arg>
</bean>
```

setter 注入：

```xml
<bean id="person" class="cn.xh.dao.Person">
 <property name="pid" value="1"></property>
 <property name="pname" value="张三"></property>
 <property name="age" value="18"></property>
</bean>
```

**构造函数注入和setter注入的区别**
1.部分依赖：假设一个类中有3个属性，有3个arg构造函数和setter方法。在这种情况下，如果您只想传递一个属性的信息，则只能通过setter方法

2.覆盖：Setter注入会覆盖构造函数注入。如果我们同时使用构造函数和setter注入，IOC容器将使用setter注入。

3.变化：我们可以通过二次注射轻松更改值。它不会像构造函数一样创建新的bean实例。因此，setter注入比构造函数注入更灵活。

### IOC 的实现机制

工厂模式+反射机制

示例：

```java
interface Fruit {
     public abstract void eat();
}
class Apple implements Fruit {
    public void eat(){
        System.out.println("Apple");
    }
}
class Orange implements Fruit {
    public void eat(){
        System.out.println("Orange");
    }
}

class Factory{
  public static Fruit getInstance(String lassName){
    Fruit f = null;
    try{
      f = (Fruit)Class.forName(className).newInstance();
    } catch (Exception e) {
      e.printStackTrace();
    }
    return f;
  }
}
class Cliend{
  public static void main(String[] a){
    Fruit f = Factory.getInstance("io.github.zronghui.spring.Apple");
    if(f!=null){
      f.eat();
    }
  }
}
```

### Spring 内部 bean

内部bean 作为外部 bean 的属性而存在

```xml

<bean id="StudentBean" class="com.edureka.Student">
    <property name="person">
        <!--This is inner bean -->
        <bean class="com.edureka.Person">
            <property name="name" value="Scott"></property>
            <property name="address" value="Bangalore"></property>
        </bean>
    </property>
</bean>
```

### 自动装配的方式

no

byName

byType

构造函数

autodetect

### 

### Aspect Advice JoinPoint PointCut 的含义

Aspect：类，用@Aspect 注解将类声明为 Aspect

Advice: 针对 joinPoint 执行的操作，是类中的方法（可以将 Advice 视为 Spring 拦截器或过滤器）。可以用 @Before @After @Around 等注解标注 Advice 的类型

JoinPoint: 在 JoinPoints 上应用 Advice。一个 joinPoint 往往是一个方法

PointCut: JoinPoint中的正则表达式

## 注解

[Spring通过注解@Autowired/@Resource获取bean实例时为什么可以直接获取接口而不是注入的类 - 覆手为云p - 博客园](https://www.cnblogs.com/aland-1415/p/11991170.html)

[使用MyBatis时为什么Dao层不需要@Repository - 街上的动物园](http://heeexy.com/2018/04/03/MyBatis-ClasspathMapperScanner/)



### 1.创建对象

类似于用 XML 配置的代码：

```xml
<bean id="" class=""></bean>
```

用 Component、 Controller、 Service、 Repository 时，有 value 属性，可以指定 bean 的 id，如果不写 value，默认当前 bean 的 id 为当前类的类名，首字母小写

Bean 与这 4 个实现的功能一样，只不过 Bean 只能修饰方法(方法返回一个对象)

bean 中的 setter 方法可以添加 Required 注解



### 2.注入数据



给 1中创建的对象注入数据（设置属性）

类似于用 XML 配置的代码：

```xml
<property name="" ref="">
<property name="" value="">
```



使用这些注解后，就不用再写这些变量的 set 方法了



相关注解有 Autowired、 Qualifier、 Resource、 Value

Autowired 与 Resource 、Inject 差不多，多用 Autowired

Autowired 不能指定 bean 的 id，若要指定，需用 Qualifier(value="id")

Resource(value="id") 有 value 属性，可以指定 bean 的 id

~~如果不指定 id 的话，id 为 要注入的对象变量名称，如果在 spring 容器中查找不到，就会报错~~

应该是寻找与 要注入的变量 同类型的 bean，如果找不到报错，找到多个也报错[Wiring in Spring: @Autowired, @Resource and @Inject | Baeldung](https://www.baeldung.com/spring-annotations-resource-inject-autowire)

Value 注入基本数据类型和 String 类型

@value几种数值填充方式

[【Spring注解驱动开发】如何使用@Value注解为bean的属性赋值，我们一起吊打面试官！ - 冰河团队 - 博客园](https://www.cnblogs.com/binghe001/p/13216798.html)







<img src="https://i.loli.net/2020/09/22/dyV8m6kAUcWuK1g.png" alt="image-20200922153845078" style="zoom:50%;" />



## spring mvc

### spring mvc 第一天

#### 函数的注解

@RequestMapping(path="/hello")

#### 加在方法参数上的注解

不指定 RequestParam 的话，自动获取相同名字的参数，甚至是 Pojo 对象

@RequestParam(value="username", required=false) // /hello?username=zhsj

@RequestBody(value="username", required=false) // 获取请求体的内容

@PathVariable(value="id") //  @RequestMapping(path="/hello/{id}") -> /hello/2

@CookieValue(value="JSESSION") // 获取指定 cookie 的值

@RequestHeader(value="Accept") // 获取 header 字段的值



@ModelAttribute 当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。

修饰方法和参数时含义不同，这块没看懂

#### 类的注解

@SessionAttributes(value={"username", "password", "age"}, types={String.class, Integer.class})

value: []String sessionMap 的 key

types: key 和 value 的类型



在类上加上 SessionArributes 注解后，方法可以向 session 中存入值，获取值，删除 session

```java
@RequestMapping(path="/save") 
public String save(Model model) {
  System.out.println("向session域中保存数据");
  model.addAttribute("username", "root");
  model.addAttribute("password", "123");
  model.addAttribute("age", 20);
	return "success";
}
@RequestMapping(path="/find") 
public String find(ModelMap modelMap) {
  String username = (String) modelMap.get("username");
  String password = (String) modelMap.get("password");
  Integer age = (Integer) modelMap.get("age");
  System.out.println(username + " : "+password +" : "+age);
  return "success";
}
@RequestMapping(path="/delete")
public String delete(SessionStatus status) { 
  status.setComplete(); 
  return "success";
}
```



### spring mvc 第二天

#### 返回值分类

**String:** 

被解析为相应的 jsp

或者 return "forward:url" 转发 或者 return "redirect:url" 重定向

**void:** 

方法内，用 request.getRequestDispatcher("url").forward(request, response) 转发

或者 response.sendRedirect("newUrl")

**ModelAndView**

优点：可以在 jsp 中访问变量

```java
@RequestMapping("/testReturnModelAndView")
public ModelAndView testReturnModelAndView() {
  ModelAndView mv = new ModelAndView();
  mv.addObject("username", "张三");
  mv.setViewName("success");
  return mv;
}
```



**响应 json：**

public @ResponseBody Account xxx(..){

​    return account;

}

#### 异常

系统的 dao、service、controller 出现都通过 throws Exception 向上抛出，最后由 springmvc 前端

控制器交由异常处理器进行异常处理



#### 拦截器

拦截器与过滤器的区别：



拦截器链

preHandle

postHandle

afterCompletion



##### 拦截器的执行顺序

<img src="https://i.loli.net/2020/09/23/3EJ2jTBFmtyUvWb.png" alt="图片来自 03.spring5mvc第二天【大纲笔记】第 20 页" style="zoom: 67%;" />





<img src="https://i.loli.net/2020/09/23/mHoB87RzpDjM5i1.png" alt="image-20200923132717312" style="zoom:40%;" />







### java对象 POJO和JavaBean的区别 

[java对象 POJO和JavaBean的区别 - 简书](https://www.jianshu.com/p/224489dfdec8)



总结：POJO  plain ordinary java object : private 的属性，有 setter and getter 没有其他方法

bean: private 的属性，用 setter and getter 和其他方法，必须实现 Serializable 接口



### 匿名内部类

[程序员你真的理解匿名内部类吗？](https://juejin.im/post/6844903991558537230)

抽象类无法实例化

new f(); // invoke method

new c(){ @Override f(){} }; // 继承class: c, 重写方法 f ，并 new 一个对象

这就是匿名内部类做的事情
