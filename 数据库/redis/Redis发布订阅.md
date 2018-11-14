##### redis发布订阅
- Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。Redis 客户端可以订阅任意数量的频道。
- Redis的架构包括两个部分：Redis Client和Redis Server。Redis客户端负责向服务器端发送请求并接受来自服务器端的响应。服务器端负责处理客户端请求，例如，存储数据，修改数据等。 
Redis通常用作数据库，缓存以及消息系统。

##### redis发布订阅架构
Redis的发布订阅机制包括三个部分，发布者，订阅者和Channel（频道）
- 发布者和订阅者都是Redis客户端
- Channel则为Redis服务器端

发布者发布消息到频道，订阅者就能够收到消息

##### 实例
1. 创建订阅频道名为 redisChat:
~~~
redis 127.0.0.1:6379> SUBSCRIBE redisChat

Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "redisChat"
3) (integer) 1
~~~
2. 发布消息
~~~
redis 127.0.0.1:6379> PUBLISH redisChat "Redis is a great caching technique"

(integer) 1

redis 127.0.0.1:6379> PUBLISH redisChat "Learn redis by runoob.com"
~~~
3. 订阅者收到消息
~~~
# 订阅者的客户端会显示如下消息
1) "message"
2) "redisChat"
3) "Redis is a great caching technique"
1) "message"
2) "redisChat"
3) "Learn redis by runoob.com"
~~~

##### 发布订阅常用命令

命令 | 含义
---|---
PSUBSCRIBE pattern [pattern ...]  | 订阅一个或多个符合给定模式的频道
PUBSUB subcommand [argument [argument ...]]  | 查看订阅与发布系统状态
PUBLISH channel message | 将信息发送到指定的频道
PUNSUBSCRIBE [pattern [pattern ...]]  | 退订所有给定模式的频道
SUBSCRIBE channel [channel ...]  | 订阅给定的一个或多个频道的信息。
UNSUBSCRIBE [channel [channel ...]]  | 指退订给定的频道。


