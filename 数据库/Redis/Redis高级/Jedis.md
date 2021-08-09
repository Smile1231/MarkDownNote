# Jedis

我们要使用 Java 来操作 Redis，知其然并知其所以然，授人以渔！ 学习不能急躁，慢慢来会很快！

> 什么是Jedis 是 Redis 官方推荐的 java连接开发工具！ 使用Java 操作Redis 中间件！如果你要使用
java操作redis，那么一定要对Jedis 十分的熟悉！

> 测试

1. 导入对应的依赖

```java
<!--导入jedis的包-->
<dependencies>
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
         <version>3.2.0</version>
    </dependency>
    <!--fastjson-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
</dependencies>
```
2. 、编码测试：
- 连接数据库
- 操作命令
- 断开连接！



```
package com.kuang;
import redis.clients.jedis.Jedis;

public class TestPing {
    public static void main(String[] args) {
        // 1、 new Jedis 对象即可
        Jedis jedis = new Jedis("127.0.0.1",6379);
        // jedis 所有的命令就是我们之前学习的所有指令！所以之前的指令学习很重要！
        System.out.println(jedis.ping());
    }
}

```

输出：
![image.png](https://i.loli.net/2020/12/10/ItPxwTasOHNMR8U.png)

# 常用的API
- String
- List
- Set
- Hash
- Zset

> 所有的api命令，就是我们对应的上面学习的指令，一个都没有变化！

> 事务


```java
public class TestTX {
    public static void main(String[] args) {
            Jedis jedis = new Jedis("127.0.0.1", 6379);
            jedis.flushDB();
            JSONObject jsonObject = new JSONObject();
            jsonObject.put("hello","world");
            jsonObject.put("name","kuangshen");
            // 开启事务
            Transaction multi = jedis.multi();
            String result = jsonObject.toJSONString();
            // jedis.watch(result)
        try {
            multi.set("user1",result);
            multi.set("user2",result);
            int i = 1/0 ; // 代码抛出异常事务，执行失败！
            multi.exec(); // 执行事务！
        } catch (Exception e) {
             multi.discard(); // 放弃事务
               e.printStackTrace();
        } finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            jedis.close(); // 关闭连接
        }
    }
}

```





