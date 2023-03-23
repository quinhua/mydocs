## 1.java: 错误: 不支持发行版本 5

```xml
# jdk版本不一致

# pom.xml配置文件添加以下 
	<properties>
        <maven.compiler.source>更换自己的jdk版本</maven.compiler.source>
        <maven.compiler.target>更换自己的jdk版本</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding
    </properties>
```

## 2.Cannot access defaults field of Properties

```xml
# 打war包时报错 `Cannot access defaults field of Properties`

# pom.xml配置文件中添加以下
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.2.0</version>
        <configuration>
          <failOnMissingWebXml>false</failOnMissingWebXml>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

## 3.web.xml配置监听器找不到ContextLoaderListener

```xml
# 直接复制粘贴
org.springframework.web.context.ContextLoaderListener

	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```

