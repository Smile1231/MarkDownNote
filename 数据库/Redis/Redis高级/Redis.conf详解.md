# Redis.conf详解

 启动的时候，就通过配置文件来启动！
工作中，一些小小的配置，可以让你脱颖而出！
行家有没有，出手就知道

> 单位

![image.png](https://i.loli.net/2020/12/10/oA9uldMJS2QLq1E.png)
1. 配置文件 unit单位 对大小写不敏感！
![image.png](https://i.loli.net/2020/12/10/y2Y93FMZNVmK1Gv.png)

就是好比我们学习Spring、Improt， include

> 网络

```
bind 127.0.0.1 # 绑定的ip
protected-mode yes # 保护模式
port 6379 # 端口设置

```
> 通用 GENERAL


```sql
daemonize yes # 以守护进程的方式运行，默认是 no，我们需要自己开启为yes！
pidfile /var/run/redis_6379.pid # 如果以后台的方式运行，我们就需要指定一个 pid 文件！
# 日志
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably) 生产环境
# warning (only very important / critical messages are logged)
loglevel notice
logfile "" # 日志的文件位置名
databases 16 # 数据库的数量，默认是 16 个数据库
always-show-logo yes # 是否总是显示LOGO
```

> 快照

持久化， 在规定的时间内，执行了多少次操作，则会持久化到文件 .rdb. aof
redis 是内存数据库，如果没有持久化，那么数据断电及失！
```sql
# 如果900s内，如果至少有一个1 key进行了修改，我们及进行持久化操作
save 900 1
# 如果300s内，如果至少10 key进行了修改，我们及进行持久化操作
save 300 10
# 如果60s内，如果至少10000 key进行了修改，我们及进行持久化操作
save 60 10000
# 我们之后学习持久化，会自己定义这个测试！
stop-writes-on-bgsave-error yes # 持久化如果出错，是否还需要继续工作！
rdbcompression yes # 是否压缩 rdb 文件，需要消耗一些cpu资源！
rdbchecksum yes # 保存rdb文件的时候，进行错误的检查校验！
dir ./ # rdb 文件保存的目录！
```

> REPLICATION 复制，我们后面讲解主从复制的，时候再进行讲解

> SECURITY 安全
 
 
```sql
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> config get requirepass # 获取redis的密码
1) "requirepass"
2) ""
127.0.0.1:6379> config set requirepass "123456" # 设置redis的密码
OK
127.0.0.1:6379> config get requirepass # 发现所有的命令都没有权限了
(error) NOAUTH Authentication required.
127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456 # 使用密码进行登录！
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "123456"
```
> 限制 CLIENTS


```sql
maxclients 10000 # 设置能连接上redis的最大客户端的数量
maxmemory <bytes> # redis 配置最大的内存容量
maxmemory-policy noeviction # 内存到达上限之后的处理策略
1、volatile-lru：只对设置了过期时间的key进行LRU（默认值）
2、allkeys-lru ： 删除lru算法的key
3、volatile-random：随机删除即将过期key
4、allkeys-random：随机删除
5、volatile-ttl ： 删除即将过期的
6、noeviction ： 永不过期，返回错误
```


> APPEND ONLY 模式 aof配置

```sql
appendonly no # 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下，
rdb完全够用！
appendfilename "appendonly.aof" # 持久化的文件的名字
# appendfsync always # 每次修改都会 sync。消耗性能
appendfsync everysec # 每秒执行一次 sync，可能会丢失这1s的数据！
# appendfsync no # 不执行 sync，这个时候操作系统自己同步数据，速度最快！
```

具体的配置，我们在 Redis持久化 中去给大家详细详解！







