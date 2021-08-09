# ES概念

## 概述
> elasticsearch是面向文档的,关系行数据库和elasticsearch客观的对比! 一切都是json


RelationalDB |	Elasticsearch
---|---
数据库(database)	| 索引(indices)
表(tables) |	types(7版本以及之后会被抛弃，默认_doc)
行(rows) |	documents
字段(columns) |	fields

面向文档 面向文档 面向文档 ~

elasticsearch(集群)中可以包含多个索引(数据库),每个索引中可以包含多个类型(表),每个类型先又包含多个文档(行),每个文档中又包含多个字段(列).

## 物理设计
elasticsearch在后台吧每个索引划分成多个分片,每个分片可以在集群中的不同服务器间迁移

![image](http://victorfengming.gitee.io/course/elasticsearch/base/07-elasticsearch-es%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5%E7%90%86%E8%A7%A3.assets/1596629957996.png)

## 逻辑设计

一个索引类型中,包含多个文档,比如所文档1,文档2.当我们索引一篇文章时,可以通过这样的一各顺序找到它:索引>类型

> 文档id,通过这个组合我们就能索引到某个具体的文档. 注意: id不必是整数,实际上他是一个字符串.

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1597974203192.png)

之前说elasticsearch是面向文档的,name就也为这索引和搜索数据的最小单位是文档,elasticsearch中,文档有几个重要属性:

- 自我包含,一篇文档同时包含字段和对应值,也就是同时包含key:value!

- 可以是层次型的,一个文档中包含文档,复杂的逻辑实体就是这么来的!

- 灵活的结构,文档不依赖预先定义的模式,我们知道关系型数据库中,要提前定义字段才能使用,在elasticsearch中,对于字段是非常灵活的,有时候,我们可以忽略该字段,或者动态的添加一个新的字段.

尽管我们可以随意的新增或者忽略某个字段,但是,每个字段的类型非常重要,比如一个年龄字段类型,可以是字符串也可以是整型.因为elasticsearch会保存字段和类型之间的映射及其他的设置.这种映射具体到每个映射的每种类型,这也是为什么在elasticsearch中,类型有时候也称为映射类型

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596630612189.png)

类型是文档的逻辑容器,就像关系型数据库一样,表格是行的容器.类型中对于字段的定义称为映射,比如name映射为字符串类型.我们说文档是无模式的,他们不需要拥有映射中所定义的所有字段,比如新增一个字段,那么elasticsearch是怎么做的呢?

elasticsearch会自动的将新的字段加入映射,但是这个字段的不确定它是什么类型,elasticsearch就开始猜,如果这个值是18,那么elasticsearch会认为他是整型.但是elasticsearch也可能猜不对,所有最安全的方式就是提前定义好所需要的映射,这点跟关系型数据库殊途同归了,先定义好字段,然后在使用,别整什么幺蛾子.

> 索引

**就是数据库**

索引是映射类型的容器,elasticsearch中的索引是一个非常大的文档集合.索引存储了映射类型字段和其他设置,然后他们呗存储到了各个分片上了.我们来研究下分片是如何工作的

### 物理设计: 节点和分片 如何工作

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596630840364.png)

一个集群至少要有一个节点,而一个节点就是一个elasticsearch进程,节点可以有多个索引默认的,如果你创建索引,那么索引将会有5个分片(primary shard,又称主分片) 构成的,每个主分片会有一个副本(replica shard,又称复制分片)

![image](http://victorfengming.gitee.io/course/elasticsearch/base/07-elasticsearch-es%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5%E7%90%86%E8%A7%A3.assets/1596630793639.png)

![image](http://victorfengming.gitee.io/course/elasticsearch/base/07-elasticsearch-es%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5%E7%90%86%E8%A7%A3.assets/1596630870602.png)

> 倒排索引

elasticsearch使用的是一种称为倒排索引的结构,采用Lucene倒排索引作为底层.这种结构适用于快速的全文搜索,一个索引由文档中所有不重复的列表构成,对于每一个词,都有一个包含它的文档列表.例如,现在有两个文档,每个文档包含如下内容.

![image](http://victorfengming.gitee.io/course/elasticsearch/base/07-elasticsearch-es%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5%E7%90%86%E8%A7%A3.assets/1596631073200.png)
为了创建倒排索引,我们首先要将每个文档拆分成独立的词(或称为词条或者tokens),然后创建一个包含所有不重复的词条的排序列表,然后列出每个词条出现在哪个文档:


![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596631181270.png)

两个文档都匹配,但是第一个文档比第二个匹配程度更高.如果没有别的条件,现在,这两个包含关键字的文档都将返回.

再来看一个示例,比如我们通过博客标签来搜索博客文章.那么倒排索引列表就是这样的一个结构:

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596631425757.png)
![如果要搜索含有python标签的文章,那相对查找所有原始数据而言,查找倒排索引后的数据将会快的多.只需要查看标签这一栏,然后获取相关文章id即可](https://img-blog.csdnimg.cn/20200805155325896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FkbWluNzQxYWRtaW4=,size_16,color_FFFFFF,t_70)





