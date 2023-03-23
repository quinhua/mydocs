### 1.pom.xml
```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.6.RELEASE</version>
        <relativePath/>
    </parent>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        
        <fastjson.version>2.0.21</fastjson.version>
        <lombok.version>1.18.26</lombok.version>
    </properties>

    <dependencies>
        <!--fastjson start-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>${fastjson.version}</version>
        </dependency>
        <!--fastjson end-->

        <!--lombok start-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
        <!--fastjson end-->

        <!--单元测试启动器 start-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
        <!--单元测试启动器 end-->

        <!--通用mapper启动器依赖 start-->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
            <version>2.1.5</version>
        </dependency>
        <!--通用mapper启动器依赖 end-->
        
        <!--JDBC启动器依赖 start-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!--JDBC启动器依赖 end-->
        
        <!--druid启动器依赖 start-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <!--druid启动器依赖 end-->
        
        <!--web启动器依赖 start-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--web启动器依赖 end-->

        <!--spring boot actuator依赖 start-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--spring boot actuator依赖 end-->

        <!--编码工具包 start-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
        </dependency>
        <!--编码工具包 end-->

        <!--热部署 start-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <!--热部署 end-->

        <!-- mysql start-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- mysql end-->

        <!--springboot整合redis启动器 starat-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <!--springboot整合redis启动器 end-->

    </dependencies>

    <build>
        <plugins>
            <!--spring boot maven插件 , 可以将项目运行依赖的jar包打到项目中-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

### 2.application.yml

```xml
server:
  # 项目访问端口
  port: 10001

spring:
  # 数据库配置
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/quinhua?serverTimezone=Asia/Shanghai
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource

  # redis配置
  redis:
    host: 127.0.0.1 #
    port: 6379

# mybatis
mybatis:
  # 实体类起别名
  type-aliases-package: com.qh.pojo
  # mapper映射文件位置
  mapper-locations: classpath:mapper/*.xml
  configuration:
    # 驼峰命名规范
    map-underscore-to-camel-case: true

# actuator
management:
  endpoints:
    web:
      exposure:
        # 对外暴露的访问入口 , 默认是/health和/info
        include: '*'
      base-path: /monitor # 默认是actuator
  endpoint:
    health:
      show-details: ALWAYS	# 显示所有健康状态
  server:
    port: 9999
```

