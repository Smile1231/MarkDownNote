# REST风格的索引基本操作

一种软件架构风格，而不是标准。更易于实现缓存等机制

method |url地址	| 描述
---|:--:|---:
PUT |	localhost:9200/索引名称/类型名称/文档id	|创建文档(指定文档id)
POST |	localhost:9200/索引名称/类型名称 |	创建文档（随机文档id）
POST |	localhost:9200/索引名称/类型名称/文档id/_update	| 修改文档
DELETE |	localhost:9200/索引名称/类型名称/文档id |	删除文档
GET |	localhost:9200/索引名称/类型名称/文档id	| 通过文档id查询文档
POST |	localhost:9200/索引名称/类型名称/_search |	查询所有的数据

> 基础测试

1. 创建一个索引
PUT /索引名/类型名(高版本都不写了，都是_doc)/文档id
{请求体}
![image](https://img-blog.csdnimg.cn/20200828224224886.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpc2VuMDEwNzAxMDc=,size_16,color_FFFFFF,t_70#pic_center)
完成了自动添加了索引！数据也成功的添加了。
![image](https://img-blog.csdnimg.cn/20200828224246679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpc2VuMDEwNzAxMDc=,size_16,color_FFFFFF,t_70#pic_center)
那么name这个字段用不用指定类型呢
![image](https://img-blog.csdnimg.cn/20200828224311944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpc2VuMDEwNzAxMDc=,size_16,color_FFFFFF,t_70#pic_center)
指定字段的类型properties 就比如sql创表
获得这个规则！可以通过GET请求获得具体的信息
![image](https://img-blog.csdnimg.cn/2020082822452110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpc2VuMDEwNzAxMDc=,size_16,color_FFFFFF,t_70#pic_center)
如果自己不设置文档字段类型，那么es会自动给默认类型
![image](https://img-blog.csdnimg.cn/20200828224539919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpc2VuMDEwNzAxMDc=,size_16,color_FFFFFF,t_70#pic_center)



