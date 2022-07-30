# Listener监听器

## 1. 是什么

1. 三大组件之一
2. javaEE的规范，也就是接口
3. 用来监听事物的变化。
4. 事件源：
   1. servletContext ：获得服务器的全局信息
   2. HttpSession
   3. ServletRequest

## 2. 实现

- 监听器有三类
  1. 监听生命周期：
     - servletRequestListener
     - HttpSessionListener
     - ServletContextListener
  2. 监听值的变化：
     - ServletREquestAttributeListener
     - HttpSeeionAttributeListener
     - ServletContextAttributeListener
  3. 针对session中的对象：
     - 监听session中的Java 对象（bean）
