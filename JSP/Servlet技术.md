# Servlet技术



## 1.创建Servlet继承Httpservlet

- 重写service方法



- 在web.xml中配置

  ```xml
   <servlet>
          <servlet-name>welcomeServlet</servlet-name>
          <servlet-class>moon.welcomeServlet </servlet-class>
      </servlet>
  
      <servlet-mapping>
          <servlet-name>welcomeServlet</servlet-name>
          <url-pattern>/wc</url-pattern>
        /* 
          注意以/开头
          */  
          
      </servlet-mapping>
  
  ```

- 令服务器启动为特定页面

  ```xml
   <welcome-file-list>
          <welcome-file>/JSP/login.jsp</welcome-file>
      </welcome-file-list>
  ```

- 总结

  1. 编写一个类取继承Httpservlet类
  2. 根据业务重写doGet或doPost方法
  3. 到web.xml中配置Servlet程序的访问地址



## 2.ServletConfig类

> 从类名上可知为Servlet程序的配置信息
>
> servlet程序在第一次访问时创建，servletConfig在servlet创建的同时也创建



1. 作用

   1. 获取servlet的别名servlet-name
   2. 获取初始化参数init-param
   3. 获取ServletContext对象

2. 注意点

   - servlet中重写init时一定要调用父类的的init操作

     

     ```JSp
       @Override
         public void init(ServletConfig config) throws ServletException{
             
     		super.init(config);
         }
     
     ```



## 3. ServletContext类

1. 是什么

   1. 一个接口，表示servleshangxiawe
   2. 一个web工程，只有一个ServletContext对象实例
   3. 是一个域对象
   4. 在web工程启动时自己创建

   - 域对象

     > 像Map一样存取数据的对象叫域对象
     >
     > 存数据的操作范围：整个web工程
     >
     >    
     >
     > 操作
     >
     > |        |     存数据     |     取数据     |       删除        |
     > | :----: | :------------: | :------------: | :---------------: |
     > | 域对象 | setAttribute() | getAttribute() | removeAttribute() |

2. 作用

   1. 获取web.xml配置中上下文参数context-parm
   2. 获取当前的工程路径，格式：/工程路径
   3. 获取工程在服务器的绝对路径
   4. 存取数据 
   

