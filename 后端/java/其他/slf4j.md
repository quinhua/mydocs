## 1.安装

```xml
    <properties>
        <lombok.version>1.18.20</lombok.version>
        <slf4j.version>1.7.30</slf4j.version>
        <logback.version>1.2.5</logback.version>
    </properties>

    <dependencies>
 		<!--lombok start-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
        <!--lombok end-->

  		<!--slf4j start-->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>${logback.version}</version>
        </dependency>
        <!--slf4j end-->
    </dependencies>
```

## 2.使用

```java
//使用@Slf4j注解实现日志输出

@Slf4j
public class UserMapperTest {
    @Test
    public void findAll() throws Exception {
        List<User> userList = userMapper.findAll();
        log.info("{}",userList);
    }
}
```

