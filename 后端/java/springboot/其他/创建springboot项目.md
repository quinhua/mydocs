### 1.新建项目

![q3yagh5hu5h4w2q](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/q3yagh5hu5h4w2q.20e9dasukt7k.webp)

![gyh3qahuw4ahu4w](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gyh3qahuw4ahu4w.7ccgpfzfkmo0.webp)

配置完成之后点击 `CREATE` 进行项目的创建

### 2.pom.xml配置

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.qh</groupId>
  <artifactId>springbootTest</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <name>springbootTest</name>
  <url>http://maven.apache.org</url>
    
  <!--
      在Spring Boot提供了一个名为 spring-boot-starter-parent 的工程,
      里面已经对各种常用依赖的版本进行了管理，我们的项目需要以这个项目为父工程,
      这样我们就不用操心依赖的版本问题了，需要什么依赖，直接引入坐标(不需要添加版本)即可
  -->

  <!-- 1.添加父工程坐标 -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.6.RELEASE</version>
    <relativePath/>
  </parent>

  <properties>
    <!-- 2.配置JDK版本-->
    <java.version>1.8</java.version>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>

    <!-- 3.添加web启动器-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
      
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <optional>true</optional>
    </dependency>

  </dependencies>

</project>
```

### 3.创建启动类

```java
# 在 `src/main/java/com/qh`下创建 `Application.java` 的启动类

package com.qh;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application
{
    public static void main( String[] args ) {
        SpringApplication.run(Application.class,args);
    }
}
```

### 4.编写控制层

```java
# 在 `src/main/java/com/qh/controller` 下创建 `HelloController.java` 
    
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        return "hello,springboot";
    }
}
```

### 5.启动测试

![ga34tg35yaergwgdfg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ga34tg35yaergwgdfg.3tdnzu06zig0.webp)

```java
通过输出的日志可以知道以下信息:
1.监听的端口是 8080
2.项目的上下文路径是 ''
    
控制台打印如下:
```

![g4wydfhg4dfh4wgfghw](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/g4wydfhg4dfh4wgfghw.1anm5pnb9o0w.webp)

```java
打开浏览器输入 `http://localhost:8080/hello`可以看到
```

![ga3g35ytgaqgty3qaw](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ga3g35ytgaqgty3qaw.1szteeezpq74.webp)

### 6.问题

#### 1.为什么在添加启动器的时候不需要在启动器的坐标中指定版本？

```xml
因为指定了项目的父工程,在spring-boot-starter-parent中已经通过Maven的版本锁定了Jar包的版本,所以就不需要再指定了。
```

#### 2.为什么就添加一个启动器依赖,项目就可以运行起来了,运行项目所需要的Jar包从何而来？

```xml
因为添加了这个启动器的依赖,它已经把自己运行所需要的必要包集成在这个启动器中,通过Maven的依赖传递性,将这些包都依赖到项目里了。
```

![y3ygq3yghh4h4wqgqa3gyq](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/y3ygq3yghh4h4wqgqa3gyq.28mcam3j0j40.webp)