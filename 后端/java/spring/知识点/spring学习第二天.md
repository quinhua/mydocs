### 01-bean生命周期

* 生命周期
  * ![ger45rtywehue5t45wr3](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ger45rtywehue5t45wr3.4eqnnyus40g.webp)

* ①实例化: 执行构造器
* ②属性赋值: 执行set方法
* ③初始化
  * 3.1, 前置处理
  * 3.2, 执行init方法
  * 3.3, 后置处理
* ④销毁
  * 执行destroy方法

### 02-BeanPostProcessor预置处理器

* 概述

  * 在对象的初始化前后做预置处理.

* 语法

  ```java
  public interface BeanPostProcessor {
  
      //前置处理
  	@Nullable
  	default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
  		return bean;
  	}
  
  	//后置处理
  	@Nullable
  	default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
  		return bean;
  	}
  
  }
  ```
  
* 开发步骤

  * ①自定义BeanPostProcessor类实现BeanPostProcessor接口
  * ②编写spring-core.xml, 配置自定义BeanPostProcessor类

* ①自定义BeanPostProcessor类实现BeanPostProcessor接口

  ```java
  public class UserPostProcessor implements BeanPostProcessor {
  
      public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
          //前置处理
          if (bean instanceof User) {
              System.out.println("前置处理");
          }
          return bean;
      }
  
      public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
          //后置处理
          if (bean instanceof User) {
              System.out.println("后置处理");
  
          }
          return bean;
      }
  }
  ```

* ②编写spring-core.xml, 配置自定义BeanPostProcessor类

  ```xml
  <bean id="user"  class="com.qh.pojo.User" init-method="init" destroy-method="destroy"></bean>
  
  <bean id="teacher" class="com.qh.pojo.Teacher" init-method="init" destroy-method="destroy"></bean>
  
  <bean class="com.qh.processor.UserPostProcessor"></bean>
  ```

### 03-bean生命周期练习

* 需求

  * 自定义链接池

* java代码实现

  ```java
  public class DataSource {
  
      private String driverClassName;
      private String jdbcUrl;
      private String username;
      private String password;
      private Integer initPoolSize = 10;//初始连接数
      private LinkedList<Connection> list = new LinkedList<Connection>();//存储Connection的容器
  
      public DataSource() {
          System.out.println("①DataSource实例化");
      }
  
      public String getDriverClassName() {
          return driverClassName;
      }
  
      public void setDriverClassName(String driverClassName) {
          this.driverClassName = driverClassName;
      }
  
      public String getJdbcUrl() {
          return jdbcUrl;
      }
  
      public void setJdbcUrl(String jdbcUrl) {
          this.jdbcUrl = jdbcUrl;
      }
  
      public String getUsername() {
          return username;
      }
  
      public void setUsername(String username) {
          this.username = username;
      }
  
      public String getPassword() {
          return password;
      }
  
      public void setPassword(String password) {
          this.password = password;
      }
  
      public Integer getInitPoolSize() {
          return initPoolSize;
      }
  
      public void setInitPoolSize(Integer initPoolSize) {
          System.out.println("②setInitPoolSize属性赋值");
          this.initPoolSize = initPoolSize;
      }
  
      public void init() throws Exception {
          System.out.println("④init初始化");
          Class.forName(driverClassName);
          for (Integer i = 0; i < initPoolSize; i++) {
              Connection connection = DriverManager.getConnection(jdbcUrl, username, password);
              list.add(connection);
          }
      }
  
      /**
       * 获取链接
       *
       * @return
       */
      public Connection getConnection() {
          return list.removeFirst();
      }
  
      /**
       * 归还链接
       *
       * @param connection
       */
      public void addBack(Connection connection) {
          list.add(connection);
      }
  
      public void destroy() {
          System.out.println("⑥destroy销毁");
          list.clear();
      }
  }
  ```
  
  ```java
  public class DataSourcePostProcessor implements BeanPostProcessor {
  
      public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
          if (bean instanceof DataSource) {
              System.out.println("③OldQiuDataSource前置处理, 修改initPoolSize=5");
              DataSource oldQiuDataSource = (DataSource) bean;
              DataSource.setInitPoolSize(5);
          }
          return bean;
      }
  
      public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
          if (bean instanceof DataSource) {
              DataSource dataSource = (DataSource) bean;
              Integer initPoolSize = dataSource.getInitPoolSize();
              System.out.println("⑤DataSource后置处理, 获取initPoolSize=" + initPoolSize);
  
          }
          return bean;
      }
  }
  ```
  
* 配置文件

  ```xml
  <bean id="DataSource" class="com.qg.datasource.DataSource" init-method="init" destroy-method="destroy">
      <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
      <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/mydb?useSSL=false"></property>
      <property name="username" value="root"></property>
      <property name="password" value="root"></property>
      <property name="initPoolSize" value="20"></property>
  </bean>
  
  <bean class="com.qh.datasource.DataSourcePostProcessor"></bean>
  ```
  
* 代码测试

  ```java
  @Test
  public void test2() {
      ClassPathXmlApplicationContext beanFactory = new ClassPathXmlApplicationContext("spring-core.xml");
      DataSource dataSource = beanFactory.getBean(DataSource.class);
      Connection connection = dataSource.getConnection();
      PreparedStatement statement = null;
      ResultSet resultSet = null;
      try {
          statement = connection.prepareStatement("select * from t_user");
          resultSet = statement.executeQuery();
          List<User> userList = new ArrayList<User>();
          while (resultSet.next()) {
              userList.add(
                      new User(
                              resultSet.getInt(1),
                              resultSet.getString(2)
                      )
              );
          }
          System.out.println("userList = " + userList);
      } catch (Exception e) {
          e.printStackTrace();
      } finally {
          dataSource.addBack(connection);
          if (statement != null) {
              try {
                  statement.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
              statement = null;
          }
  
          if (resultSet != null) {
              try {
                  resultSet.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
              resultSet = null;
          }
  
          beanFactory.close();
      }
  
  }
  ```

### 04-依赖注入DI

* 概述
  * DI: dependency injection , 依赖注入, 将Spring容器中的资源注入到Java程序中使用.
  * DI是IOC的具体体现.
  * IOC 解耦只是降低他们的依赖关系，但不会消除。例如：业务层仍会调用持久层的方法,那这种业务层 和持久层的依赖关系，在使用Spring之后，就让Spring来维护了。简单的说，就是框架把持久层对象 传入业务层，而不用我们自己去获取。
* 依赖注入的类型
  * 简单类型(基本数据类型/包装类/String)
  * 复杂类型(对象类型/集合类型)
* 依赖注入的方式
  * 构造器注入
  * set方法注入
  * p命名空间
  * 注解注入

### 05-构造器注入

* 概述

  * 通过`constructor-arg标签`使用构造器注入.

* 需求

  * ①注入简单类型
  * ②注入对象类型

* 代码实现

  ```xml
  <!--05-构造器注入-->
  <!--①注入简单类型-->
  <bean id="student1" class="com.qh.pojo.Student">
      <constructor-arg name="studentId" value="1"></constructor-arg>
      <constructor-arg name="studentName" value="张三"></constructor-arg>
  </bean>
  
  <!--②注入对象类型-->
  <bean id="student2" class="com.qh.pojo.Student">
      <constructor-arg name="studentId" value="2"></constructor-arg>
      <constructor-arg name="studentName" value="张三"></constructor-arg>
      <constructor-arg name="teacher" ref="teacher1"></constructor-arg>
  </bean>
  
  <bean id="teacher1" class="com.qh.pojo.Teacher">
      <constructor-arg name="teacherId" value="1"></constructor-arg>
      <constructor-arg name="teacherName" value="张三"></constructor-arg>
  </bean>
  ```

* 注意事项

  * 必须要有对应的构造器方法


### 06-set方法注入

* 概述

  * 通过`property`标签使用set方法注入.

* 需求

  * ①注入简单类型
  * ②注入对象类型

* 代码实现

  ```xml
  <bean id="teacher1" class="com.qh.pojo.Teacher">
      <constructor-arg name="teacherId" value="1"></constructor-arg>
      <constructor-arg name="teacherName" value="张三"></constructor-arg>
  </bean>
  
  <!--06-set方法注入-->
  <!--①注入简单类型-->
  <bean id="student3" class="com.qh.pojo.Student">
      <property name="studentId" value="3"></property>
      <property name="studentName" value="张三"></property>
  </bean>
  <!--②注入对象类型-->
  <bean id="student4" class="com.qh.pojo.Student">
      <property name="studentId" value="4"></property>
      <property name="studentName" value="张三"></property>
      <property name="teacher" ref="teacher1"></property>
  </bean>
  ```
  
* 注意事项

  * ①set方法不能私有化, 否则报错`BeanCreationException: Bean property 'studentName' is not writable or has an invalid setter method. `
  * ②set方法不能没有, 否则报错`BeanCreationException: Bean property 'studentName' is not writable or has an invalid setter method. `

* 总结

  * value给简单类型赋值
  * ref给对象类型赋值


### 07-容器注入

* 代码实现

  ```java
  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  public class Bean01 {
      private String[] myStrs;
      private List<String> myList;
      private Set<String> mySet;
      private Map<String, String> myMap;
      private Properties myProps;
  }
  ```

  ```xml
  <bean id="bean01" class="com.qh.pojo.Bean01">
      <property name="myStrs">
          <array>
              <value>a</value>
              <value>b</value>
              <value>c</value>
          </array>
      </property>
      <property name="myList">
          <list>
              <value>a</value>
              <value>b</value>
              <value>c</value>
          </list>
      </property>
      <property name="mySet">
          <set>
              <value>a</value>
              <value>a</value>
              <value>a</value>
              <value>b</value>
              <value>c</value>
          </set>
      </property>
      <property name="myMap">
          <map>
              <entry key="1" value="a"></entry>
              <entry key="2">
                  <value>b</value>
              </entry>
              <entry key="3" value="c">
              </entry>
          </map>
      </property>
      <property name="myProps">
          <props>
              <prop key="1">a</prop>
              <prop key="2">b</prop>
              <prop key="3">c</prop>
          </props>
      </property>
  </bean>
  ```

### 08-p命名空间注入

* 概述

  * 简化依赖注入的代码.

* 语法

  ```xml
  <bean
  	id="唯一标识"
  	class="全限定类名"
  	p:propertyName="propertyValue"
  	p:propertyName-ref="beanId"></bean>
  ```

* 开发步骤

  * ①开启p空间
  * ②使用p空间

* ①开启p空间

  * ![gser567u5w3eujhn](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gser567u5w3eujhn.7krmulg6f8c0.webp)

* ②使用p空间

  ```xml
  <bean id="student5"
        class="com.qh.pojo.Student"
        p:studentId="5"
        p:studentName="张三"
        p:teacher-ref="teacher1"></bean>
  ```

### 09-BeanFactory继承结构

* 继承结构1: uml用例图
  * ![ge45ytyhuehujsuhje56](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ge45ytyhuehujsuhje56.1yv0sf45mb8g.webp)

* 继承结构2
  * ![hyg4weywse6hyweshy](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hyg4weywse6hyweshy.19u9u3a58jds.webp)

### 10-BeanFactory和ApplicationContext的区别

* ①BeanFactory

  * 是Spring的顶层接口
  * 具备Spring框架最基本的功能(IOC,DI)
  * 默认是单例模式, 对象在第一次使用时`创建/初始化`,  在Spring容器关闭时`销毁`

* ②ApplicationContext

  * 是Spring的子接口
  * 具备Spring框架所有的功能(IOC,DI,AOP...)
  * 默认是单例模式, 对象在Spring容器加载时`创建/初始化`, 在Spring容器关闭时`销毁`

* 代码实现

  ```java
  @Test
  public void test1() {
      //加载Spring容器
      XmlBeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource("spring-core3.xml"));
      //第一次使用User, User就创建/初始化
      User user1 = (User) beanFactory.getBean("user1");
      //第二次使用User, 直接使用第一次创建的User对象
      User user2 = (User) beanFactory.getBean("user1");
      //默认是单例
      System.out.println(user1 == user2);
  }
  ```


### 11-ApplicationContext的三个实现子类

* 三个实现子类

  * ①ClasspathXmlApplicationContext
    * 根据类路径下的xml配置文件加载获取Spring容器
  * ②FileSystemXmlApplicationContext
    * 根据磁盘路径下的xml配置文件加载获取Spring容器
  * ③AnnotationConfigApplicationContext
    * 根据注解配置类加载获取Spring容器

* ①ClasspathXmlApplicationContext

* ②FileSystemXmlApplicationContext

  ```java
  @Test
  public void test2(){
      //在项目中
      //绝对路径
      //ApplicationContext applicationContext = new FileSystemXmlApplicationContext("D:\\springTest\\src\\main\\resources\\spring-core3.xml");
      //相对路径
      //ApplicationContext applicationContext = new FileSystemXmlApplicationContext("src\\main\\resources\\spring-core3.xml");
      //不在项目中
      ApplicationContext applicationContext = new FileSystemXmlApplicationContext("C:\\Users\\Desktop\\spring-core3.xml");
  
      Object user = applicationContext.getBean("user1");
      System.out.println("user = " + user);
  }
  ```

* ③AnnotationConfigApplicationContext

  ```java
  @Configuration
  public class SpringConfiguration {
  
      @Bean("user1")//将方法返回值放入到Spring容器中
      public User getUser(){
          return new User(1, "张龙飞");
      }
  
  }
  ```

  ```java
  @Test
  public void test3(){
      ApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfiguration.class);
      Object user1 = applicationContext.getBean("user1");
      System.out.println("user1 = " + user1);
  }
  ```

### 12-Spring操作数据库

* 需求
  * 查询所有用户记录
* 技术栈
  * spring + mysql + druid + dbutils
* 开发步骤
  * ①引入依赖
  * ②编写service代码
  * ③编写dao代码
  * ④编写spring-core.xml
  * ⑤代码测试

* ①引入依赖

  ```xml
  <dependencies>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.24</version>
      </dependency>
  
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.13.2</version>
      </dependency>
  
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.18.24</version>
      </dependency>
  
      <dependency>
          <groupId>commons-dbutils</groupId>
          <artifactId>commons-dbutils</artifactId>
          <version>1.7</version>
      </dependency>
  
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.48</version>
      </dependency>
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.2.15</version>
      </dependency>
  
  </dependencies>
  ```
  
* ②编写service代码

  ```java
  public class UserServiceImpl implements UserService {
  
      private UserDao userDao;
  
      public UserServiceImpl(UserDao userDao) {
          this.userDao = userDao;
      }
  
      public List<User> findAll() throws Exception {
          return userDao.findAll();
      }
  
  }
  ```

* ③编写dao代码

  ```java
  public class UserDaoImpl implements UserDao {
  
      private QueryRunner queryRunner;
  
      public void setQueryRunner(QueryRunner queryRunner) {
          this.queryRunner = queryRunner;
      }
  
      public List<User> findAll() throws Exception {
  
          return queryRunner.query(
                  "select user_id userId , user_name userName, user_pwd userPwd from t_user",
                  new BeanListHandler<User>(User.class)
          );
      }
  }
  ```
  
* ④编写spring-core.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  
      <bean id="userService" class="com.qh.service.impl.UserServiceImpl">
          <constructor-arg name="userDao" ref="userDao"></constructor-arg>
      </bean>
  
      <bean id="userDao" class="com.qh.dao.impl.UserDaoImpl">
          <property name="queryRunner" ref="queryRunner"></property>
      </bean>
  
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
          <property name="url" value="jdbc:mysql://localhost:3306/mydb?useSSL=false"></property>
          <property name="username" value="root"></property>
          <property name="password" value="root"></property>
      </bean>
  
      <bean id="queryRunner" class="org.apache.commons.dbutils.QueryRunner">
          <constructor-arg name="ds" ref="dataSource"></constructor-arg>
      </bean>
  
  </beans>
  ```

* ⑤代码测试

  ```java
  public class UserServiceTest {
  
      @Test
      public void findAll() throws Exception {
          //创建Spring容器
          BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core.xml");
          UserService userService = beanFactory.getBean(UserService.class);
          List<User> userList = userService.findAll();
          System.out.println("userList = " + userList);
      }
  }
  ```

* 注意事项

  ```java
  //自动获取链接, 自动归还链接
  new QueryRunner(dataSource).query(sql,...)
  //手动获取链接, 手动归还链接
  new QueryRunner().query(connection, sql, ...)
  ```

### 13-注解创建对象

* 概述

  * 通过`@Controller`, `@Service`, `@Repository`, `@Component`四个创建对象并存入Spring容器.

* 注解

  * @Controller: 标记在控制层, 本质是@Component
  * @Service: 标记在业务层, 本质是@Component
  * @Repository: 标记在持久层, 本质是@Component
  * @Component: 标记其他的类

* 开发步骤

  * ①修改spring-core.xml, 扫描注解
  * ②使用四个注解
  * ③代码测试

* ①修改spring-core.xml, 扫描注解

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 
         http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
  
      <!--扫描注解-->
      <context:component-scan base-package="com.qh"></context:component-scan>
  
  </beans>
  ```
  
* ②使用四个注解

  ```java
  @Controller
  public class UserController {
  
      public void findAll() {
          System.out.println("UserController findAll");
      }
  
  }
  ```

  ```java
  @Service
  public class UserServiceImpl implements UserService {
      public void findAll() throws Exception {
          System.out.println("UserServiceImpl findAll");
      }
  }
  ```

  ```java
  @Repository
  public class UserDaoImpl implements UserDao {
      public void findAll() throws Exception {
          System.out.println("UserDao findAll");
      }
  }
  ```

  ```java
  @Component
  public class User {
  
      private Integer userId;
      private String userName;
      private String userPwd;
  
  }
  ```

* ③代码测试

  ```java
  @Test
  public void test1() {
      BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core.xml");
      beanFactory.getBean(UserController.class);
      beanFactory.getBean(UserServiceImpl.class);
      beanFactory.getBean(UserDaoImpl.class);
      beanFactory.getBean(User.class);
  }
  ```

### 14-注解扫描说明

* 概述

  * 注解扫描有三种方式: `①基本扫描`, `②排除扫描`, `③包含扫描`

* ①基本扫描: 扫描所有注解

  ```xml
  <context:component-scan base-package="com.qh"></context:component-scan>
  ```

* ②排除扫描: 在基本扫描基础上将指定注解排除

  ```xml
  <context:component-scan base-package="com.qh">
      <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
  </context:component-scan>
  ```

* ③包含扫描: 只扫描指定注解

  ```xml
  <context:component-scan base-package="com.qh" use-default-filters="false">
      <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
  </context:component-scan>
  ```

### 15-注解依赖注入之@Autowired

* 概述

  * 用于将Spring容器中的资源注入到java程序
  * 属于Spring框架, 只能在Spring框架中使用

* 工作流程

  * ![fwee45y6tgregyw4](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fwee45y6tgregyw4.4444yhtes3i0.webp)

  * 先根据类型查找bean, 再根据名称查找bean

* 代码实现

  ```java
  @Controller
  public class UserController {
  
      //成员变量类型就是`对象类型`
      //成员变量名称就是`对象名称`
      @Autowired
      private UserService userServiceImpl;
  
      public void findAll() throws Exception {
          System.out.println("UserController findAll");
          userServiceImpl.findAll();
      }
  
  }
  ```
  
* 注意事项
  * 不需要提供`set方法`或`构造器方法`

### 16-注解依赖注入之@Qualifier

* 概述

  * 用于配合`@Autowired`使用, 用于确定对象名称.

* 代码实现

  ```java
  @Controller
  public class UserController {
  
      //成员变量类型就是`对象类型`
      //成员变量名称就是`对象名称`
      @Autowired
      @Qualifier("userServiceImpl2")
      private UserService userService;
  
      public void findAll() throws Exception {
          System.out.println("UserController findAll");
          userService.findAll();
      }
  
  }
  ```

### 17-@Autowired的其他细节

* 概述

  * 除了可以作用于`成员变量`上,  也可以作用于`构造器`, `set方法`等等.

* 语法

  ```java
  @Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  public @interface Autowired {
  
      //是否必须注入成功
  	boolean required() default true;
  
  }
  ```

* ①标记在构造器

  ```java
  private UserService userService;
  
  @Autowired
  public UserServiceWrapper(UserService userServiceImpl2) {
      this.userService = userServiceImpl2;
  }
  ```

* ②标记在set方法

  ```java
  private UserService userService;
  
  @Autowired
  @Qualifier("userServiceImpl2")
  public void setUserService(UserService userService) {
      this.userService = userService;
  }
  ```

* ③佛系装配

  ```java
  private UserService userService;
  
  @Autowired(required = false)
  @Qualifier("userServiceImpl3")
  public void setUserService(UserService userService) {
      this.userService = userService;
  }
  ```

### 18-注解依赖注入之@Resource

* 概述
  * 用于将Spring容器中的资源注入到java程序
  * 属于jdk自带注解, 可以在相关框架中使用(包含Spring框架)
* 属性
  * ![tgw34eu7j6y3qyt6hr5](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/tgw34eu7j6y3qyt6hr5.ml17sjituow.webp)

* 代码实现

  ```java
  @Controller
  public class UserController {
  
      @Resource
      private UserService userService;
  
      public void findAll() throws Exception {
          System.out.println("UserController findAll");
          userService.findAll();
      }
  
  }
  ```

### 19-注解依赖注入之@Value

* 概述

  * 用于将简单类型注入到java程序

* 代码实现1: 不使用Spring注入

  ```java
  @Component
  public class User {
  
      private Integer userId = 250;
      private String userName = "张三";
      private String userPwd = "zs";
  
  }
  ```

* 代码实现2: 使用Spring注入简单版

  ```java
  @Component
  public class User2 {
  
      @Value("250")
      private Integer userId ;
      @Value("张三")
      private String userName ;
      @Value("zs")
      private String userPwd ;
  
  }
  ```

* 代码实现3: 使用Spring注入豪华版

  ```properties
  userId=250
  userName=张三
  userPwd=zs
  ```

  ```xml
  <context:property-placeholder location="userinfo.properties"></context:property-placeholder>
  ```

  ```java
  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  @Component
  public class User3 {
  
      @Value("${userId}")
      private Integer userId ;
      @Value("${userName}")
      private String userName ;
      @Value("${userPwd}")
      private String userPwd ;
  
  }
  ```