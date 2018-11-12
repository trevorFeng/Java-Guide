##### JMS的概念和规范
![aa](../../imgs/20181112-7.jpg)

##### 消息传递域
点对点（P2P）
- 1.每个消息只能有一个消费者
- 2.消息的生产者和消费者之间没有时间上的相关性。无论消费者在生产者发送消息的时候是否处于运行状态，都可以提取消息

发布订阅(pub/sub)
- 1.每个消息可以有多个消费者
- 2.消息的生产者和消费者之间存在时间上的相关性，订阅一个主题的消费者只能消费自它订阅之后发布的消息。
JMS规范允许提供客户端创建持久订阅

![AA](../../imgs/20181112-8.png)

##### JMS API
- ConnectionFactory    连接工厂
- Connection    封装客户端与JMS provider之间的一个虚拟的连接
- Session	生产和消费消息的一个单线程上下文； 用于创建producer、consumer、message、queue..
- Destination	消息发送或者消息接收的目的地
- MessageProducer/consumer	消息生产者/消费者

##### 消息组成
- 消息头

包含消息的识别信息和路由信息

- 消息体

TextMessage

MapMessage

BytesMessage

StreamMessage   输入输出流

ObjectMessage  可序列化对象

##### JMS的可靠性机制
JMS消息之后被确认后，才会认为是被成功消费。消息的消费包含三个阶段： 
客户端接收消息、客户端处理消息、消息被确认

- 事务性会话
```angularjs
Session session = connection.createSession(true, Session.AUTO_ACKNOWLEDGE);
```
设置为true的时候，消息会在session.commit以后自动签收
- 非事务性会话
```angularjs
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
```
在该模式下，消息何时被确认取决于创建会话时的应答模式
- AUTO_ACKNOWLEDGE
当客户端成功从recive方法返回以后，
或者[MessageListener.onMessage] 方法成功返回以后，会话会自动确认该消息
- CLIENT_ACKNOWLEDGE
客户端通过调用消息的textMessage.acknowledge();确认消息。
在这种模式中，如果一个消息消费者消费一共是10个消息，那么消费了5个消息，
然后在第5个消息通过textMessage.acknowledge()，那么之前的所有消息都会被消确认
- DUPS_OK_ACKNOWLEDGE
延迟确认



