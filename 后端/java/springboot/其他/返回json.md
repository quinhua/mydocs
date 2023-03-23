## 1.springboot对json的处理

>  Spring Boot 中默认使用的 json 解析框架是 jackson

### 1.创建实体类

```java
@NoArgsConstructor
@AllArgsConstructor
@Data
public class User {
    private Long id;
    private String username;
    private String password;
}
```

### 2.创建Controller类

> 分别返回 `User`对象、`List` 和 `Map`

```java
@RestController
@RequestMapping("/json")
public class UserController {

    @RequestMapping("/user")
    public User getUser(){
        return new User(1L,"张三","zhangsan");
    }

    @RequestMapping("/list")
    public List<User> getUserList(){
        List<User> userList=new ArrayList<>();
        User user1 = new User(2L, "李四", "lisi");
        User user2 = new User(3L, "王五", "wangwu");
        userList.add(user1);
        userList.add(user2);
        return userList;
    }

    @RequestMapping("/map")
    public Map<String,Object> getUserMap(){
        Map<String, Object> map = new HashMap<>();
        User user = new User(4L, "赵六", "zhaoaliu");
        map.put("用户信息",user);
        map.put("用户年龄",23);
        return map;
    }

}
```

### 3.测试

分别测试 ：

![gyh3hawe3y4q2wgy](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gyh3hawe3y4q2wgy.7cf7yi8b4400.webp)

![gyh3ae4wgy34w2y](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gyh3ae4wgy34w2y.2y5ztxryma80.webp)

![gaq3y342qwyg3q42aw](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gaq3y342qwyg3q42aw.3jvelqh45fo0.webp)

### 4.jackson 中对null的处理

> 在实际项目中，我们会遇到一些 null 值，转 json 时是不希望有这些 null 出现的，比如我想所有的 null 在转化为 json 时都变成 "" 这种空字符串，在 Spring Boot 中，我们可以做一下配置即可，新建一个 jackson 的配置类：

```java
@Configuration
public class JacksonConfig {
    @Bean
    @Primary
    @ConditionalOnMissingBean(ObjectMapper.class)
    public ObjectMapper jacksonObjectMapper(Jackson2ObjectMapperBuilder builder) {
        ObjectMapper objectMapper = builder.createXmlMapper(false).build();
        objectMapper.getSerializerProvider().setNullValueSerializer(new JsonSerializer<Object>() {
            @Override
            public void serialize(Object o, JsonGenerator jsonGenerator, SerializerProvider serializerProvider) throws IOException {
                jsonGenerator.writeString("");
            }
        });
        return objectMapper;
    }
}
```

> 再次修改上面的接口进行测试

```java
    @RequestMapping("/user")
    public User getUser(){
        return new User(1L,"张三",null);
    }
```

![hg4wegh4ewrghergy](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hg4wegh4ewrghergy.2w6wjzkdt3c0.webp)

## 2.阿里巴巴FastJson

### 1.导入依赖

```xml
     <properties> 
        <fastjson.version>2.0.21</fastjson.version>
  	</properties>

	<!--fastjson start-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>${fastjson.version}</version>
    </dependency>
    <!--fastjson end-->
```

### 2.使用 fastJson 处理 null

>使用 fastJson 时，对 null 的处理和 jackson 有些不同，需要继承 WebMvcConfigurationSupport 类，然后覆盖 configureMessageConverters 方法，在方法中，可以选择对要实现 null 转换的场景，在 Spring Boot 中，我们可以做一下配置即可，新建一个 fastjson 的配置类：

```java
@Configuration
public class fastJsonConfig extends WebMvcConfigurationSupport {
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        FastJsonHttpMessageConverter converter = new FastJsonHttpMessageConverter();
        FastJsonConfig config = new FastJsonConfig();
        config.setSerializerFeatures(
                // 保留map空的字段
                SerializerFeature.WriteMapNullValue,
                // 将String类型的null转成""
                SerializerFeature.WriteNullStringAsEmpty,
                // 将Number类型的null转成0
                SerializerFeature.WriteNullNumberAsZero,
                // 将List类型的null转成[]
                SerializerFeature.WriteNullListAsEmpty,
                // 将Boolean类型的null转成false
                SerializerFeature.WriteNullBooleanAsFalse,
                // 避免循环引用
                SerializerFeature.DisableCircularReferenceDetect);

        converter.setFastJsonConfig(config);
        converter.setDefaultCharset(StandardCharsets.UTF_8);
        List<MediaType> mediaTypeList = new ArrayList<>();
        // 解决中文乱码问题，相当于在Controller上的@RequestMapping中加了个属性produces = "application/json"
        mediaTypeList.add(MediaType.APPLICATION_JSON);
        converter.setSupportedMediaTypes(mediaTypeList);
        converters.add(converter);
    }
}
```

### 3.测试

![gae34w4yghwgy](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gae34w4yghwgy.79udaw1bj9k0.webp)

![gha3ehuw435shyuw44](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gha3ehuw435shyuw44.28n13vocupes.webp)

## 3.封装统一返回的数据结构

### 1.定义统一的 json 结构

```java
@Data
public class ResultVo {

    private Integer status;

    private String message;

    private Map<String, Object> data = new HashMap<>();

    private ResultVo() {
    }

    public static ResultVo ok() {
        ResultVo resultUtil = new ResultVo();
        resultUtil.setStatus(ResultEnum.SUCCESS.getStatus());
        resultUtil.setMessage(ResultEnum.SUCCESS.getMessage());
        return resultUtil;
    }

    public static ResultVo fail() {
        ResultVo resultUtil = new ResultVo();
        resultUtil.setStatus(ResultEnum.FAIL.getStatus());
        resultUtil.setMessage(ResultEnum.SUCCESS.getMessage());
        return resultUtil;
    }

    public static ResultVo setResult(ResultEnum resultEnum){
        ResultVo resultUtil = new ResultVo();
        resultUtil.setStatus(resultEnum.getStatus());
        resultUtil.setMessage(resultEnum.getMessage());
        return resultUtil;
    }

    public ResultVo message(String message){
        this.setMessage(message);
        return this;
    }

    public ResultVo status(Integer status){
        this.setStatus(status);
        return this;
    }

    public ResultVo data(String key, Object value){
        this.data.put(key, value);
        return this;
    }

    public ResultVo data(Map<String, Object> map){
        this.setData(map);
        return this;
    }

}

```

### 2.编写枚举类

```java
@Getter
@AllArgsConstructor
@ToString
public enum ResultEnum {
    SUCCESS(200, "成功"),
    FAIL(400, "失败"),
    SUCCESS_SELECT(200,"查询成功"),
    FAIL_SELECT(400, "查询失败"),
    SUCCESS_DELETE(200,"删除成功"),
    FAIL_DELETE(400, "删除失败"),
    ;

    private final Integer status;
    private final String message;
}
```

### 3.修改 Controller 中的返回值类型及测试

```java
@RestController
@RequestMapping("/json")
public class UserController {

    @RequestMapping("/user")
    public ResultVo getUser(){
        return ResultVo.ok().data("user",new User(1L,"张三",null));
    }

    @RequestMapping("/list")
    public ResultVo getUserList(){
        List<User> userList=new ArrayList<>();
        User user1 = new User(2L, "李四", "lisi");
        User user2 = new User(3L, "王五", null);
        userList.add(user1);
        userList.add(user2);
        return ResultVo.ok()
                .message(ResultEnum.SUCCESS_SELECT.getMessage())
                .data("userList",userList);
    }

    @RequestMapping("/map")
    public ResultVo getUserMap(){
        Map<String, Object> map = new HashMap<>();
        User user = new User(4L, "赵六", "zhaoaliu");
        map.put("用户信息",user);
        map.put("用户年龄",null);
        return ResultVo.ok()
                .status(ResultEnum.SUCCESS_SELECT.getStatus())
                .message(ResultEnum.SUCCESS_SELECT.getMessage())
                .data(map);
    }

}
```

### 3.测试

![gh4ewghyweghyehywg](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gh4ewghyweghyehywg.5tlb9vm54r80.webp)

![h4qwghy4gyhwgyh5](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/h4qwghy4gyhwgyh5.4mm5c64thse0.webp)

![huj45werh4erhwbb45eswrh](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/huj45werh4erhwbb45eswrh.4x0luiy3z120.webp)

