# Set（集合）

set中的值是不能重读的！

```sh
##########################################################################
127.0.0.1:6379> sadd myset "hello" # set集合中添加匀速
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset "lovekuangshen"
(integer) 1
127.0.0.1:6379> SMEMBERS myset # 查看指定set的所有值
1) "hello"
2) "lovekuangshen"
3) "kuangshen"
127.0.0.1:6379> SISMEMBER myset hello # 判断某一个值是不是在set集合中！
(integer) 1
127.0.0.1:6379> SISMEMBER myset world
(integer) 0
##########################################################################
127.0.0.1:6379> scard myset # 获取set集合中的内容元素个数！
(integer) 4
##########################################################################
rem
127.0.0.1:6379> srem myset hello # 移除set集合中的指定元素
(integer) 1
127.0.0.1:6379> scard myset
(integer) 3
127.0.0.1:6379> SMEMBERS myset
1) "lovekuangshen2"
2) "lovekuangshen"
3) "kuangshen"
##########################################################################
set 无序不重复集合。抽随机！
127.0.0.1:6379> SMEMBERS myset
1) "lovekuangshen2"
2) "lovekuangshen"
3) "kuangshen"
127.0.0.1:6379> SRANDMEMBER myset # 随机抽选出一个元素
"kuangshen"
127.0.0.1:6379> SRANDMEMBER myset
"kuangshen"
127.0.0.1:6379> SRANDMEMBER myset
"kuangshen"
127.0.0.1:6379> SRANDMEMBER myset
"kuangshen"
127.0.0.1:6379> SRANDMEMBER myset 2 # 随机抽选出指定个数的元素
1) "lovekuangshen"
2) "lovekuangshen2"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "lovekuangshen"
2) "lovekuangshen2"
127.0.0.1:6379> SRANDMEMBER myset # 随机抽选出一个元素
"lovekuangshen2"
##########################################################################
删除定的key，随机删除key！
127.0.0.1:6379> SMEMBERS myset
1) "lovekuangshen2"
2) "lovekuangshen"
3) "kuangshen"
127.0.0.1:6379> spop myset # 随机删除一些set集合中的元素！
"lovekuangshen2"
127.0.0.1:6379> spop myset
"lovekuangshen"
127.0.0.1:6379> SMEMBERS myset
1) "kuangshen"
##########################################################################
将一个指定的值，移动到另外一个set集合！
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "world"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset2 "set2"
(integer) 1
127.0.0.1:6379> smove myset myset2 "kuangshen" # 将一个指定的值，移动到另外一个set集
合！
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "world"
2) "hello"
127.0.0.1:6379> SMEMBERS myset2
1) "kuangshen"
2) "set2"
##########################################################################
微博，B站，共同关注！(并集)
数字集合类：
- 差集 SDIFF
- 交集
- 并集
127.0.0.1:6379[8]> sadd key2 a
(integer) 1
127.0.0.1:6379[8]> sadd key2 b
(integer) 1
127.0.0.1:6379[8]> sadd key2 c
(integer) 1
127.0.0.1:6379[8]> sadd key3 c
(integer) 1
127.0.0.1:6379[8]> sadd key3 d
(integer) 1
127.0.0.1:6379[8]> sadd key3 e
(integer) 1
127.0.0.1:6379> SDIFF key2 key3 # 差集
1) "b"
2) "a"
127.0.0.1:6379> SINTER key2 key3 # 交集 共同好友就可以这样实现
1) "c"
127.0.0.1:6379> SUNION key2 key3 # 并集
1) "b"
2) "c"
3) "e"
4) "a"
5) "d"
```

微博，A用户将所有关注的人放在一个set集合中！将它的粉丝也放在一个集合中！
共同关注，共同爱好，二度好友，推荐好友！（六度分割理论）





