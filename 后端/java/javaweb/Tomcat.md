## 1.下载

[Tomcat官网](http://tomcat.apache.org/)

![rhtg34rdrftgvtyh56y7ae](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/rhtg34rdrftgvtyh56y7ae.2boite2hyvok.webp)

选择系统

![gtrs45rtgtgyh45frdgt](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gtrs45rtgtgyh45frdgt.mc9qie1ut6o.webp)

## 2.解压

解压到没有中文路径的文件夹中,解压路径会在配置环境变量时用到

<img src="https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/rfew54erfcgtrs.31amyy52tcg0.webp" title="" alt="rfew54erfcgtrs" data-align="inline">
```java
bin      存放启动、停止服务器和其他脚本文件
conf     存放服务器的配置文件
lib      存放Tomcat服务器的jar包
logs     存放Tomcat服务器的日志文件
temp     存放Tomcat的临时文件
webapps  Web应用的部署目录
work     Tomcat工作目录
```
## 3.配置环境变量

在电脑点击鼠标右键->点击属性>点击高级系统设置->点击环境变量->新建系统变量

![erfg456yededrfgrftgh34red](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/erfg456yededrfgrftgh34red.2q19f98oujs0.webp)

### 1.新建系统变量

变量名为`CATALINA_HOME`，变量值为`Tomcat解压文件夹的路径`

### 2.编辑系统变量Path

`Path`中双击空白处或新建即可在末尾加上`%CATALINA_HOME%\bin`

## 4.配置信息

找到`Tomcat`解压目录下的 `bin` 目录，分别在 `startup.bat` 和 `shutdown.bat` 文件中添加如下 [一般可解决启动`startup.bat`一闪而过的问题]：

```batch
SET JAVA_HOME=F:\jdk\jdk8.0                                  [ 你自己的 Java jdk目录 ]
SET TOMCAT_HOME=F:\tomcat\apache-tomcat-9.0.70               [ 你自己的 Tomcat解压目录 ]
```

然后在 `cmd` 中输入 `startup.bat` 能看到如下即为安装成功：

![gtre345tgyhw34rgtfrvd](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/gtre345tgyhw34rgtfrvd.2gwl78xrb960.webp)

## 5.乱码解决

在 `Tomcat` 解压目录中找到 `conf`目录下的 `logging.properties`

找到 `java.util.logging.ConsoleHandler.encoding` ，将其值修改为 `GBK` 即可，如下：

```bash
java.util.logging.ConsoleHandler.encoding = GBK
```

## 6.启动

步骤4运行 `startup.bat` 后不要关闭窗口，在浏览器输入 `localhost:8080`，出现如下即为成功：

![drfg4tgrtghs](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/drfg4tgrtghs.59eqwei7oko0.webp)

## 7.not found解决

1.若关闭 `Tomcat` 窗口，则会显示 `not found`，重新启动

2.端口号占用也会导致显示 `not found`，需要修改端口号，具体如下：

```java
1.在 `Tomcat` 解压目录中找到 `conf`目录下的 `server.xml`
2.找到<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"/>
修改 `port` 的值 [端口号范围处于0-65535]
```

![hrt534ghrthytage](https://cdn.staticaly.com/gh/quinhua/pics@main/markdown/hrt534ghrthytage.6giqiyvo6zo0.webp)

## 8.本地项目运行

在 `Tomcat` 解压目录中找到 `webapps`目录，将自己的项目或项目压缩包war放进来运行即可
