# DML语言(Data Manipulation Language)

**数据库意义 ：** 数据存储、数据管理

**管理数据库数据方法：**

- 通过SQLyog等管理工具管理数据库数据

- 通过DML语句管理数据库数据

**DML语言:** 数据操作语言

- 用于操作数据库对象中所包含的数据

- 包括 :

    - INSERT (添加数据语句)

    - UPDATE (更新数据语句)

    - DELETE (删除数据语句)

## 添加数据

> **INSERT命令**

**语法**：


```sql
INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3')
```

注意 :

- 字段或值之间用英文逗号隔开 .

- ' 字段1,字段2...' 该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致 .

- 可同时插入多条数据 , values 后用英文逗号隔开 .


```sql
-- 使用语句如何增加语句?
-- 语法 : INSERT INTO 表名[(字段1,字段2,字段3,...)] VALUES('值1','值2','值3')
INSERT INTO grade(gradename) VALUES ('大一');
 
-- 主键自增,那能否省略呢?
INSERT INTO grade VALUES ('大二');
 
-- 查询:INSERT INTO grade VALUE ('大二')错误代码：1136
Column count doesn`t match value count at row 1
 
-- 结论:'字段1,字段2...'该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致.
 
-- 一次插入多条数据
INSERT INTO grade(gradename) VALUES ('大三'),('大四');
```

## 修改数据
> **update命令**


**语法**：


```sql
UPDATE 表名 SET column_name=value [,column_name2=value2,...] [WHERE condition];
```

**注意 :**

- column_name 为要更改的数据列

- value 为修改后的数据 , 可以为变量 , 具体指 , 表达式或者嵌套的SELECT结果

- condition 为筛选条件 , 如不指定则修改该表的所有列数据

> **where条件子句**

可以简单的理解为 : 有条件地从表中筛选数据

![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3TENJNnhHS0o3YktpYUJ1ZE9TQkhkOWR5SldQeHAzSDlHaWNwaFBYTUV2Q3d0VXlLWDN2aWJVQ0VTcVNhRG5Lbkx6bHdZcGNSVEpzZFVJZy82NDA?x-oss-process=image/format,png)


## 删除数据
 
> **DELETE命令**

**语法：**


```sql
DELETE FROM 表名 [WHERE condition];
```
**注意**：condition为筛选条件 , 如不指定则删除该表的所有列数据


```sql
-- 删除最后一个数据
DELETE FROM grade WHERE gradeid = 5
```

> **TRUNCATE命令**

**作用**：用于完全清空表数据 , 但表结构 , 索引 , 约束等不变 ;

**语法：**

```sql
TRUNCATE [TABLE] table_name;
-- 清空年级表
TRUNCATE grade
```

**注意：区别于DELETE命令**

- 相同 : 都能删除数据 , 不删除表结构 , 但TRUNCATE速度更快

- 不同 :

    - 使用TRUNCATE TABLE 重新设置AUTO_INCREMENT计数器

    - 使用TRUNCATE TABLE不会对事务有影响 （事务后面会说）

**测试：**


```sql
-- 创建一个测试表
CREATE TABLE `test` (
  `id` INT(4) NOT NULL AUTO_INCREMENT,
  `coll` VARCHAR(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
 
-- 插入几个测试数据
INSERT INTO test(coll) VALUES('row1'),('row2'),('row3');
 
-- 删除表数据(不带where条件的delete)
DELETE FROM test;
-- 结论:如不指定Where则删除该表的所有列数据,自增当前值依然从原来基础上进行,会记录日志.
 
-- 删除表数据(truncate)
TRUNCATE TABLE test;
-- 结论:truncate删除数据,自增当前值会恢复到初始值重新开始;不会记录日志.
 
-- 同样使用DELETE清空不同引擎的数据库表数据.重启数据库服务后
-- InnoDB : 自增列从初始值重新开始 (因为是存储在内存中,断电即失)
-- MyISAM : 自增列依然从上一个自增数据基础上开始 (存在文件中,不会丢失)
```