

# 安装
要安装Maven，可以从Maven官网下载最新的Maven 3.8.x，然后在本地解压，设置几个环境变量：
```
M2_HOME=/path/to/maven-3.8.x
PATH=$PATH:$M2_HOME/bin
```

# 使用
1. idea创建一个Maven项目，会自动创建 pom.xml 文件。
2. Maven使用groupId，artifactId和version唯一定位一个依赖。
3. 在Maven中声明一个依赖项可以自动下载并导入classpath；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>org.example</groupId>
    <artifactId>order</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>22</maven.compiler.source>
        <maven.compiler.target>22</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>


    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>com.zaxxer</groupId>
            <artifactId>HikariCP</artifactId>
            <version>5.1.0</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>2.1.0-alpha1</version>
        </dependency>
    </dependencies>
</project>

```


# 依赖关系
Maven定义了几种依赖关系，分别是compile、test、runtime和provided：

|   scope |	说明	| 示例|
|---------|--------|--------|
| compile	| 编译时需要用到该jar包（默认）|	commons-logging|
|test	|编译Test时需要用到该jar包	|junit|
|runtime	|编译时不需要，但运行时需要用到	|mysql|
|provided	|编译时需要用到，但运行时由JDK或某个服务器提供|	servlet-api|

其中，默认的compile是最常用的，Maven会把这种类型的依赖直接放入classpath。
```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.3.2</version>
    <scope>test</scope>
</dependency>
```


## 搜索第三方组件
最后一个问题：如果我们要引用一个第三方组件，比如okhttp，如何确切地获得它的groupId、artifactId和version？方法是通过search.maven.org搜索关键字，找到对应的组件后，直接复制：