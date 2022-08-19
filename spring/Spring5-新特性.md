## Spring5新特性

## 1. 日志

1. 自带日志封装，推荐Log4j2

1. 配置Log4j2.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL --> 
<!--Configuration后面的status用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，可以看到log4j2内部各种详细输出--> 
<configuration status="INFO"> 
    <!--先定义所有的appender--> 
    <appenders> 
        <!--输出日志信息到控制台--> 
        <console name="Console" target="SYSTEM_OUT">
            <!--控制日志输出的格式--> 
    			<PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/> 
        </console>
    </appenders>
    <!--然后定义logger，只有定义logger并引入的appender，appender才会生效--> 
    <!--root：用于指定项目的根日志，如果没有单独指定Logger，则会使用root作为默认的日志输出--> <loggers> 
    <root level="info"> 
        <appender-ref ref="Console"/> 
    </root>
</loggers> 

</configuration>
```

## 2. 注解

1. 支持返回空 @Nullable注解可以在方法，参数，属性上

2. 核心容器支持函数式风格GenericApplicationContext

   ```java
   @Test 
   public void testGenericApplicationContext() { 
       //1 创建GenericApplicationContext对象
       GenericApplicationContext context = new GenericApplicationContext(); 
       //2 调用context的方法对象注册 
       context.refresh(); context.registerBean("user1",User.class,() -> new User()); 
       //3 获取在spring注册的对象 //
       User user = (User)context.getBean("com.atguigu.spring5.test.User");
       User user = (User)context.getBean("user1"); System.out.println(user); 
   }
   ```

3. 整合了JUnit5

   ```java
   @SpringJUnitConfig(locations = "classpath:bean1.xml")
   public class JTest5 { 
       @Autowired
       private UserService userService;
       @Test 
       public void test1() { 
           userService.accountMoney(); 
       } 
   }
   ```

## 4. Webflux

> 用于 web 开发，功能和 SpringMVC 类似，Webflux 使用当前比较流行响应式编程出现的框架

1. Webflux 是一种异步非阻塞的框架，异步非阻塞框架在 Servlet 3.1 以后才支持，核心是基于 Reactor 的相关 API 实现的
2. **异步和同步针对调用者：**调用者不等待回应就做其他的事情就是异步
3. **阻塞和非阻塞针对被调用者：**被调用者受到请求后，马上给出反馈再去做事情就是非阻塞

### 响应式编程

> 面向数据流和变化传播的编程范式，可以方便的表达静态或者动态的数据流

 

