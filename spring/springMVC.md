## springMVC

## 一、创建maven工程

1. 添加web模块（在main目录下添加一个webapp目录）

2. 在maven的pom.xml中设置打包方式（设置为war）

3. 在pom.xml中引入依赖

   ```xml
   <dependencies>
       <!-- SpringMVC -->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.3.1</version>
       </dependency>
   
       <!-- 日志 -->
       <dependency>
           <groupId>ch.qos.logback</groupId>
           <artifactId>logback-classic</artifactId>
           <version>1.2.3</version>
       </dependency>
   
       <!-- ServletAPI -->
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>javax.servlet-api</artifactId>
           <version>3.1.0</version>
           <scope>provided</scope>
       </dependency>
   
       <!-- Spring5和Thymeleaf整合包 -->
       <dependency>
           <groupId>org.thymeleaf</groupId>
           <artifactId>thymeleaf-spring5</artifactId>
           <version>3.0.12.RELEASE</version>
       </dependency>
   </dependencies>
   ```

4. 配置webapp下的web.xml

   > 注册前端控制器DispatcherServlet

   ```xml
   <!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
   <servlet>
       <servlet-name>springMVC</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
       <init-param>
           <!-- contextConfigLocation为固定值 -->
           <param-name>contextConfigLocation</param-name>
           <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的src/main/resources -->
           <param-value>classpath:springMVC.xml</param-value>
       </init-param>
       <!-- 
    		作为框架的核心组件，在启动过程中有大量的初始化操作要做
   		而这些操作放在第一次请求时才执行会严重影响访问速度
   		因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
   	-->
       <load-on-startup>1</load-on-startup>
   </servlet>
   <servlet-mapping>
       <servlet-name>springMVC</servlet-name>
       <!--
           设置springMVC的核心控制器所能处理的请求的请求路径
           /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
           但是/不能匹配.jsp请求路径的请求
       -->
       <url-pattern>/</url-pattern>
   </servlet-mapping>
   
   ```

5. 创建springMVC的配置文件

   > 在resource目录下创建spring的配置文件

   ```xml
   <!-- 自动扫描包 -->
   <context:component-scan base-package="com.atguigu.mvc.controller"/>
   
   <!-- 配置Thymeleaf视图解析器 -->
   <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
       <property name="order" value="1"/>
       <property name="characterEncoding" value="UTF-8"/>
       <property name="templateEngine">
           <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
               <property name="templateResolver">
                   <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
       
                       <!-- 视图前缀 -->
                       <property name="prefix" value="/WEB-INF/templates/"/>
       
                       <!-- 视图后缀 -->
                       <property name="suffix" value=".html"/>
                       <property name="templateMode" value="HTML5"/>
                       <property name="characterEncoding" value="UTF-8" />
                   </bean>
               </property>
           </bean>
       </property>
   </bean>
   
   <!-- 
      处理静态资源，例如html、js、css、jpg
     若只设置该标签，则只能访问静态资源，其他请求则无法访问
     此时必须设置<mvc:annotation-driven/>解决问题
    -->
   ```

6. 在请求控制器中创建处理请求的方法

   ```java
   // @RequestMapping注解：处理请求和控制器方法之间的映射关系
   // @RequestMapping注解的value属性可以通过请求地址匹配请求，/表示的当前工程的上下文路径
   // localhost:8080/springMVC/
   @RequestMapping("/")
   public String index() {
       //设置视图名称
       return "index";
   }
   ```

7. 设置超链接跳转到指定页面

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>首页</title>
   </head>
   <body>
       <h1>首页</h1>
       <a th:href="@{/hello}">HelloWorld</a><br/>
   </body>
   </html>
   ```
> 总结：
> 1. 浏览器发送请求
> 2. 被前端控制器DispatcherServlet处理
> 3. 前端控制器读取SpringMVC核心配置文件，通过扫面组件找到控制器
> 4. 请求地址和@RequestMapping的value进行匹配，返回该方法的字符串
> 5. 该字符串会被视图解析器（thymeleaf）解析成某个页面，转发到对应页面

## 二、 RequestMapping注解

> 用来将请求和处理请求的控制器方法关联起来，建立映射关系

### 1. 注解的功能

@RequestMapping标识一个类：设置映射请求的请求路径的初始信息

@RequestMapping标识一个方法：设置映射请求请求路径的具体信息

```java
@Controller
@RequestMapping("/test")
public class RequestMappingController {

	//此时请求映射所映射的请求的请求路径为：/test/testRequestMapping
    @RequestMapping("/testRequestMapping")
    public String testRequestMapping(){
        return "success";
    }

}
```

### 2. @RequstMapping注解的value的属性

> 可以有多个请求地址所对应的请求

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
)
public String testRequestMapping(){
    return "success";
}
```

### 3. @RequestMapping注解的method属性

> 表示能匹配的请求方式

如果不满足method属性，则会报错**405：Request method 'POST' not supported**

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"},
        method = {RequestMethod.GET, RequestMethod.POST}
)
public String testRequestMapping(){
    return "success";
}
```

> 注：
>
> 1、对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解
>
> 处理get请求的映射-->@GetMapping
>
> 处理post请求的映射-->@PostMapping
>
> 处理put请求的映射-->@PutMapping
>
> 处理delete请求的映射-->@DeleteMapping
>
> 2、常用的请求方式有get，post，put，delete
>
> 但是目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符串（put或delete），则按照默认的请求方式get处理
>
> 若要发送put和delete请求，则需要通过spring提供的过滤器HiddenHttpMethodFilter，在RESTful部分会讲到

### 4. params属性

@RequestMapping注解的params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

"param"：要求请求映射所匹配的请求必须携带param请求参数

"!param"：要求请求映射所匹配的请求必须不能携带param请求参数

"param=value"：要求请求映射所匹配的请求必须携带param请求参数且param=value

"param!=value"：要求请求映射所匹配的请求必须携带param请求参数但是param!=value

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
        ,method = {RequestMethod.GET, RequestMethod.POST}
        ,params = {"username","password!=123456"}
)
public String testRequestMapping(){
    return "success";
}
```

> 注：
>
> 若当前请求满足@RequestMapping注解的value和method属性，但是不满足params属性，此时**页面回报错400**：Parameter conditions "username, password!=123456" not met for actual request parameters: username={admin}, password={123456}

### 5. @RequestMapping注解的headers属性（了解）

@RequestMapping注解的headers属性通过请求的请求头信息匹配请求映射

@RequestMapping注解的headers属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系

"header"：要求请求映射所匹配的请求必须携带header请求头信息

"!header"：要求请求映射所匹配的请求必须不能携带header请求头信息

"header=value"：要求请求映射所匹配的请求必须携带header请求头信息且header=value

"header!=value"：要求请求映射所匹配的请求必须携带header请求头信息且header!=value

若当前请求满足@RequestMapping注解的value和method属性，但是不满足headers属性，此时页面显示404错误，即资源未找到

### 6. SpringMVC支持ant风格的路径

> 在路径中适合

？：表示任意的单个字符

*：表示任意的0个或多个字符

\**：表示任意的一层或多层目录

注意：在使用\**时，只能使用/**/xxx的方式

### 7. SpringMVC支持路径中的占位符

原始方式：/deleteUser?id=1

rest方式：/deleteUser/1

如果将数据通过路径的方式传输到服务器中，，就应该在相应的@RequestMapping注解的value属性中通过占位符{***}表示传输的数据

通过@PathVariable注解，将占位符所表示的数据赋值给控制器方法的形参

```html
<a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest</a><br>
```

```java
@RequestMapping("/testRest/{id}/{username}")
public String testRest(@PathVariable("id") String id, @PathVariable("username") String username){
    System.out.println("id:"+id+",username:"+username);
    return "success";
}
//最终输出的内容为-->id:1,username:admin
```

## 三、SpringMVC获取请求参数

**1. 通过ServletAPI获取**

将HttpServletRequest作为控制器方法的形参，此时HttpServletRequest类型的参数表示封装了当前请求的请求报文的对象

传统的Servlet方式

```java
@RequestMapping("/testParam")
public String testParam(HttpServletRequest request){
    String username = request.getParameter("username");
    String password = request.getParameter("password");
    System.out.println("username:"+username+",password:"+password);
    return "success";
}
```

**2. 通过控制器方法的形参获取请求参数**

控制器的形参名字和请求参数的形参一致的话，会自动赋值给形参

```html
<a th:href="@{/testParam(username='admin',password=123456)}">测试获取请求参数-->/testParam</a><br>
```

```java
@RequestMapping("/testParam")
public String testParam(String username, String password){
    System.out.println("username:"+username+",password:"+password);
    return "success";
}
```

> 1. 如果有多个同名参数，则需要用**字符串数组或字符串**来接收
> 2. 使用字符串接收多个值，值为多个数据用逗号拼接的结果

**3、@RequestParam**

@RequestParam是将请求参数和控制器方法的形参创建映射关系

@RequestParam注解一共有三个属性：

1. value：指定为形参赋值的请求参数的参数名

2. required：设置是否必须传输此请求参数，默认值为true

3. defaultValue：不管required属性值为true或false，当value所指定的请求参数没有传输或传输的值为""时，则使用默认值为形参赋值

**4、@RequestHeader**

@RequestHeader是将请求头信息和控制器方法的形参创建映射关系

@RequestHeader注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

**5、@CookieValue**

@CookieValue是将cookie数据和控制器方法的形参创建映射关系

@CookieValue注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

**6. 通过POJO获取参数**

如果请求的参数和实体类的属性名一致，那么就会直接赋值

```html
<form th:action="@{/testpojo}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    性别：<input type="radio" name="sex" value="男">男<input type="radio" name="sex" value="女">女<br>
    年龄：<input type="text" name="age"><br>
    邮箱：<input type="text" name="email"><br>
    <input type="submit">
</form>
```

```java
@RequestMapping("/testpojo")
public String testPOJO(User user){
    System.out.println(user);
    return "success";
}
//最终结果-->User{id=null, username='张三', password='123', age=23, sex='男', email='123@qq.com'}
```

**7. 解决获取请求参数的乱码问题**

SpringMVC提供了编码过滤器 CharacterEncodingFilter，**在web.xml中注册**

```xml
<!--配置springMVC的编码过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <!--为true才会进行设置-->
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
	<!--过滤的匹配路径-->
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 4. 设置域对象

### 1. 用传统的 servlet 来获取参数

1. 重定向到其他页面并设置**Request域的键值**

   ```java
           @RequestMapping("/textservlet")
           public String textservlet(HttpServletRequest httpServletRequest){
               httpServletRequest.setAttribute("servlet","hello,servlet");
               return "success";
           }
   ```

2. 首页页面（用 themeleaf 的 @{} 来设置跳转的绝对路径）

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.themeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
       <h3>首页</h3>
       <a th:href="@{/textservlet}">通过servletapi实现</a>
   </body>
   </html>
   ```

3. 跳转页面(用themeleaf的 ${} 来获取**Request域对象**中的值)

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.themeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>成功页面</title>
   </head>
   <body>
     <p th:text="${servlet}"></p>
   </body>
   </html>
   ```

### 2. 用ModelAndView向request域对象共享数据

 ```java
 @RequestMapping("/testModelAndView")
 public ModelAndView testModelAndView(){
     /**
      * ModelAndView有Model和View的功能
      * Model主要用于向请求域共享数据
      * View主要用于设置视图，实现页面跳转
      */
     ModelAndView mav = new ModelAndView();
     //向请求域共享数据
     mav.addObject("testScope", "hello,ModelAndView");
     //设置视图，实现页面跳转
     mav.setViewName("success");
     return mav;
 }
 ```

**使用Model向Request域对象共享数据**

```java
@RequestMapping("/testModel")
public String testModel(Model model){
    model.addAttribute("testScope", "hello,Model");
    return "success";
}
```

**使用map向request域对象共享数据**

```java
@RequestMapping("/testMap")
public String testMap(Map<String, Object> map){
    map.put("testScope", "hello,Map");
    return "success";
}
```

**使用ModelMap向request域对象共享数据**

```java
@RequestMapping("/testModelMap")
public String testModelMap(ModelMap modelMap){
    modelMap.addAttribute("testScope", "hello,ModelMap");
    return "success";
}
```

**Model、ModelMap、Map的关系**

> 本质都是 BindingAwareModelMap 类型

```java
public interface Model{}
public class ModelMap extends LinkedHashMap<String, Object> {}
public class ExtendedModelMap extends ModelMap implements Model {}
public class BindingAwareModelMap extends ExtendedModelMap {}
```

### 3. 向session域共享数据

```java
@RequestMapping("/testSession")
public String testSession(HttpSession session){
    session.setAttribute("testSessionScope", "hello,session");
    return "success";
}
```

### 4. 向application域共享数据

```java
@RequestMapping("/testApplication")
public String testApplication(HttpSession session){
	ServletContext application = session.getServletContext();
    application.setAttribute("testApplicationScope", "hello,application");
    return "success";
}
```

## 四、SpringMVC的视图

> 视图的作用是渲染数据，将模型Model中的数据展示给用户
>
> 默认种类有转发和重定向
>
> 使用Thymeleaf视图技术，在SpringMVC配置文件中配置了Thymeleaf解析器，由此解析之后的就是Thymeleaf

**1. ThymeleafView**

> 当控制器方法中的视图名称没有前缀时，会被SprngMVC配置的视图解析器解析，用拼接前缀和后缀得到最终路径，通过转发的方式实现跳转

```java
@RequestMapping("/testHello")
public String testHello(){
    return "hello";
}
```

**2. 转发视图**

> SpringMVC默认的转发视图是InternalResourceView
>
> 当控制器方法中所设置的视图名称以“forward:”为前缀时，创建internalResourceView，此时不会被SpringMVC视图解析器解析，而是把前缀去掉，剩余部分作为最终路径转发跳转(不会改变任务栏地址)

```java
@RequestMapping("/testForward")
public String testForward(){
    return "forward:/testHello";
}
```

**3. 重定向视图**

> SpringMVC默认的重定向是RedirectView
>
> 当控制器方法中所设置的视图名称以“redirect:”为前缀时，创建RedirectView食土兽
>
> 此时不会被SpringMVC视图解析器解析，而是把前缀去掉，剩余部分作为最终路径通过重定向的方式实现跳转

```java
@RequestMapping("/testRedirect")
public String testRedirect(){
    return "redirect:/testHello";
}
```

> 重定向视图解析时，如果有/，会自动拼接上下文

**4. 视图控制器view-contoller**

> 如果只是用来实现页面跳转，就可以只设置视图名称，将处理器方法用view-controller标签进行表示

```xml
<!--
	path：设置处理的请求地址
	view-name：设置请求地址所对应的视图名称
-->
<mvc:view-controller path="/testView" view-name="success"></mvc:view-controller>
	
<!--为避免请求映射全部失效，需要开启mvc注解驱动-->
<mvc:annotation-driven />
```

## 五、RESTful

> rest 风格提倡URL地址使用同一的风格设计。不使用问号键值对方式携带请求参数，而是把发送给服务器的数据作为URL地址的一部分

| 操作     | 传统方式         | REST风格                |
| -------- | ---------------- | ----------------------- |
| 查询操作 | getUserById?id=1 | user/1-->get请求方式    |
| 保存操作 | saveUser         | user-->post请求方式     |
| 删除操作 | deleteUser?id=1  | user/1-->delete请求方式 |
| 更新操作 | updateUser       | user-->put请求方式      |

### 1、HiddenHttpMethodFilter过滤器

> 浏览器只支持发送get和post请求使用HiddenHttpMethodFilter来发送put和delete请求

**将post请求转换为delete或put请求**

1. 当前请求必须为post
2. 当前请求必须传输请求参数——method

满足上述条件，HiddenHttpMethodFilter过滤器会将当前请求方式转换为请求参数_method的值，因此 _method的值才是最终的请求方式

**注册HiddenHttpMethodFilter**（在web.xml)

```xml
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 六、HttpMessageConverter 

> 报文信息转换器，将请求报文转换为java对象，将java对象转换为响应报文
>
> 有两个注解和两个类型：@RequestBody，@ResponseBody，RequestEntity，ResponseEntity

### 1. @RequestBody

> 在控制器方法设置一个形参，使用@RequestBody进行标识，当前请求的请求体就会为该形参赋值



```html
<form th:action="@{/testRequestBody}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit">
</form>
```

```java
@RequestMapping("/testRequestBody")
public String testRequestBody(@RequestBody String requestBody){
    System.out.println("requestBody:"+requestBody);
    return "success";
}
```

输出结果：

requestBody:username=admin&password=123456

### 2. RequestEntity

> 封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求报文就会赋值给该形参，同时有许多的方法可以获取相关的信息



```java
@RequestMapping("/testRequestEntity")
public String testRequestEntity(RequestEntity<String> requestEntity){
    System.out.println("requestHeader:"+requestEntity.getHeaders());
    System.out.println("requestBody:"+requestEntity.getBody());
    return "success";
}
```

输出结果：
requestHeader:[host:"localhost:8080", connection:"keep-alive", content-length:"27", cache-control:"max-age=0", sec-ch-ua:"" Not A;Brand";v="99", "Chromium";v="90", "Google Chrome";v="90"", sec-ch-ua-mobile:"?0", upgrade-insecure-requests:"1", origin:"http://localhost:8080", user-agent:"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36"]
requestBody:username=admin&password=123

### 3. @ResonseBody

> 用来标识一个控制器的方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器



```java
@RequestMapping("/testResponseBody")
@ResponseBody
public String testResponseBody(){
    return "success";
}
```

结果：浏览器页面显示success

### 4. SpringMVC处理JSON

1. 导入jackson的依赖

   ```xml
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.12.1</version>
   </dependency>
   ```

2. 在springMVC的核心配置文件中开启mvc的注解驱动，此时在HandlerAdaptor中会自动装配一个消息转换器：MappingJackson2HttpMessageConverter，可以将响应到浏览器的Java对象转换为Json格式的字符串

   ```xml
   <mvc:annotation-driven />
   ```

3. 用@ResponseBody对方法进行标识

4. 将java对象直接作为控制器方法的返回值返回，就会自动转换为json格式的字符串

   ```java
   @RequestMapping("/testResponseUser")
   @ResponseBody
   public User testResponseUser(){
       return new User(1001,"admin","123456",23,"男");
   }
   ```

   浏览器的页面中展示的结果：

   {"id":1001,"username":"admin","password":"123456","age":23,"sex":"男"}



## 7. 文件

## 1. 文件下载

**使用responEntity来设置响应头**

```java
public ResponseEntity<byte[]> textDown(HttpSession session) throws IOException{
        //获取上下文对象
        ServletContext servletContext =session.getServletContext();
        //获取上下文路径
        String real=servletContext.getRealPath("/static/img/1.jpg");
        //把路径中的文件读入流中
        InputStream is=new FileInputStream(real);
        //把流放进字节数组中
        byte[] bytes=new byte[is.available()];
        is.read(bytes);
         
//        设置一个响应体
        MultiValueMap<String,String> heads=new HttpHeaders();
//       响应头
        heads.add("Content-Disposition","attchment;filename=1.jpg");
//        响应状态码
        HttpStatus httpStatus=HttpStatus.OK;
//        设置返回的响应报文
        ResponseEntity<byte[]> responseEntity=new ResponseEntity<>(bytes,heads,httpStatus);
//        关闭流
        is.close();
        return responseEntity;
    }
```

## 2. 文件上传

```java
   @RequestMapping("/textUp")
    public String textUp(MultipartFile photo ,HttpSession session) throws IOException {
       	//获取名字
        String fileName=photo.getOriginalFilename();
		
        //获取后缀
        String suffixName=fileName.substring(fileName.lastIndexOf("."));
        //设置UUID随机数来命名
        String uuid= UUID.randomUUID().toString();
        fileName=uuid+suffixName;
		
        //获取上下文路径
        ServletContext servletContext=session.getServletContext();

        String photopath=servletContext.getRealPath("photo");
       //创建一个文件目录
        File file=new File(photopath);
        if(!file.exists()){
            file.mkdir();
        }
        //实际文件目录
        String filpath=photopath+File.separator+fileName;
        //上传
        photo.transferTo(new File(filpath));
        return "success";
    }
```

