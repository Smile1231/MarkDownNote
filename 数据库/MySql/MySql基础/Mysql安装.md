# Mysql安装

> 安装MySQL

**这里建议大家使用压缩版,安装快,方便.不复杂.**

**软件下载**

[mysql5.7 64位下载](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-winx64.zip)

> 安装步骤

1. 下载后得到zip压缩包.

2. 解压到自己想要安装到的目录，本人解压到的是D:\Environment\mysql-5.7.19

3. 添加环境变量：我的电脑->属性->高级->环境变量

```java
选择PATH,在其后面添加: 你的mysql 安装文件下面的bin文件夹
```

4. 编辑 my.ini 文件 ,注意替换路径位置


```
[mysqld]
basedir=D:\Program Files\mysql-5.7\
datadir=D:\Program Files\mysql-5.7\data\
port=3306
skip-grant-tables
```
5. 启动管理员模式下的CMD，并将路径切换至mysql下的bin目录，然后输入mysqld –install (安装mysql)

6. 再输入  mysqld --initialize-insecure --user=mysql 初始化数据文件

7. 然后再次启动mysql 然后用命令 mysql –u root –p 进入mysql管理界面（密码可为空）

8. 进入界面后更改root密码


```
update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';
```
9. 刷新权限


```
flush privileges;
```
10. 修改 my.ini文件删除最后一句skip-grant-tables

11. 重启mysql即可正常使用

```
net stop mysql
net start mysql
```
12. 连接上测试出现以下结果就安装好了

![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3SkRBbGdFaGljUWZ5VWVrbGVmclVoWWliNWpuMGdnV0x0SXJWaWF2QWNCcE9YVzJpY2syaWJJMnVuNjNnTHJGTWR0dmlhbVl4dHRYMmtub1BpYlEvNjQw?x-oss-process=image/format,png)


==数据库基本的操作指令==


```
update user set password=password('123456')where user='root'; 修改密码
flush privileges;  刷新数据库
show databases; 显示所有数据库
use dbname；打开某个数据库
show tables; 显示数据库mysql中所有的表
describe user; 显示表mysql数据库中user表的列信息
create database name; 创建数据库
use databasename; 选择数据库
 
exit; 退出Mysql
? 命令关键词 : 寻求帮助
-- 表示注释
```





