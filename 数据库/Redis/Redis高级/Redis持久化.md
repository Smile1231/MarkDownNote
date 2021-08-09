 # Redis持久化
 
 面试和工作，持久化都是重点！
Redis 是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中
的数据库状态也会消失。所以 Redis 提供了持久化功能！

## RDB（Redis DataBase）

> 什么是RDB

在主从复制中，rdb就是备用了！从机上面！

![image.png](https://i.loli.net/2020/12/10/8bZ21fKTvtBWjdl.png)

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快
照文件直接读到内存里。

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程
都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。
这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那
RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。我们默认的就是
RDB，一般情况下不需要修改这个配置！

有时候在生产环境我们会将这个文件进行备份！

==rdb保存的文件是dump.rdb都是在我们的配置文件中快照中进行配置的！==


![image.png](https://i.loli.net/2020/12/10/CYywKnkc1j4irWQ.png)

> 触发机制 

1. save的规则满足的情况下，会自动触发rdb规则
2. 执行 flushall 命令，也会触发我们的rdb规则！
3. 退出redis，也会产生 rdb 文件！

备份就自动生成一个 dump.rdb

![image.png](https://i.loli.net/2020/12/10/MPuOmgse2VhQpiz.png)


> 如果恢复rdb文件！

1. 只需要将rdb文件放在我们redis启动目录就可以，redis启动的时候会自动检查dump.rdb 恢复其中
的数据！
2. 查看需要存在的位置

```sql
127.0.0.1:6379> config get dir
1) "dir"
2) "/usr/local/bin" # 如果在这个目录下存在 dump.rdb 文件，启动就会自动恢复其中的数据

```

> 几乎就他自己默认的配置就够用了，但是我们还是需要去学习！

**优点**：
1. 适合大规模的数据恢复！
2. 对数据的完整性要不高！
**缺点：**
1. 需要一定的时间间隔进程操作！如果redis意外宕机了，这个最后一次修改数据就没有的了！
2. fork进程的时候，会占用一定的内容空间！！

## AOF（Append Only File）

将我们的所有命令都记录下来，history，恢复的时候就把这个文件全部在执行一遍！

> 是什么 

![image.png](https://i.loli.net/2020/12/11/R9bNlX8x27iBQtM.png)


以日志的形式来记录每个写操作，将Redis执行过的所有指令记录下来（读操作不记录），只许追加文件
但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件
的内容将写指令从前到后执行一次以完成数据的恢复工作


Aof保存的是 appendonly.aof 文件

> append 
 
 ![image.png](https://i.loli.net/2020/12/10/gh7NDSxLMc5eyvj.png)
 
 默认是不开启的，我们需要手动进行配置！我们只需要将 appendonly 改为yes就开启了 aof！
重启，redis 就可以生效了！

如果这个 aof 文件有错位，这时候 redis 是启动不起来的吗，我们需要修复这个aof文件

redis 给我们提供了一个工具 ==redis-check-aof --fix==

 ![image.png](https://i.loli.net/2020/12/10/CR783Gf9d5X46jY.png)
 
 > 如果文件正常，重启就可以直接恢复了！
 
 ![image.png](https://i.loli.net/2020/12/10/kSuZWoPEqVidfaD.png)
 
 > 重写规则说明
 
aof 默认就是文件的无限追加，文件会越来越大！

![image.png](https://i.loli.net/2020/12/10/i7jlzDIPphJ3k4K.png)
 
 如果 aof 文件大于 64m，太大了！ fork一个新的进程来将我们的文件进行重写！

 > 优点和缺点！
 
 
```sql
appendonly no # 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下，
rdb完全够用！
appendfilename "appendonly.aof" # 持久化的文件的名字
# appendfsync always # 每次修改都会 sync。消耗性能
appendfsync everysec # 每秒执行一次 sync，可能会丢失这1s的数据！
# appendfsync no # 不执行 sync，这个时候操作系统自己同步数据，速度最快！
# rewrite 重写，
```

优点：
1. 每一次修改都同步，文件的完整会更加好！
2. 每秒同步一次，可能会丢失一秒的数据
3. 从不同步，效率最高的！
缺点：
1. 相对于数据文件来说，aof远远大于 rdb，修复的速度也比 rdb慢！
2. Aof 运行效率也要比rdb慢，所以我们redis默认的配置就是rdb持久化
 

**扩展：**
1. RDB 持久化方式能够在指定的时间间隔内对你的数据进行快照存储
2. AOF 持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始
的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重
写，使得AOF文件的体积不至于过大。
3. 只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化
4. 同时开启两种持久化方式
- 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF
文件保存的数据集要比RDB文件保存的数据集要完整。
- RDB 的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者
建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有
AOF可能潜在的Bug，留着作为一个万一的手段。
5. 性能建议
- 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够
了，只保留 save 900 1 这条规则。
- 如果Enable AOF ，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自
己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产
生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite
的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重
写可以改到适当的数值。
- 如果不Enable AOF ，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也
减少了rewrite时带来的系统波动。代价是如果Master/Slave 同时倒掉，会丢失十几分钟的数据，
启动脚本也要比较两个 Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。
 
 
