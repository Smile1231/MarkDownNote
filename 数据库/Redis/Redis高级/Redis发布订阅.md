# Redis发布订阅


Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。微信、
微博、关注系统！

Redis 客户端可以订阅任意数量的频道。

订阅/发布消息图：

第一个：消息发送者， 第二个：频道 第三个：消息订阅者！
![image.png](https://i.loli.net/2020/12/10/DB7uzngTAlRO4by.png)

下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的
关系：
![image.png](https://i.loli.net/2020/12/10/1hJRPkSUxTDuWsp.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户
端： 
![image.png](https://i.loli.net/2020/12/10/uzlj9fb23KLaerw.png)

> 命令

这些命令被广泛用于构建即时通信应用，比如网络聊天室(chatroom)和实时广播、实时提醒等。

![image.png](https://i.loli.net/2020/12/11/HWzekTxsLGJKQSt.png)


> 测试

**订阅端:**

```sql
127.0.0.1:6379> SUBSCRIBE kuangshenshuo  # 订阅一个频道 kuangshenshuo Reading messages... (press Ctrl-C to quit) 
1) "subscribe" 
2) "kuangshenshuo"
3) (integer) 1
# 等待读取推送的信息 
1) "message"  # 消息 
2) "kuangshenshuo"  # 那个频道的消息
3) "hello,kuangshen"  # 消息的具体内容

1) "message"
2) "kuangshenshuo"
3) "hello,redis"
```
**发送端:**


```sql
127.0.0.1:6379> PUBLISH kuangshenshuo "hello,kuangshen"   # 发布者发布消息到频道！
(integer) 1
127.0.0.1:6379> PUBLISH kuangshenshuo "hello,redis"   # 发布者发布消息到频道！
(integer) 1 
127.0.0.1:6379> 
```
> 原理

Redis是使用C实现的，通过分析 Redis 源码里的 pubsub.c 文件，了解发布和订阅机制的底层实现，籍 此加深对 Redis 的理解。

Redis 通过 PUBLISH 、SUBSCRIBE 和 PSUBSCRIBE 等命令实现发布和订阅功能。

微信：

通过 SUBSCRIBE 命令订阅某频道后，redis-server里维护了一个字典，字典的键是一个个 频道！， 而字典的值则是一个链表，链表中保存了所有订阅这个channel客户端。SUBSCRIBE 命令的关键， 就是将客户端添加到给定 channel的订阅链表中。

![image.png](https://i.loli.net/2020/12/11/AcIqOSE3nyplQKM.png)

通过 PUBLISH 命令向订阅者发送消息，redis-server会使用给定的频道作为键，它所维护的 channel 字典中查找记录了订阅这个频道的所有客户端的链表，遍历这表，将消息发布给所有订阅者。
 
 
 
Pub/Sub 从字面上理解就是发布（Publish）与订阅（Subscribe），在Redis中，可以设定对某一个 key值进行消息发布及消息订阅，当一个key值上进行了消息发后，所有订阅它的客户端都会收到相应的消息。这一功能明显的用法就是用作实时消系统，比如普通的即时聊天，群聊等功能。

**使用场景：**

1. 实时消息系统！
2. 事实聊天！（频道当做聊天室，将信息回显给所有人即可！） 
3. 订阅，关注系统都是可以的！ 稍微复杂的场景我们就会使用 消息中间件MQ（）



