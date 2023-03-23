### 01-程序的耦合和解耦

* 什么是耦合?
  * 编写程序的时候，通常会用多个功能模块，共同实现我们的功能，这时，各个功能模块间联系的紧密度就可以理解为我们常说 的耦合度
  * 低耦合, 高内聚
    * 低耦合: 完成一个功能, 和其他的模块关联度越低越好;
    * 高内聚: 完成一个功能, 独立完成的能力越高越好.
* 好处
  * 代码复用率大大提高.
  * 提高程序的维护性

### 02-三层架构的耦合性问题

* 需求
  * 手撕Spring框架

* ①控制层: controller

  ```java
  public class UserController {
  
      public static void main(String[] args) {
          findAll();
      }
  
      public static void findAll() {
          System.out.println("UserController findAll");
          UserService userService = new UserServiceImpl();
          userService.findAll();
      }
  
  }
  ```
  
* ②业务层: service

  ```java
  public class UserServiceImpl implements UserService {
      
      public void findAll() {
          System.out.println("UserServiceImpl findAll");
          UserDao userDao = new UserDaoImpl();
          userDao.findAll();
      }
      
  }
  ```

* ③持久层: dao

  ```java
  public class UserDaoImpl implements UserDao {
      
      public void findAll() {
          System.out.println("UserDaoImpl findAll");
      }
      
  }
  ```

* 存在的问题

  * ①`UserController`使用`UserServiceImpl`构造器创建对象, 如果UserServiceImpl的构造器发生变化, 意味着UserController也要随着变化, 可以理解为`控制层`和`业务层`和`持久层`存在高耦合;
  * ②`UserServiceImpl`使用package声明包, `UserController`使用import导入包, 如果UserServiceImpl的包发生变化, 意味着UserController也要随着变化, 可以理解为`控制层`和`业务层`和`持久层`存在高耦合;

### 03-解耦方案使用工厂模式

* 分析
  * ![ntdjhjew5gshrerggdgg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ntdjhjew5gshrerggdgg.6w58yc197mc.webp)

* BeanFactory

  ```java
  public class BeanFactory {
  
      public Object getBean(String beanName) {
          if ("userService".equals(beanName)) {
              return new UserServiceImpl();
          } else if ("userDao".equals(beanName)) {
              return new UserDaoImpl();
          }
          return null;
      }
  
  }
  ```
  
* ①控制层

  ```java
  public class BeanFactory {
  
      public Object getBean(String beanName) {
          if ("userService".equals(beanName)) {
              return new UserServiceImpl();
          } else if ("userDao".equals(beanName)) {
              return new UserDaoImpl();
          }
          return null;
      }
  
  }
  ```
  
* ②业务层

  ```java
  public class UserServiceImpl implements UserService {
  
      public void findAll() {
          System.out.println("UserServiceImpl findAll");
          BeanFactory beanFactory = new BeanFactory();
          UserDao userDao = (UserDao) beanFactory.getBean("userDao");
          userDao.findAll();
      }
      
  }
  ```

* ③持久层

  ```java
  public class UserDaoImpl implements UserDao {
      
      public void findAll() {
          System.out.println("UserDaoImpl findAll");
      }
      
  }
  ```

* 存在的问题

  * ①BeanFactory中依然需要import导入包;
  * ②getBean方法中存在大量的if...else if...

### 04-解耦方案使用反射

* BeanFactory

  ```java
  /**
   * ②反射
   */
  public class BeanFactory {
      /**
       *
       * @param className: 全限定类名
       * @return: 对象
       */
      public Object getBean(String className) throws Exception {
          return Class.forName(className).newInstance();
      }
  
  }
  ```
  
* ①控制层

  ```java
  public class UserController {
  
      public static void main(String[] args) throws Exception {
          findAll();
      }
  
      public static void findAll() throws Exception {
          System.out.println("UserController findAll");
          BeanFactory beanFactory = new BeanFactory();
          UserService userService = (UserService) beanFactory.getBean("com.qh.service.impl.UserServiceImpl");
          userService.findAll();
      }
  
  }
  ```
  
* ②业务层

  ```java
  public class UserServiceImpl implements UserService {
  
      public void findAll() throws Exception {
          System.out.println("UserServiceImpl findAll");
          BeanFactory beanFactory = new BeanFactory();
          UserDao userDao = (UserDao) beanFactory.getBean("com.qh.dao.impl.UserDaoImpl");
          userDao.findAll();
      }
  }
  ```

* ③持久层

  ```java
  public class UserDaoImpl implements UserDao {
      public void findAll() {
          System.out.println("UserDaoImpl findAll");
      }
  }
  ```

* 存在的问题
  * `com.qh.service.impl.UserServiceImpl`, `com.qh.dao.impl.UserDaoImpl`存在字符串硬编码.

### 05-解耦方案使用xml配置文件

* beans.xml

  ```xml
  <?xml version="1.0" encoding="utf-8" ?>
  <beans>
      <bean id="userService" class="com.qh.service.impl.UserServiceImpl"></bean>
      <bean id="userDao" class="com.qh.dao.impl.UserDaoImpl"></bean>
  </beans>
  ```

* BeanFactory

  ```java
  /**
   * ③xml配置文件
   */
  public class BeanFactory {
  
      private Map<String, Object> beanDefinitionMap = new HashMap<>();
  
      public BeanFactory() throws Exception {
          parseXML();
      }
  
      /**
       * 解析xml
       */
      private void parseXML() throws Exception {
          SAXReader saxReader = new SAXReader();
          Document document = saxReader.read(BeanFactory.class.getClassLoader().getResourceAsStream("beans.xml"));
          Element rootElement = document.getRootElement();
          List<Element> beanElements = rootElement.elements("bean");
          for (Element beanElement : beanElements) {
              String id = beanElement.attribute("id").getValue();
              String className = beanElement.attributeValue("class");
              Object instance = Class.forName(className).newInstance();
              //名称=对象
              beanDefinitionMap.put(id, instance);
          }
      }
  
      /**
       * 根据名称获取对象
       *
       * @param beanId: 对象名称
       * @return: 对象
       */
      public Object getBean(String beanId) throws Exception {
  
          return beanDefinitionMap.get(beanId);
      }
  
  }
  ```
  
* ①控制层

  ```java
  public class UserController {
  
      public static void main(String[] args) throws Exception {
          findAll();
      }
  
      public static void findAll() throws Exception {
          System.out.println("UserController findAll");
          BeanFactory beanFactory = new BeanFactory();
          UserService userService = (UserService) beanFactory.getBean("userService");
          userService.findAll();
      }
  
  }
  ```
  
* ②业务层

  ```java
  public class UserServiceImpl implements UserService {
  
      public void findAll() throws Exception {
          System.out.println("UserServiceImpl findAll");
          BeanFactory beanFactory = new BeanFactory();
          UserDao userDao = (UserDao) beanFactory.getBean("userDao");
          userDao.findAll();
      }
  }
  ```

* ③持久层

  ```java
  public class UserDaoImpl implements UserDao {
      public void findAll() {
          System.out.println("UserDaoImpl findAll");
      }
  }
  ```

### 06-Spring概述

* 概述

  * Spring是分层的 Java SE/EE应用轻量级开源框架，以 IoC（Inverse Of Control：反转控制）和 AOP （Aspect Oriented Programming：面向切面编程）为内核。
  * 提供了视图层SpringMVC和持久层Spring JDBCTemplate以及业务层事务管理等众多的企业级应用 技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的JavaEE企业应用开源框 架

* 官网

  * ![fgb7k8j64hy5trwre4g](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/fgb7k8j64hy5trwre4g.6p69yzyrbhk0.webp)

  * [Spring | Home](https://spring.io/)

* 结构

  * ![ghswbr44t5ewq](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/ghswbr44t5ewq.4euetyyz7ua0.webp)

  * ORM: object releationship map, 对象关系映射.

* 好处

  * 方便解耦，简化开发 
  * AOP 编程的支持 
  * 声明式事务的支持 
  * 方便程序的测试

### 07-Spring入门案例

* 开发步骤

  * ①引入依赖
    * spring-beans, spring-core, spring-context, spring-expression, spring-jcl
  * ②编写spring-core.xml配置文件
  * ③代码测试

* ①引入依赖

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-core</artifactId>
          <version>5.3.24</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-beans</artifactId>
          <version>5.3.24</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.3.24</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-expression</artifactId>
          <version>5.3.24</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jcl</artifactId>
          <version>5.3.24</version>
      </dependency>
  
  </dependencies>
  ```
  
* ②编写spring-core.xml配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <bean id="userController" class="com.qh.controller.UserController"></bean>
      <bean id="userService" class="com.qh.service.impl.UserServiceImpl"></bean>
      <bean id="userDao" class="com.qh.dao.impl.UserDaoImpl"></bean>
  
  </beans>
  ```
  
* ③代码测试

  ```java
  public class UserController {
  
      public  void findAll() throws Exception {
          System.out.println("UserController findAll");
          //获取Spring容器对象
          BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core.xml");
          UserService userService = (UserService) beanFactory.getBean("userService");
          userService.findAll();
      }
      
  }
  ```
  
  ```java
  public class UserServiceImpl implements UserService {
  
      public void findAll() throws Exception {
          System.out.println("UserServiceImpl findAll");
          BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core.xml");
          UserDao userDao = (UserDao) beanFactory.getBean("userDao");
          userDao.findAll();
      }
  }
  ```
  
  ```java
  public class UserDaoImpl implements UserDao {
      public void findAll() {
          System.out.println("UserDaoImpl findAll");
      }
  }
  ```
  
* 回顾

  * dtd: 标签名称, 属性名称
  * schema: 标签名称, 属性名称, 标签内容, 属性值

* 存在的问题

  * BeanFactory会创建多次.

* 解决方案1: 手撕SpringMVC框架

  * 第一次请求时, 创建BeanFactory对象并存储到应用域; 第二次请求时, 直接从应用域获取BeanFactory对象.
  * 存在的问题: 第一次请求时长太长.

* 解决方案2: 手撕SpringMVC框架

  * 当项目启动时(ServletContextListener), 创建BeanFactory对象并存储到应用域; 第一次请求时,第二次请求时... 直接从应用域获取BeanFactory对象.

### 08-IOC控制反转

* 概述

  * IOC: inversion of controller, 控制反转.
  * 控制反转: 对资源(对象, 配置文件...)的控制权, 之前资源的控制权在Java程序手上, 现在资源的控制权反转到了Spring容器手上.

* 好处

  * ![edrf3e4rhjty63w4er](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/edrf3e4rhjty63w4er.fr9musfz2wo.webp)

  * 为了降低资源和Java程序之间的耦合度.

### 09-spring使用构造器IOC对象

* 概述

  * Spring通过xml配置, 自动使用构造器创建对象并将对象放入到Spring容器.

* 代码实现

  ```xml
  <bean id="user" class="com.qh.pojo.User"></bean>
  ```

* 注意事项

  * 使用`<bean>标签`默认通过无参构造器创建对象, 所以类中必须提供无参构造器

### 10-spring使用静态工厂IOC对象

* 概述

  * 通过工厂类的静态方法返回一个对象, 并将该对象存入到Spring容器.

* 代码实现

  ```java
  public class StaticUserFactory {
  
      public static User getUser() {
          return new User(1 , "静态工厂");
      }
  
  }
  ```
  
  ```xml
  <bean id="user2" class="com.qh.factory.StaticUserFactory" factory-method="getUser" ></bean>
  ```

### 11-spring使用实例工厂IOC对象

* 概述

  * 通过工厂类的普通方法返回一个对象, 并将该对象存入到Spring容器.

* 代码实现

  ```java
  public class UserFactory {
  
      public User getUser() {
          return new User(2, "实例工厂");
      }
  
  }
  ```

  ```xml
  <bean id="userFactory" class="com.qh.factory.UserFactory"></bean>
  <bean id="user3" factory-bean="userFactory" factory-method="getUser"></bean>
  ```

### 12-FactoryBean机制

* 概述

  * FactoryBean是Spring提供的一种整合第三方框架的常用机制。和普通的bean不同，配置一个 FactoryBean类型的bean，在获取bean的时候得到的并不是class属性中配置的这个类的对象，而是 getObject()方法的返回值。通过这种机制，Spring可以帮我们把复杂组件创建的详细过程和繁琐细 节都屏蔽起来，只把最简洁的使用界面展示给我们。
  * 将来我们整合Mybatis时，Spring就是通过FactoryBean机制来帮我们创建SqlSessionFactory对象 的。

* 源码

  ```java
  public interface FactoryBean<T> {
  
      //返回的对象实例
     @Nullable
     T getObject() throws Exception;
  
      //Bean的类型	
     @Nullable
     Class<?> getObjectType();
  
      //返回true是单例对象, 返回false是多例对象
     default boolean isSingleton() {
        return true;
     }
  
  }
  ```
  
* 开发步骤

  * ①自定义FactoryBean类实现FactoryBean接口
  * ②修改spring-core.xml , 配置自定义FactoryBean类
  * ③代码测试

* ①自定义FactoryBean类实现FactoryBean接口

  ```java
  public class UserFactoryBean implements FactoryBean<User> {
      @Override
      public User getObject() throws Exception {
          return new User(4, "FactoryBean机制");
      }
  
      @Override
      public Class<?> getObjectType() {
          return User.class;
      }
  
      @Override
      public boolean isSingleton() {
          return false;
      }
      
  }
  ```

* ②修改spring-core.xml , 配置自定义FactoryBean类

  ```xml
  <bean id="user4" class="com.qh.factorybean.UserFactoryBean"></bean>
  ```

### 13-bean标签属性

* 语法

  ```xml
  <!--
  id: 对象唯一标识
  class: 对象全限定类名
  factory-bean: 工厂对象引用
  factory-method: 工厂方法
  name: 对象别名
  scope: 对象作用域 
  	singleton:单一实例
  	prototype:多个实例
  	request
  	session
  init-method: 对象初始化方法
  destroy-method: 对象销毁方法
  -->
  <bean id="" 
        class="" 
        factory-bean="" 
        factory-method="" 
        name="" 
        scope="" 
        init-method="" 
        destroy-method="" 
  ></bean>
  ```

* 代码实现

  ```xml
  <bean id="user5"
        class="com.qh.pojo.User"
        name="myUser5"
  ></bean>
  ```
  
  ```java
  @Test
  public void test5() {
      BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core.xml");
      Object user = beanFactory.getBean("user5");
      Object user2 = beanFactory.getBean("myUser5");
      System.out.println(user == user2);
  }
  ```

### 14-Spring团队开发

* 概述

  * 项目开发中, 可能有多个spring配置文件, 根据多个配置文件获取Spring容器.

* 方式一: 使用可变参

  ```java
  @Test
  public void test6() {
      BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core2.xml" , "spring-core3.xml");
      System.out.println(beanFactory.getBean("user"));
      System.out.println(beanFactory.getBean("teacher"));
  }
  ```

* 方式二: 使用数组

  ```java
  @Test
  public void test7() {
      String[] configLocations = {"spring-core2.xml", "spring-core3.xml"};
      BeanFactory beanFactory = new ClassPathXmlApplicationContext(configLocations);
      System.out.println(beanFactory.getBean("user"));
      System.out.println(beanFactory.getBean("teacher"));
  }
  ```

* 方式三: 在主配置文件中加载次配置文件, 推荐

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!--主配置文件-->
  
      <bean id="user" class="com.qh.pojo.User"></bean>
  
      <import resource="spring-core3.xml"></import>
  
  </beans>
  ```

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!--次配置文件-->
  
      <bean id="teacher" class="com.qh.pojo.Teacher"></bean>
  
  </beans>
  ```

  ```java
  @Test
  public void test8() {
      BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-core2.xml");
      System.out.println(beanFactory.getBean("user"));
      System.out.println(beanFactory.getBean("teacher"));
  }
  ```

* 注意事项
  * 如果多个配置文件中有相同的对象冲突, 后配置会覆盖先配置.

### 15-根据类型获取bean场景一

* 场景一

  * ①同类型的bean在IOC容器只有一个
  * ②同类型的bean在IOC容器中有多个

* ①同类型的bean在IOC容器只有一个

  * 根据类型可以正常获取

* ②同类型的bean在IOC容器中有多个 [报错]

  * 根据类型不能获取, 根据对象名称获取即可

### 16-根据类型获取bean场景二

* 场景二
  * 声明一个接口, 有一个实现子类, 将实现子类对象放入到IOC容器.
  * ①根据接口类型获取对象
  * ②根据实现子类类型获取对象
* ①根据接口类型获取对象
  * 可以正常获取
* ②根据实现子类类型获取对象
  * 可以正常获取

### 17-根据类型获取bean场景三

* 场景三

  * 声明一个接口, 有多个实现子类, 将所有实现子类对象放入到IOC容器.
  * ①根据接口类型获取对象
  * ②根据实现子类类型获取对象

* ①根据接口类型获取对象

  * 不能正常获取, 根据id获取即可
  
* ②根据实现子类类型获取对象

  * 可以正常获取

* 工作流程

  * ![geavsr4yw357ui5j6](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/geavsr4yw357ui5j6.7bls39uw6wo0.webp)

  * 等价于`@Autowired`





### 18-bean作用域

* 作用域

  | 作用域    | 说明                              |
  | --------- | --------------------------------- |
  | singleton | 单例, 默认值                      |
  | prototype | 多例                              |
  | request   | 了解, 仅用于web项目, 当前请求有效 |
  | session   | 了解, 仅用于web项目, 当前会话有效 |

* ①singleton

  * 当Spring容器创建时, bean就会创建和初始化; 当Spring容器关闭时, bean就会销毁

* ②prototype

  * 当使用bean时, bean就会创建和初始化; 由JVM的垃圾回收机制负责销毁对象

* 常用方法

  | 方法                                  | 说明   |
  | ------------------------------------- | ------ |
  | 构造器                                | 创建   |
  | 使用init-method参数指定方法init       | 初始化 |
  | 使用destroy-method参数指定方法destroy | 销毁   |

* 代码实现

  ```java
  public class User {
  
      private Integer userId;
      private String userName;
  
      private User() {
          System.out.println("①User创建");
      }
  
      public void init(){
          System.out.println("②User初始化");
      }
  
      public void destroy() {
          System.out.println("③User销毁");
      }
  }
  ```

  ```xml
  <bean id="user"
        class="com.qh.pojo.User"
        scope="prototype"
        init-method="init"
        destroy-method="destroy"
  ></bean>
  ```
  
  ```java
  @Test
  public void test1() {
      //创建Spring容器
      ClassPathXmlApplicationContext beanFactory = new ClassPathXmlApplicationContext("spring-core3.xml");
      Object user1 = beanFactory.getBean("user");
      Object user2 = beanFactory.getBean("user");
      System.out.println(user1 == user2);
      //关闭Spring容器
      beanFactory.close();
  }
  ```