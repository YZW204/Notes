# Filter



## 1.使用步骤

1. 编写类去实线Filter接口

2. 实线过滤的方法doFilter（）

3. web.xml中去配置Filter的拦截路径

4. 配置过程

   ```xml
   <!--filter 标签用于配置一个Filter 过滤器-->
   <filter>
       <!--给filter 起一个别名-->
       <filter-name>AdminFilter</filter-name>
       <!--配置filter 的全类名-->
       <filter-class>com.atguigu.filter.AdminFilter</filter-class>
   </filter>
   
   
   <!--filter-mapping 配置Filter 过滤器的拦截路径-->
   <filter-mapping>
       <!--filter-name 表示当前的拦截路径给哪个filter 使用-->
       <filter-name>AdminFilter</filter-name>
       <!--url-pattern 配置拦截路径
       / 表示请求地址为：http://ip:port/工程路径/ 映射到IDEA 的web 目录
       /admin/* 表示请求地址为：http://ip:port/工程路径/admin/*
       -->
       <url-pattern>/admin/*</surl-pattern>
   </filter-mapping>
   ```

## 2.生命周期

1. 构造器
2. init初始化
   - 在web工程中就执行
3. doFilter
   - 每次拦截就执行
4. destroy
   - 停止时web工程时执行、



## 3. FliterConfig类

1. 获取filter-name的内容
2. 获取在web.xml中的init-param的初始化参数
3. 获取servletContext的对象

```java
filterConfig.getFilterName());
```

 ```xml
 如何配置参数
 <filter>
     <!--给filter 起一个别名-->
     <filter-name>AdminFilter</filter-name>
     <!--配置filter 的全类名-->
 	<filter-class>com.atguigu.filter.AdminFilter</filter-class>
     
     <init-param>
     <param-name>username</param-name>
     <param-value>root</param-value>
     </init-param>
     
     <init-param>
     <param-name>url</param-name>
     <param-value>jdbc:mysql://localhost3306/test</param-value>
     </init-param>
 </filter>
 ```



## 4. FliterChain过滤器链

> 多个过滤器如何一起工作

### 1.FilterChain.doFilter()

1. 执行下一个Filter过滤器
2. 执行目标资源（没有Fliter）

### 2.多个Fliter执行特点：

1. 默认一个线程
2. 共同使用一个request对象

1. 注意

- 多个过滤器执行时候，执行顺序时由==配置顺序决定==！！！

## 5. 匹配路径

### 1. 精确匹配

```xml
<url-pattern>/target.jsp</url-pattern>
```

请求地址必须是以web目录下某个文件

### 2. 目录匹配

```xml
<url-pattern>/admin/*</url-pattern>
```

请求的地址在该目录下即可

### 3. 后缀名匹配

```xml
<url-pattern>*.html</url-pattern>
```

只要后缀名相同则匹配



>只关心请求是否匹配，而不关心资源是否存在



