
# Hyperloglog

> 什么是基数？

A {1,3,5,7,8,7}
B{1，3,5,7,8}
基数（不重复的元素） = 5，可以接受误差！

> 简介

Redis 2.8.9 版本就更新了 Hyperloglog 数据结构！
Redis Hyperloglog 基数统计的算法！
优点：占用的内存是固定，2^64 不同的元素的技术，只需要废 12KB内存！如果要从内存角度来比较的
话 Hyperloglog 首选！

**网页的 UV （一个人访问一个网站多次，但是还是算作一个人！）**

传统的方式， set 保存用户的id，然后就可以统计 set 中的元素数量作为标准判断 !

这个方式如果保存大量的用户id，就会比较麻烦！我们的目的是为了计数，而不是保存用户id；
0.81% 错误率！ 统计UV任务，可以忽略不计的！

> 测试使用


```sh
127.0.0.1:6379> PFadd mykey a b c d e f g h i j # 创建第一组元素 mykey
(integer) 1
127.0.0.1:6379> PFCOUNT mykey # 统计 mykey 元素的基数数量
(integer) 10
127.0.0.1:6379> PFadd mykey2 i j z x c v b n m # 创建第二组元素 mykey2
(integer) 1
127.0.0.1:6379> PFCOUNT mykey2
(integer) 9
127.0.0.1:6379> PFMERGE mykey3 mykey mykey2 # 合并两组 mykey mykey2 => mykey3 并集
OK
127.0.0.1:6379> PFCOUNT mykey3 # 看并集的数量！
(integer) 15

```

如果允许容错，那么一定可以使用 Hyperloglog ！
如果不允许容错，就使用 set 或者自己的数据类型即可!





