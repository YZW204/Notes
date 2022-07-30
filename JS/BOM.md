# BOM

## 1.BOM是什么

BOM（Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window。

​	BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性。

​	BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C，BOM 最初是Netscape 浏览器标准的一部分。

## 2. 顶级对象Window的常用事件

- window.onload = function(){ }

- window.addEventListener('load',function(){});

  > window.onload 是窗口 (页面）加载事件，**当文档内容完全加载完成**会触发该事件(包括图像、脚本文件、CSS 文件等), 就调用的处理函数。
  >
  >  
  >
  > 只能写一个，如果有多个，则前面的会被覆盖

   

- window.addEventListener('DOMContentLoded',function(){});

  > DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等。 
  >
  >  
  >
  > IE9以上支持
  >
  >  
  >
  > 如果页面的图片很多的话, 从用户访问到onload触发可能需要较长的时间, 交互效果就不能实现，必然影响用户的体验，此时用 DOMContentLoaded 事件比较合适。

- 调整窗口大小事件

  > window.addEventListener('resize',function(){}); 
  >
  >  
  >
  >  window.innerWidth  获取当前屏幕的宽度

## 3. 定时器

开始定时器

- setTimeout(函数，[回调毫秒] ) 

  > 1. 回调函数可以是直接写一个‘ 匿名函数 ’  ，也可以是一个函数名称
  >
  > 2. 延迟的毫秒数缺省是 0
  >
  > 3. 可以位这个定时器命名，方便关闭 

  ```js
  <script>
          // 回调函数是一个匿名函数
           setTimeout(function() {
               console.log('时间到了');
  
           }, 2000);
          function callback() {
              console.log('爆炸了');
          }
  		// 回调函数是一个有名函数
          var timer1 = setTimeout(callback, 3000);
          var timer2 = setTimeout(callback, 5000);
      </script>
  ```

  

- setInterval(函数，[间隔的毫秒] )  

  >  相隔多少时间重复调用此函数
  >
  > 相关特性同上
  >
  > 1. 回调函数可以是直接写一个‘ 匿名函数 ’  ，也可以是一个函数名称
  >
  > 2. 延迟的毫秒数缺省是 0
  >
  > 3. 可以位这个定时器命名，方便关闭 

  ```js
    <script>
          // 1. setInterval 
          setInterval(function() {
              console.log('继续输出');
          }, 1000);
      </script>
  ```

  

停止定时器

- clearTimeout(timeoutID)

  > 取消setTimeout的定时器

  ```js
   <button>点击停止定时器</button>
      <script>
          var btn = document.querySelector('button');
  		// 开启定时器
          var timer = setTimeout(function() {
              console.log('爆炸了');
          }, 5000);
  		// 给按钮注册单击事件
          btn.addEventListener('click', function() {
              // 停止定时器
              clearTimeout(timer);
          })
      </script>
  ```

   

