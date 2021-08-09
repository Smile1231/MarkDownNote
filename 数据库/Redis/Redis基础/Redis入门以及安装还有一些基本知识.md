# Redis 入门

## 概述

> Redis是什么？

Redis（Remote Dictionary Server )，即远程字典服务。

是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、**Key-Value**数据库，并提供多种语言的API。

与memcached一样，为了保证效率，**数据都是缓存在内存中**。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。
> Redis能该干什么？

1. 内存存储、持久化，内存是断电即失的，所以需要持久化（RDB、AOF）
2. 高效率、用于高速缓冲
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器(eg：浏览量)
6. 。。。

> 特性

- 多样的数据类型

- 持久化

- 集群

- 事务
- ...
…


## 环境搭建

官网：https://redis.io/

推荐使用Linux服务器学习。

windows版本的Redis已经停更很久了…

## Windows安装
https://github.com/dmajkic/redis

1. 解压安装包
![image](https://img-blog.csdnimg.cn/20200820103922318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

2. 开启redis-server.exe

3. 启动redis-cli.exe测试

![image](https://img-blog.csdnimg.cn/20200820103950934.png#pic_center)


## Linux安装

1. 下载安装包！redis-5.0.8.tar.gz

2. 解压Redis的安装包！程序一般放在 /opt 目录下

![image](https://img-blog.csdnimg.cn/20200820104016426.png#pic_center)

3. 基本环境安装

```
yum install gcc-c++
# 然后进入redis目录下执行
# 如果安装redies6.X 版本 , 出现error错误 , 重新回退到5.X版本 ,
## 或者安装一个依赖  yum install tcl  出错是因为6.X的redis需要5.3以上的版本的gcc 
make
# 然后执行
make install
```
![image](https://img-blog.csdnimg.cn/20200820104048327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

4. redis默认安装路径 /usr/local/bin
![image](https://img-blog.csdnimg.cn/20200820104140692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

5. 将redis的配置文件复制到 程序安装目录 /usr/local/bin/kconfig下

![image](https://img-blog.csdnimg.cn/20200820104157817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

6. redis默认不是后台启动的，需要修改配置文件！

![image](https://img-blog.csdnimg.cn/20200820104213706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

7. 通过制定的配置文件启动redis服务

![image](https://img-blog.csdnimg.cn/20200820104228556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

8. 使用redis-cli连接指定的端口号测试，Redis的默认端口6379

![image](https://img-blog.csdnimg.cn/20200820104243223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

9. 查看redis进程是否开启

![image](https://img-blog.csdnimg.cn/20200820104300532.png#pic_center)
10. 关闭Redis服务 shutdown

![image](https://img-blog.csdnimg.cn/20200820104314297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

11. 再次查看进程是否存在

12. 后面我们会使用单机多Redis启动集群测试

## 测试性能

**redis-benchmark**                             Redis官方提供的性能测试工具，参数选项如下：
 
![image](https://img-blog.csdnimg.cn/20200513214125892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70)

### 简单测试：


```
# 测试：100个并发连接 100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000

```
![image](https://img-blog.csdnimg.cn/20200820104343472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)


## 基础知识
> redis默认有16个数据库

![image](https://img-blog.csdnimg.cn/20200820104357466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

默认使用的第0个;

16个数据库为：DB 0~DB 15
默认使用DB 0 ，可以使用select n切换到DB n，dbsize可以查看当前数据库的大小，与key数量相关。


```sh
127.0.0.1:6379> config get databases # 命令行查看数据库数量databases
1) "databases"
2) "16"

127.0.0.1:6379> select 8 # 切换数据库 DB 8
OK
127.0.0.1:6379[8]> dbsize # 查看数据库大小
(integer) 0

# 不同数据库之间 数据是不能互通的，并且dbsize 是根据库中key的个数。
127.0.0.1:6379> set name sakura 
OK
127.0.0.1:6379> SELECT 8
OK
127.0.0.1:6379[8]> get name # db8中并不能获取db0中的键值对。
(nil)
127.0.0.1:6379[8]> DBSIZE
(integer) 0
127.0.0.1:6379[8]> SELECT 0
OK
127.0.0.1:6379> keys *
1) "counter:__rand_int__"
2) "mylist"
3) "name"
4) "key:__rand_int__"
5) "myset:__rand_int__"
127.0.0.1:6379> DBSIZE # size和key个数相关
(integer) 5

```

==keys *== ：查看当前数据库中所有的key。

==flushdb==：清空当前数据库中的键值对。

==flushall==：清空所有数据库的键值对。

> Redis是单线程的，Redis是基于内存操作的。

所以Redis的性能瓶颈不是CPU,而是机器内存和网络带宽。

那么为什么Redis的速度如此快呢，性能这么高呢？QPS达到10W+

> Redis为什么单线程还这么快？

- 误区1：高性能的服务器一定是多线程的？
- 误区2：多线程（CPU上下文会切换！）一定比单线程效率高！


**核心**：Redis是将所有的数据放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文会切换：耗时的操作！），==对于内存系统来说，如果没有上下文切换效率就是最高的，多次读写都是在一个CPU上的==，在内存存储数据情况下，单线程就是最佳的方案。











