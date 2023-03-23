```xml
junit :单元测试
spring-context :spring核心
log4j-api & log4j-core :日志
lombok :简化代码
mysql-connector-java :数据库
commons-dbutils :dbutils
druid :连接池
spring-test :注解整合junit
	@RunWith(SpringJUnit4ClassRunner.class)//当前单元测试类运行在Spring容器中
	@ContextConfiguration(locations = "classpath:spring-core.xml")//根据spring-core.xml加载得到Spring容器
```

