# JS

1. JS的作用
   - 用于表单动态校验（产生的主要原因）
2. 关系

- 结构（html）、表现(CSS)、行为(JS)
- 浏览器执行过程
  - 渲染引擎：解析HTML和CSS，内核
  - JS引擎：处理JS代码



3. ECMAScript、DOM、BOM
4. 实现，行内、内嵌，外部 。
5. 输出
   - ![JS输出](D:\Markdown\image\JS\JS输出.png)

------



### 变量

```html
typeof 变量
console.log(typeof name)
```



### 转换

1. 隐式转换

   - 用+链接字符串--->字符串
   - 用（-、*、/）--->数字型

2. 函数

   ```
   parseInt() 截取整数位
   parseFloat() 小数 
   Number() 数字型
   ```



---



#### 标识（zhi）符、关键字、保留字



## 运算符



## 函数

```html
function 名字( 参数){


}
```

函数的封装

- 函数的封装是把一个或者多个功能通过==函数的形式封装起来==，对外只提供一个简单的接口

1. 参数的个数
   1. 和声明一样
      - 正常输出
   2. 大于声明
      - 正常
   3. 小于声明
      - NAN
2. 匿名函数

```
var fun=function(){
内容
}

调用：	fun();
```

3. **arguments**

>  存储了传递的所有实参

   1. 伪数组，没有数组的方法：pop，push
   1. 具有length属性
   1. 按照索引进行存储

## 作用域

1. 如果在函数内部没有声明直接赋值，那么默认为全局变量
   - 全局变量只有浏览器关闭才会销毁，比较占内存
2. 作用域链：
   - 内部函数访问外部函数的变量，采取就近原则，按层级查找变量



## 预解析

>JS 运行分两步：预解析和代码执行

1. 预解析：变量预解析和函数预解析

   1. 把所有==变量声明==提升到作用域最前面

   2. 把所有==函数声明==提升到作用域最前面

      >赋值并不提前



## 对象

1. 创建对象

   1. 创建方法

      ```html
      /*第一种方法*/
      var obj={
      	name:' ',
      	age: 18,
      	sex:'男',
      	sayhi:function(){
      	console.log('hi~');
      	}
      }
      ```

      

      ```
      /*第二钟方法*/
      var obj=new Object();
      obj.age=18;
      obj.sayhi=function(){
      
      }
      /*
      用等于号（=）来添加属性和方法
      */
      ```

       

      **构造函数（创建一个类）**

      ```，
      function Star(uname,age,sex){
      	this.name=uname;
      	this.age=age;
      	this.sex=sex;
      }
      var ldh=new Star('刘德华',18,’男);
      
      ```

       >1. 函数名必须第一个字母必须大写Star
       >2. 需要用new声明，且返回的是一个对象
   
2. 调用：

      ```html
      对象.属性
      对象['属性']
      
      ```

3. 遍历

```
var obj={
name:'jia',
age:18,
sex;'boy'
}

for(var k in obj){
	console.log(obj[k]);
}
```



## 内置对象

> JS有3种对象：自定义对象、内置对象、浏览器对象

- 常见内置对象Math、Date、Array、String

> Math.max( [value1[,value2, ...]])
>
> 带有中括号的参数的是可以省略的

- 

  - 数学对象

    - 不是构造函数，不需要new就可以直接使用
    - Math.round中 ==.5==往大了取
    - 随机数：Math.floor(Math.random()*(max-min+1))+min

  - 日期对象

    - 是构造函数，必须使用new来调用

    - ```js
      var date new Date();
      ```

    - 参数按某种格式

    - 日期格式化

    - ```js
      Date.now()
      获得距离1970.1.1的时间，俗称时间戳
      ```

  - 数组对象

    - ```js
      var arr=[1,3];
      ```

    - ```js
      var arr1=new Array(2); 长度为2，空值
      var arr1=new Array(2,3);长度为2，有值
      ```

    - 检测是否为数组

      - ```js
        arr instanceof Array
        返回值是true和false
        ```

      - ```js
        Array.isArray()
        ```

    - 添加和删除元素类似vector

      - 末尾添加：push(); 

        > 返回时数组的长度

      - 前面添加：unshitf();

        > 同样返回数组的长度

      - 删除：pop()

        > 返回删除的元素

      - 删除第一个元素 shitf()

        > 删除第一元素
      
    - 翻转数组
    
      - reverse
    
    - 排序：sort
    
      - ```js
        arr.sort(function(a,b)){
                 return a-b;//升序排序
                 }
        ```
    
    - 返回索引号：indexof() 从前开始查找，lastindexof从后开始查找
    
      > 返回第一个满足，找不到就返回-1
    
    - 数组转换为字符串
    
      ```js
      arr.toString();默认以','分割
      ```
    
      ```js
      arr1.join(',');//分割符号
      ```
    
  - 字符串对象
  
    - 基本包装类型：把简单数据类型包装成了复杂数据类型
      - 有3个：String、Number、Boolean
    - 字符串不可变，通过指针的指向来改变自己的值
    - 不会销毁原本的空间
    - 方法：
      - indexof（查找的字符，查找的位置）
      - charAt（查找索引的字符），charAtcode(返回字符的Ascall)
      - str[]
    - cancat 链接两个字符串 （+也可以）
    - substr（a，b）从第a位置取b个数
    - split 字符串转数组，以（）中的参数进行分割成数组



## 数据类型



### 1. 简单数据类型，复杂数据类型

- 简单数据类型：放到栈里面
- String，number，Boolean，null（obj）
- 复杂数据类型：栈中放地址。到堆中找数据

