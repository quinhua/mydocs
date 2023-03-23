### 01-通知获取切入点返回值

* 概述

  * before: 无法获取
  * **afterReturning: 可以获取**
  * afterThrowing: 无法获取
  * after: 无法获取
  * **around: 可以获取**

* 代码实现

  ```java
  @Component
  public class MyAdvice01 {
  
      //正常后置通知
      //Object result: 切入点返回值
      public void afterReturning(Object result) {
          System.out.println("MyAdvice01 afterReturning 切入点返回值 : " + result);
      }
  
      //环绕通知
      public Object around(ProceedingJoinPoint pjp) {
          Object result = null;
          try {
              result = pjp.proceed();
              System.out.println("MyAdvice01 around 切入点返回值 : " + result);
              return result;
          } catch (Throwable e) {
              throw new RuntimeException(e);
          }
      }
  
  }
  ```
  
  ```xml
  <aop:config>
  
      <aop:aspect ref="myAdvice01">
          <aop:pointcut id="p1" expression="execution(* *..*Service.*(..))"/>
          <aop:after-returning method="afterReturning"
                               pointcut-ref="p1"
                               returning="result"></aop:after-returning>
          <aop:around method="around"
                      pointcut-ref="p1"></aop:around>
      </aop:aspect>
  
  </aop:config>
  ```

### 02-通知获取切入点异常信息

* 概述

  * before: 无法获取
  * afterReturning: 无法获取
  * **afterThrowing: 可以获取**
  * after: 无法获取
  * **around: 可以获取**

* 代码实现

  ```java
  //02-通知获取切入点异常信息
  @Component
  public class MyAdvice02 {
  
      //异常后置通知
      //Object result: 切入点返回值
      public void afterThrowing(Throwable exception) {
          System.out.println("MyAdvice01 afterThrowing 切入点异常信息 : " + exception);
      }
  
      //环绕通知
      public Object around(ProceedingJoinPoint pjp) {
          Object result = null;
          try {
              result = pjp.proceed();
              return result;
          } catch (Throwable e) {
              System.out.println("MyAdvice01 around 切入点异常信息 : " + e);
              throw new RuntimeException(e);
          }
      }
  
  }
  ```
  
  ```xml
  <aop:config>
  
      <aop:aspect ref="myAdvice02">
          <aop:pointcut id="p1" expression="execution(* *..*Service.*(..))"/>
          <aop:after-throwing method="afterThrowing"
                               pointcut-ref="p1"
                               throwing="exception"></aop:after-throwing>
          <aop:around method="around"
                      pointcut-ref="p1"></aop:around>
      </aop:aspect>
  
  </aop:config>
  ```

### 03-AOP注解开发

* 开发步骤

  * ①编写spring-core.xml, 开启aop注解支持
  * ②制作目标对象类
  * ③制作通知类1
  * ④制作通知类2: 独立维护切入点表达式

* ①编写spring-core.xml, 开启aop注解支持

  ```xml
  <!--开启aop注解支持-->
  <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
  ```

* ②制作目标对象类

* ③制作通知类

  ```java
  @Aspect
  @Component
  public class MyAdvice03 {
  
      @Before("execution(* *..*Service.*(..))")
      public void before() {
          System.out.println("MyAdvice03 before");
      }
  
      @AfterReturning("execution(* *..*Service.*(..))")
      public void afterReturning() {
          System.out.println("MyAdvice03 afterReturning");
      }
  
      @AfterThrowing("execution(* *..*Service.*(..))")
      public void afterThrowing() {
          System.out.println("MyAdvice03 afterThrowing");
      }
  
      @After("execution(* *..*Service.*(..))")
      public void after() {
          System.out.println("MyAdvice03 after");
      }
      @Around("execution(* *..*Service.*(..))")
      public Object around(ProceedingJoinPoint pjp) throws Throwable {
          System.out.println("MyAdvice03 around 之前");
          Object result = pjp.proceed();
          System.out.println("MyAdvice03 around 之后");
          return result;
      }
  
  }
  ```
  
* ④制作通知类2: 独立维护切入点表达式

  ```java
  @Aspect
  @Component
  public class MyAdvice04 {
  
      @Pointcut("execution(* *..*Service.*(..))")
      public void p1() {
  
      }
  
      @Before("p1()")
      public void before() {
          System.out.println("MyAdvice04 before");
      }
  
      @AfterReturning("p1()")
      public void afterReturning() {
          System.out.println("MyAdvice04 afterReturning");
      }
  
      @AfterThrowing("p1()")
      public void afterThrowing() {
          System.out.println("MyAdvice04 afterThrowing");
      }
  
      @After("p1()")
      public void after() {
          System.out.println("MyAdvice04 after");
      }
      @Around("p1()")
      public Object around(ProceedingJoinPoint pjp) throws Throwable {
          System.out.println("MyAdvice04 around 之前");
          Object result = pjp.proceed();
          System.out.println("MyAdvice04 around 之后");
          return result;
      }
  }
  ```

### 04-AOP注解获取切入点输入参数

* 概述

  * `非环绕通知`和`环绕通知`获取切入点输入参数

* 代码实现

  ```java
  @Aspect
  @Component
  public class MyAdvice05 {
  
      @Pointcut("execution(* *..*Service.*(..))")
      public void p1() {
  
      }
  
      //JoinPoint jp: 切入点对象
      @Before("p1()")
      public void before(JoinPoint jp) {
          System.out.println("MyAdvice05 before 切入点输入参数 : " + Arrays.toString(jp.getArgs()));
      }
  
  
      //ProceedingJoinPoint pjp: 切入点对象
      @Around("p1()")
      public Object around(ProceedingJoinPoint pjp) throws Throwable {
          System.out.println("MyAdvice05 around 切入点输入参数 : " + Arrays.toString(pjp.getArgs()));
          Object result = pjp.proceed();
          return result;
      }
  
  }
  ```

### 05-AOP注解获取切入点返回值

* 概述

  * `afterReturning`, `around`可以获取切入点返回值

* 代码实现

  ```java
  @Aspect
  @Component
  public class MyAdvice06 {
  
      @Pointcut("execution(* *..*Service.*(..))")
      public void p1() {
  
      }
  
      @AfterReturning(pointcut = "p1()", returning = "result")
      public void afterReturning(Object result) {
          System.out.println("MyAdvice06 afterReturning 切入点返回值 : " + result);
      }
  
      @Around("p1()")
      public Object around(ProceedingJoinPoint pjp) throws Throwable {
          Object result = pjp.proceed();
          System.out.println("MyAdvice06 around 切入点返回值 : " + result);
          return result;
      }
  
  }
  ```

### 06-AOP注解获取切入点异常信息

* 概述

  * `afterThrowing`, `around`获取切入点异常信息

* 代码实现

  ```java
  @Aspect
  @Component
  public class MyAdvice07 {
  
      @Pointcut("execution(* *..*Service.*(..))")
      public void p1() {
      }
  
      @AfterThrowing(pointcut = "p1()", throwing = "exception")
      public void afterThrowing(Throwable exception) {
          System.out.println("MyAdvice04 afterThrowing 切入点异常信息 : " + exception);
      }
  
      @Around("p1()")
      public Object around(ProceedingJoinPoint pjp) {
          Object result = null;
          try {
              result = pjp.proceed();
          } catch (Throwable e) {
              System.out.println("MyAdvice04 around 切入点异常信息 : " + e);
              throw new RuntimeException(e);
          }
          return result;
      }
  
  }
  ```
  

### 07-AOP对根据类型获取bean的影响场景一

* 场景一
  * 声明一个接口，接口有一个实现类 
  * 创建一个通知类，对上面接口的实现子类应用通知
  * ①根据接口类型获取bean 
  * ②根据实现子类类型获取bean
* ①根据接口类型获取bean 
  * 可以
* ②根据实现子类类型获取bean
  * 不可以
* 分析
  * ![https://jsd.cdn.zzko.cn/gh/quinhua/pics@main/markdown/ws54jwyuhsehbsh.3qrqchdsb440.webp](https://jsd.cdn.zzko.cn/gh/quinhua/pics@main/markdown/ws54jwyuhsehbsh.3qrqchdsb440.webp)

### 08-AOP对根据类型获取bean的影响场景二

* 场景二
  * 声明一个类 
  * 创建一个通知类，对上面的类应用通知
  * ①根据这个类的类型获取bean
* ①根据这个类的类型获取bean
  * 可以
* 分析
  * ![image-20230218111130076](https://qiuzhiwei1989.oss-cn-hangzhou.aliyuncs.com/WH220718/image-20230218111130076.png)

* 总结
  * 如果有应用AOP通知, 放入到Spring容器中的是代理类对象.
    * 如果有接口, 使用jdk proxy , 代理类和被代理类实现同一个接口
    * 如果没有接口, 使用cglib, 代理类继承被代理类

### 09-JdbcTemplate模板类的使用

* 概述

  * Spring提供有DAO支持模板类，功能类似于Apache 的DbUtils.
  * 后续Spring整合MyBatis之后, 就不再使用. 

* 开发步骤

  * ①引入依赖
    * spring-jdbc
  * ②定义dao接口及其实现子类
  * ③编写spring-core.xml
  * ④代码测试

* ①引入依赖

* ②定义dao接口及其实现子类

  ```java
  @Repository
  public class UserMapperImpl implements UserMapper {
  
      @Autowired
      private JdbcTemplate jdbcTemplate;
  
      @Override
      public int insert(User user) {
          return jdbcTemplate.update(
                  "insert into t_user values(null, ?,?)",
                  user.getUserName(),
                  user.getUserPwd()
          );
      }
  
      @Override
      public int delete(Integer id) {
          return jdbcTemplate.update(
                  "delete from t_user where user_id = ?",
                  id
          );
      }
  
      @Override
      public int update(User user) {
          return jdbcTemplate.update(
                  "update t_user set user_name = ? , user_pwd = ? where user_id = ?",
                  user.getUserName(),
                  user.getUserPwd(),
                  user.getUserId()
          );
      }
  
      @Override
      public User getById(Integer id) {
          return jdbcTemplate.queryForObject(
                  "select user_id userId, user_name userName, user_pwd userPwd from t_user where user_id = ? ",
                  new RowMapper<User>() {
                      @Override
                      public User mapRow(ResultSet rs, int rowNum) throws SQLException {
                          User user = new User(
                                  rs.getInt(1),
                                  rs.getString(2),
                                  rs.getString(3)
                          );
                          return user;
                      }
                  },
                  id
          );
      }
  
      @Override
      public List<User> findAll() {
          return jdbcTemplate.query(
                  "select user_id userId, user_name userName, user_pwd userPwd from t_user",
                  new BeanPropertyRowMapper<User>(User.class)
          );
      }
  }
  ```

* ③编写spring-core.xml

  ```properties
  driverClass=com.mysql.cj.jdbc.Driver
  jdbcUrl=jdbc:mysql://localhost:3306/mydb?useSSL=false
  user=root
  password=root
  ```

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
      <context:component-scan base-package="com.qh"></context:component-scan>
  
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
          <property name="dataSource" ref="dataSource"></property>
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
  
* ④代码测试

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations = "classpath:spring-core.xml")
  public class UserMapperTest {
  
      @Autowired
      private UserMapper userMapper;
  
      @Test
      public void insert() {
          userMapper.insert(new User(null ,"王八蛋","badan"));
      }
  
      @Test
      public void delete() {
      }
  
      @Test
      public void update() {
      }
  
      @Test
      public void getById() {
          User user = userMapper.getById(1);
          System.out.println("user = " + user);
      }
  
      @Test
      public void findAll() {
          List<User> userList = userMapper.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```

### 10-Spring事务概念

* 概述
  * Spring事务管理有两种模式: `①编程式事务`, `②声明式事务`
* ①编程式事务: 开发人员写java代码进行事务管理
* ②声明式事务: 开发人员写配置进行事务管理
  * 2.1, xml声明式事务
  * 2.2, 注解声明式事务
* 常用API
  * PlatformTransactionManager: 事务管理类
  * TransactionDefinition: 事务配置类
  * TransactionStatus: 事务状态类

### 11-PlatformTransactionManager概述

* 概述

  * Spring进行事务管理的核心类: 开启事务, 提交事务, 回滚事务

* 常用方法

  ```java
  //开启事务
  TransactionStatus getTransaction(@Nullable TransactionDefinition definition);
  //提交事务
  void commit(TransactionStatus status);
  //回滚事务
  void rollback(TransactionStatus status);
  ```

* 继承结构

  * JtaTransactionManager: 适用于Spring Data JPA技术.
  * DataSourceTransactionManager: 针对`mybatis`或`JdbcTemplate`, 推荐
  * HibernateTransactionManager: 针对`hibernate框架`

### 12-TransactionDefinition概述

* 概述

  * 对Spring事务属性进行设置: `事务传播行为`, `隔离级别`, `超时`, `只读性`

* 常用方法

  ```java
  //获取传播行为
  default int getPropagationBehavior() {
  	return PROPAGATION_REQUIRED;
  }
  //设置传播行为, 根据名称: "PROPAGATION_REQUIRED"
  void setPropagationBehaviorName(String constantName);
  //设置传播行为, 根据数值: 0
  void setPropagationBehavior(int propagationBehavior);
  
  //获取隔离级别
  default int getIsolationLevel() {
      return ISOLATION_DEFAULT;
  }
  //设置隔离级别, 根据名称: ISOLATION_REPEATABLE_READ
  void setIsolationLevelName(String constantName);
  //设置隔离级别, 根据数值: 4
  void setIsolationLevel(int isolationLevel);
  
  //获取超时
  default int getTimeout() {
      return TIMEOUT_DEFAULT;
  }
  //设置超时
  void setTimeout(int timeout);
  
  //获取只读性
  default boolean isReadOnly() {
      return false;
  }
  //设置只读性
  void setReadOnly(boolean readOnly);
  ```
  
* 继承结构

  * DefaultTransactionDefinition
    * DefaultTransactionAttribute

### 13-TransactionStatus概述

* 概述
  * 事务状态类
  * 由Spring事务代码内部使用.
* 继承结构
  * DefaultTransactionStatus

### 14-Spring事环境搭建

* 需求

  * 完成转账功能

* 开发步骤

  * ①定义service代码
  * ②定义dao代码
  * ③代码测试

* ①定义service代码

  ```java
  @Service
  public class UserServiceImpl implements UserService {
  
      @Autowired
      private UserMapper userMapper;
  
      @Override
      public void transfer(String outName, String inName, Integer money) throws Exception {
          //出账
          userMapper.outMoney(outName, money);
  
          System.out.println(1 / 0);
          //入账
          userMapper.inMoney(inName, money);
      }
  
      @Override
      public int insert(User user) {
          return userMapper.insert(user);
      }
  
      @Override
      public int delete(Integer id) {
          return userMapper.delete(id);
      }
  
      @Override
      public int update(User user) {
          return userMapper.update(user);
      }
  
      @Override
      public User getById(Integer id) {
          return userMapper.getById(id);
      }
  
      @Override
      public List<User> findAll() {
          return userMapper.findAll();
      }
  }
  ```

* ②定义dao代码

  ```java
  @Override
  public void outMoney(String outName, Integer money) {
      jdbcTemplate.update(
              "update t_user set money = money - ? where user_name = ?",
              money,
              outName
      );
  }
  
  @Override
  public void inMoney(String inName, Integer money) {
      jdbcTemplate.update(
              "update t_user set money = money + ? where user_name = ?",
              money,
              inName
      );
  }
  ```

* ③代码测试

  ```java
  @Test
  public void transfer() throws Exception {
      userService.transfer("张三", "李四" , 100);
  }
  ```

### 15-Spring编程式事务基础版

* 概述

  * 开发人员手动调用`PlatformTransactionManager`, `TransactionDefinition`, `TransactionStatus`进行事务管理.

* 代码实现

  ```java
  @Service
  public class UserServiceImpl implements UserService {
  
      @Autowired
      private UserMapper userMapper;
  
      @Autowired
      private DataSourceTransactionManager transactionManager;
      @Autowired
      @Qualifier("definition1")
      private TransactionDefinition definition1;
  
      @Override
      public void transfer(String outName, String inName, Integer money) {
          TransactionStatus transactionStatus = null;
          try {
              //开启事务
              transactionStatus = transactionManager.getTransaction(definition1);
              //出账
              userMapper.outMoney(outName, money);
              //System.out.println(1 / 0);
              //入账
              userMapper.inMoney(inName, money);
              //没有异常, 提交事务
              transactionManager.commit(transactionStatus);
          } catch (Exception e) {
              //有异常, 回滚事务
              e.printStackTrace();
              transactionManager.rollback(transactionStatus);
          }
      }
  
      @Override
      public int insert(User user) {
          return userMapper.insert(user);
      }
  
      @Override
      public int delete(Integer id) {
          return userMapper.delete(id);
      }
  
      @Override
      public int update(User user) {
          return userMapper.update(user);
      }
  
      @Override
      public User getById(Integer id) {
          return userMapper.getById(id);
      }
  
      @Override
      public List<User> findAll() {
          return userMapper.findAll();
      }
  }
  ```

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
      <context:component-scan base-package="com.qh"></context:component-scan>
  
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
  
  
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
      <!--针对写操作-->
      <bean id="definition1" class="org.springframework.transaction.support.DefaultTransactionDefinition">
          <property name="propagationBehaviorName" value="PROPAGATION_REQUIRED"></property>
          <property name="isolationLevelName" value="ISOLATION_REPEATABLE_READ"></property>
          <property name="timeout" value="30"></property>
          <property name="readOnly" value="false"></property>
      </bean>
  
      <!--针对读操作-->
      <bean id="definition2" class="org.springframework.transaction.support.DefaultTransactionDefinition">
          <property name="propagationBehaviorName" value="PROPAGATION_REQUIRED"></property>
          <property name="isolationLevelName" value="ISOLATION_REPEATABLE_READ"></property>
          <property name="timeout" value="30"></property>
          <property name="readOnly" value="true"></property>
      </bean>
  
  
  </beans>
  ```
  
* 存在的问题
  * 在业务方法中会重复写事务管理代码, 可以将事务管理代码抽取成工具类.

### 16-Spring编程式事务优化版

* 代码实现

  ```java
  /**
   * 开启事务
   * 提交事务
   * 回滚事务
   */
  @Component
  public class MyTransactionManager {
  
      @Autowired
      private DataSourceTransactionManager transactionManager;
      @Autowired
      @Qualifier("definition1")
      private DefaultTransactionDefinition definition1;
  
      @Autowired
      @Qualifier("definition2")
      private DefaultTransactionDefinition definition2;
  
      /**
       * 开启事务, 针对写操作
       */
      public TransactionStatus writeStartTransaction() {
          return transactionManager.getTransaction(definition1);
      }
  
      /**
       * 开启事务, 针对读操作
       */
      public TransactionStatus readStartTransaction() {
          return transactionManager.getTransaction(definition2);
      }
  
  
      /**
       * 提交事务
       * @param status
       */
      public void commit(TransactionStatus status) {
          transactionManager.commit(status);
      }
  
      /**
       * 回滚事务
       * @param status
       */
      public void rollback(TransactionStatus status) {
          transactionManager.rollback(status);
      }
  
  }
  ```
  
  ```java
  @Service
  public class UserServiceImpl implements UserService {
  
      @Autowired
      private UserMapper userMapper;
      @Autowired
      private MyTransactionManager myTransactionManager;
  
      @Override
      public void transfer(String outName, String inName, Integer money) {
          TransactionStatus transactionStatus = null;
          try {
              //开启事务
              transactionStatus = myTransactionManager.writeStartTransaction();
              //出账
              userMapper.outMoney(outName, money);
              //System.out.println(1 / 0);
              //入账
              userMapper.inMoney(inName, money);
              //没有异常, 提交事务
              myTransactionManager.commit(transactionStatus);
          } catch (Exception e) {
              //有异常, 回滚事务
              e.printStackTrace();
              myTransactionManager.rollback(transactionStatus);
          }
      }
  
      @Override
      public User getById(Integer id) {
          TransactionStatus transactionStatus = null;
          try {
              //开启事务
              transactionStatus = myTransactionManager.readStartTransaction();
              User user = userMapper.getById(id);
              //没有异常, 提交事务
              myTransactionManager.commit(transactionStatus);
              return user;
          } catch (Exception e) {
              e.printStackTrace();
              //有异常, 回滚事务
              myTransactionManager.rollback(transactionStatus);
  
          }
          return null;
      }
  
      @Override
      public int insert(User user) {
  
          return userMapper.insert(user);
      }
  
      @Override
      public int delete(Integer id) {
          return userMapper.delete(id);
      }
  
      @Override
      public int update(User user) {
          return userMapper.update(user);
      }
  
  
      @Override
      public List<User> findAll() {
          return userMapper.findAll();
      }
  }
  ```
  
* 存在问题
  * ①UserServiceImpl不满足单一职责原则
  * ②事务管理代码(`辅助功能`)维护起来比较麻烦.

### 17-Spring编程式事务终极版

* 概述
  * 将事务管理代码(`辅助功能`)放入到AOP通知
    * **方案一: 环绕通知, 推荐**
    * 方案二: before通知中开启事务, afterReturning提交事务, afterThrowing回滚事务

* 代码实现1: xml配置

  ```java
  @Component
  public class TxAdvice {
  
      @Autowired
      private MyTransactionManager myTransactionManager;
  
      public Object txAround(ProceedingJoinPoint pjp) {
          TransactionStatus status = null;
          try {
              //获取方法名
              String methodName = pjp.getSignature().getName();
              if (methodName.startsWith("find") || methodName.startsWith("get")) {
                  //开启事务, 针对读操作
                  status = myTransactionManager.readStartTransaction();
              } else {
                  //开启事务, 针对写操作
                  status = myTransactionManager.writeStartTransaction();
              }
  
              Object result = pjp.proceed();
              //没有异常, 提交事务
              myTransactionManager.commit(status);
              return result;
          } catch (Throwable e) {
              e.printStackTrace();
              myTransactionManager.rollback(status);
              throw new RuntimeException(e);
          }
      }
  
  }
  ```

  ```xml
  <aop:config>
      <aop:aspect ref="txAdvice">
          <aop:pointcut id="p1" expression="execution(* *..*Service.*(..))"/>
          <aop:around method="txAround" pointcut-ref="p1"></aop:around>
      </aop:aspect>
  </aop:config>
  ```

* 代码实现2: 注解配置

  ```xml
  <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
  ```

  ```java
  @Aspect
  @Component
  public class TxAdvice {
  
      @Autowired
      private MyTransactionManager myTransactionManager;
  
      @Around("execution(* *..*Service.*(..))")
      public Object txAround(ProceedingJoinPoint pjp) {
          TransactionStatus status = null;
          try {
              //获取方法名
              String methodName = pjp.getSignature().getName();
              if (methodName.startsWith("find") || methodName.startsWith("get")) {
                  //开启事务, 针对读操作
                  status = myTransactionManager.readStartTransaction();
              } else {
                  //开启事务, 针对写操作
                  status = myTransactionManager.writeStartTransaction();
              }
  
              Object result = pjp.proceed();
              //没有异常, 提交事务
              myTransactionManager.commit(status);
              return result;
          } catch (Throwable e) {
              e.printStackTrace();
              myTransactionManager.rollback(status);
              throw new RuntimeException(e);
          }
      }
  
  }
  ```

### 18-Spring声明式事务概述

* 概述
  * 将编程式事务中的通用代码抽取出来，制作成独立的around通知使用AOP工作原理，将事务管理的代码动态 织入到原始方法中。
  * 由于该功能使用量较大，Spring已经将该通知制作完毕。 开发人员只需要通过xml配置或注解配置的方式进行使用即可
* 分类
  * xml声明式事务
  * 注解声明式事务

### 19-xml声明式事务

* 概述

  * 通过xml文件配置Spring内置的事务通知类.

* 开发步骤

  * ①编写spring-core.xml
    * 开启tx空间
    * 配置事务通知类
  * ②代码测试

* ①编写spring-core.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xsi:schemaLocation="
         http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
         http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
  
  
      <context:component-scan base-package="com.qh"></context:component-scan>
  
  
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
  
  
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
      <!--配置事务通知类-->
      <tx:advice transaction-manager="transactionManager" id="txAdvice">
          <tx:attributes>
              <tx:method name="transfer"
                         isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
              <tx:method name="getById"
                         isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="true"/>
          </tx:attributes>
      </tx:advice>
  
      <!--AOP配置-->
      <aop:config>
          <aop:advisor advice-ref="txAdvice" pointcut="execution(* *..*Service.*(..))"></aop:advisor>
      </aop:config>
  
  </beans>
  ```
  

### 20-xml声明式事务配置说明

* 代码实现1: 挨个配置每个方法

  ```xml
  <tx:advice transaction-manager="transactionManager" id="txAdvice">
      <tx:attributes>
          <tx:method name="transfer"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="insert"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="delete"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="update"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="getById"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="true"/>
          <tx:method name="findAll"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="true"/>
      </tx:attributes>
  </tx:advice>
  ```

* 代码实现2: 将方法分类

  ```xml
  <tx:advice transaction-manager="transactionManager" id="txAdvice">
      <tx:attributes>
          <tx:method name="transfer"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="insert*"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="delete*"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="update*"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="false"/>
          <tx:method name="get*"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="true"/>
          <tx:method name="find*"
                     isolation="REPEATABLE_READ" propagation="REQUIRED" timeout="30" read-only="true"/>
      </tx:attributes>
  </tx:advice>
  ```

### 21-注解声明式事务

* 开发步骤

  * ①编写spring-core.xml, 开启事务注解支持
  * ②修改业务类, 使用@Transactional注解

* ①编写spring-core.xml, 开启事务注解支持

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xsi:schemaLocation="
         http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
         http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
  
      <context:component-scan base-package="com.qh"></context:component-scan>
  
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
  
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
      <!--开启事务注解支持-->
      <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
  
  </beans>
  ```
  
* ②修改业务类, 使用@Transactional注解

  ```java
  @Transactional(readOnly = true)
  @Service
  public class UserServiceImpl implements UserService {
  
      @Autowired
      private UserMapper userMapper;
  
      @Transactional(readOnly = false)
      @Override
      public void transfer(String outName, String inName, Integer money) {
          TransactionStatus transactionStatus = null;
          try {
              //出账
              userMapper.outMoney(outName, money);
              System.out.println(1 / 0);
              //入账
              userMapper.inMoney(inName, money);
          } catch (Exception e) {
              //有异常, 回滚事务
              e.printStackTrace();
              throw new RuntimeException(e);
          }
      }
  
      @Override
      public List<User> findAll() {
          return userMapper.findAll();
      }
      @Override
      public User getById(Integer id) {
          TransactionStatus transactionStatus = null;
          try {
              User user = userMapper.getById(id);
              return user;
          } catch (Exception e) {
              e.printStackTrace();
          }
          return null;
      }
  
      @Transactional(readOnly = false)
      @Override
      public int insert(User user) {
  
          return userMapper.insert(user);
      }
  
      @Transactional(readOnly = false)
      @Override
      public int delete(Integer id) {
          return userMapper.delete(id);
      }
  
      @Transactional(readOnly = false)
      @Override
      public int update(User user) {
          return userMapper.update(user);
      }
  
  }
  ```

### 22-事务的readOnly属性

* 概述

  * 对一个查询操作来说，如果我们把它设置成只读，就能够明确告诉数据库，这个操作不涉及写操作。 这样数据库就能够针对查询操作来进行优化。

* 代码实现

  ```java
  @Transactional(readOnly = true)
  @Override
  public User getById(Integer id) {
      TransactionStatus transactionStatus = null;
      try {
          User user = userMapper.getById(id);
          return user;
      } catch (Exception e) {
          e.printStackTrace();
      }
      return null;
  }
  
  @Transactional(readOnly = false)
  @Override
  public int insert(User user) {
  
      return userMapper.insert(user);
  }
  ```

* 注意事项

  * ①写操作, 将readOnly=true, 会报错` Connection is read-only. Queries leading to data modification are not allowed`;
  * ②读操作, 将readOnly=false, 不会报错.
  * 建议, 读操作(readOnly=true), 写操作(readOnly=false)

### 23-事务的timeout属性

* 概述

  * 事务在执行过程中，有可能因为遇到某些问题，导致程序卡住，从而长时间占用数据库资源。而长时 间占用资源，大概率是因为程序运行出现了问题。 此时这个很可能出问题的程序应该被回滚，撤销它已做的操作，事务结束，把资源让出来，让其他正 常程序可以执行。 总结：超时回滚，释放资源。

* 代码实现

  ```java
  @Transactional(timeout = 5)
  @Override
  public int insert(User user) throws InterruptedException {
      //数据库操作完成之前, 可以触发timeout属性
      //Thread.sleep(6000);
      int insert = userMapper.insert(user);
      //数据库操作完成之后, 不会触发timeout属性
      //Thread.sleep(6000);
      return insert;
  }
  ```

* 注意事项

  * 如果超时操作在数据库操作完成之后, 不会触发timeout属性

### 24-事务的回滚和不回滚的异常

* 概述

  * 默认情况，遇到运行时异常回滚，遇到编译期异常不回滚。
  * rollbackFor设置需要回滚的异常, noRollbackFor设置不需要回滚的异常

* 代码实现

  ```java
  @Transactional(rollbackFor = Exception.class, noRollbackFor = ArithmeticException.class)
  @Override
  public int insert(User user) throws FileNotFoundException {
      int insert = userMapper.insert(user);
      //产生运行时异常, 可以回滚
      System.out.println(1 / 0);
      //产生编译期异常, 不可以回滚
      //new FileInputStream("");
      return insert;
  }
  ```


### 25-事务的隔离级别

* 隔离级别
  * read uncommitted: 读未提交
    * 脏读, 不可重复读, 幻读
  * read committed: 读已提交
    * 不可重复读, 幻读
  * repeatable read: 可重复读
    * 幻读
  * serializable: 串行化
    * 效率非常低

* 问题解释

  * 脏读: 一个事务可以读取到另一个事务中未提交的数据
  * 不可重复读: 一个事务读取另一个事务中的数据前后不一致
  * 幻读: 一个事务A查询另一个事务B中未提交的记录(id=250)显示不存在, 当事务A添加该记录(id=250)显示已存在

* 概述

  * Spring使用了mysql默认的隔离级别: `repeatable read`

* 代码实现

  ```JAVA
  @Transactional(isolation = Isolation.REPEATABLE_READ)
  @Override
  public int insert(User user) throws FileNotFoundException {
      int insert = userMapper.insert(user);
      return insert;
  }
  
  @Transactional(isolation = Isolation.REPEATABLE_READ)
  @Override
  public List<User> findAll() {
      List<User> userList = userMapper.findAll();
      userList = userMapper.findAll();
      return userList;
  }
  ```
  

### 26-事务传播行为概述

* 概述

  * ![gh45h5e6hujrtsdftbhsar](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gh45h5e6hujrtsdftbhsar.1tdekgjsgqcg.webp)

  * UserServiceImpl类中的transfer方法调用LogServiceImpl类中的addLog方法 
  * 可以将transfer方法中的事务看作事务管理员 
  * 可以将addLog方法中的事务看作事务协调员

* 传播行为

  * 事务管理员将事务传递给事务协调员的策略

* 分类

  * ![efgaegw4sbrdfgtghaqrhaq](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/efgaegw4sbrdfgtghaqrhaq.3ujm1dvgkps0.webp)

  * REQUIRED: 协调员必须有事务
    * 管理员有事务T1, 协调员`加入T1`
    * 管理员没有事务, 协调员`新建T2`
  * REQUIRES_NEW: 协调员必须有新事务
    * 管理员有事务T1, 协调员`新建T2`
    * 管理员没有事务, 协调员`新建T2`
  * SUPPORTS: 协调员和管理员一致
    * 管理员有事务T1, 协调员`加入T1`
    * 管理员没有事务, 协调员`没有事务`
  * NOT_SUPPORTED: 协调员不支持事务
    * 管理员有事务T1, 协调员`没有事务`
    * 管理员没有事务, 协调员`没有事务`
  * MANDATORY: 协调员强制管理员有事务
    * 管理员有事务T1, 协调员`加入T1`
    * 管理员没有事务, 协调员`报错`
  * NEVER: 协调员强制管理员没有事务
    * 管理员有事务T1, 协调员`报错`
    * 管理员没有事务, 协调员`没有事务`

### 27-事务传播行为

* 应用场景
  * 操作用户不应该影响日志的记录.

* 代码实现

  ```java
  @Service
  public class UserServiceImpl implements UserService {
  
      @Autowired
      private UserMapper userMapper;
  
      @Autowired
      private LogService logService;
  
      //事务管理员
      @Transactional
      @Override
      public int insert(User user) throws Exception {
          logService.insert(new Log(null,
                  "UserServiceImpl insert",
                  new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date())));
          int insert = userMapper.insert(user);
          //System.out.println(1 / 0);
          return insert;
      }
  }
  ```

  ```java
  @Service
  public class LogServiceImpl implements LogService {
  
      @Autowired
      private LogMapper logMapper;
  
      //事务协调员
      @Transactional(propagation = Propagation.NEVER)
      @Override
      public void insert(Log log) throws Exception {
  
          logMapper.insert(log);
         // System.out.println(1 / 0);
      }
  }
  ```
  
* 注意事项

  * 传播行为应该在`事务协调员`中设置.