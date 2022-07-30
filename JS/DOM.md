# DOM





## 1. DOM树

- 文档：一个页面就是一个文档，DOM中使用document表示
- 元素：页面中的所有标签都是元素，DOM中使用element表示
- 节点：网页中的素有内容都是节点（标签、属性、文本、注释），DOM中使用node表示

##  2.获取元素

- script写到标签下面，先有标签，后才能操作
- 

### 1. 根据id获取元素

```js
document.getElementById()
参数用标签的id
返回的是一个元素对象
```



### 2. 根据标签名获取

```js
document.getElementsByTagName()
参数：name
返回：以伪数组形式存储元素对象的集合
是动态的
```

- 可以通过某个元素获取所指定的子元素

```js
element.getElementsByTagName('标签名');

element必须是一个元素对象
```



- H5新增

```js
根据类名：document.getElementsByClassName('类名')
	var boxs = document.getElementsByClassName('box');

根据选择器返回第一个元素：document.querySelector('选择器')
	 var firstBox = document.querySelector('.box');
	 var nav = document.querySelector('#nav');
	 var li = document.querySelector('li');
根据选择器返回所有元素：document.querySelectorAll('选择器')
	 var allBox = document.querySelectorAll('.box');

```

### 3.获取特殊标签

1. body

   ```js
   document.body
   ```

2. html

   ```js
   docunmet.documentElement
   ```

## 3. 事件

### 1.事件

1. 事件源：被触发的对象
2. 事件类型：如何触发--（鼠标、键盘）

3. 事件处理程序：由函数处理

### 2.步骤

1. 获取事件源
2. 绑定事件
3. 添加事件处理程序

## 3.操作元素

1. 改变元素内容

   ```js
   element.innerText
   
   它会去除html标签，空格和换行
   ```

   ```js
   element.innerHTML （W3C标准）
   
   识别html标签
   ```

    两者的区别：

   - 获取内容时的区别：

   ​	innerText会去除空格和换行，而innerHTML会保留空格和换行	

   - 设置内容时的区别：

   ​	innerText不会识别html，而innerHTML会识别

   

2. 改变元素的属性
	
	```js
	element.属性=值
	```

## 4.样式操作

> 修改元素的大小、颜色、位置



#### 方式1：通过操作style属性

> 元素对象的style属性也是一个对象！
>
> 元素对象.style.样式属性 = 值;

```js
    div.onclick = function() {
            // div.style里面的属性 采取驼峰命名法 
            this.style.backgroundColor = 'purple';
            this.style.width = '250px';
        }
```

#### 方式2：通过操作className属性

> 元素对象.className = 值;
>
> 因为class是关键字，所有使用className。
>
> 常用于修改非常多的样式：一个个样式的修改十分不方便、直接替换类名



1.  获取属性值

   - ```js
     element.属性 //获取属性值 内置属性
     ```

   - ```js
     element.getAttribute('属性')； 程序员定义的属性
     ```

     ```js
       console.log(div.getAttribute('id'));
     ```

     

2. 设置属性值

   ```js
    div.setAttribute('index', 2);
           div.setAttribute('class', 'footer'); // class 特殊  这里面写的就是
   ```

3. 移出属性

 ```js
  div.removeAttribute('index');
 ```

> 自定义属性最简单的应用就是“index”，如在页面内部的切换中可以设置自定义属性编号，然后使其显示

## 5.节点操作

1. 父级节点

   ```js  
   node.parentNode 
   ```

   返回最近的一个节点

2. 子节点

   ```js
   parentNode.childNodes 返回所有的节点、包括文本、空格（标准、但是不建议）
   parentNode.children 
   ```

3. 兄弟节点

   ```js
    // 1.nextSibling 下一个兄弟节点 包含元素节点或者 文本节点等等
           console.log(div.nextSibling);
           console.log(div.previousSibling);
           // 2. nextElementSibling 得到下一个兄弟元素节点
           console.log(div.nextElementSibling);
           console.log(div.previousElementSibling);
   ```

4. 创建节点

   ```js
   document.createElement('tagName')
   创建HTML元素
   ```

   

5. 添加节点

   ```js
   添加到末尾：node.appendChild(child)
   添加到前面：node.insertBefore(child，指定元素)
   ```

