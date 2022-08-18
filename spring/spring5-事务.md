## 事务

> 数据库中逻辑上的一组操作，要么都成功，要么就失败
>
> 四个特性：原子性、一致性、隔离性、持久性

## 1. 事务操作

> 事务一般添加到 javaee 三层结构里面的 Service 层（业务逻辑层）
>
> 两种方式实现（编程式事务管理和声明式事务管理）

### 1. 声明式事务管理

> 基于注解方式和 xml 配置文件方式
>
> 底层使用 AOP 原理

### 2. 注解声明事务管理

1. 在 Spring 配置文件中配置事务管理器

   ```xml
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"> <!--注入数据源--> 
       <property name="dataSource" ref="dataSource"></property> </bean>
   ```

2. 引入名称空间 tx

   ```xml
   <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
   http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
   ```

3. 开启事务注解

   ```xml
   <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
   ```

4. 在 Service 类上添加注解

   `Transactional`

## 3. 事务传播行为

### 1. required

> A方法本身有事务，调用方法B后，方法B使用方法A的事务；如果A方法没有事务，调用B方法后，B方法开启事务

### 2.  required

> A方法调用B方法，直接开启新事务，不管A是否有事务

## 4. 事务隔离级别

> 问题：脏读、不可重复读、幻读（虚读）

1. 脏读、

   > 读到未提交的事务，即事务中的任何修改，都能被另一个事务读到

2. 不可重复读

   > 当前事务读取的是其他已经提交的事务**修改**数据

3. 幻读

   > 当前事务读取的是其他已提交事务**添加**的数据

|          | 脏读 | 不可重复读 | 幻读 |
| :------: | :--: | :--------: | :--: |
| 读未提交 |  √   |     √      |  √   |
| 读已提交 |  ×   |     √      |  √   |
| 可重复读 |  ×   |     ×      |  √   |
|  串行化  |  ×   |     ×      |  ×   |

4. timeout：超出时间

   > 在规定时间提交，否则就进行回滚
   >
   > 默认-1，按秒计算

5. readOnly：只读

   > 默认 false
   >
   > 是否只能进行查询操作

6. rollbackFor:回滚

   > 手动设置出现异常回滚

7. noRollbackFor：不回滚

   > 手动设置出现什么异常不回滚

### 使用注解

> @Service
>
> @Transactional()
>
> 添加相应的属性

### 使用配置文件

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"> 
    <!--1 注入数据源--> 
    <property name="dataSource" ref="dataSource"></property> 
</bean>

	<!--2 配置通知--> 
<tx:advice id="txadvice"> 
    <!--配置事务参数--> 
    <tx:attributes> 
        <!--指定哪种规则的方法上面添加事务--> 
        <tx:method name="accountMoney" propagation="REQUIRED"/> 
        <!--<tx:method name="account*"/>--> 
 </tx:attributes> 
</tx:advice> 
<!--3 配置切入点和切面--> 
<aop:config>
        <!--配置切入点--> 
        <aop:pointcut id="pt" expression="execution(* com.atguigu.spring5.service.UserService.*(..))"/> 
        <!--配置切面--> 
        <aop:advisor advice-ref="txadvice" pointcut-ref="pt"/> 
</aop:config>

```



### 完全注解方式

```java
@Configuration //配置类 
@ComponentScan(basePackages = "com.atguigu") //组件扫描 
@EnableTransactionManagement //开启事务 
public class TxConfig { 
    //创建数据库连接池 
    @Bean 
    public DruidDataSource getDruidDataSource() {
        DruidDataSource dataSource = new DruidDataSource(); 				 	    dataSource.setDriverClassName("com.mysql.jdbc.Driver"); 	      dataSource.setUrl("jdbc:mysql:///user_db"); 
        	dataSource.setUsername("root");
        dataSource.setPassword("root"); 
        return dataSource; 
    } 
    //创建JdbcTemplate对象 
    @Bean 
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
        //到ioc容器中根据类型找到dataSource 
        JdbcTemplate jdbcTemplate = new JdbcTemplate(); 
        //注入dataSource
        jdbcTemplate.setDataSource(dataSource);
		return jdbcTemplate;
    }
    //创建事务管理器 
    @Bean 
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource); 
        return transactionManager; 
    }
}
```

