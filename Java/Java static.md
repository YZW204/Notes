# Java static

> 1. 变量(也称为类变量)
> 2. 方法(也称为类方法)
> 3. 代码块
> 4. 嵌套类



## 1. 静态变量

### 和实例变量的区别

### 1.1 静态变量

- 运行时，Java 虚拟机只为静态变量==分配一次内存==，在加载类的过程中完成静态变量的内存分配。
- 在类的内部，可以在任何方法内==直接访问==静态变量。
- 在其他类中，可以通过==类名访问==该类中的静态变量。

- 所有对象的公共属性（如明确的对象的公有属性）
- 优点:
  - 可以节约内存

### 1.2 实例变量

- 每创建一个实例，Java 虚拟机就会为实例变量分配一次内存。
- 在类的内部，可以在非静态方法中直接访问实例变量。
- 在本类的静态方法或其他类中则需要通过类的实例对象进行访问。



## 2. 静态方法

### 2.1 和实例方法的区别



- 静态方法属于类，不属于类的对象
- 可以直接==类.静态方法==直接调用静态方法，而无需创建类的实例
- 静态方法只能访问静态数据成员，并可以改变静态成员的值。
- 注意限制：

  - 静态方法不能直接使用非静态数据成员或非静态方法
  - this和super不能在静态上下文中使用
  



## 3. private static 修饰变量

- private限制了范围范围，只能在该类中被访问，是一个私有的共享的数据