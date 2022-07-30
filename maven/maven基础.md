## maven

## 1. maven 的配置

### 1.指定本地仓库

在 Maven 的核心配置文件：**conf/settings.xml**中

```xml
<!-- localRepository
| The path to the local repository maven will use to store artifacts.
|
| Default: ${user.home}/.m2/repository
<localRepository>/path/to/local/repo</localRepository>
-->
<localRepository>D:\maven-repository</localRepository>
```

### 2. 配置阿里云提供的镜像仓库

在 Maven 的核心配置文件：**conf/settings.xml**中

```xml
	<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
```



因为 maven 项目中的依赖会有搜索顺序：

maven项目使用的仓库一共有如下几种方式：

1. 中央仓库，这是默认的仓库
2. 镜像仓库，通过 sttings.xml 中的 settings.mirrors.mirror 配置
3. 全局profile仓库，通过 settings.xml 中的 settings.repositories.repository 配置
4. 项目仓库，通过 pom.xml 中的 project.repositories.repository 配置
5. 项目profile仓库，通过 pom.xml 中的 project.profiles.profile.repositories.repository 配置
6. 本地仓库

当每一个都进行配置的时候，搜索顺序是从下到上

### 3. 配置 Maven 工程的基础 JDK 版本

```xml
<profile>
	  <id>jdk-1.8</id>
	  <activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	  </activation>
	  <properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	  </properties>
	</profile>
```

配置环境变量：

MAVEN_HOME

path 配置到 bin 目录

## 2. 一些概念

### 1. 坐标

- groupId：公司或组织域名的倒序，通常也会加上项目名称
  - 例如：com.atguigu.maven
- artifactId：模块的名称，将来作为 Maven 工程的工程名
- version：模块的版本号，根据自己的需要设定
  - 例如：SNAPSHOT 表示快照版本，正在迭代过程中，不稳定的版本
  - 例如：RELEASE 表示正式版本

### 2. 生成Maven工程

运行 **mvn archetype:generate** 命令

> TIP
>
> Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 7:【直接回车，使用默认值】
>
> Define value for property 'groupId': com.atguigu.maven
>
> Define value for property 'artifactId': pro01-maven-java
>
> Define value for property 'version' 1.0-SNAPSHOT: :【直接回车，使用默认值】
>
> Define value for property 'package' com.atguigu.maven: :【直接回车，使用默认值】
>
> Confirm properties configuration: groupId: com.atguigu.maven artifactId: pro01-maven-java version: 1.0-SNAPSHOT package: com.atguigu.maven Y: :【直接回车，表示确认。如果前面有输入错误，想要重新输入，则输入 N 再回车。】

### 3. 生成的pom.xml

```xml
 <!-- 当前Maven工程的坐标 -->
  <groupId>com.atguigu.maven</groupId>
  <artifactId>pro01-maven-java</artifactId>
  <version>1.0-SNAPSHOT</version>
  
  <!-- 当前Maven工程的打包方式，可选值有下面三种： -->
  <!-- jar：表示这个工程是一个Java工程  -->
  <!-- war：表示这个工程是一个Web工程 -->
  <!-- pom：表示这个工程是“管理其他工程”的工程 -->
  <packaging>jar</packaging>

  <name>pro01-maven-java</name>
  <url>http://maven.apache.org</url>

  <properties>
	<!-- 工程构建过程中读取源码时使用的字符集 -->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <!-- 当前工程所依赖的jar包 -->
  <dependencies>
	<!-- 使用dependency配置一个具体的依赖 -->
    <dependency>
	
	  <!-- 在dependency标签内使用具体的坐标依赖我们需要的一个jar包 -->
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
	  
	  <!-- scope标签配置依赖的范围 -->
      <scope>test</scope>
    </dependency>
  </dependencies>
```

## 3. 主要的 maven 操作

### 1. 清理操作

mvn clean

> 删除 target 目录

### 2.编译操作

- 主程序：	mvn compile
- 测试程序：mvn test-compile

主体程序编译结果存放的目录：target/classes

测试程序编译结果存放的目录：target/test-classes

### 3. 测试操作

mvn test

测试的报告存放的目录：target/surefire-reports

### 4. 打包操作

mvn package

变成各种 jar 包 ，war 包，存放的目录：target

### 5. 安装操作

mvn install

把本地生成的 jar 包 存入 maven 本地残酷中，jar 包的路径是根据坐标信息生成的。同时会把pom.xml 转化成 .pom 文件

## 4. 对 web 工程

### 1. 生成工程

```xml
mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4
```

### 2. 生成的 xml 需要手动修改 packaging

```xml
<packaging>war</packaging>
```

## 5. 其他

- web 依赖 java 工程
- 依赖是具有范围的
- 依赖是具有传递性的
- 依赖可以排除（阻断）
- 项目管理底下的模块，聚合

## 6. IDEA

1. 创建 maven 的时候注意设置本地仓库
2. 在 web 的 facts 还是 artc…… 中注意修改web.xml的路径为 `scr/main/webapp`
3. 模块导入的时候，Project Structure -> Module -> import Module ->import module from external model 选择 Maven；记得修改父工程坐标



## 7. 链接：

专门搜索 Maven 依赖信息的网站：https://mvnrepository.com/