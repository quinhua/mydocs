### 01-注解整合junit

* 开发步骤

  * ①引入依赖
    * spring-test
  * ②修改单元测试类
    * 使用@RunWith, @ContextConfiguration

* 代码实现1: 之前

  ```
  //之前
  public class UserServiceTest2 {
  
      @Test
      public void findAll() throws Exception {
          //加载Spring容器
          BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core.xml");
          //获取UserService对象
          UserService userService = beanFactory.getBean(UserService.class);
          userService.findAll();
  
      }
  }
  ```

* 代码实现2: 现在

  ```java
  //现在
  @RunWith(SpringJUnit4ClassRunner.class)//当前单元测试类运行在Spring容器中
  @ContextConfiguration(locations = "classpath:spring-core.xml")//根据spring-core.xml加载得到Spring容器
  public class UserServiceTest {
  
      @Autowired
      private UserService userService;
  
      @Test
      public void findAll() throws Exception {
          //获取UserService对象
          userService.findAll();
      }
  }
  ```
  
* 注意事项
  * 先加载Spring容器, 再注入资源

### 02-新注解说明

* ①@Configuration
  * 标记当前类是一个Spring配置类
  * 相当于`spring-core.xml`
* ②@ComponentScan
  * 扫描注解
  * 相当于`<context:component-scan base-package="com.qh"></context:component-scan>`
* ③@Bean
  * 将对象存入到Spring容器(IOC)
  * 相当于`<bean id="" class="">`
* ④@PropertySource
  * 加载properties配置文件
  * 相当于`<context:property-placeholder location="">`
* ⑤@Import
  * 导入其他的spring注解配置类
  * 相当于`<import resource="">`

### 03-纯注解开发版本一

* 需求
  * 自定义类使用注解配置, 第三方类使用xml配置 

* 开发步骤

  * ①定义service代码
  * ②定义dao代码
  * ③编写spring-service.xml, spring-persist.xml
  * ④编写jdbc.properties
  * ⑤代码测试

* ①定义service代码

  ```java
  @Service
  public class UserServiceImpl implements UserService {
  
      @Autowired
      private UserDao userDao;
  
      public List<User> findAll() throws Exception {
          return userDao.findAll();
      }
  
  }
  ```

* ②定义dao代码

  ```java
  @Repository
  public class UserDaoImpl implements UserDao {
  
      @Autowired
      private QueryRunner queryRunner;
  
      public List<User> findAll() throws Exception {
          return queryRunner.query(
                  "select user_id userId , user_name userName, user_pwd userPwd from t_user",
                  new BeanListHandler<User>(User.class)
          );
      }
  }
  ```
  
* ③编写spring-core.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
      <context:component-scan base-package="com.qh"></context:component-scan>
  
      <import resource="spring-persist.xml"></import>
  
  </beans>
  ```
  
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
  
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
      
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
  
      <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
          <constructor-arg name="ds" ref="dataSource"></constructor-arg>
      </bean>
  
  </beans>
  ```
  
* ④编写jdbc.properties

  ```properties
  driverClass=com.mysql.cj.jdbc.Driver
  jdbcUrl=jdbc:mysql://localhost:3306/mydb?useSSL=false
  user=root
  password=root
  ```

* ⑤代码测试

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
      @ContextConfiguration(locations = {"classpath:spring-core.xml"})
  public class UserServiceTest {
  
      @Autowired
      private UserService userService;
  
      @Test
      public void findAll() throws Exception {
          //创建Spring容器
          List<User> userList = userService.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```

### 04-纯注解开发版本二

* 需求

  * 将所有的xml配置改为注解配置.

* 存在的问题

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
  	<!--①扫描注解-->
      <context:component-scan base-package="com.qh"></context:component-scan>
  
      <!--②加载jdbc.properties-->
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
      
      <!--③创建DruidDataSource并存入IOC容器-->
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${driverClass}"></property>
          <property name="url" value="${jdbcUrl}"></property>
          <property name="username" value="${user}"></property>
          <property name="password" value="${password}"></property>
      </bean>
      
   	<!--④创建QueryRunner对象并存入IOC容器-->
      <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
          <constructor-arg name="ds" ref="dataSource"></constructor-arg>
      </bean>
  
  </beans>
  ```

* 开发步骤
  * ①定义SpringServiceConfiguration, SpringPersistConfiguration
  * ②修改UserServiceTest

* ①定义SpringServiceConfiguration, SpringPersistConfiguration

  ```java
  @Configuration
  @ComponentScan("com.qh")
  @Import(SpringPersistConfiguration.class)
  public class SpringServiceConfiguration {
  
  }
  ```
  
  ```java
  @Configuration
  @PropertySource("jdbc.properties")
  public class SpringPersistConfiguration {
  
      @Value("${driverClass}")
      private String driverClassName;
      @Value("${jdbcUrl}")
      private String url;
      @Value("${user}")
      private String username;
      @Value("${password}")
      private String password;
  
      @Bean
      public DataSource getDataSource() {
          DruidDataSource dataSource = new DruidDataSource();
          dataSource.setDriverClassName(driverClassName);
          dataSource.setUrl(url);
          dataSource.setUsername(username);
          dataSource.setPassword(password);
          return dataSource;
      }
  
      @Bean
      public QueryRunner getQueryRunner(DataSource dataSource) {
          return new QueryRunner(dataSource);
      }
  
  }
  ```
  
* ②修改UserServiceTest

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(classes = {SpringServiceConfiguration.class})
  public class UserServiceTest {
  
      @Autowired
      private UserService userService;
  
      @Test
      public void findAll() throws Exception {
          //创建Spring容器
          List<User> userList = userService.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```

### 05-注解开发的作用和弊端

* 作用
  * ![hj5redu5h46ej74wh3yu6](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hj5redu5h46ej74wh3yu6.5nb8mryu4ug0.webp)

* 总结
  * 怎么简单就怎么写: 自定义类用注解配置, 第三方类用xml配置

### 06-AOP概述

* 概述
  * AOP: Aspect Oriented Programming : 面向切面编程。
  * ![gaew345yhje5ty6agwe4h](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gaew345yhje5ty6agwe4h.7k15bojfdw00.webp)

* 核心思想
  * AOP关注的是程序中的共性功能，开发时，将共性功能抽取出来制作成独立的功能模块，此时原始功能中将不具有 这些被抽取出的共性功能代码。在被抽取的共性功能的模块运行时候，将共性功能模块也运行，即可完成原始的功 能。
* 作用
  * 在程序运行期间，不修改源码, 对已有方法进行增强。
* 好处
  * 减少重复代码, 提高开发效率, 维护方便
* 实现方式
  * ①使用设计模式
    * 装饰者设计模式
    * 静态代理设计模式
  * **②使用反射技术**
    * jdk自带动态代理技术: JDKProxy
    * spring自带动态代理技术: cglib

### 07-AOP原理环境搭建

* 需求

  * 对用户进行增删改，在insert、delete方法中增加权限校验、日志记录辅助功能

* 代码实现

  ```java
  public interface UserService {
  
      int insert(User user) throws Exception;
  
      int delete(Integer id) throws Exception;
  
      int update(User user) throws Exception;
  
      User getById(Integer id) throws Exception;
  
      List<User> findAll() throws Exception;
  
  }
  ```

  ```java
  /**
   * 主要功能: insert, delete, update, getById, findAll
   * 辅助功能: 权限校验, 日志记录
   */
  public class UserServiceImpl implements UserService {
      public int insert(User user) throws Exception {
          System.out.println("权限校验");
          System.out.println("UserServiceImpl insert user : " + user);
          System.out.println("日志记录");
          return 0;
      }
  
      public int delete(Integer id) throws Exception {
          System.out.println("权限校验");
          System.out.println("UserServiceImpl delete id : " + id);
          System.out.println("日志记录");
          return 0;
      }
  
      public int update(User user) throws Exception {
          System.out.println("UserServiceImpl update user : " + user);
  
          return 0;
      }
  
      public User getById(Integer id) throws Exception {
          System.out.println("UserServiceImpl getById id : " + id);
          return null;
      }
  
      public List<User> findAll() throws Exception {
          System.out.println("UserServiceImpl findAll");
          return null;
      }
  }
  ```

* 存在的问题

  * ①UserService不满足单一职责原则, 主要功能和辅助功能之间耦合度较高
  * ②辅助功能的维护非常麻烦, 如果要修改辅助功能, 变动地方太多了.


### 08-动态代理之JDKProxy

* 解决方案
  * 将辅助功能(权限校验, 日志记录)从UserService中彻底删除.

* 工作流程
  * ![fahue546q3tbhwsj5e76](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fahue546q3tbhwsj5e76.4amkwcudyvi0.webp)

* 代码实现

  ```java
  /**
   * 工具类
   * 获取代理类对象
   */
  public class JDKProxyUtil {
      /**
       * @param instance 被代理类
       * @param flag 是否代理
       * @param <T> 返回被代理类的原方法执行之后的结果
       * @return
       */
      public static <T> T getJDKProxyUtils(final T instance, boolean flag) {
          /**
           * ClassLoader loader: 被代理类的类加载器
           * Class<?>[] interfaces: 被代理类所实现的所有接口
           * InvocationHandler h: 生成代理类方法
           */
          Object proxyInstance = Proxy.newProxyInstance(
                  instance.getClass().getClassLoader(),
                  instance.getClass().getInterfaces(),
                  new InvocationHandler() {
                      /**
                       * @param proxy 代理对象
                       * @param method  被代理类的方法
                       * @param args 被代理类方法的实际参数
                       * @return Object 返回被代理类的原方法执行之后的结果
                       * @throws Throwable
                       */
                      @Override
                      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                          Object result = null;
                          if (flag) {
                              System.out.println("before");
                              //执行被代理类的原方法, 并返回结果result
                              result = method.invoke(instance, args);
                              System.out.println("after");
                          } else {
                              result = method.invoke(instance, args);
                          }
                          return result;
                      }
                  }
          );
          return (T) proxyInstance;
      }
  
  }
  ```

* 代码测试

  ```java
  public class UserServiceTest {
  
      @Test
      public void insert() throws Exception {
          //获取被代理类对象
          UserService userService = new UserServiceImpl();
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = JDKProxyUtil.getJDKProxyUtils(userService,true);
          //使用代理类对象运行
          int insert = userServiceProxy.insert(new User(1, "邓昊"));
          System.out.println("insert = " + insert);
      }
  
      @Test
      public void delete() throws Exception {
          //获取被代理类对象
          UserService userService = new UserServiceImpl();
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = JDKProxyUtil.getJDKProxyUtils(userService,false);
          //使用代理类对象运行
          int delete = userServiceProxy.delete(1);
          System.out.println("delete = " + delete);
      }
  
      @Test
      public void update() throws Exception {
          //获取被代理类对象
          UserService userService = new UserServiceImpl();
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = JDKProxyUtil.getJDKProxyUtils(userService,true);
          //使用代理类对象运行
          int update = userServiceProxy.update(new User(1, "张龙飞"));
          System.out.println("update = " + update);
      }
  
      @Test
      public void getById() throws Exception {
          //获取被代理类对象
          UserService userService = new UserServiceImpl();
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = JDKProxyUtil.getJDKProxyUtils(userService,false);
          //使用代理类对象运行
          User user = userServiceProxy.getById(1);
          System.out.println("user = " + user);
      }
  
      @Test
      public void findAll() throws Exception {
          //获取被代理类对象
          UserService userService = new UserServiceImpl();
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = JDKProxyUtil.getJDKProxyUtils(userService,true);
          //使用代理类对象运行
          List<User> userList = userServiceProxy.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```

* 注意事项
  * 被代理类必须实现接口

### 09-动态代理之CGLIB

* 概述
  * JDKProxy只能针对有接口的类进行动态代理, 如果一个类没有实现接口就可以使用CGLIB进行动态代理.
  * 属于Spring框架自带的动态代理技术
  
* 工作流程
  * ![afyh4we56q3jiksbhag](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/afyh4we56q3jiksbhag.4ownsrq7qa00.webp)****

* 代码实现

  ```java
  /**
   * 工具类
   * 获取UserServiceImpl的代理对象
   */
  public class CGLBProxyUtil {
      //获取CGLB动态代理对象-Enhancer-继承
  
      /**
       *
       * @param clazz 被代理类的字节码对象
       * @param flag 是否被代理
       * @return <T> 返回被代理类的原方法执行后的结果
       * @param <T>
       */
      public static <T> T getCGLBProxyUtil(Class<T> clazz, boolean flag) {
          //代理类继承于被代理类
          Enhancer enhancer = new Enhancer();
          enhancer.setSuperclass(clazz);
          //拦截被代理类中每一个方法,并生成代理类的方法
          enhancer.setCallback(new MethodInterceptor() {
              /**
               * @param obj 代理类对象
               * @param method 被代理类的原方法
               * @param args 被代理类方法的实际参数
               * @param proxy 代理类的方法
               * @return 返回被代理类的原方法执行之后的结果
               * @throws Throwable
               */
              @Override
              public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
                  Object result = null;
                  if (flag) {
                      System.out.println("before");
                      //被代理类对象->执行代理类的方法,代理类对象->代理类的方法
                      //method.invoke(clazz.newInstance(),args);//创建了被代理类对象,不推荐
                      result = proxy.invokeSuper(obj, args);
                      System.out.println("after");
                  } else {
                      result = proxy.invokeSuper(obj, args);
                  }
                  return result;
              }
          });
          Object proxyInstance = enhancer.create();
          return (T) proxyInstance;
      }
  
  }
  ```

* 代码测试

  ```java
  //cglib测试
  public class UserServiceTest3 {
  
      @Test
      public void insert() throws Exception {
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = CGLBProxyUtil.getCGLBProxyUtil(UserServiceImpl.class,true);
          //使用代理类对象运行
          int insert = userServiceProxy.insert(new User(1, "邓昊"));
          System.out.println("insert = " + insert);
      }
  
      @Test
      public void delete() throws Exception {
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = CGLBProxyUtil.getCGLBProxyUtil(UserServiceImpl.class,false);
          //使用代理类对象运行
          int delete = userServiceProxy.delete(1);
          System.out.println("delete = " + delete);
      }
  
      @Test
      public void update() throws Exception {
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = CGLBProxyUtil.getCGLBProxyUtil(UserServiceImpl.class,true);
          //使用代理类对象运行
          int update = userServiceProxy.update(new User(1, "张龙飞"));
          System.out.println("update = " + update);
      }
  
      @Test
      public void getById() throws Exception {
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = CGLBProxyUtil.getCGLBProxyUtil(UserServiceImpl.class,true);
          //使用代理类对象运行
          User user = userServiceProxy.getById(1);
          System.out.println("user = " + user);
      }
  
      @Test
      public void findAll() throws Exception {
          //根据被代理类对象获取代理类对象
          UserService userServiceProxy = CGLBProxyUtil.getCGLBProxyUtil(UserServiceImpl.class,false);
          List<User> userList = userServiceProxy.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```

### 10-AOP名词解释

* ①链接点
  * 被代理类的所有方法
  * 比如: insert, delete, update, getById, findAll
* ②切入点
  * 被代理类中具有共性功能的方法
  * 比如: insert, delete
* ③通知
  * 将共性功能抽取出来独立维护的方法
  * 比如: 权限校验, 日志记录
* ④切面
  * 通知+切入点
* ⑤目标对象类
  * 包含切入点的类/被代理类
* ⑥通知类
  * 包含通知方法的类
* ⑦织入
  * 使用动态代理技术将通知根据切面关系动态放入到切入点执行的过程

### 11-AOP入门案例

* 概述

  * 如果被代理类有实现接口, 那么使用JDKProxy; 如果没有实现接口, 那么使用CGLIB.
    * JDKProxy: 被代理类和代理类实现同一个接口.
    * CGLIB: 代理类继承被代理类.
    * 最终程序运行的一定是代理类对象.

* 开发步骤

  * ①引入依赖
    * spring-aop, spring-aspect
  * ②制作目标对象类
  * ③制作通知类
  * ④编写spring-core.xml, 配置切面关系
  * ⑤代码测试

* ①引入依赖

  ```xml
  <dependencies>
  
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.18.24</version>
      </dependency>
  
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.13.2</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.24</version>
      </dependency>
      
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-aspects</artifactId>
          <version>5.3.24</version>
      </dependency>
      
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>5.3.24</version>
      </dependency>
  
  </dependencies>
  ```

* ②制作目标对象类

  ```java
  /**
   * 被代理类/目标对象类
   */
  @Service
  public class UserServiceImpl implements UserService {
      public int insert(User user) throws Exception {
          System.out.println("UserServiceImpl insert user : " + user);
          return 0;
      }
  
      public int delete(Integer id) throws Exception {
          System.out.println("UserServiceImpl delete id : " + id);
          return 0;
      }
  
      public int update(User user) throws Exception {
          System.out.println("UserServiceImpl update user : " + user);
  
          return 0;
      }
  
      public User getById(Integer id) throws Exception {
          System.out.println("UserServiceImpl getById id : " + id);
          return null;
      }
  
      public List<User> findAll() throws Exception {
          System.out.println("UserServiceImpl findAll");
          return null;
      }
  }
  ```

* ③制作通知类

  ```java
  /**
   * 通知类
   */
  @Component
  public class MyAdvice01 {
  
      /**
       * 通知
       */
      public void checkPermission() {
          System.out.println("权限校验");
      }
  
      /**
       * 通知
       */
      public void printLog() {
          System.out.println("日志记录");
      }
  
  }
  ```

* ④编写spring-core.xml, 配置切面关系

  ```xml
  <context:component-scan base-package="com.qh"></context:component-scan>
  
  <aop:config>
      <!--
          ref="myAdvice01": 通知类
      -->
      <aop:aspect ref="myAdvice01">
          <!--
              method="checkPermission": 通知方法
              pointcut: 切入点
          -->
          <aop:before method="checkPermission" pointcut="execution(public int com.qh.service.impl.UserServiceImpl.insert(com.qh.pojo.User)) "></aop:before>
          <aop:after method="printLog" pointcut="execution(public int com.qh.service.impl.UserServiceImpl.insert(com.qh.pojo.User)) "></aop:after>
      </aop:aspect>
  </aop:config>
  ```
  
* ⑤代码测试

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations = {"classpath:spring-core.xml"})
  public class UserServiceTest {
  
      @Autowired
      private UserService userService;
  
      @Test
      public void insert() throws Exception {
          //使用代理类对象运行
          int insert = userService.insert(new User(1, "邓昊"));
          System.out.println("insert = " + insert);
      }
  
      @Test
      public void delete() throws Exception {
          //使用代理类对象运行
          int delete = userService.delete(1);
          System.out.println("delete = " + delete);
      }
  
      @Test
      public void update() throws Exception {
          //使用代理类对象运行
          int update = userService.update(new User(1, "张龙飞"));
          System.out.println("update = " + update);
      }
  
      @Test
      public void getById() throws Exception {
          //使用代理类对象运行
          User user = userService.getById(1);
          System.out.println("user = " + user);
      }
  
      @Test
      public void findAll() throws Exception {
          List<User> userList = userService.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```

### 12-切入点表达式

* 语法

  ```
  execution(权限修饰符 返回值类型 包名.类名.方法名(形参类型1,形参类型2,...))
  ```

* ①标准写法

  ```
  execution(public int com.atguigu.service.impl.UserServiceImpl.insert(com.atguigu.pojo.User))
  ```

  ```
  execution(public int com.atguigu.service.UserService.insert(com.atguigu.pojo.User))
  ```

* ②省略权限修饰符

  ```
  execution( int com.atguigu.service.impl.UserServiceImpl.insert(com.atguigu.pojo.User))
  ```

* ③返回值类型使用通配符`*`

  ```
  execution( * com.atguigu.service.impl.UserServiceImpl.insert(com.atguigu.pojo.User))
  ```

* ④包名使用通配符`*`, 不限制包名

  ```
  execution( * *.*.*.*.UserServiceImpl.insert(com.atguigu.pojo.User))
  ```

* ⑤包名使用通配符`..`, 不限制包层级

  ```
  execution( * *..UserServiceImpl.insert(com.atguigu.pojo.User))
  ```

* ⑥类名使用通配符`*`, 限制在service层

  ```
  execution( * *..*ServiceImpl.insert(com.atguigu.pojo.User))
  ```

* ⑦方法名使用通配符`*`, 不限制方法名称

  ```
  execution( * *..*ServiceImpl.*(com.atguigu.pojo.User))
  ```

* ⑧形参类型使用通配符`*`, 不限制参数类型

  ```
  execution( * *..*ServiceImpl.*(*))
  ```

* ⑨形参类型使用通配符`..`, 不限制参数类型和个数: **推荐写法**

  ```
  execution( * *..*ServiceImpl.*(..))
  ```

* ⑩类名使用通配符`*`, 作用于任何层

  ```
  execution( * *..*.*(..))
  ```


### 13-切入点表达式抽取

* 方式一

  ```xml
  <aop:config>
      <aop:aspect ref="myAdvice01">
          <aop:before method="checkPermission" pointcut="execution( * *..*Service.*(..)) "></aop:before>
          <aop:after method="printLog" pointcut="execution( * *..*Service.*(..)) "></aop:after>
      </aop:aspect>
  </aop:config>
  ```

* 方式二

  ```xml
  <aop:config>
      <aop:aspect ref="myAdvice01">
          <aop:pointcut id="p1" expression="execution( * *..*Service.*(..))"/>
          <aop:before method="checkPermission" pointcut-ref="p1"></aop:before>
          <aop:after method="printLog" pointcut-ref="p1"></aop:after>
      </aop:aspect>
  </aop:config>
  ```

* 方式三

  ```xml
  <aop:config>
      <aop:pointcut id="p1" expression="execution( * *..*Service.*(..))"/>
  
      <aop:aspect ref="myAdvice01">
          <aop:before method="checkPermission" pointcut-ref="p1"></aop:before>
      </aop:aspect>
  
      <aop:aspect ref="myAdvice01">
          <aop:after method="printLog" pointcut-ref="p1"></aop:after>
      </aop:aspect>
  </aop:config>
  ```

### 14-AOP通知类别

* 通知类别

  | 通知         | 说明                                                   |
  | ------------ | ------------------------------------------------------ |
  | 前置通知     | before, 在切入点之前执行                               |
  | 正常后置通知 | afterReturning, 在切入点之后执行, 切入点没有异常才执行 |
  | 异常后置通知 | afterThrowing, 在切入点之后执行, 切入点有异常才执行    |
  | 后置通知     | after, 在切入点之后执行, 不管切入点有没有异常都执行    |
  | 环绕通知     | around, 在切入点之前执行, 在切入点之后执行             |

* 代码实现

  ```java
  @Component
  public class MyAdvice02 {
  
      //前置通知
      public void before() {
          System.out.println("MyAdvice02 before");
      }
  
      //正常后置通知
      public void afterReturning() {
          System.out.println("MyAdvice02 afterReturning");
      }
  
      //异常后置通知
      public void afterThrowing() {
          System.out.println("MyAdvice02 afterThrowing");
      }
  
      //后置通知
      public void after() {
          System.out.println("MyAdvice02 after");
      }
  
      //环绕通知
      //ProceedingJoinPoint pjp: 正在执行的链接点/切入点
      public Object around(ProceedingJoinPoint pjp) {
          System.out.println("MyAdvice02 around 之前");
          //执行切入点(被代理类具有共性功能的方法)
          Object result = null;
          try {
              result = pjp.proceed();
          } catch (Throwable throwable) {
              throwable.printStackTrace();
              throw new RuntimeException(throwable);
          }
          System.out.println("MyAdvice02 around 之后");
          return result;
      }
  
  }
  ```
  
  ```xml
  <aop:config>
  
      <aop:aspect ref="myAdvice02">
          <aop:pointcut id="p1" expression="execution(* *..*Service.*(..))"/>
          <aop:before method="before" pointcut-ref="p1"></aop:before>
          <aop:after-returning method="afterReturning" pointcut-ref="p1"></aop:after-returning>
          <aop:after-throwing method="afterThrowing" pointcut-ref="p1"></aop:after-throwing>
          <aop:after method="after" pointcut-ref="p1"></aop:after>
          <aop:around method="around" pointcut-ref="p1"></aop:around>
      </aop:aspect>
  </aop:config>
  ```
  
* 注意事项
  * ①around环绕通知需要将切入点执行之后的结果继续返回.
  * ②around环绕通知捕获的异常需要继续抛出给下一个通知.

### 15-通知获取切入点输入参数

* 概述

  * `非环绕通知`和`环绕通知`获取切入点输入参数.

* 代码实现

  ```java
  @Component
  public class MyAdvice03 {
  
      //非环绕通知
      public void before(JoinPoint jp) {
          System.out.println("MyAdvice03  before 切入点输入参数 : " + Arrays.toString(jp.getArgs()));
      }
  
      //环绕通知
      public Object around(ProceedingJoinPoint pjp) {
          System.out.println("MyAdvice03  around 切入点输入参数 : " + Arrays.toString(pjp.getArgs()));
          try {
              return pjp.proceed();
          } catch (Throwable throwable) {
              throwable.printStackTrace();
              throw new RuntimeException(throwable);
          }
      }
  
  }
  ```

  