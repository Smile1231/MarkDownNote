# Nosql概述

## 什么是Nosql
Not Only Structured Query Language

关系型数据库：列+行，同一个表下数据的结构是一样的。

非关系型数据库：数据存储没有固定的格式，并且可以进行横向扩展。

NoSQL泛指非关系型数据库，随着web2.0互联网的诞生，传统的关系型数据库很难对付web2.0时代！尤其是超大规模的高并发的社区，暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅速，Redis是发展最快的。

## Nosql特点
1. 方便扩展（数据之间没有关系，很好扩展！）

2. 大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）

3. 数据类型是多样型的！（不需要事先设计数据库，随取随用）

4. 传统的 RDBMS 和 NoSQL

```sql
传统的 RDBMS(关系型数据库)
- 结构化组织
- SQL
- 数据和关系都存在单独的表中 row col
- 操作，数据定义语言
- 严格的一致性
- 基础的事务
- ...
```

```sql
Nosql
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储，列存储，文档存储，图形数据库（社交关系）
- 最终一致性
- CAP定理和BASE
- 高性能，高可用，高扩展
- ...

```

> 了解：3V + 3高

大数据时代的3V ：主要是**描述问题**的

海量Velume

多样Variety

实时Velocity

大数据时代的3高 ： 主要是对**程序的要求**

高并发

高可扩

高性能

真正在公司中的实践：NoSQL + RDBMS 一起使用才是最强的。


## 阿里巴巴演进分析
推荐阅读：阿里云的这群疯子https://yq.aliyun.com/articles/653511

![image](https://img-blog.csdnimg.cn/20200820103829446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

![image](https://img-blog.csdnimg.cn/20200820103851613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)


```
# 商品信息
- 一般存放在关系型数据库：Mysql,阿里巴巴使用的Mysql都是经过内部改动的。

# 商品描述、评论(文字居多)
- 文档型数据库：MongoDB

# 图片
- 分布式文件系统 FastDFS
- 淘宝：TFS
- Google: GFS
- Hadoop: HDFS
- 阿里云: oss

# 商品关键字 用于搜索
- 搜索引擎：solr,elasticsearch
- 阿里：Isearch 多隆

# 商品热门的波段信息
- 内存数据库：Redis，Memcache

# 商品交易，外部支付接口
- 第三方应用

```

## Nosql的四大分类
> KV键值对

- 新浪：Redis
- 美团：Redis + Tair
- 阿里、百度：Redis + Memcache

> 文档型数据库（bson数据格式）：

- **MongoDB(掌握)**

基于分布式文件存储的数据库。C++编写，用于处理大量文档。
MongoDB是RDBMS和NoSQL的中间产品。MongoDB是非关系型数据库中功能最丰富的，NoSQL中最像关系型数据库的数据库。
- ConthDB

> 列存储数据库

- HBase(大数据必学)
- 分布式文件系统

> 图关系数据库

用于广告推荐，社交网络

- **Neo4j**、InfoGrid
![image.png](https://i.loli.net/2020/12/10/6gSA4VcG9oZptfy.png)

