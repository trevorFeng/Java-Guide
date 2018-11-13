##### consumer获取消息是pull还是（broker的主动 push）
默认情况下，mq服务器（broker）采用异步方式向客户端主动推送消息(push)。
也就是说broker在向某个消费者会话推送消息后，不会
等待消费者响应消息，直到消费者处理完消息以后，主动向broker返回处理结果

##### broker的prefetchsize参数：“预取消息数量“

broker端一旦有消息，就主动按照默认设置的规则推送给当前活动的消费者。
 每次推送都有一定的数量限制，而这个数量就是prefetchSize
 
 Queue
 
 - 持久化消息   prefetchSize=1000（默认值）
 - 非持久化消息 1000
 
 topic
 
 - 持久化消息        100
 - 非持久化消息      32766

假如prefetchSize=0 . 此时对于consumer来说，就是一个pull模式

##### 消息确认
ACK_TYPE，消费端和broker交换ack指令的时候，还需要告知broker  ACK_TYPE。

ACK_TYPE表示确认指令的类型，broker可以根据不同的ACK_TYPE去针对当前消息做不同的应对策略

- REDELIVERED_ACK_TYPE (broker会重新发送该消息)  重发侧策略
- DELIVERED_ACK_TYPE  消息已经接收，但是尚未处理结束
- STANDARD_ACK_TYPE  表示消息处理成功


