## spring Jdbc

> Spring对 jdbc 进行封装，使用 JDBCTemplate 方便实现对数据库的操作

## 1.准备工作

1. 导入相关的 jar 包

2. 进行 xml 数据库配置

   ```xml
   <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close"> 
       <property name="url" value="jdbc:mysql:///user_db" /> 
       <property name="username" value="root" /> 
       <property name="password" value="root" /> 
       <property name="driverClassName" value="com.mysql.jdbc.Driver" /> 
   </bean>
   ```

3. 配置 JdbcTemplate对象，注入DataSource

   ```xml
   <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"> 
       <!--注入dataSource-->
       <property name="dataSource" ref="dataSource">
       </property>
   </bean>
   ```

4. 开启注解扫描，并创建各个 bean 对象

   ```xml
   <context:component-scan base-package="com.atguigu"></context:component-scan>
   ```

## 2. jdbc操作

### 1. 添加，修改，删除

> jdbcTemplate.update(String sql , Obj  arg );

1. 添加

```java

String sql = "insert into t_book values(?,?,?)";
Object[] args = {book.getUserId(), book.getUsername(), book.getUstatus()};
int update = jdbcTemplate.update(sql,args);
```

2. 修改

```java
String sql = "update t_book set username=?,ustatus=? where user_id=?";
Object[] args = {book.getUsername(), book.getUstatus(),book.getUserId()}; 
int update = jdbcTemplate.update(sql, args);
```

3. 删除

```java
String sql = "delete from t_book where user_id=?"; 
int update = jdbcTemplate.update(sql, id); 
```

### 2.查询操作

>  jdbvTemplate.queryForObject(String sql,Class<T> requiredType)
>
> 第二个参数是指，返回的类型

```java
String sql = "select count(*) from t_book"; 
Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
```

### 3. 查询返回对象（结果集）

>  jdbvTemplate.queryForObject(String sql,RowMapper<T> rowMapper,Object ..args)
>
> 第二个参数也是表示返回的类型
>
> 第三个参数表示查询的条件

```java
String sql = "select * from t_book where user_id=?"; //调用方法
Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
```

注：

对象也可以事集合，所以可以返回一个结果集

### 4. 批量操作

> jdbcTemplate.batchUpdate( String sql , List<Object[]> batchArgs )、

```java
创建一个 List 集合
List<Object[]> batchArgs = new ArrayList<>(); 
Object[] o1 = {"3","java","a"}; 
Object[] o2 = {"4","c++","b"};
Object[] o3 = {"5","MySQL","c"}; 
batchArgs.add(o1); batchArgs.add(o2); batchArgs.add(o3);
```



1. 批量插入

```java
String sql = "insert into t_book values(?,?,?)"; 
int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
```

2. 批量修改

```java
String sql = "update t_book set username=?,ustatus=? where user_id=?"; 
int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs); 
```

3. 批量删除

```java
String sql = "delete from t_book where user_id=?"; 
int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
```



