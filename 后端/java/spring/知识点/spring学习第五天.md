### 1-Spring5.0整合日志框架

* System.out.println存在的问题

  * ①底层使用`synchronized`关键字加锁, 效率非常低;
  * ②在测试阶段开启日志, 在部署阶段关闭日志 , 没有日志开关;
  * ③无法将日志记录到文件或数据库.

* 日志框架

  * jul: java.util.logging, 没人用
  * log4j: log for java, 好用, 比较流行, 后续被apache收购
  * log4j2: apache, 全新的日志架构.
  * logback: 和log4j同作者

* 日志门面

  * jcl: Jakarta Commons Logging, 底层大量使用算法, 一旦出现问题排错麻烦.
  * slf4j: simple logging facade for java, 简单日志门面, 解决了使用大量算法问题

* 日志方案

  * slf4j + log4j
  * slf4j + log4j2
  * slf4 + logback

* 日志级别

  * 优先级 : off > error > warn > info > debug > all
    * off: 关闭所有日志
    * error: 错误信息
    * warn: 警告提示信息
    * info: 运行信息
    * debug: 调试信息
    * all: 打印所有日志
  * 假设, 日志级别=info, 意味着error, warn, info的日志都会打印

* 开发步骤

  * ①引入依赖
    * slf4j, log4j
  * ②编写log4j2.properties文件
  * ③代码测试

* ①引入依赖

  ```xml
  <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.25</version>
  </dependency>
  <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-slf4j-impl</artifactId>
      <version>2.19.0</version>
  </dependency>
  ```
  
* ②编写log4j2.properties文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!--日志级别以及优先级排序: OFF  > ERROR > WARN > INFO > DEBUG  > ALL -->
  <configuration status="INFO">
      <!--先定义所有的appender-->
      <appenders>
          <!--输出日志信息到控制台-->
          <console name="Console" target="SYSTEM_OUT">
              <!--控制日志输出的格式-->
              <PatternLayout pattern="%-5p %d{yyyy-MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n"/>
          </console>
      </appenders>
      <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
      <loggers>
  
          <!--root：用于指定项目的根日志，如果没有单独指定Logger，则会使用root作为默认的日志输出-->
          <!--全局日志级别-->
          <root level="debug">
              <appender-ref ref="Console"/>
          </root>
  
          <!--局部日志级别-->
          <logger level="info" name="com.atguigu.log.MyTest">
          </logger>
      </loggers>
  
  </configuration>
  ```
  
* ③代码测试

  ```java
  public class LoggerTest {
  
      Logger logger = LoggerFactory.getLogger(LoggerTest.class);
  
      @Test
      public void test1(){
          logger.error("test1 error");
          logger.warn("test1 warn");
          logger.info("test1 info");
          logger.debug("test1 debug");
      }
  
  }
  ```
  

### 06-Spring5.0新注解@Nullable

* 概述
  * 在Spring5.0中大量使用了 @Nullable 注解 , 该注解可以用在属性, 方法, 形参上面. 可以明确地告诉开 发人员使用了 @Nullable 注解的位置传递null也不会造成空指针异常.
* ①用在属性上面 : 属性值可以为空
  * ![ftgwa4e3yhe56wq2hbw](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ftgwa4e3yhe56wq2hbw.2c87rbbyjaas.webp)

* ②用在方法上面 : 方法返回值可以为空
  * ![g4whbsrhbaehaghet](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/g4whbsrhbaehaghet.2hnv33becia0.webp)

* ③用在形参前面 : 参数值可以为空
  * ![efgw4qgyhw4hyghwsa](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/efgw4qgyhw4hyghwsa.1s15nvmiuslc.webp)

### 07-Spring5.0整合junit5

* ①整合junit4

  ```xml
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
  </dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.3.24</version>
  </dependency>
  ```

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations = "classpath:spring-core.xml")
  public class UserServiceTest {
  
      @Autowired
      private UserService userService;
  
      @Test
      public void insert() throws Exception {
          userService.insert(new User(null , "张三", "zhangsan"));
      }
  
      @Test
      public void findAll() {
          List<User> userList = userService.findAll();
          System.out.println("userList = " + userList);
      }
  
      @Test
      public void delete() {
      }
  
      @Test
      public void update() {
      }
  
      @Test
      public void getById() {
          User user = userService.getById(1);
          System.out.println("user = " + user);
      }
  
  }
  ```
  
* ②整合junit5: 基础版

  ```xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.3.24</version>
  </dependency>
  <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.7.2</version>
      <scope>test</scope>
  </dependency>
  ```

* ③整合junit5: 优化版

  ```java
  @SpringJUnitConfig(locations = "classpath:spring-core.xml")
  public class UserServiceTest3 {
  
      Logger logger = LoggerFactory.getLogger(UserServiceTest3.class);
      @Autowired
      private UserService userService;
  
  
      @Test
      public void insert() throws Exception {
          userService.insert(new User(null, "张三", "zhangsan"));
      }
  
      @Test
      public void findAll() {
          List<User> userList = userService.findAll();
          logger.debug("" + userList);
      }
  
      @Test
      public void delete() {
      }
  
      @Test
      public void update() {
      }
  
      @Test
      public void getById() {
          User user = userService.getById(1);
          System.out.println("user = " + user);
      }
  
  }
  ```

### 08-Spring整合MyBatis概述

* 概述
  * Spring整合MyBatis有两种方式: ①传统dao开发模式, ②dao接口代理模式 

* ①传统dao模式
  * dao接口 + dao实现子类 + mapper映射文件
  * 本质: 将dao实现子类的对象放入到Spring容器进行管理
* ②dao接口代理模式
  * dao接口 + mapper映射文件
  * 本质: 将dao接口代理对象放入到Spring容器进行管理

### 09-Spring整合MyBatis传统dao模式基础版

* 概述
  * 本质: 将dao实现子类的对象放入到Spring容器进行管理

* 开发步骤

  * ①引入依赖
  * ②定义dao接口及其实现子类
  * ③编写spring-core.xml
  * ④编写mybatis-config.xml
  * ⑤编写UserMapper.xml
  * ⑥编写jdbc.properties
  * ⑦代码测试

* ①引入依赖

  ```xml
  <properties>
       <maven.compiler.source>17</maven.compiler.source>
      <maven.compiler.target>17</maven.compiler.target>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <log4j.version>1.2.17</log4j.version>
      <slf4j-api.version>2.0.6</slf4j-api.version>
      <slf4j-log4j12.version>2.0.6</slf4j-log4j12.version>
      <junit-jupiter-api.version>5.9.2</junit-jupiter-api.version>
      <junit.version>4.13.2</junit.version>
      <lombok.version>1.18.26</lombok.version>
      <spring.version>6.0.4</spring.version>
      <druid.version>1.2.15</druid.version>
      <mysql.version>8.0.32</mysql.version>
      <shiro-core.version>1.11.0</shiro-core.version>
      <mybatis.version>3.5.11</mybatis.version>
      <mybatis-spring.version>3.0.1</mybatis-spring.version>
      <pagehelper.version>5.3.2</pagehelper.version>
    </properties>
  
    <dependencies>
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${spring.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
      </dependency>
  
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>${mybatis-spring.version}</version>
      </dependency>
  
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>${mybatis.version}</version>
      </dependency>
  
      <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>${pagehelper.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>${slf4j-api.version}</version>
      </dependency>
  
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>${slf4j-log4j12.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>${shiro-core.version}</version>
      </dependency>
  
      <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>${junit-jupiter-api.version}</version>
      </dependency>
  
    </dependencies>
  ```

* ②定义dao接口及其实现子类

  ```java
  public interface UserMapper {
      List<User> findAll() throws Exception;
  }
  ```

  ```java
  @Repository
  public class UserMapperImpl implements UserMapper {
  
      @Autowired
      private SqlSessionFactory sqlSessionFactory;//单例对象
  
      @Override
      public List<User> findAll() throws Exception {
          SqlSession sqlSession = sqlSessionFactory.openSession();
          List<User> userList = sqlSession.selectList("findAll");
          sqlSession.close();
          return userList;
      }
  
  }
  ```
  
* ③编写spring-core.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="
         http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
  
      <!--扫描注解-->
      <context:component-scan base-package="com.qh"></context:component-scan>
  
      <!--加载SqlSessionFactory-->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <!--关联DataSource-->
          <property name="dataSource" ref="dataSource"></property>
          <!--加载mybatis-config.xml-->
          <property name="configLocation" value="mybatis-config.xml"></property>
      </bean>
  
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
  
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
  
  </beans>
  ```
  
* ④编写mybatis-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "https://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  
      <settings>
          <!--自动驼峰-->
          <setting name="mapUnderscoreToCamelCase" value="true"/>
      </settings>
  
      <typeAliases>
          <!--别名-->
          <package name="com.atguigu.pojo"/>
      </typeAliases>
  
      <!--加载映射文件-->
      <mappers>
          <mapper resource="com/qh/mapper/UserMapper.xml"/>
      </mappers>
  </configuration>
  ```

* ⑤编写UserMapper.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.qh.mapper.UserMapper">
      <select id="findAll" resultType="User">
          select * from t_user
      </select>
  </mapper>
  ```

* ⑥编写jdbc.properties

  ```properties
  driverClass=com.mysql.cj.jdbc.Driver
  jdbcUrl=jdbc:mysql://localhost:3306/mydb?useSSL=false
  user=root
  password=root
  ```

* ⑦代码测试

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations = "classpath:spring-core.xml")
  public class UserMapperTest {
  
      @Autowired
      private UserMapper userMapper;
  
      @Test
      public void findAll() throws Exception {
          List<User> userList = userMapper.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```
  
* 存在的问题
  * 在dao实现子类的方法中, 需要频繁地`创建/提交/关闭`SqlSession对象.

### 10-Spring整合MyBatis传统dao模式优化版

* 概述

  * 使用SqlSessionDaoSupport优化SqlSession对象的`创建/提交/关闭`

* 源码

  ```java
  /**
  * Users should use this method to get a SqlSession to call its statement methods This is SqlSession is managed by
  * spring. Users should not commit/rollback/close it because it will be automatically done.
  *
  * @return Spring managed thread safe SqlSession
  */
  public SqlSession getSqlSession() {
  	return this.sqlSessionTemplate;
  }
  ```

* 代码实现

  ```java
  @Repository
  public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
  
      @Autowired
      @Override
      public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
          //调用父类的setSqlSessionFactory方法 -> SqlSessionDaoSupport的setSqlSessionFactory
          super.setSqlSessionFactory(sqlSessionFactory);
      }
  
      @Override
      public List<User> findAll() throws Exception {
          List<User> userList = getSqlSession().selectList("findAll");
          return userList;
      }
  
      @Override
      public int insert(User user) throws Exception {
          int insert = getSqlSession().insert("insert", user);
          return insert;
      }
  
  }
  ```
  
* 存在的问题
  * 每一个dao接口的实现子类中都得重写`setSqlSessionFactory方法`, 并通过这个方法将Spring容器中的`SqlSessionFactory对象`注入给`SqlSessionDaoSupport`.

### 11-Spring整合MyBatis传统dao模式终极版

* 代码实现

  ```java
  @Component
  public class BaseMapperImpl extends SqlSessionDaoSupport {
  
      @Autowired
      @Override
      public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
          //调用父类的setSqlSessionFactory方法 -> SqlSessionDaoSupport的setSqlSessionFactory
          super.setSqlSessionFactory(sqlSessionFactory);
      }
  
  }
  ```
  
  ```java
  @Repository
  public class UserMapperImpl extends BaseMapperImpl implements UserMapper {
  
      @Override
      public List<User> findAll() throws Exception {
          List<User> userList = getSqlSession().selectList("findAll");
          return userList;
      }
  
      @Override
      public int insert(User user) throws Exception {
          int insert = getSqlSession().insert("insert", user);
          return insert;
      }
  
  }
  ```

### 12-Spring整合MyBatis传统dao模式补充

* 概述

  * 将`mybatis-config.xml`文件中参数全部交给`spring-core.xml`文件来进行配置.

* 代码实现

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="
         http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
  
      <!--扫描注解-->
      <context:component-scan base-package="com.qh"></context:component-scan>
  
      <!--加载SqlSessionFactory-->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <!--关联DataSource-->
          <property name="dataSource" ref="dataSource"></property>
          <!--别名-->
          <property name="typeAliasesPackage" value="com.qh.pojo"></property>
          <!--加载映射文件-->
          <property name="mapperLocations" value="mappers/*Mapper.xml"></property>
          <property name="configuration" ref="configuration"></property>
          <!--分页插件-->
          <property name="plugins">
              <array>
                  <bean class="com.github.pagehelper.PageInterceptor">
                      <property name="properties">
                          <props>
                              <prop key="reasonable">true</prop>
                              <prop key="helperDialect">mysql</prop>
                          </props>
                      </property>
                  </bean>
              </array>
          </property>
      </bean>
  
      <bean id="configuration" class="org.apache.ibatis.session.Configuration">
          <!--驼峰标识-->
          <property name="mapUnderscoreToCamelCase" value="true"></property>
          <!--缓存-->
          <property name="cacheEnabled" value="true"></property>
          <!--懒加载-->
          <property name="lazyLoadingEnabled" value="true"></property>
      </bean>
  
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
  
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
  
  </beans>
  ```
  
* 注意事项
  * mapper映射文件存储的文件夹的层级不要太多了, 会报错!!!
* 总结
  * Spring参数由`spring-core.xml文件`配置, MyBatis参数由`mybatis-config.xml文件`配置

### 13-Spring整合MyBatis接口代理模式基础版

* 概述

  * 本质: 将dao接口代理对象放入到Spring容器进行管理

* 开发步骤

  * ①引入依赖
  * ②定义dao接口
  * ③编写spring-core.xml
  * ④编写mybatis-config.xml
  * ⑤编写UserMapper.xml
  * ⑥编写jdbc.properties
  * ⑦代码测试

* ①引入依赖

* ②定义dao接口

* ③编写spring-core.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
         http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!--加载接口代理对象-->
      <bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
          <!--找到接口, 就能够找到映射文件-->
          <property name="mapperInterface" value="com.qh.mapper.UserMapper"></property>
          <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
      </bean>
      
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <!--关联DataSource-->
          <property name="dataSource" ref="dataSource"></property>
          <!--加载mybatis-config.xml-->
          <property name="configLocation" value="mybatis-config.xml"></property>
      </bean>
  
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
  
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
  
  </beans>
  ```
  
* ④编写mybatis-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "https://mybatis.org/dtd/mybatis-3-config.dtd">
  
  <configuration>
  
      <settings>
          <!--自动驼峰-->
          <setting name="mapUnderscoreToCamelCase" value="true"/>
      </settings>
  
      <typeAliases>
          <!--设置别名-->
          <package name="com.qh.pojo"/>
      </typeAliases>
  
      <plugins>
          <plugin interceptor="com.github.pagehelper.PageInterceptor">
              <!--合理化-->
              <property name="reasonable" value="true"/>
              <!--方言-->
              <property name="helperDialect" value="mysql"/>
          </plugin>
      </plugins>
  
  </configuration>
  ```
  
* ⑤编写UserMapper.xml

* ⑥编写jdbc.properties

* ⑦代码测试

* 注意事项

  * 在`mybatis-config.xml文件`中可以不用加载mapper映射文件, 因为在`spring-core.xml文件`中通过`<property name="mapperInterface" value="com.atguigu.mapper.UserMapper"></property>`参数已经加载到了mapper映射文件.

* 存在的问题

  * 一个dao接口就对应一个MapperFactoryBean, 维护起来太麻烦了.

### 14-Spring整合MyBatis接口代理模式优化版

* 代码实现

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:Context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">
  
      <!--声明使用注解配置-->
      <Context:annotation-config/>
      <!--声明Spring工厂注解的扫描范围-->
      <Context:component-scan base-package="com.qh"/>
  
      <!--引用外部文件-->
      <Context:property-placeholder location="jdbc.properties"/>
  
      <!--配置DruidDataSources-->
      <bean id="DruidDataSources" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${jdbc.driverClassName}"/>
          <property name="url" value="${jdbc.url}"/>
          <property name="username" value="${jdbc.userName}"/>
          <property name="password" value="${jdbc.password}"/>
          <property name="initialSize" value="${jdbc.pool.initialSize}"/>
          <property name="minIdle" value="${jdbc.pool.minIdle}"/>
          <property name="maxActive" value="${jdbc.pool.maxActive}"/>
          <property name="maxWait" value="${jdbc.pool.maxWait}"/>
      </bean>
  
      <!--  配置sqlsessionFactory  -->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <!--配置数据源-->
          <property name="dataSource" ref="DruidDataSources"/>
          <!--配置mapper的路径-->
          <property name="mapperLocations" value="classpath*:mappers/*Mapper.xml">
          </property>
          <!--配置需要定义别名的实体类的包-->
          <property name="typeAliasesPackage" value="com.qh.pojo"/>
          <!--配置需要mybatis的主配置文件-->
          <property name="configLocation" value="mybatis-config.xml"/>
      </bean>
  
      <!--配置MapperScannerConfigurer-->
      <!--
        在配置MapperScannerConfigurer中主要是加载dao包中的所有dao接口，
        通过sqlsessionFactory获取sqlsession，然后创建所有dao接口对象，存储在spring容器
       -->
      <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer" >
          <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
          <property name="basePackage" value="com.qh.mapper"/>
      </bean>
  
      <!--  将spring事务管理配置给spring  -->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="DruidDataSources"/>
      </bean>
  
      <!--将spring事务管理配置给spring-->
      <!--
      通过Spring jdbc提供的<tx>标签声明事物的管理策略，并给事务设置隔离级别以及传播机制
      -->
      <tx:advice id="TxAdvice" transaction-manager="transactionManager">
          <tx:attributes>
              <tx:method name="Insert*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
              <tx:method name="Update*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
              <tx:method name="Delete*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
              <tx:method name="Query*" isolation="REPEATABLE_READ" propagation="SUPPORTS"/>
          </tx:attributes>
      </tx:advice>
  
      <!--将事务管理以Aop配置，应用于ServiceI方法（ServiceImp）-->
      <aop:config>
          <aop:pointcut id="crud" expression="execution(* *..*Service.*(..))"/>
          <aop:advisor advice-ref="TxAdvice" pointcut-ref="crud"/>
      </aop:config>
  
  </beans>
  ```