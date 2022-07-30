# EL表达式&&JSTL

> Expression Language
>
> 为了简化jsp输出脚本
>
> 不会输出null

输出是${表达式}

- 主要是输出域中的数据

## 1. 输出顺序

1.  输出域中的数据

- 从小范围到大范围的输出
  - page-->request-->session-->application

2. 输出bean中的数据

​		加.输出

​		如：person.name

##  2. 关系运算

1.  逻辑运算

可以使用 eq,ne lt

​				and or not

2. 算术运算

​	div， mod

3. empty

> 判断是否为空，返回bool类型

```jsp
${empty 变量名}
```

## 3. 隐含对象

> pageScope ====== pageContext 域
> requestScope ====== Request 域
> sessionScope ====== Session 域
> applicationScope ====== ServletContext 域
>
> ServletConetxt有另外一个名称：**application。**(EL表达式就是取这个名字)



1. param Map<String,String> 它可以获取请求参数的值
   paramValues Map<String,String[]> 它也可以获取请求参数的值，获取多个值的时候使用

```jsp
输出请求参数username 的值：${ param.username } <br>
```

2. header Map<String,String> 它可以获取请求头的信息

```jsp
输出请求头【User-Agent】的值：${ header['User-Agent'] } <br>
```

3. cookie Map<String,Cookie> 它可以获取当前请求的Cookie 信息

```jsp 
获取Cookie 的名称：${ cookie.JSESSIONID.name } <br>
获取
```

4. initParam Map<String,String> 它可以获取在web.xml 中配置的<context-param>上下文参数

```jsp
：${ initParam.username } <br>
```

## 缺点

- EL只能读取域对象的数据，不能将数据写入域对象、不能修改数据。





# JSTL

> 可以利用这些标签取代JSP页面上的Java代码，

- c:set 往域中保存数据

```jsp
<c:set scope="session" var="abc" value="abcValue"/>
```



- c:if 做if判读

```jsp
<c:if test="${ 12 == 12 }">
<h1>12 等于12</h1>
</c:if>
```



- c:choose

  - c:when

  做多路判断，switch

- c:forEach

```jsp
<c:forEach begin="2" end="7" step="2" varStatus="status" items="${requestScope.stus}" var="stu">
    
<c:forEach items="${ requestScope.map }" var="entry">
```



- 