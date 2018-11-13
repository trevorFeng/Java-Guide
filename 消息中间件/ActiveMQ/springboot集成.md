##### 1.添加依赖：
```angularjs
<!-- activemq -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-pool</artifactId>
</dependency>
```
##### 2.在application.properties中加入activemq的配置
```angularjs
spring.activemq.broker-url=tcp://192.168.74.135:61616
spring.activemq.user=admin
spring.activemq.password=admin
spring.activemq.pool.enabled=true
spring.activemq.pool.max-connections=50
spring.activemq.pool.expiry-timeout=10000
spring.activemq.pool.idle-timeout=30000
```
##### 3.创建一个消息生产者
```java
@Component
public class JMSProducer {
    @Autowired
    private JmsTemplate jmsTemplate;

    public void sendMessage(Destination destination,String message) {
        this.jmsTemplate.convertAndSend(destination,message);
    }
}
```
##### 4.创建一个消息消费者
```java
@Component
public class JMSConsumer {
    private final static Logger logger = LoggerFactory.getLogger(JMSConsumer.class);

    @JmsListener(destination = "springboot.queue.test")
    public void receiveQueue(String msg) {
        logger.info("接收到消息：{}",msg);
    }
}
```
##### 5.测试类
```java
public class JmsTest extends BaseTest{
    @Autowired
    private JMSProducer jmsProducer;

    @Test
    public void testJms() {
        Destination destination = new ActiveMQQueue("springboot.queue.test");

        for (int i=0;i<10;i++) {
            jmsProducer.sendMessage(destination,"hello,world!" + i);
        }
    }
}
```
##### BaseTest代码如下：
```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = com.sample.activity.web.Application.class)
public abstract class BaseTest {
}
```

##### 6.发送和接收TOPIC消息
默认只能发送和接收queue消息，如果要发送和接收topic消息，需要在application.properties文件中加入：
```angularjs
pring.jms.pub-sub-domain=true
```
发送和接收的代码同queue一样。 
##### 7.支持同时发送和接收queue/topic
- 新建一个JMS的配置类
```java
@Configuration
public class JmsConfig {
    public final static String TOPIC = "springboot.topic.test";
    public final static String QUEUE = "springboot.queue.test";
    @Bean
    public Queue queue() {
        return new ActiveMQQueue(QUEUE);
    }

    @Bean
    public Topic topic() {
        return new ActiveMQTopic(TOPIC);
    }

    // topic模式的ListenerContainer
    @Bean
    public JmsListenerContainerFactory<?> jmsListenerContainerTopic(ConnectionFactory activeMQConnectionFactory) {
        DefaultJmsListenerContainerFactory bean = new DefaultJmsListenerContainerFactory();
        bean.setPubSubDomain(true);
        bean.setConnectionFactory(activeMQConnectionFactory);
        return bean;
    }
    // queue模式的ListenerContainer
    @Bean
    public JmsListenerContainerFactory<?> jmsListenerContainerQueue(ConnectionFactory activeMQConnectionFactory) {
        DefaultJmsListenerContainerFactory bean = new DefaultJmsListenerContainerFactory();
        bean.setConnectionFactory(activeMQConnectionFactory);
        return bean;
    }

}
```
- 消息消费者的代码改成如下：
```java
@Component
public class JMSConsumer {
    private final static Logger logger = LoggerFactory.getLogger(JMSConsumer.class);

    @JmsListener(destination = JmsConfig.TOPIC,containerFactory = "jmsListenerContainerTopic")
    public void onTopicMessage(String msg) {
        logger.info("接收到topic消息：{}",msg);
    }

    @JmsListener(destination = JmsConfig.QUEUE,containerFactory = "jmsListenerContainerQueue")
    public void onQueueMessage(String msg) {
        logger.info("接收到queue消息：{}",msg);
    }
}
```

- 测试类：
```java
public class JmsTest extends BaseTest{
    @Autowired
    private JMSProducer jmsProducer;
    @Autowired
    private Topic topic;
    @Autowired
    private Queue queue;

    @Test
    public void testJms() {
        for (int i=0;i<10;i++) {
            jmsProducer.sendMessage(queue,"queue,world!" + i);
            jmsProducer.sendMessage(topic, "topic,world!" + i);
        }
    }
}
```