### 01-拦截器概述

* 概述
  * 在程序中，使用拦截器在请求到达具体 handler 方法前，统一执行检测。
* 拦截器 VS 过滤器
  * 相同点
    * 过滤, 放行
  * 不同点
    * 工作平台不同, 拦截器工作在Spring容器中, 过滤器工作在Servlet容器中.
    * 对Spring资源支持不同, 拦截器可以直接获取Spring资源, 过滤不可以直接获取Spring资源
    * 拦截范围不同,  拦截器只能拦截Spring容器的资源, 过滤器可以过滤所有

### 02-拦截器入门

* 开发步骤

  * ①自定义Interceptor类实现HandlerInterceptor
  * ②编写spring-mvc.xml, 配置自定义Interceptor类
  * ③代码测试

* 分析

  * ![geagy4hhw456hut4yuw5](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/geagy4hhw456hut4yuw5.xzx9m76locg.webp)

* ①自定义Interceptor类实现HandlerInterceptor

  ```java
  //02-拦截器入门
  public class MyInterceptor01 implements HandlerInterceptor {
  
      @Override
      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
          //在handler方法之前执行, 返回true就放行, 返回false就拦截
          System.out.println("MyInterceptor01 preHandle");
          return true;
      }
  
      @Override
      public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
          //在hanlder方法之后执行, 在渲染视图之前执行
          System.out.println("MyInterceptor01 postHandle");
      }
  
      @Override
      public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
          //在hanlder方法之后执行, 在渲染视图之后执行
          System.out.println("MyInterceptor01 afterCompletion");
  
      }
  }
  ```

* ②编写spring-mvc.xml, 配置自定义Interceptor类

  ```xml
  <!--拦截器-->
  <mvc:interceptors>
      <bean class="com.qh.interceptor.MyInterceptor01"></bean>
  </mvc:interceptors>
  ```

* ③代码测试

  ```java
  //02-拦截器入门
  @RequestMapping("/interceptor/test1")
  public String test1() {
      System.out.println("InterceptorController test1");
      return "index";
  }
  ```

### 03-拦截器的拦截路径

* 概述

  * 默认情况下, 拦截器会拦截当前项目的Spring容器中的所有资源.

* 拦截路径

  * ①精确路径: 资源的路径和拦截路径完全一样
  * ②单层路径: 资源的路径匹配目录的单级资源即可
  * ③多层路径: 资源的路径匹配目录的多级资源即可
  * ④不拦截路径: 设置指定资源不拦截

* ①精确路径

  ```xml
  <mvc:interceptors>
      <mvc:interceptor>
          <!--精确路径-->
          <mvc:mapping path="/interceptor/test1"/>
          <bean class="com.qh.interceptor.MyInterceptor02"></bean>
      </mvc:interceptor>
  </mvc:interceptors>
  ```

* ②单层路径

  ```xml
  <mvc:interceptors>
      <mvc:interceptor>
          <mvc:mapping path="/interceptor/*"/>
          <bean class="com.qh.interceptor.MyInterceptor02"></bean>
      </mvc:interceptor>
  </mvc:interceptors>
  ```

* ③多层路径

  ```xml
  <mvc:interceptors>
      <mvc:interceptor>
          <mvc:mapping path="/interceptor/**"/>
          <bean class="com.qh.interceptor.MyInterceptor02"></bean>
      </mvc:interceptor>
  </mvc:interceptors>
  ```

* ④不拦截路径

  ```xml
  <mvc:interceptors>
      <mvc:interceptor>
          <mvc:mapping path="/interceptor/**"/>
          <mvc:exclude-mapping path="/interceptor/test1"/>
          <bean class="com.qh.interceptor.MyInterceptor02"></bean>
      </mvc:interceptor>
  </mvc:interceptors>
  ```

### 04-多拦截器配置

* 概述

  * 先配置, 先拦截, 后放行.

* 代码实现

  ```xml
  <mvc:interceptors>
      <bean class="com.qh.interceptor.MyInterceptor01"></bean>
      <mvc:interceptor>
          <mvc:mapping path="/**"/>
          <bean class="com.qh.interceptor.MyInterceptor02"></bean>
      </mvc:interceptor>
  </mvc:interceptors>
  ```


### 05-SpringMVC文件上传

* 开发步骤

  * ①引入依赖
    * commons-fileupload
  * ②前端代码: 文件上传三要素
    * 2.1, 请求方式=post
    * 2.2, enctype=multipart/form-data
    * 2.3,文件上传项`<input type="file">`
  * ③编写spring-mvc.xml
    * 配置文件上传解析器
  * ④编写UploadController
    * 处理文件上传的逻辑代码

* ①引入依赖

  ```xml
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.4</version>
  </dependency>
  ```

* ②前端代码: 文件上传三要素

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>05-SpringMVC文件上传</title>
  </head>
  <body>
  
  <form action="/file/upload" method="post" enctype="multipart/form-data">
      照片:<input type="file" name="pic"><br>
      <button type="submit">上传</button>
  </form>
  
  </body>
  </html>
  ```

* ③编写spring-mvc.xml

  ```xml
  <bean id="multipartResolver"
        class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!-- 设置上传文件的最大尺寸为5MB -->
      <property name="maxUploadSize">
          <value>5242880</value>
      </property>
  </bean>
  ```

* ④编写UploadController

  ```java
  @Controller
  public class FileController {
      private final String FILE_UPLOAD = "pages/upload/upload.html";
  
      private final String FILE_UPLOAD_SUCCESS = "pages/upload/success.html";
  
      @Autowired
      private ServletContext servletContext;
      @RequestMapping("/upload.html")
      public String toUploadPage(){
          return FILE_UPLOAD;
      }
  
      /**
       * @param srcFile: 源文件
       * @return
       */
      @PostMapping("/file/upload")
      public String upload(@RequestParam("pic") MultipartFile srcFile) throws IOException {
         //System.out.println(srcFile);
          //获取部署路径
          String serverRealPath = servletContext.getRealPath("upload/img");
          //没有则创建
          File createRealFile = new File(serverRealPath);
          if(!createRealFile.exists()){
              createRealFile.mkdirs();
          }
          //获取上传文件的原始名称
          String originalFilename = srcFile.getOriginalFilename();
          String newUUidFileName = UUID.randomUUID().toString().replaceFirst("-", "")+"."+originalFilename.split("\\.")[1];
          String newServerRealName=serverRealPath+File.separator+newUUidFileName;
          File newRealPath = new File(newServerRealName);
          //上传
          srcFile.transferTo(newRealPath);
          return FILE_UPLOAD_SUCCESS;
      }
  
  }
  ```
  
* 注意事项

  * ①上传文件中文乱码, 使用`CharacterEncodingFilter`过滤器; 
  * ②上传文件名称重复导致文件覆盖, 使用随机文件名称

### 06-SpringMVC文件下载

* 概述

  * 默认情况下，浏览器会尽可能解析对应的文件，只要是能够在浏览器窗口展示的，就都会直接显示， 而不是提示下载。所以，开发人员必须通过代码明确要求浏览器提示下载。

* 开发步骤

  * ①获取下载文件名称
  * ②告诉浏览器下载文件的类型
  * ③告诉浏览器必须打开下载窗口
  * ④传递下载文件的字节数据

* 工作流程

  * ![gahjw435yqg267qa](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gahjw435yqg267qa.3eb14ok2daw0.webp)

* 代码实现

  ```java
  /**
   * @param fileName: 下载文件名称
   * @return
   */
  @RequestMapping("/file/download")
      public ResponseEntity<byte[]> download(String fileName)throws Exception{
          //1.获取下载文件的输入流
          String serverRealPath = servletContext.getRealPath("upload/img")+File.separator+fileName;
          FileInputStream fis = new FileInputStream(serverRealPath);
          //下载文件的字节数据（数组)
          byte[] body = new byte[fis.available()];
          fis.read(body);
          fis.close();
          //设置响应头
          MultiValueMap<String,String> headers = new HttpHeaders();
          //2.1.告诉浏览器下载文件的类型
          //服务器文件类型 mimeType()
          String mimeType = servletContext.getMimeType(fileName);
          headers.add("Content-Type",mimeType);
          //2.2告诉浏览器必须打开下载窗口
          //设置编码方式 UTF-8
          String encode = URLEncoder.encode(fileName, "utf-8");
          headers.add("Content-Disposition","attachment;filename="+encode);
          //下载状态
          HttpStatus ok = HttpStatus.OK;
          ResponseEntity<byte[]> responseEntity = new ResponseEntity<byte[]>(
                  body,headers,ok
          );
          return responseEntity;
  
  }
  ```
  
* 注意事项

  * 下载文件中文乱码, 因为服务器代码没有编码, 浏览器有解码, 所以需要在服务器代码中对下载文件名称进行手动编码.


### 07-RESTful风格介绍

* 概述

  * WebAPI : 如果一个URL返回的不包含HTML，而是数据(json字符串/xml字符串)
    * 同步请求: form表单, 返回的是HTML
    * 异步请求: AJAX/axios, 返回的是json字符串.
  * REST: Representational State Transfer
    * 是WebAPI的一种.
  * RESTful: 是按照Rest风格访问网络资源
    * ①返回的是json字符串;
    * ②不同的操作(CRUD)对应不同的请求方式
      * insert --- post
      * delete --- delete
      * update --- put
      * select --- get

* 代码实现

  ```
  //之前
  //根据id查询记录: get
  http://localhost:8080/user/getById?id=1
  //根据id删除记录: post
  http://localhost:8080/user/delete?id=1
  
  //现在
  //根据id查询记录: get
  http://localhost:8080/user/1
  //根据id删除记录: delete
  http://localhost:8080/user/1
  ```

* 注意事项

  * 规则: 必须遵守, 否则语法报错
  * 规范: 必须遵守, 语法不会报错, 比如: 驼峰标识 
  * 风格: 推荐遵守, 不遵守也没问题

### 08-HiddenHttpMethodFilter过滤器

* 概述

  * 默认情况下, form表单只能够发起`get`和`post`请求; 如果想要让form表单支持`put`和`delete`请求, 就需要用到`HiddenHttpMethodFilter`过滤器, 该过滤器可以通过`post请求`来模拟`put`和`delete`请求.

* 需求

  * 发起`get`,`post`,`put`,`delete`请求

* 代码实现

  ```xml
  <filter>
      <filter-name>HiddenHttpMethodFilter</filter-name>
      <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  </filter>
  <filter-mapping>
      <filter-name>HiddenHttpMethodFilter</filter-name>
      <servlet-name>mvc</servlet-name>
  </filter-mapping>
  ```

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>08-HiddenHttpMethodFilter过滤器</title>
  </head>
  <body>
  
  <h2>get请求</h2>
  <form action="/rest/test1_1" method="get">
      <button type="submit">get请求</button>
  </form>
  
  <h2>post请求</h2>
  
  <form action="/rest/test1_2" method="post">
      <button type="submit">post请求</button>
  </form>
  
  <h2>put请求</h2>
  <form action="/rest/test1_3" method="post">
      <input type="hidden" name="_method" value="put">
      <button type="submit">put请求</button>
  </form>
  
  <h2>delete请求</h2>
  <form action="/rest/test1_4" method="post">
      <input type="hidden" name="_method" value="delete">
      <button type="submit">delete请求</button>
  </form>
  
  </body>
  </html>
  ```

  ```java
  //08-HiddenHttpMethodFilter过滤器
  @RequestMapping("/rest/test1.html")
  public String toTest1Page() {
      return "rest/test1";
  }
  
  @GetMapping("/rest/test1_1")
  public String test1_1() {
      System.out.println("RESTFulController test1_1 get");
      return "index";
  }
  
  @PostMapping("/rest/test1_2")
  public String test1_2() {
      System.out.println("RESTFulController test1_2 post");
      return "index";
  }
  
  @PutMapping("/rest/test1_3")
  public String test1_3() {
      System.out.println("RESTFulController test1_3 put");
      return "index";
  }
  
  @DeleteMapping("/rest/test1_4")
  public String test1_4() {
      System.out.println("RESTFulController test1_4 delete");
      return "index";
  }
  ```

### 09-RESTful风格查询用户

* 需求

  * Restful风格查询用户

* 分析

  ```
  //之前: get
  http://localhost:8080/user/getById?id=250
  
  //现在: get
  http://localhost:8080/user/250
  ```

* 代码实现

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>RESTful风格练习</title>
  </head>
  <body>
  
  <!--09-RESTful风格查询用户-->
  <a href="/user/250">查询用户</a>
  
  </body>
  </html>
  ```
  
  ```java
  private static final String PAGE_INDEX = "user/index";
  
  //09-RESTful风格查询用户
  @RequestMapping("/index.html")
  public String index() {
      return PAGE_INDEX;
  }
  
  @GetMapping("/{id}")
  public String getById(@PathVariable("id") Integer userId) {
      System.out.println("UserController getById userId = " + userId);
      return "index";
  }
  ```

### 10-RESTful风格删除用户

* 需求

  * 根据id删除用户

* 分析

  ```
  //之前: post
  http://localhost:8080/user/delete?id=250
  
  //现在: delete
  http://localhost:8080/user/250
  ```

* 代码实现

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>RESTful风格练习</title>
  </head>
  <body>
  
  <div id="app">
  
      <!--09-RESTful风格查询用户-->
      <a href="/user/250">查询用户</a>
  
      <a href="/user/250" @click.prevent="deleteById()">删除用户</a>
      <form method="post" id="post2Delete">
          <input type="hidden" name="_method" value="delete">
      </form>
  
  </div>
  <script src="/js/vue.js"></script>
  <script>
  
      var vue = new Vue({
          el: "#app",
          data: {},
          methods: {
              deleteById() {
                  console.log("deleteById");
                  //获取form
                  var formEle = document.getElementById("post2Delete");
                  //设置action属性
                  formEle.action = event.target.href;
                  //提交form
                  formEle.submit();
              }
          }
      })
  
  </script>
  
  </body>
  </html>
  ```
  
  ```java
  //10-RESTful风格删除用户
  @DeleteMapping("/{id}")
  public String delete(@PathVariable Integer id) {
      System.out.println("UserController delete id = " + id);
      return "index";
  }
  ```

### 11-RESTful风格添加用户(掌握)

* 需求

  * 添加用户记录

* 分析

  ```
  //之前: post
  http://localhost:8080/user/insert?username=root&password=root
  
  //现在: post
  http://localhost:8080/user?username=root&password=root
  ```

* 代码实现

  ```html
  <form action="/user" method="post">
      账户:<input type="text" name="username"><br>
      密码:<input type="text" name="password"><br>
      <button type="submit">添加</button>
  </form>
  ```

  ```java
  @PostMapping
  public String insert(User user){
      System.out.println("UserController insert user = " + user);
      return "index";
  }
  ```

### 12-RESTful风格登录功能

* 分析

  ```
  //之前: post
  http://localhost:8080/user/login?username=root&password=root
  
  //现在: post
  http://localhost:8080/user/login?username=root&password=root
  ```

* 代码实现

  ```html
  <form action=" /user/login" method="post">
      账户:<input type="text" name="username"><br>
      密码:<input type="text" name="password"><br>
      <button type="submit">添加</button>
  </form>
  ```

  ```java
  @PostMapping("/login")
  public String login(User user){
      System.out.println("UserController login user = " + user);
      return "index";
  }
  ```

### 13-RESTful风格修改用户功能

* 分析

  ```
  //之前: post
  http://localhost:8080/user/update?username=root&password=root
  
  //现在: put
  http://localhost:8080/user?username=root&password=root
  ```

* 代码实现

  ```html
  <form action="/user" method="post">
      <input type="hidden" name="_method" value="put">
      <input type="hidden" name="id" value="250">
      账户:<input type="text" name="username"><br>
      密码:<input type="text" name="password"><br>
      <button type="submit">修改</button>
  </form>
  ```

  ```java
  //13-RESTful风格修改用户功能
  @PutMapping
  public String update(User user){
      System.out.println("UserController update user = " + user);
      return "index";
  }
  ```

### 14-SpringMVC获取AJAX发送的普通参数(掌握)

* 概述

  * 普通参数: 与请求一起发送的 URL 参数 , 以键值对形式.

* axios参数

  ```
  url: 请求路径,
  method: 请求方式(get, post, put, delete),
  headers: 请求头,
  params: 普通参数,
  data: 请求体参数,
  timeout: 超时,
  responseType: 响应正文数据类型,
  ```

* 代码实现

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>14-SpringMVC获取AJAX发送的普通参数</title>
  </head>
  <body>
  
  <div id="app">
  
      <button @click="fn1()">发送普通参数</button>
  
  </div>
  
  </body>
  <script src="/js/vue.js"></script>
  <script src="/js/axios.js"></script>
  <script>
  
      var vue = new Vue({
          el: "#app",
          data: {},
          methods: {
              fn1() {
                  //发送普通参数
                  axios({
                      url: "/ajax/test1",
                      method: "get",
                      params: {
                          username: "qh",
                          password: "qh"
                      }
                  }).then(function (response) {
                      var data = response.data;
                      console.log(data);
                  });
              }
          }
      })
  
  </script>
  </html>
  ```
  
  ```java
  //14-SpringMVC获取AJAX发送的普通参数
  @RequestMapping("/ajax/test1.html")
  public String toTest1Page() {
      return "ajax/test1";
  }
  
  @GetMapping("/ajax/test1")
  public String test1(User user) {
      System.out.println("user = " + user);
      return "index";
  }
  ```

### 15-SpringMVC获取AJAX发送的请求体参数(掌握)

* 概述

  * 请求体参数: 参数在请求体中, 以json字符串的形式.
  * @RequestBody: 获取请求体的json字符串, 并将json字符串转换为实体类对象.

* 代码实现

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>15-SpringMVC获取AJAX发送的请求体参数</title>
  </head>
  <body>
  
  <div id="app">
  
      <button @click="fn1()">发送请求体参数</button>
  
  </div>
  
  </body>
  <script src="/js/vue.js"></script>
  <script src="/js/axios.js"></script>
  <script>
  
      var vue = new Vue({
          el: "#app",
          data: {},
          methods: {
              fn1() {
                  //发送普通参数
                  axios({
                      url: "/ajax/test2_3",
                      method: "post",
                      data: {
                          username: "qh",
                          password: "qh"
                      }
                  }).then(function (response) {
                      var data = response.data;
                      console.log(data);
                  });
              }
          }
      })
  
  </script>
  </html>
  ```
  
  ```java
  //15-SpringMVC获取AJAX发送的请求体参数
  @RequestMapping("/ajax/test2.html")
  public String toTest2Page() {
      return "ajax/test2";
  }
  
  //方式一
  @PostMapping("/ajax/test2")
  public String test2(HttpServletRequest request) throws Exception {
      //获取请求体中的json字符串
      BufferedReader bufferedReader = request.getReader();
      StringBuilder sb = new StringBuilder();
      String lineContent = null;
      while ((lineContent = bufferedReader.readLine()) != null) {
          sb.append(lineContent);
      }
      String inputJonStr = sb.toString();
      //json字符串 -> User对象
      User user = new ObjectMapper().readValue(inputJonStr, User.class);
      System.out.println("user = " + user);
  
      return "index";
  }
  
  //方式二
  @PostMapping("/ajax/test2_2")
  public String test2_2(@RequestBody String inputJonStr) throws Exception {
      //1,获取请求体中的json字符串
  
      //2,json字符串 -> User对象
      User user = new ObjectMapper().readValue(inputJonStr, User.class);
      System.out.println("user = " + user);
  
      return "index";
  }
  
  //方式三: 掌握
  @PostMapping("/ajax/test2_3")
  public String test2_3(@RequestBody User user) throws Exception {
      //1,获取请求体中的json字符串
      //2,json字符串 -> User对象
      System.out.println("AjaxController test2_3 user = " + user);
      return "index";
  }
  ```
  

### 16-SpringMVC响应json字符串(掌握)

* 概述

  * @ResponseBody: 将实体类对象转换为json字符串; 并将该json字符串作为响应正文返回给浏览器; 并解决响应正文中文乱码问题.

  * 安装jar包

    ```xml
            <dependency>
                <groupId>com.fasterxml.jackson.core</groupId>
                <artifactId>jackson-databind</artifactId>
                <version>2.14.1</version>
            </dependency>
            <dependency>
                <groupId>com.fasterxml.jackson.core</groupId>
                <artifactId>jackson-annotations</artifactId>
                <version>2.14.1</version>
            </dependency>
            <dependency>
                <groupId>com.fasterxml.jackson.core</groupId>
                <artifactId>jackson-core</artifactId>
                <version>2.14.1</version>
            </dependency>
    ```

* 代码实现

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>15-SpringMVC获取AJAX发送的请求体参数</title>
  </head>
  <body>
  
  <div id="app">
  
      <button @click="fn1()">发送请求体参数</button>
  
  </div>
  
  </body>
  <script src="/js/vue.js"></script>
  <script src="/js/axios.js"></script>
  <script>
  
      var vue = new Vue({
          el: "#app",
          data: {},
          methods: {
              fn1() {
                  //发送普通参数
                  axios({
                      url: "/ajax/test3_3",
                      method: "post",
                      data: {
                          username: "qh",
                          password: "qh"
                      }
                  }).then(function (response) {
                      var data = response.data;
                      console.log(data);
                  });
              }
          }
      })
  
  </script>
  </html>
  ```
  
  ```java
  //16-SpringMVC响应json字符串
  @RequestMapping("/ajax/test3.html")
  public String toTest3Page() {
      return "ajax/test3";
  }
  
  //方式一
  @PostMapping("/ajax/test3_1")
  public void test3_1(@RequestBody User user , HttpServletResponse response) throws Exception {
      System.out.println("AjaxController test3_1 user = " + user);
      //响应json字符串给浏览器
      //1, 准备json字符串
      //1.1, 准备结果对象
      ResultVO<User> resultVO = new ResultVO<>(
              true,
              "查询用户成功!",
              user
      );
      //1.2, 将resultVO转换为json字符串
      String resultJsonStr = new ObjectMapper().writeValueAsString(resultVO);
      //2,将json字符串作为响应体返回给浏览器
      response.setContentType("application/json;charset=utf-8");
      response.getWriter().write(resultJsonStr);
  }
  
  //方式二
  @ResponseBody
  @PostMapping("/ajax/test3_2")
  public String test3_2(@RequestBody User user , HttpServletResponse response) throws Exception {
      System.out.println("AjaxController test3_2 user = " + user);
      //1, 准备json字符串 , 将resultVO转换为json字符串
      String resultJsonStr = new ObjectMapper().writeValueAsString(new ResultVO<>(
              true,
              "查询用户成功!",
              user
      ));
      //2, 解决响应正文中文乱码问题
      //3,将json字符串作为响应体返回给浏览器
      return resultJsonStr;
  }
  
  //方式三
  @ResponseBody
  @PostMapping("/ajax/test3_3")
  public ResultVO<User> test3_3(@RequestBody User user , HttpServletResponse response) throws Exception {
      System.out.println("AjaxController test3_3 user = " + user);
      //1, 将resultVO转换为json字符串
      //2, 解决响应正文中文乱码问题
      //3,将json字符串作为响应体返回给浏览器
      return new ResultVO<>(
              true,
              "查询用户成功!",
              user
      );
  }
  ```