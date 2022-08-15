## 1. IOC

### 1. 什么是IOC

1. 控制翻转，把对象创建和对象之间的调用过程，交给Spring进行管理
2. 目的：降低耦合度

### 2. 底层原理

> XML 解析、工厂模式、反射

1. IOC 思想基于 IOC 容器完成，IOC 容器底层就是对象工厂

2. Spring 提供 IOC 容器的两种实现方式：（两个接口）

   1. BeanFactory：IOC 容器基本实现，是 Spring内部的使用接口，不提供开发人员进行使用（不常用）

      > 加载配置文件时不会创建对象，在获取（使用对象）才去创建对象

   2. ApplicationCnotext：BeanFactory接口的子接口，一般由开发人员及进行使用

      > 加载配置文件时候会把配置文件对象进行创建

### 3. IOC 操作 Bean 管理概念

> 基于 XML 配置文件方式实现
>
> 基于注解方式实现

#### 1.注入基本数据类型 

1. 用 set 方法进行注入（需要有 set 方法）

```xml
在XML中编写，默认是无参构造函数生成对象，因此类中需要有对应的无参构造方法
<bean id = "user" class="com.atguigu.spring5.User"> 
	<property name="bname" value="易筋经"></property>
    <property name="bauthor" value="达摩老祖"></property>
</bean>
// id 是实例名，
	class是类名的全路径
	
property中
	name 是属性名
	value 是属性值

```

2. 用有参构造注入属性（**constructor-arg**）

```xml
<bean id = "user" class="com.atguigu.spring5.User"> 
	<constructor-arg name="oname" value="电脑"></constructor-arg>
    <constructor-arg name="address" value="China"></constructor-arg>
</bean>
```

3. p 名称空间注入（了解）

   > 同样是基于 set 方法进行注入

```xml
1. 在 xml 配置文件中添加 p空间的引入
xmlns:p="http://www.springframework.org/schema/p"

2. 用 set 模式进行属性注入
<bean id="book" class="com.atguigu.spring5.Book" p:bname="九阳神功" p:bauthor="无名氏"></bean>
```

4. 附加

> 注入 null
>
> 注入 需要转义的特殊符号

1. null

```xml
<property name="address">
	<null>
</property>
```

2. 特殊符号

```xml
比如 <>
1. 用转义符
2. 用 CDATA 写法
格式 <value><![CDATA[ "特殊符号内容" ] ]> </value>

<property name="address">
	<value><![CDATA[<<南京>>]]> </value>
</property?>
```



#### 2. 外部注入Bean

> 把其他的类作为属性注入到当前 Bean 中

```xml
1. 需要在外部声明对应的对象
 <bean id="userService" class="com.atguigu.service.Userservice">

                <property name="userDao" ref="useDaoImpl"></property>
 </bean>

 <bean id="useDaoImpl" class="com.atguigu.dao.UserDaoImpl"></bean>

2.用属性 ref 表明通过外部导入 Bean，导入的对象是外部声明的 id 名字

注意 property 中的 name 是该类中的自定义的属性名
```

#### 3. 内部注入 Bean 

> 通过设置属性的时候，在里面创建一个 Bean

```xml
<property name="dept">
	<bean id="dept" class="com.atguigu.bean.Dept">
    	<property name="dname" valude="保安"> </property>	
    </bean>
</property>
```

3. 1级联赋值

```xml
1.在外部把 bean 的属性配置好，然后导入 
	<bean> <property name="dept" ref="dept"></property> </bean>	

<bean id="dept" class="……">
	<property name="dname" value="……"> </property>
</bean>

2. 需要对应的对象先生成一个 get 方法
<bean>
	<property name="dept.dname" value="……">
    	
    </property>
</bean>

```

#### 4. 注入数组、map、集合类型

``` xml
1. 创建相应的相应数据结构、并生成相应的 set 方法

2. 进行相应的配置
<bean>
	<property>
    	//数组
        <array>
        	<value>……</value>
            <value>……</value>
        </array>        
    </property>
    
    <property>           
        //链表
        <list>
        	<value>……</value>
            <value>……</value>
            <value>……</value>
            <value>……</value>
        </list>
    </property>
    
    //map类型属性注入
    <property>
    	<map>
        	<entry key="" valude=""></entry>	
        	<entry key="" valude=""></entry>	
            <entry key="" valude=""></entry>	
        </map>
    </property>

    <property>
   		<Set>
        	<value>……</value>
            <value>……</value>
            <value>……</value>
            <value>……</value>
        </Set>
    </property>	
</bean>
```

- 在集合内设置对象类型值

```xml
1. 先创建多个对象
<bean id="1">……</bean>
<bean id="2">……</bean>

2. 用外部注入 bean 的方法
<property>
	<list>
    	<ref bean="1"></ref>
        <ref bean="2"></ref>
    </list>
</property>
```

#### 5. 使用 util 标签简化注入

```xml
1. 先设置 util 所携带的值
<util:list id="booklist">
	<value>……</value>
    <value>……</value>
    <value>……</value>
    <value>……</value>
</util:list>

2. 通过 ref 引用 util 进行注入
<bean id="book" class="">
	<property name="list" ref="bookList">
    	
    </property>
</bean>
```

#### 6. FactoryBean

> Spring 中有两种 Bean ：
>
> 1. 普通 bean--->在配置文件中定义 bean 类型就是返回类型
>
> 2. 工厂bean ---->在配置文件中定义 bean 类型可以和返回类型不一样
>
>    因为工厂 bean 会 继承 FactoryBean 接口，并实现他的 getObject 函数，该函数的返回值才是 bean 的返回类型

#### 7. Spring 中 bean 的作用域

> bean 分为单实例（singleton）和多实例（prototype）
>
> 默认单实例

```xml
在设置 bean 标签中有一个属性 scope ，用来设置多实例
<bean scope="prototype"></bean>

区别：
1. 单实例： Spring 会在加载配置文件时候创建单实例对象
2. 多实例：	Spring 会在调用 get 方法时候创建对象、
```

#### 8. Bean 的生命周期

> 从对象创建到对象销毁的过程

生命周期分别是：

1. 通过构造器创建 bean 实例（无参数构造，在getBean 的时候就会执行）
2. 设置 bean 的属性值和对其他 bean 的引用（调用 set 方法）
3. 把 bean 实例 传递 bean 后置处理器的方法 postProcessBeforeInitialization（通过配置文件配置一个实现 BeanPostProcessor 接口的类的bean对象，就表明配置了后置处理器）
4. 调用 bean 的初始化方法（配置文件中 bean 属性 init-method=”initMethod“）
5. 把 bean 实例传递 bean 后置处理器的方法 postProcessAfterInitialization
6. bean 的使用 （对象获取）
7. 使用容器关闭的函数 close，调用 bean 的销毁方法（配置文件中 bean 属性destoryMethod）

#### 9. 自动装配

> 根据指定装配规则（属性名称或者属性类型），Spring 自动将匹配的属性值进行注入

```xml
通过 bean 标签属性 autowire 进行配置自动装配
	1. byName 根据属性名注入（会查找对应的 bean 的 id 和该类属性名称一样的配置）
	2. byType 根据属性类型注入（但是只能一个属性类型相同，多个会报错）
```

#### 10. 外部属性文件

> 用配置文件配置 bean 对象的值

```xml
1. 设置引入的文件
xmlns:context=http://www.springframework.org/schema/context

http://www.springframework.org/schema/context/spring-context.xsd
2. 编写或者复制导入外部文件（.properties）

<!--引入外部属性文件--> <context:property-placeholder location="classpath:jdbc.properties"/>
配置连接池
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"> 				<property name="driverClassName" value="${prop.driverClass}"></property> 		
    <property name="url" value="${prop.url}"></property> 
    <property name="username" value="${prop.userName}"></property> 
    <property name="password" value="${prop.password}"></property>
</bean>
```

### 3. 基于 Bean 来进行管理

> 1. 注解是代码特殊标记格式：@注解名称（属性名称=属性值，属性名称=属性值……）
>
> 2. 范围：可以作用在类上面，方法上面，属性上面
> 3. 目的：简化 XML 配置

#### 1. 使用注解

- 分类

  > 以下四个注解功能一致，主要是便于开发者识别

  1. @Component
  2. @Service
  3. @Controller
  4. @Repository

- 步骤
  1. 引入依赖（spring-aop 依赖）
  2. 在 XML 中开启组件扫描（`<context:component-scan base-package="com.gtguigu"></context:component-scan`>）
  3. 创建类，并在类上面添加对象注解（ value 值默认是类的首字母小写）

#### 2. 设置注解过滤器扫描器

```xml
<!--示例1 use-default-filters="false" 表示现在不使用默认filter，自己配置filter context:include-filter ，设置扫描哪些内容 --> 
<context:component-scan base-package="com.atguigu" use-default-filters="false"> 		<context:include-filter type="annotation"expression="org.springframework.stereotype.Controller"/> 
</context:component-scan> 

<!--示例2 下面配置扫描包所有内容 context:exclude-filter： 设置哪些内容不进行扫描 --> <context:component-scan base-package="com.atguigu"> 
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/> 
</context:component-scan>
```

#### 3. 注解注入属性

1. @Autowired：根据属性类型进行自动装配（autowire 的 byType）

2. @Qualifier：根据名称进行注入（使用时必须和@Autowired 一起使用）

   > 使用的情况：初始化的对象是实现某一个接口，会出现多个相同类型，而自动装配是的类型要求只能 有一个，所以需要使用名称注入

3. @Resource：可以根据类型注入，也可以根据名称注入（来源是 javax 的包）

   > 默认是类型注入
   >
   > @Resource (name="userDaoImpl1") 添加 name 属性就是名称注入

4. @value：注入普通类型属性

   > @value(value="abc")  就会给name 进行赋值
   >
   > private String name;

#### 4. 完全注解开发

> 创建配置类，替代 xml 配置文件

```java
@Configuration //告诉 Spring 这个是配置类
@ComponentScan(basePackage={"com.atguigu"}) //设置扫描器的包

```

## 4. AOP

### 1. 概念

> 面向切面编程，对业务逻辑的各个部分进行隔离，从而使得这些业务功能的耦合度降低，提高程序的可重用性。不修改源代码，往主干里面增删新功能

### 2. 使用 JDK 动态代理

> 使用 Proxy 类里面的方法创建代理对象
>
> `staic object newProxyInstance(ClassLodader loader,类<?>[]interfaces,InvocationHandler h)`
>
> 第一个	 ClassLodader loader：类加载器
>
> 第二个 	类<?>[]interfaces：	增强方法所在的类，这个类实现的接口，支持多个接口
>
> 第三个	 InvocationHandler h：实现这个接口 InvocationHandler，创建代理对象，写增强的部分

 
