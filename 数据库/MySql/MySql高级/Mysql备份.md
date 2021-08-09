# MySQL备份

数据库备份必要性

- 保证重要数据不丢失

- 数据转移

MySQL数据库备份方法

- mysqldump备份工具

- 数据库管理工具,如SQLyog

- 直接拷贝数据库文件和相关配置文件


**mysqldump客户端**

作用 :

- 转储数据库

- 搜集数据库进行备份

- 将数据转移到另一个SQL服务器,不一定是MySQL服务器

![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3SmY3ZGVvbHdRYTQ0clh2aWNJaFhaME56Z1dKV2V5WVljZjFEeTNpYmZONjZTaWFaUW1xVEYzSHY4SEJqcjF6SW93WGgyMDFwRWpVenlKdy82NDA?x-oss-process=image/format,png)


```sql
-- 导出
1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)
 
可以-w携带备份条件
 
-- 导入
1. 在登录mysql的情况下：-- source D:/a.sql
　　source  备份文件
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```

## 三大范式

==**第一范式 (1st NF)**==

第一范式的目标是确保每列的原子性,如果每列都是不可再分的最小数据单元,则满足第一范式

==**第二范式(2nd NF)**==

第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一范式（1NF）。

第二范式要求每个表只描述一件事情

==**第三范式(3rd NF)**==

如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式.

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

**==规范化和性能的关系==**

为满足某种商业目标 , 数据库性能比规范化数据库更重要

在数据规范化的同时 , 要综合考虑数据库的性能

通过在给定的表中添加额外的字段,以大量减少需要从中搜索信息所需的时间

通过在给定的表中插入计算列,以方便查询

