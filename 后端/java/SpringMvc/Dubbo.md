# Apache Dubbo

## 一、分布式RPC框架Apache Dubbo

### 1、软件架构的演进过程

软件架构的发展经历了由单体架构、垂直架构、SOA架构到微服务架构的演进过程，下面我们分别了解一下这几个架构。

#### 1.1 集群和分布式

![hw4eew4huyhuyws](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hw4eew4huyhuyws.2kl0tw6h63q0.webp)

集群: 很多"人"一起，干一样的事

分布式: 很多"人"一起，干不一样的事。这些不一样的事，合起来是一件大事

#### 1.2 单体架构

![gy3qwa5u4h32w6](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gy3qwa5u4h32w6.2sgaxzpxskg0.webp)

##### 1.2.1 架构说明

一个归档包（例如war格式或者Jar格式）包含了应用所有功能的应用程序，我们通常称之为单体应用。单体架构中，全部功能集中在一个项目内（All in one），这是一种比较传统的架构风格。

##### 1.2.2 优点

架构简单，前期开发成本低、开发周期短，适合小型项目（OA、CRM、ERP 企业内部应用）。

##### 1.2.3 缺点

① 复杂性高

整个项目包含的模块非常多，模块的边界模糊，依赖关系不清晰，代码质量参差不齐，整个项目非常复杂。每次修改代码都心惊胆战，甚至添加一个简单的功能，或者修改一个BUG都会造成隐含的缺陷。

② 技术债务逐渐上升

随着时间推移、需求变更和人员更迭，会逐渐形成应用程序的技术债务，并且越积越多。已使用的系统设计或代码难以修改，因为应用程序的其他模块可能会以意料之外的方式使用它。

③ 部署速度逐渐变慢

随着代码的增加，构建和部署的时间也会增加。而在单体应用中，每次功能的变更或缺陷的修复都会导致我们需要重新部署整个应用。全量部署的方式耗时长、影响范围大、风险高，这使得单体应用项目上线部署的频率较低，从而又导致两次发布之间会有大量功能变更和缺陷修复，出错概率较高。

④ 扩展能力受限，无法按需伸缩

单体应用只能作为一个整体进行扩展，无法结合业务模块的特点进行伸缩。

⑤ 阻碍技术创新

单体应用往往使用统一的技术平台或方案解决所有问题，团队的每个成员都必须使用相同的开发语言和架构，想要引入新的框架或技术平台非常困难。

#### 1.3 垂直架构

<img src="https://typora2020.oss-cn-beijing.aliyuncs.com/img_002.png" style="zoom: 67%;" />

##### 1.3.1 架构说明

按照业务进行切割，形成小的单体项目。

垂直MVC项目主要有表现层，业务逻辑层，数据访问层组成的MVC架构，整个项目打包放在一个tomcat里面。适合于 访问量小，用户数不多的业务。

##### 1.3.2 优点

技术栈可扩展（不同的系统可以用不同的编程语言编写）。

##### 1.3.3 缺点

① 垂直架构中每一个系统还是单体架构

不利于开发、扩展、维护，项目的部署效率很低，代码全量编译和部署一次发布需要很长时间，更重要的是 如果某个功能出错有问题，所有的功能都需要再重新打包编译，部署效率极低。

② 项目之间功能冗余、数据冗余。

因为垂直架构中多个项目是独立部署的，相互无法访问，导致多个项目中可能要编写重复的功能代码，加载重复的数据。

③ 团队协作难度高，如多人使用 **git** 很可能在同一个功能上，多人同时进行了修改，作为一个大而全的项目，可能个人只是需要开发其中一个小的模块的需求，却需要导入整个项目全量的代码。

#### 1.4 SOA架构

![hag3e4yyhuwqa3455gy73q](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hag3e4yyhuwqa3455gy73q.3f2ztqzeduc0.webp)

##### 1.4.1 架构说明

**SOA** 全称为 **Service-Oriented Architecture**，即面向服务的架构。它可以根据需求通过网络对松散耦合的粗粒度应用组件(服务)进行分布式部署、组合和使用。一个服务通常以独立的形式存在于操作系统进程中。

站在功能的角度，把业务逻辑抽象成可复用的服务，通过服务的编排实现业务的快速再生，目的：把原先固有的业务功能转变为通用的业务服务，实现业务逻辑的快速复用。

##### 1.4.2 优点

① 可重用性高

重复功能或模块抽取为服务，提高开发效率。

② 维护成本低

修改一个功能，可以直接修改一个项目，单独部署。

③ 可扩展性强

不同的系统可以用不同的语言、不同的技术进行编写。并且可以按照需要，对不同的系统进行集群扩充

##### 1.4.3 缺点

① 抽取服务的粒度大

各系统之间业务不同，很难确认功能或模块是重复的

#### 1.5 微服务架构

![ygh3qaw5yhgw534a](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ygh3qaw5yhgw534a.3c54tkpm4p8.webp)

##### 1.5.1 架构说明

其实和 SOA 架构类似,微服务是在 SOA 上做的升华，微服务架构强调的一个重点是“业务需要彻底的组件化和服务化”，原有的单个业务系统会拆分为多个可以独立开发、设计、运行的小应用。这些小应用之间通过服务完成交互和集成。

##### 1.5.2 优点

① 服务拆分粒度更细，遵循单一职责原则，有利于提高开发效率。

② 可采用Http协议进行服务间的调用

③ 可以针对不同服务制定对应的优化方案，适用于互联网时代，产品迭代周期更短。

##### 1.5.3 缺点

① 对开发团队技术要求高

### 2、Apache Dubbo概述

#### 2.1 Dubbo简介

Apache Dubbo是一款高性能的Java RPC框架。其前身是阿里巴巴公司开源的一个高性能、轻量级的开源Java RPC框架，可以和Spring框架无缝集成。Dubbo官网地址：http://dubbo.apache.org ，Dubbo提供了三大核心能力:面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

#### 2.2 RPC概念

RPC全称为remote procedure call，即**远程过程调用**。比如两台服务器A和B，A服务器上部署一个应用，B服务器上部署一个应用，A服务器上的应用想调用B服务器上的应用提供的方法，由于两个应用不在一个内存空间，不能直接调用，所以需要通过网络来表达调用的语义和传达调用的数据。需要注意的是RPC并不是一个具体的技术，而是指整个网络远程调用过程。

RPC是一个泛化的概念，严格来说一切远程过程调用手段都属于RPC范畴。各种开发语言都有自己的RPC框架。Java中的RPC框架比较多，广泛使用的有RMI、Hessian、Dubbo等。

#### 2.3 Dubbo架构

Dubbo架构图（Dubbo官方提供）如下：

![tgq3awy35q2w6ty](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/tgq3awy35q2w6ty.2vhi0rdym6e0.webp)

节点角色说明：

![y53w4y7u24w3y7ghu2q4](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/y53w4y7u24w3y7ghu2q4.29mkqu8nvj6s.webp)

虚线都是异步访问，实线都是同步访问
蓝色虚线:在启动时完成的功能
红色虚线(实线)都是程序运行过程中执行的功能

调用关系说明:

1. 服务容器负责启动，加载，运行服务提供者。

2. 服务提供者在启动时，向注册中心注册自己提供的服务。

3. 服务消费者在启动时，向注册中心订阅自己所需的服务。

4. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。

5. 服务消费者，从提供者地址列表中，基于负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

6. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

### 3、服务注册中心Zookeeper

通过前面的Dubbo架构图可以看到，Registry（服务注册中心）在其中起着至关重要的作用。Dubbo官方推荐使用Zookeeper作为服务注册中心。

#### 3.1 Zookeeper简介

Zookeeper 是 Apache Hadoop 的子项目，是一个树型的目录服务，支持变更推送，适合作为 Dubbo 服务的注册中心，工业强度较高，可用于生产环境，并推荐使用 。为了便于理解Zookeeper的树型目录服务，我们先来看一下我们电脑的文件系统(也是一个树型目录结构)：

![ygh35qw2qawhygh3w](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ygh35qw2qawhygh3w.7guc777bt8g0.webp)

我的电脑可以分为多个盘符（例如C、D、E等），每个盘符下可以创建多个目录，每个目录下面可以创建文件，也可以创建子目录，最终构成了一个树型结构。通过这种树型结构的目录，我们可以将文件分门别类的进行存放，方便我们后期查找。而且磁盘上的每个文件都有一个唯一的访问路径。

Zookeeper树型目录服务：

![hbyuwe4ahbuy4waeqhuyehu](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hbyuwe4ahbuy4waeqhuyehu.16fs82x7vem8.webp)

流程说明：

· 服务提供者(Provider)启动时: 向 **/dubbo/com.foo.BarService/providers** 目录下写入自己的 URL 地址

· 服务消费者(Consumer)启动时: 订阅 **/dubbo/com.foo.BarService/providers** 目录下的提供者 URL 地址。并向 **/dubbo/com.foo.BarService/consumers** 目录下写入自己的 URL 地址

· 监控中心(Monitor)启动时: 订阅 **/dubbo/com.foo.BarService** 目录下的所有提供者和消费者 URL 地址

#### 3.2 安装Zookeeper

下载地址：http://archive.apache.org/dist/zookeeper/ 

![gafwe3rsgfawe3fwesfgr](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gafwe3rsgfawe3fwesfgr.1pu39l5brom8.webp)

将压缩包解压到Windows/Linux电脑的没有中文的路径下即可

#### 3.3 配置Zookeeper

1. 将`conf`目录中的`zoo_sample.cfg`文件复制一份到`conf`目录并命名为`zoo.cfg`
2. 修改`zoo.cfg`文件: 大概在第12行，设置`dataDir=../data`
3. 在zookeeper的安装目录中，创建`data`目录

#### 3.4 linux系统运行zookeeper

1. 启动Zookeeper: 进入`bin`目录，执行指令`./zkServer.sh start`
2. 查看Zookeeper状态: 进入`bin`目录，执行指令`./zkServer.sh status`
3. 停止Zookeeper: 进入`bin`目录，执行指令`./zkServer.sh stop`

#### 3.5 注意事项

zookeeper运行需要使用java的环境变量(JAVA_HOME).

#### 3.6 windows系统运行zookeeper

1. 启动Zookeeper: 进入`bin`目录，双击`zkServer.cmd`
2. 停止Zookeeper: 关闭Zookeeper的DOS窗口

## 二、Dubbo快速入门

### 1、创建空工程dubbo-project

### 2、开发服务提供者dubbo_provider

#### 2.1 创建提供者工程引入依赖

创建服务提供者工程，命名为`dubbo_provider`，骨架=maven-archetype-webapp

```xml
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring.version>5.2.8.RELEASE</spring.version>
    <juint.version>4.13.2</juint.version>

    <slf4j-version>1.7.25</slf4j-version>
    <logback-version>1.2.3</logback-version>
    <dubbo.version>3.0.5</dubbo.version>
    <zookeeper.version>4.0.0</zookeeper.version>
    <servlet-api.version>4.0.1</servlet-api.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>${servlet-api.version}</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${juint.version}</version>
    </dependency>

    <!-- SpringMVC相关 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!--spring封装的jdbc数据库访问-->

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>

    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>${spring.version}</version>

    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>

    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jcl</artifactId>
      <version>${spring.version}</version>

    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>${spring.version}</version>

    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!--Spring提供的对AspectJ框架的整合-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <!--用于spring测试-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>


    <!-- 日志 -->
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>${slf4j-version}</version>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>${logback-version}</version>
    </dependency>


    <dependency>
      <groupId>org.apache.dubbo</groupId>
      <artifactId>dubbo</artifactId>
      <version>${dubbo.version}</version>
    </dependency>
    <!--ZooKeeper客户端实现 -->
    <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-framework</artifactId>
      <version>${zookeeper.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-recipes</artifactId>
      <version>${zookeeper.version}</version>
    </dependency>

    <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-x-discovery</artifactId>
      <version>${zookeeper.version}</version>
    </dependency>

  </dependencies>

  <build>
  <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>

        <configuration>
          <path>/</path>
          <port>81</port>
          <uriEncoding>UTF-8</uriEncoding><!-- 非必需项 -->
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

#### 2.2 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-service.xml</param-value>
    </context-param>
    
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
</web-app>
```

#### 2.3 创建服务接口及实现类

```java
package com.atguigu.service;
public interface HelloService {
    public String sayHello();
}
```

注意`@Service`注解要导入`com.alibaba.dubbo.config.annotation.Service`,用于对外发布服务

```java
package com.qh.service.impl;

import com.qh.service.HelloService;
import org.apache.dubbo.config.annotation.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHello() {
        return "hello";
    }
}
```

#### 2.4 创建spring配置文件

##### 2.4.1 创建spring-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <import resource="classpath:spring-registry.xml"></import>
</beans>
```

##### 2.4.2 创建spring-registry.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
    
    <!-- 当前应用名称，用于注册中心计算应用间依赖关系，注意：消费者和提供者应用名不要一样 -->
    <dubbo:application name="dubbo_provider"/>
    <!-- 连接服务注册中心zookeeper ip为zookeeper所在服务器的ip地址-->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>
    <!-- 注册  协议和port   端口默认是20880 -->
    <dubbo:protocol name="dubbo" port="20881"></dubbo:protocol>
    <!-- 扫描指定包，加入@Service注解的类会被发布为服务  -->
    <dubbo:annotation package="com.atguigu.service"/>

</beans>
```

#### 2.5 启动服务提供者

通过`tomcat`插件启动服务提供者

### 2、开发服务消费者dubbo_consumer

#### 2.1 创建消费者工程引入依赖

创建服务消费者工程，命名为`dubbo_consumer`，通过插件改成`javaweb`工程

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <spring.version>5.2.8.RELEASE</spring.version>

    <slf4j-version>1.7.25</slf4j-version>
    <logback-version>1.2.3</logback-version>
    <dubbo.version>3.0.5</dubbo.version>
    <zookeeper.version>4.0.0</zookeeper.version>
    <servlet-api.version>4.0.1</servlet-api.version>
</properties>

<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>${servlet-api.version}</version>
        <scope>provided</scope>
    </dependency>

    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
    </dependency>

    <!-- SpringMVC相关 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--spring封装的jdbc数据库访问-->

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>

    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>

    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>

    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jcl</artifactId>
        <version>${spring.version}</version>

    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-expression</artifactId>
        <version>${spring.version}</version>

    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--Spring提供的对AspectJ框架的整合-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--用于spring测试-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>

    <!-- 日志 -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>${slf4j-version}</version>
    </dependency>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>${logback-version}</version>
    </dependency>

    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo</artifactId>
        <version>${dubbo.version}</version>
    </dependency>
    <!--ZooKeeper客户端实现 -->
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>${zookeeper.version}</version>
    </dependency>
    <!--ZooKeeper客户端实现 -->
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-recipes</artifactId>
        <version>${zookeeper.version}</version>
    </dependency>

    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-x-discovery</artifactId>
        <version>${zookeeper.version}</version>
    </dependency>

</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>

            <configuration>
                <path>/</path>
                <port>80</port>
                <uriEncoding>UTF-8</uriEncoding><!-- 非必需项 -->
            </configuration>
        </plugin>
    </plugins>
</build>
```

#### 2.2 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 指定加载的配置文件 ，通过参数contextConfigLocation加载 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>
```

#### 2.3 创建服务接口

和提供者服务的接口一模一样

```java
package com.qh.service;
public interface HelloService {
    public String sayHello(String name);
}
```

#### 2.4 创建HelloController

```java
package com.qh.controller;

import com.qh.service.HelloService;
import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @Reference
    private HelloService helloService;

    @ResponseBody
    @RequestMapping("/hello")
    public String getName(){
        //远程调用
        String result = helloService.sayHello();
        System.out.println(result);
        return result;
    }
}
```

注意：Controller中注入HelloService使用的是Dubbo提供的@Reference注解

#### 2.5 创建spring配置文件

##### 2.5.1 创建spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 
       http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.qh"></context:component-scan>

    <import resource="classpath:spring-registry.xml"></import>

</beans>
```

##### 2.5.2 创建spring-registry.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://code.alibabatech.com/schema/dubbo
         http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
    
	<context:component-scan base-package="com.qh"></context:component-scan>
    <!-- 当前应用名称，用于注册中心计算应用间依赖关系，注意：消费者和提供者应用名不要一样 -->
    <dubbo:application name="dubbo_consumer" />
    <!-- 连接服务注册中心zookeeper ip为zookeeper所在服务器的ip地址-->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>
    
    <!-- 运行dubbo不检查提供者是否提前开启  -->
    <!-- <dubbo:consumer check="false"></dubbo:consumer> -->
</beans>
```

#### 2.6 启动服务消费者

使用`tomcat`插件启动服务消费者，然后在浏览器输入http://localhost/hello，查看浏览器输出结果

#### 2.7 后续问题思考

**思考一**：上面的Dubbo入门案例中我们是将HelloService接口从服务提供者工程(dubbodemo_provider)复制到服务消费者工程(dubbodemo_consumer)中，这种做法是否合适？还有没有更好的方式？

答：这种做法显然是不好的，同一个接口被复制了两份，不利于后期维护。更好的方式是单独创建一个maven工程，将此接口创建在这个maven工程中。需要依赖此接口的工程只需要在自己工程的pom.xml文件中引入maven坐标即可。

**思考二**：在服务消费者工程(dubbodemo_consumer)中只是引用了HelloService接口，并没有提供实现类，Dubbo是如何做到远程调用的？

答：Dubbo底层是基于代理技术为HelloService接口创建代理对象，远程调用是通过此代理对象完成的。

**思考三**：上面的Dubbo入门案例中我们使用Zookeeper作为服务注册中心，服务提供者需要将自己的服务信息注册到Zookeeper，服务消费者需要从Zookeeper订阅自己所需要的服务，此时Zookeeper服务就变得非常重要了，那如何防止Zookeeper单点故障呢？

答：Zookeeper其实是支持集群模式的，可以配置Zookeeper集群来达到Zookeeper服务的高可用，防止出现单点故障。

### 3、创建dubbo_interface模块

#### 3.1 创建接口工程

创建项目：`dubbo_interface`，并且将服务提供者和服务消费者中的`HelloService`接口拷贝到`dubbo_interface`工程中

#### 3.2 修改服务提供者和服务消费者

① 删除服务提供者和服务消费者中的`HelloService`接口

② 在服务提供者和服务消费者的`pom.xml`中添加依赖

```xml
<dependency>
    <groupId>com.qh</groupId>
    <artifactId>dubbo_interface</artifactId>
    <version>1.0</version>
</dependency>
```

#### 3.3 加入log4j

运行程序发现**dubbo**建议大家使用 **log4j**日志，我们就需要在 **resources** 文件夹下面引入**log4j.properties**或者**log4j.xml**日志配置文件

```properties
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=d:\\mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
### set log levels - for more verbose logging change 'info' to 'debug' ###
log4j.rootLogger=debug, stdout
```

#### 3.4 重新启动测试运行

重新启动服务提供者和服务消费者，并在浏览器上输入地址: http://localhost:8002/consumer/hello，测试访问

## 三、Dubbo管理控制台

我们在开发时，需要知道Zookeeper注册中心都注册了哪些服务，有哪些消费者来消费这些服务。我们可以通过部署一个管理中心来实现。其实管理中心就是一个web应用，部署到tomcat即可。

## 四、Dubbo参数配置

### 1、Dubbo包扫描

`<dubbo:annotation>标签`作用是①将HelloServiceImpl对象放入到Spring容器中, ②并将其发布为Dubbo服务.

#### 1.1 提供者

```java
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHello() {
        return "hello";
    }
}
```

```xml
<bean id="helloService" class="com.qh.service.impl.HelloServiceImpl" />
<dubbo:service interface="com.qh.api.HelloService" ref="helloService" />
```

#### 1.2 消费者

```xml
<!-- 生成远程服务代理，可以和本地bean一样使用helloService -->
<dubbo:reference id="helloService" interface="com.qh.service.HelloService" />
```

```java
@Controller
public class HelloController {
    @Autowired
    private HelloService helloService;

    @ResponseBody
    @RequestMapping("/hello")
    public String sayHello(){
        return helloService.sayHello();
    }

}
```

#### 1.3 注意事项

上面这种方式发布和引用服务，一个配置项(<dubbo:service>、<dubbo:reference>)只能发布或者引用一个服务，如果有多个服务，这种方式就比较繁琐了。推荐使用包扫描方式。

### 2、Dubbo常用协议

Dubbo支持九种协议：dubbo、rmi、hessian、http、webservice、thrift、redis、rest、memcached，推荐并默认使用的是dubbo协议。

#### 2.1、长连接和短链接

![yg534qwa6hu834w](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/yg534qwa6hu834w.509dv76rjf80.webp)



![t3qqgy75423wyg7324w](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/t3qqgy75423wyg7324w.5tghl06g85k0.webp)

#### 2.2、协议比较

##### 2.2.1 dubbo协议

连接个数：单连接

连接方式：长连接

传输协议：TCP

传输方式：NIO异步传输

序列化：Hessian 二进制序列化

适用范围：传入传出参数数据包较小（建议小于100K），消费者比提供者个数多，单一消费者无法压满提供者，尽量不要用dubbo协议传输大文件或超大字符串。

适用场景：常规远程服务方法调用

##### 2.2.2 rmi协议

连接个数：多连接

连接方式：短连接

传输协议：TCP

传输方式：同步传输

序列化：Java标准二进制序列化

适用范围：传入传出参数数据包大小混合，消费者与提供者个数差不多，可传文件。

适用场景：常规远程服务方法调用，与原生RMI服务互操作

##### 2.2.3 hessian协议

连接个数：多连接

连接方式：短连接

传输协议：HTTP

传输方式：同步传输

序列化：Hessian二进制序列化

适用范围：传入传出参数数据包较大，提供者比消费者个数多，提供者压力较大，可传文件。

适用场景：页面传输，文件传输，或与原生hessian服务互操作

##### 2.2.4 http协议

连接个数：多连接

连接方式：短连接

传输协议：HTTP

传输方式：同步传输

序列化：表单序列化 ，即 json

适用范围：传入传出参数数据包大小混合，提供者比消费者个数多，可用浏览器查看，可用表单或URL传入参数，暂不支持传文件。

适用场景：需同时给应用程序和浏览器JS使用的服务。

##### 2.2.5 webservice协议

连接个数：多连接

连接方式：短连接

传输协议：HTTP

传输方式：同步传输

序列化：SOAP文本序列化

适用场景：系统集成，跨语言调用

#### 2.3 配置实现

也可以在同一个工程中配置多个协议，不同协议使用的端口号不能相同，例如：

```xml
<!-- 多协议配置 -->
<dubbo:protocol name="dubbo" port="20880" />
```

```java
@Service(protocol = "dubbo")
public class UserServiceImpl implements UserService {
    @Override
    public String hello() {
        return "hello";
    }
}
```

### 3、启动时检查

```xml
<dubbo:consumer check="false"/>
```

上面这个配置需要配置在服务消费者一方，如果不配置默认check值为true。Dubbo 缺省会在启动时检查依赖的服务是否可用，不可用时会抛出异常，阻止 Spring 初始化完成，以便上线时，能及早发现问题。可以通过将check值改为false来关闭检查。建议在开发阶段将check值设置为false，在生产环境下改为true。

### 4、地址缓存

注册中心zookeeper挂了,服务是否可以正常访问?

可以，因为dubbo服务消费者在第一次调用时，会将服务提供方地址缓存到本地，以后在调用则不会访问注册中心。

### 5、多版本

灰度发布:   当出现新功能时，会让一部分用户先使用新功能，用户反馈没问题时，再将所有用户迁移到新功能。

```java
@DubboService(version = "1.0")
public class HelloServiceImpl implements HelloService {
    @Override
    public String sayHello() {
        return "hello";
    }
}
```

```java
@DubboService(version = "2.0")
public class HelloServiceImpl2 implements HelloService {
    @Override
    public String sayHello() {
        return "hello2";
    }
}
```

```java
@Controller
public class HelloController {
    @Reference(version = "2.0")
    private HelloService helloService;

    @ResponseBody
    @RequestMapping("/hello")
    public String sayHello(){
        return helloService.sayHello();
    }

}
```

### 6、Dubbo负载均衡

#### 6.1 负载均衡概述

负载均衡（Load Balance）：其实就是将请求分摊到多个操作单元上进行执行，从而共同完成工作任务。在集群负载均衡时，Dubbo 提供了多种均衡策略（包括随机、轮询、最少活跃调用数、一致性Hash），缺省为random随机调用。

1. 随机算法 RandomLoadBalance（默认）
2. 轮询算法 RoundRobinLoadBalance
3. 最小活跃数算法 LeastActiveLoadBalance
4.  一致性hash算法 ConsistentHashLoadBalance

配置负载均衡策略，既可以在服务提供者一方配置，也可以在服务消费者一方配置，两者选其一即可: 

① 在服务消费者一方配置

```java
@Controller
public class HelloController {
    //在服务消费者一方配置负载均衡策略
    @Reference(check = false,loadbalance = "random")
    private HelloService helloService;

    @RequestMapping("/hello")
    @ResponseBody
    public String getName(){
        //远程调用
        return helloService.sayHello();
    }
}
```

② 在服务提供者一方配置

```java
@Service(loadbalance = "random")
public class HelloServiceImpl implements HelloService {
    public String sayHello() {
        return "hello";
    }
}
```

#### 6.2 负载均衡实践

##### 6.2.1 启动第一个服务提供者实例

**注意:**要关闭`jetty`的热部署自动刷新

① `pom.xml`文件中`jetty`插件的端口为`81`

② `applicationContext-service.xml`中`dubbo`的端口号设置为`20881`

③ 为了演示方便，将`HelloServiceImpl`的`sayHello`方法的返回值改成`return "81 hello"`

④ 启动第一个服务提供者实例

##### 6.2.2 启动第二个服务提供者实例

① 将第一个服务提供者实例复制一份

![r534wefq2tg3fq](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/r534wefq2tg3fq.1yi7z83u5cjk.webp)

②  `pom.xml`文件中`jetty`插件的端口为`82`

③ `spring-registry.xml`中`dubbo`的端口号设置为`20882`

④ 为了演示方便，将`HelloServiceImpl`的`sayHello`方法的返回值改成`return "82 hello"`

⑤ 启动第二个服务提供者实例

##### 6.2.3 启动服务消费者实例

使用`jetty`插件启动服务消费者实例

##### 6.2.4 测试访问

多次请求 http://localhost:8082/demo/hello，看浏览器的效果