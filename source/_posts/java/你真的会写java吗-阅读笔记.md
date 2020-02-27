---
title: 你真的会写java吗 阅读笔记
description: # 你真的会写java吗 阅读笔记
date: 2020-02-16 12:20:53
categories:
- java
toc: true
tags:
---

[toc]

# bean

## domain 包名

一个数据库表 应该对应一个 普通的 entity 对象，因此包名应为 com.xxx.entity

## DTO

只要是用于网络传输的对象，都应该被当做 DTO 对象，若约定某对象是 DTO 对象，就把名称改为 XXDTO, 比如订单下发 OMS: OMSOrderInputDTO.

## DTO转化

DTO 是系统与外界交互的模型对象，那么肯定会有一个步骤是将 DTO 对象转换成 BO 对象（？）或者普通 entity 对象，让 service 层处理。

### 场景

```java
@RequestMapping("/v1/api/user")
@RestController
public class UserApi {

    @Autowired
    private UserService userService;

    @PostMapping
    public User addUser(UserInputDTO userInputDTO){
        User user = new User();
        user.setUsername(userInputDTO.getUsername());
        user.setAge(userInputDTO.getAge());

        return userService.addUser(user);
    }
}
```



### 使用工具

网上有很多工具，支持浅拷贝或深拷贝的Utils. 举个例子，我们可以使用org.springframework.beans.BeanUtils#copyProperties对代码进行重构和优化

BeanUtils.copyProperties是一个浅拷贝方法，复制属性时，我们只需要把DTO对象和要转化的对象两个的属性值设置为一样的名称，并且保证一样的类型就可以了。如果你在做DTO转化的时候一直使用set进行属性赋值，那么请尝试这种方式简化代码，让代码更加清晰!

```java
@PostMapping
public User addUser(UserInputDTO userInputDTO){
    User user = new User();
    BeanUtils.copyProperties(userInputDTO,user);

    return userService.addUser(user);
}
```



### 转化的语义

```java
@PostMapping
 public User addUser(UserInputDTO userInputDTO){
         User user = convertFor(userInputDTO);
         return userService.addUser(user);
 }

 private User convertFor(UserInputDTO userInputDTO){

         User user = new User();
         BeanUtils.copyProperties(userInputDTO,user);
         return user;
 }
```

这是一个更好的语义写法，虽然他麻烦了些，但是可读性大大增加了，在写代码时，我们应该尽量把语义层次差不多的放到一个方法中



### 抽象接口定义

当实际工作中，完成了几个api的DTO转化时，我们会发现，这样的操作有很多很多，那么应该定义好一个接口，让所有这样的操作都有规则的进行。
如果接口被定义以后，那么convertFor这个方法的语义将产生变化，他将是一个实现类。

看一下抽象后的接口:

```java
public interface DTOConvert<S,T> {
    T convert(S s);
}
```

虽然这个接口很简单，但是这里告诉我们一个事情，要去使用泛型，如果你是一个优秀的java程序员，请为你想做的抽象接口，做好泛型吧。

我们再来看接口实现:

```java
public class UserInputDTOConvert implements DTOConvert {
  @Override
  public User convert(UserInputDTO userInputDTO) {
    User user = new User();
    BeanUtils.copyProperties(userInputDTO,user);
    return user;
  }
}
```



我们这样重构后，我们发现现在的代码是如此的简洁，并且那么的规范:

```java
RequestMapping("/v1/api/user")
@RestController
public class UserApi {

    @Autowired
    private UserService userService;

    @PostMapping
    public User addUser(UserInputDTO userInputDTO){
        User user = new UserInputDTOConvert().convert(userInputDTO);

        return userService.addUser(user);
    }
}
```

### review code

```java
public class UserInputDTO {
    private String username;
    private int age;

    // getters and setters

    public User convertToUser(){
        UserInputDTOConvert userInputDTOConvert = new UserInputDTOConvert();
        User convert = userInputDTOConvert.convert(this);
        return convert;
    }

    private static class UserInputDTOConvert implements DTOConvert<UserInputDTO,User> {
        @Override
        public User convert(UserInputDTO userInputDTO) {
            User user = new User();
            BeanUtils.copyProperties(userInputDTO,user);
            return user;
        }
    }

}
```

```java
// 然后api中的转化则由:
// User user = new UserInputDTOConvert().convert(userInputDTO);
// User saveUserResult = userService.addUser(user);

// 变成了:
User user = userInputDTO.convertToUser();
User saveUserResult = userService.addUser(user);
```

DTO对象中添加了转化的行为，我相信这样的操作可以让代码的可读性变得更强，并且是符合语义的。

### 利用工具类优化

GUAVA的源码，发现了com.google.common.base.Convert这样的定义:

```java
public abstract class Converter<A, B> implements Function<A, B> {
    protected abstract B doForward(A a);
    protected abstract A doBackward(B b);
    //其他略
}
```

从源码可以了解到，GUAVA中的Convert可以完成正向转化和逆向转化，继续修改我们DTO中转化的这段代码:

```java
private static class UserInputDTOConvert implements DTOConvert<UserInputDTO,User> {
        @Override
        public User convert(UserInputDTO userInputDTO) {
                User user = new User();
                BeanUtils.copyProperties(userInputDTO,user);
                return user;
        }
}
```

修改后:

```java
private static class UserInputDTOConvert extends Converter<UserInputDTO, User> {
         @Override
         protected User doForward(UserInputDTO userInputDTO) {
                 User user = new User();
                 BeanUtils.copyProperties(userInputDTO,user);
                 return user;
         }

         @Override
         protected UserInputDTO doBackward(User user) {
                 UserInputDTO userInputDTO = new UserInputDTO();
                 BeanUtils.copyProperties(user,userInputDTO);
                 return userInputDTO;
         }
 }
```

看了这部分代码以后，你可能会问，那逆向转化会有什么用呢？其实我们有很多小的业务需求中，入参和出参是一样的，那么我们变可以轻松的进行转化，我将上边所提到的UserInputDTO和UserOutputDTO都转成UserDTO展示给大家:

DTO：

```java
public class UserDTO {
    private String username;
    private int age;

    // getters and setters

    public User convertToUser(){
            UserDTOConvert userDTOConvert = new UserDTOConvert();
            User convert = userDTOConvert.convert(this);
            return convert;
    }

    public UserDTO convertFor(User user){
            UserDTOConvert userDTOConvert = new UserDTOConvert();
            UserDTO convert = userDTOConvert.reverse().convert(user);
            return convert;
    }

    private static class UserDTOConvert extends Converter<UserDTO, User> {
            @Override
            protected User doForward(UserDTO userDTO) {
                    User user = new User();
                    BeanUtils.copyProperties(userDTO,user);
                    return user;
            }

            @Override
            protected UserDTO doBackward(User user) {
                    UserDTO userDTO = new UserDTO();
                    BeanUtils.copyProperties(user,userDTO);
                    return userDTO;
            }
    }

}
```

api:

```java
@PostMapping
 public UserDTO addUser(UserDTO userDTO){
         User user =  userDTO.convertToUser();
         User saveResultUser = userService.addUser(user);
         UserDTO result = userDTO.convertFor(saveResultUser);
         return result;
 }
```

当然，上述只是表明了转化方向的正向或逆向，很多业务需求的出参和入参的DTO对象是不同的，那么你需要更明显的告诉程序：逆向是无法调用的:

```java
private static class UserDTOConvert extends Converter<UserDTO, User> {
         @Override
         protected User doForward(UserDTO userDTO) {
                 User user = new User();
                 BeanUtils.copyProperties(userDTO,user);
                 return user;
         }

         @Override
         protected UserDTO doBackward(User user) {
                 throw new AssertionError("不支持逆向转化方法!");
         }
 }
```

看一下doBackward方法，直接抛出了一个断言异常，而不是业务异常，这段代码告诉代码的调用者，这个方法不是准你调用的，如果你调用，我就”断言”你调用错误了。

关于异常处理的更详细介绍，可以参考我之前的文章:[如何优雅的设计java异常](http://lrwinx.github.io/2016/04/28/如何优雅的设计java异常/) ，应该可以帮你更好的理解异常。

## bean的验证

应该保证任何数据的入参到方法体内都是合法的



## [4.5. 拥抱lombok](http://lrwinx.github.io/2017/03/04/细思极恐-你真的会写java吗/#拥抱lombok)

### [4.5.1. 去掉Setter和Getter](http://lrwinx.github.io/2017/03/04/细思极恐-你真的会写java吗/#去掉Setter和Getter)

```Java
@Setter
@Getter
// @Data,@AllArgsConstructor,@NoArgsConstructor 。。。
```

### [4.5.2. bean中的链式风格](http://lrwinx.github.io/2017/03/04/细思极恐-你真的会写java吗/#bean中的链式风格)

什么是链式风格？我来举个例子，看下面这个Student的bean:

```Java
public class Student {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public Student setName(String name) {
        this.name = name;
        return this;
    }

    public int getAge() {
        return age;
    }

    public Student setAge(int age) {
        return this;
    }
}
```

仔细看一下set方法，这样的设置便是chain的style，调用的时候，可以这样使用:

```Java
Student student = new Student()
        .setAge(24)
        .setName("zs");
```

相信合理使用这样的链式代码，会更多的程序带来很好的可读性，那看一下如果使用lombok进行改善呢，请使用 @Accessors(chain = true),看如下代码:

```Java
@Accessors(chain = true)
@Setter
@Getter
public class Student {
    private String name;
    private int age;
}
```

这样就完成了一个对于bean来讲很友好的链式操作。

### [4.5.3. 静态构造方法](http://lrwinx.github.io/2017/03/04/细思极恐-你真的会写java吗/#静态构造方法)

静态构造方法的语义和简化程度真的高于直接去new一个对象。比如new一个List对象，过去的使用是这样的:

```Java
List<String> list = new ArrayList<>();
```

看一下guava中的创建方式:

```Java
List<String> list = Lists.newArrayList();
```

Lists命名是一种约定(俗话说：约定优于配置)，它是指Lists是List这个类的一个工具类，那么使用List的工具类去产生List，这样的语义是不是要比直接new一个子类来的更直接一些呢，答案是肯定的，再比如如果有一个工具类叫做Maps，那你是否想到了创建Map的方法呢：

```Java
HashMap<String, String> objectObjectHashMap = Maps.newHashMap();
```

好了，如果你理解了我说的语义，那么，你已经向成为java程序员更近了一步了。

再回过头来看刚刚的Student，很多时候，我们去写Student这个bean的时候，他会有一些必输字段，比如Student中的name字段，一般处理的方式是将name字段包装成一个构造方法，只有传入name这样的构造方法，才能创建一个Student对象。

接上上边的静态构造方法和必传参数的构造方法，使用lombok将更改成如下写法（@RequiredArgsConstructor 和 @NonNull）:

```Java
@Accessors(chain = true)
@Setter
@Getter
@RequiredArgsConstructor(staticName = "ofName")
public class Student {
    @NonNull private String name;
    private int age;
}
```

测试代码:

```Java
Student student = Student.ofName("zs");
```

这样构建出的bean语义是否要比直接new一个含参的构造方法(包含 name的构造方法)要好很多。

当然，看过很多源码以后，我想相信将静态构造方法ofName换成of会先的更加简洁:

```Java
@Accessors(chain = true)
@Setter
@Getter
@RequiredArgsConstructor(staticName = "of")
public class Student {
        @NonNull private String name;
        private int age;
}
```

测试代码:

```Java
Student student = Student.of("zs");
```

当然他仍然是支持链式调用的:

```Java
Student student = Student.of("zs").setAge(24);
```

这样来写代码，真的很简洁，并且可读性很强。

### [4.5.4. 使用builder  （？）](http://lrwinx.github.io/2017/03/04/细思极恐-你真的会写java吗/#使用builder)

Builder模式我不想再多解释了，读者可以看一下《Head First》(设计模式) 的建造者模式。

今天其实要说的是一种变种的builder模式，那就是构建bean的builder模式，其实主要的思想是带着大家一起看一下lombok给我们带来了什么。

看一下Student这个类的原始builder状态:

```Java
public class Student {
    private String name;
    private int age;

    public String getName() {
            return name;
    }

    public void setName(String name) {
            this.name = name;
    }

    public int getAge() {
            return age;
    }

    public void setAge(int age) {
            this.age = age;
    }

    public static Builder builder(){
            return new Builder();
    }
    public static class Builder{
            private String name;
            private int age;
            public Builder name(String name){
                    this.name = name;
                    return this;
            }

            public Builder age(int age){
                    this.age = age;
                    return this;
            }

            public Student build(){
                    Student student = new Student();
                    student.setAge(age);
                    student.setName(name);
                    return student;
            }
    }

}
```

调用方式:

```Java
Student student = Student.builder().name("zs").age(24).build();
```

这样的builder代码，让我是在恶心难受，于是我打算用lombok重构这段代码:

```Java
@Builder
public class Student {
    private String name;
    private int age;
}
```

调用方式:

```Java
Student student = Student.builder().name("zs").age(24).build();
```

### [4.5.5. 代理模式 （？）](http://lrwinx.github.io/2017/03/04/细思极恐-你真的会写java吗/#代理模式) 

正如我们所知的，在程序中调用rest接口是一个常见的行为动作，如果你和我一样使用过spring 的RestTemplate,我相信你会我和一样，对他抛出的非http状态码异常深恶痛绝。

所以我们考虑将RestTemplate最为底层包装器进行包装器模式的设计:

```Java
public abstract class FilterRestTemplate implements RestOperations {
        protected volatile RestTemplate restTemplate;

        protected FilterRestTemplate(RestTemplate restTemplate){
                this.restTemplate = restTemplate;
        }

        //实现RestOperations所有的接口
}
```

然后再由扩展类对FilterRestTemplate进行包装扩展:

```Java
public class ExtractRestTemplate extends FilterRestTemplate {
    private RestTemplate restTemplate;
    public ExtractRestTemplate(RestTemplate restTemplate) {
            super(restTemplate);
            this.restTemplate = restTemplate;
    }

    public <T> RestResponseDTO<T> postForEntityWithNoException(String url, Object request, Class<T> responseType, Object... uriVariables)
                    throws RestClientException {
            RestResponseDTO<T> restResponseDTO = new RestResponseDTO<T>();
            ResponseEntity<T> tResponseEntity;
            try {
                    tResponseEntity = restTemplate.postForEntity(url, request, responseType, uriVariables);
                    restResponseDTO.setData(tResponseEntity.getBody());
                    restResponseDTO.setMessage(tResponseEntity.getStatusCode().name());
                    restResponseDTO.setStatusCode(tResponseEntity.getStatusCodeValue());
            }catch (Exception e){
                    restResponseDTO.setStatusCode(RestResponseDTO.UNKNOWN_ERROR);
                    restResponseDTO.setMessage(e.getMessage());
                    restResponseDTO.setData(null);
            }
            return restResponseDTO;
    }
}
```

包装器ExtractRestTemplate很完美的更改了异常抛出的行为，让程序更具有容错性。在这里我们不考虑ExtractRestTemplate完成的功能，让我们把焦点放在FilterRestTemplate上，“实现RestOperations所有的接口”,这个操作绝对不是一时半会可以写完的，当时在重构之前我几乎写了半个小时,如下:

```Java
public abstract class FilterRestTemplate implements RestOperations {

    protected volatile RestTemplate restTemplate;

    protected FilterRestTemplate(RestTemplate restTemplate) {
            this.restTemplate = restTemplate;
    }

    @Override
    public <T> T getForObject(String url, Class<T> responseType, Object... uriVariables) throws RestClientException {
            return restTemplate.getForObject(url,responseType,uriVariables);
    }

    @Override
    public <T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables) throws RestClientException {
            return restTemplate.getForObject(url,responseType,uriVariables);
    }

    @Override
    public <T> T getForObject(URI url, Class<T> responseType) throws RestClientException {
            return restTemplate.getForObject(url,responseType);
    }

    @Override
    public <T> ResponseEntity<T> getForEntity(String url, Class<T> responseType, Object... uriVariables) throws RestClientException {
            return restTemplate.getForEntity(url,responseType,uriVariables);
    }
    //其他实现代码略。。。
}
```

我相信你看了以上代码，你会和我一样觉得恶心反胃，后来我用lombok提供的代理注解优化了我的代码(@Delegate):

```Java
@AllArgsConstructor
public abstract class FilterRestTemplate implements RestOperations {
    @Delegate
    protected volatile RestTemplate restTemplate;
}
```

这几行代码完全替代上述那些冗长的代码。
是不是很简洁，做一个拥抱lombok的程序员吧。



