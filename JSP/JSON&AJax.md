# JSON&AJax

## JSON

### 1. 是什么

- 轻量级的数据交换格式

> 轻量级是和xml做比较
>
> 数据交换是客户端和服务器 的交换格式



### 2.json定义

大括号包围键值对进行，键值对之间进行分隔

### 3. json的访问

类似于访问一个json对象



### 4.json的两个常用方法

> json会有两种形式存在
>
> 一种是：对象
>
> 一种是：字符串
>
> 操作json中的数据时，用json对象
>
> 进行客户和服务器的数据交换时，用json字符串

- JSON.stringfy() 	对象-->字符串
- JSON.parse()        字符串-->对象

### 5. json在java中的使用



1. javaBean和json的互转

   - 1. 首先导入gson包
     2. 创建Gson实例

   - 1. java对象转json字符串

     ```json
     gson.toJson(对象)   返回一个对象
     ```

     	2. json字符串转java对象

     ```json
     gson.fromJson(字符串, 对象类)gson.fromJson(字符串, 对象类)
     ```

2. List 和 json的转换

   - gson.fromJson(personListJsonString, new PersonListType().getType())
   - 需要重写一下类PersonListType()

3. 与map也同理

   - TypeToken<HashMap<对象>(){}.getType()
     - 使用一个匿名内部类



## Ajax

### 1. 原生写法

1. 创建XMLHttpReque对象

```js
var xmlhttprequest = new XMLHttpRequest();
```

2. 调用open方法设置请求参数

```js
xmlhttprequest.open("GET","http://localhost:8080/16_json_ajax_i18n/ajaxServlet?action=javaScriptAjax",true)
```

3. 在send方法前绑定onreadystatechange事件

```js
xmlhttprequest.onreadystatechange = function(){
if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) {
var jsonObj = JSON.parse(xmlhttprequest.responseText);
// 把响应的数据显示在页面上
document.getElementById("div01").innerHTML = "编号：" + jsonObj.id + " , 姓名：" +
jsonObj.name;
}
}
```

4. 调用send方法发送请求

```js
xmlhttprequest.send();
```

### 2. 使用jQuery中的Ajax

- $.ajax方法

  - url 	请求的地址

  - type   请求的类型

  - data   发送给服务器的数据

  - success   成功请求，响应的回调函数

  - dataType  相应的数据类型

    - ​			text

      ​			xml

      ​			json

  ```jso
  ```

  

- \$.get和 \$.post

  - url
  - data
  - callback
  - type  返回数据的类型
  - 注意和，Ajax相比少了type（请求的类型）