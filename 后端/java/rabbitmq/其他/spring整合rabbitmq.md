## 1.创建项目

```bash
1.创建父项目`spring-rabbitmq`并删除`src`
2.创建子项目`spring_rabbitmq_producer`和`spring_rabbitmq_consumer`,并添加以下依赖

    <properties>
        <spring.version>5.1.7.RELEASE</spring.version>
        <spring-rabbit.version>2.1.8.RELEASE</spring-rabbit.version>
        <junit.version>4.12</junit.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.amqp</groupId>
            <artifactId>spring-rabbit</artifactId>
            <version>${spring-rabbit.version}</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## 2.生产者

### 1.创建配置文件

```
# rabbitmq.properties

rabbitmq.host=192.168.66.110
rabbitmq.port=5672
rabbitmq.username=admin
rabbitmq.password=admin
rabbitmq.virtual-host=/
```

```xml
# spring-rabbitmq.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/rabbit
       http://www.springframework.org/schema/rabbit/spring-rabbit.xsd">

    <!--加载配置文件-->
    <context:property-placeholder location="classpath:rabbitmq.properties"/>

    <!-- 定义rabbitmq connectionFactory -->
    <rabbit:connection-factory
            id="connectionFactory"
            host="${rabbitmq.host}"
            port="${rabbitmq.port}"
            username="${rabbitmq.username}"
            password="${rabbitmq.password}"
            virtual-host="${rabbitmq.virtual-host}"
    />

    <!--定义rabbitTemplate对象操作可以在代码中方便发送消息-->
    <rabbit:template id="rabbitTemplate" connection-factory="connectionFactory"/>

    <!--定义管理交换机、队列-->
    <rabbit:admin connection-factory="connectionFactory"/>

    <!--定义持久化队列,不存在则自动创建；不绑定到交换机则绑定到默认交换机
    默认交换机类型为direct,名字为："",路由键为队列的名称
    -->
    <rabbit:queue id="spring_queue" name="spring_queue" auto-declare="true"/>

    <!--发布订阅-->
    <!--定义广播交换机中的持久化队列，不存在则自动创建-->
    <rabbit:queue id="spring_fanout_queue_1" name="spring_fanout_queue_1" auto-declare="true"/>
    <rabbit:queue id="spring_fanout_queue_2" name="spring_fanout_queue_2" auto-declare="true"/>

    <!--定义广播类型交换机并绑定队列-->
    <rabbit:fanout-exchange id="spring_fanout_exchange" name="spring_fanout_exchange" auto-declare="true">
        <rabbit:bindings>
            <rabbit:binding queue="spring_fanout_queue_1"/>
            <rabbit:binding queue="spring_fanout_queue_2"/>
        </rabbit:bindings>
    </rabbit:fanout-exchange>

    <!--路由-->
    <!--定义广播交换机中的持久化队列，不存在则自动创建-->
    <rabbit:queue id="spring_topic_queue_1" name="spring_topic_queue_1" auto-declare="true"/>
    <rabbit:queue id="spring_topic_queue_2" name="spring_topic_queue_2" auto-declare="true"/>
    <rabbit:queue id="spring_topic_queue_3" name="spring_topic_queue_3" auto-declare="true"/>

    <!--定义广播类型交换机并绑定队列-->
    <rabbit:topic-exchange id="spring_topic_exchange" name="spring_topic_exchange" auto-declare="true">
        <rabbit:bindings>
            <rabbit:binding pattern="user.*" queue="spring_topic_queue_1"/>
            <rabbit:binding pattern="user.#" queue="spring_topic_queue_2"/>
            <rabbit:binding pattern="admin.#" queue="spring_topic_queue_3"/>
        </rabbit:bindings>
    </rabbit:topic-exchange>

</beans>
```

### 2.发送消息测试

```java
# 创建测试文件 `spring_rabbitmq_producer/src/test/ProducerTest.java`
    
@RunWith(SpringRunner.class)
@ContextConfiguration(locations = "classpath:spring-rabbitmq.xml")
public class ProducerTest {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    /**
     * 只发队列消息
     * 默认交换机类型为 direct
     * 交换机的名称为空,路由键为队列的名称
     */
    @Test
    public void queueTest(){
        rabbitTemplate.convertAndSend("spring_queue","这是队列spring_queue发送的的消息");
    }

    @Test
    public void fanoutTest(){
        for (int i = 1; i <= 10; i++) {
            rabbitTemplate.convertAndSend("spring_fanout_exchange","","这是广播交换机spring_fanout_exchange的广播消息"+"---"+i);
        }
    }

    /**
     * 通配符
     * 交换机类型为 topic
     * 匹配路由键的通配符,*表示一个单词,#表示多个单词
     * 绑定到该交换机的匹配队列能够收到对应消息
     */
    @Test
    public void topicTest(){
        rabbitTemplate.convertAndSend("spring_topic_exchange","user.vip","这是广播交换机spring_topic_exchange的user.svip广播消息");
        rabbitTemplate.convertAndSend("spring_topic_exchange","user.vip.1","这是广播交换机spring_topic_exchange的user.vip.1广播消息");
        rabbitTemplate.convertAndSend("spring_topic_exchange","user.vip.2","这是广播交换机spring_topic_exchange的user.vip.2广播消息");
        rabbitTemplate.convertAndSend("spring_topic_exchange","admin.svip","这是广播交换机spring_topic_exchange的admin.svip广播消息");
    }

}
```

## 3.消费者

### 1.创建配置文件

```xml
# rabbitmq.properties

rabbitmq.host=192.168.66.110
rabbitmq.port=5672
rabbitmq.username=admin
rabbitmq.password=admin
rabbitmq.virtual-host=/
```

```xml
# spring-rabbitmq.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/rabbit
       http://www.springframework.org/schema/rabbit/spring-rabbit.xsd">

    <!--加载配置文件-->
    <context:property-placeholder location="classpath:rabbitmq.properties"/>

    <!-- 定义rabbitmq connectionFactory -->
    <rabbit:connection-factory id="connectionFactory" host="${rabbitmq.host}"
                               port="${rabbitmq.port}"
                               username="${rabbitmq.username}"
                               password="${rabbitmq.password}"
                               virtual-host="${rabbitmq.virtual-host}"/>

    <bean id="springQueueListener" class="com.qh.rabbitmq.listener.SpringQueueListener"/>
    <bean id="fanoutListener1" class="com.qh.rabbitmq.listener.FanoutListener1"/>
    <bean id="fanoutListener2" class="com.qh.rabbitmq.listener.FanoutListener2"/>
    <bean id="topicListener1" class="com.qh.rabbitmq.listener.TopicListener1"/>
    <bean id="topicListener2" class="com.qh.rabbitmq.listener.TopicListener2"/>
    <bean id="topicListener3" class="com.qh.rabbitmq.listener.TopicListener3"/>

    <!--定义监听器-->
    <rabbit:listener-container connection-factory="connectionFactory" auto-declare="true">
        <rabbit:listener ref="springQueueListener" queue-names="spring_queue"/>
        <rabbit:listener ref="fanoutListener1" queue-names="spring_fanout_queue_1"/>
        <rabbit:listener ref="fanoutListener2" queue-names="spring_fanout_queue_2"/>
        <rabbit:listener ref="topicListener1" queue-names="spring_topic_queue_1"/>
        <rabbit:listener ref="topicListener2" queue-names="spring_topic_queue_2"/>
        <rabbit:listener ref="topicListener3" queue-names="spring_topic_queue_3"/>
    </rabbit:listener-container>

</beans>
```

### 2.创建消息监听器

#### 1.队列监听器

```java
# `spring_rabbitmq_consumer/src/java/com/qh/rabbitmq/listener/SpringQueueListener.java`

public class SpringQueueListener implements MessageListener {
    @Override
    public void onMessage(Message message) {
        try {
            String msg = new String(message.getBody(), StandardCharsets.UTF_8);
            System.out.printf("接收路由名称为：%s,路由键为：%s,队列名为：%s的消息：%s \n",
                    message.getMessageProperties().getReceivedExchange(),
                    message.getMessageProperties().getReceivedRoutingKey(),
                    message.getMessageProperties().getConsumerQueue(),
                    msg);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 2.广播监听器1

```java
# `spring_rabbitmq_consumer/src/java/com/qh/rabbitmq/listener/FanoutListener1.java`

public class FanoutListener1 implements MessageListener {
    @Override
    public void onMessage(Message message) {
        try {
            String msg = new String(message.getBody(), StandardCharsets.UTF_8);
            System.out.printf("接收路由名称为：%s,路由键为：%s,队列名为：%s的消息：%s \n",
                    message.getMessageProperties().getReceivedExchange(),
                    message.getMessageProperties().getReceivedRoutingKey(),
                    message.getMessageProperties().getConsumerQueue(),
                    msg);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3.广播监听器2

```java
# `spring_rabbitmq_consumer/src/java/com/qh/rabbitmq/listener/FanoutListener2.java`
    
public class FanoutListener2 implements MessageListener {
    @Override
    public void onMessage(Message message) {
        try {
            String msg = new String(message.getBody(), StandardCharsets.UTF_8);
            System.out.printf("接收路由名称为：%s,路由键为：%s,队列名为：%s的消息：%s \n",
                    message.getMessageProperties().getReceivedExchange(),
                    message.getMessageProperties().getReceivedRoutingKey(),
                    message.getMessageProperties().getConsumerQueue(),
                    msg);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 4.星号通配符监听器

```java
# `spring_rabbitmq_consumer/src/java/com/qh/rabbitmq/listener/TopicListener1.java`
    
public class TopicListener1 implements MessageListener {
    @Override
    public void onMessage(Message message) {
        try {
            String msg = new String(message.getBody(), StandardCharsets.UTF_8);
            System.out.printf("接收路由名称为：%s,路由键为：%s,队列名称为：%s的消息：%s\n",
                    message.getMessageProperties().getReceivedExchange(),
                    message.getMessageProperties().getReceivedRoutingKey(),
                    message.getMessageProperties().getConsumerQueue(),
                    msg);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

#### 5.井号通配符监听器1

```java
# `spring_rabbitmq_consumer/src/java/com/qh/rabbitmq/listener/TopicListener2.java`
    
public class TopicListener2 implements MessageListener {
    @Override
    public void onMessage(Message message) {
        try {
            String msg = new String(message.getBody(), StandardCharsets.UTF_8);
            System.out.printf("接收路由名称为：%s,路由键为：%s,队列名称为：%s的消息：%s\n",
                    message.getMessageProperties().getReceivedExchange(),
                    message.getMessageProperties().getReceivedRoutingKey(),
                    message.getMessageProperties().getConsumerQueue(),
                    msg);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

#### 6.井号通配符监听器2

```java
# `spring_rabbitmq_consumer/src/java/com/qh/rabbitmq/listener/TopicListener3.java`
    
public class TopicListener3 implements MessageListener {
    @Override
    public void onMessage(Message message) {
        try {
            String msg = new String(message.getBody(), StandardCharsets.UTF_8);
            System.out.printf("接收路由名称为：%s,路由键为：%s,队列名称为：%s的消息：%s\n",
                    message.getMessageProperties().getReceivedExchange(),
                    message.getMessageProperties().getReceivedRoutingKey(),
                    message.getMessageProperties().getConsumerQueue(),
                    msg);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

### 3.接收消息测试

```java
# 创建测试文件 `spring_rabbitmq_consumer/src/test/ConsumerTest.java`
    
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-rabbitmq.xml")
public class ConsumerTest {
    @Test
    @SuppressWarnings("InfiniteLoopStatement")
    public void consumerTest(){
        while (true){
        }
    }
}
```





