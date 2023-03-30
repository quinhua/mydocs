```bash
1、准好两个版本的jdk [`jdk7`和`jdk17`]
文件路径分别为: `C:/jdk7`和`C:/jdk17`
2、打开电脑设置->系统->系统信息->高级系统设置->环境变量->系统变量
3、新建三个系统变量分别为
    ①变量名：`JAVA_HOME`，变量值：`%JAVA_HOME_17%`
    ②变量名：`JAVA_HOME_8`，变量值：`C:/jdk7`
    ③变量名：`JAVA_HOME_17`，变量值：`C:/jdk17`
    # ④是可选项，高版本不需要，默认自动生成
    ④变量名：`CLASSPATH`，变量值：``.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar`
4、继续找到系统环境变量中的`path`，新建变量
变量值：`%JAVA_HOME%\bin`
# `path`环境变量中存在`C:\Program Files\Common Files\Oracle\Java\javapath`，则需要将刚刚新建的变量值上移动到其之前
# 或创建完变量值后直接上移到最高
```

