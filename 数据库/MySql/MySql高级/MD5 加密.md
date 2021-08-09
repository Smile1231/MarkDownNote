# MD5 加密

## 实现数据加密

新建一个表 testmd5

```sql
 CREATE TABLE `testmd5` (
   `id` INT(4) NOT NULL,
   `name` VARCHAR(20) NOT NULL,
   `pwd` VARCHAR(50) NOT NULL,
   PRIMARY KEY (`id`)
 ) ENGINE=INNODB DEFAULT CHARSET=utf8

```
插入一些数据


```sql
 INSERT INTO testmd5 VALUES(1,'kuangshen','123456'),(2,'qinjiang','456789')
```
如果我们要对pwd这一列数据进行加密，语法是：


```sql
 update testmd5 set pwd = md5(pwd);
```

如果单独对某个用户(如kuangshen)的密码加密：


```
 INSERT INTO testmd5 VALUES(3,'kuangshen2','123456')
 update testmd5 set pwd = md5(pwd) where name = 'kuangshen2';
```

插入新的数据自动加密

```sql
 INSERT INTO testmd5 VALUES(4,'kuangshen3',md5('123456'));
```


查询登录用户信息（md5对比使用，查看用户输入加密后的密码进行比对）


```sql
 SELECT * FROM testmd5 WHERE `name`='kuangshen' AND pwd=MD5('123456');
```
### 小结

```sql
 -- ================ 内置函数 ================
 -- 数值函数
 abs(x)            -- 绝对值 abs(-10.9) = 10
 format(x, d)    -- 格式化千分位数值 format(1234567.456, 2) = 1,234,567.46
 ceil(x)            -- 向上取整 ceil(10.1) = 11
 floor(x)        -- 向下取整 floor (10.1) = 10
 round(x)        -- 四舍五入去整
 mod(m, n)        -- m%n m mod n 求余 10%3=1
 pi()            -- 获得圆周率
 pow(m, n)        -- m^n
 sqrt(x)            -- 算术平方根
 rand()            -- 随机数
 truncate(x, d)    -- 截取d位小数
 
 -- 时间日期函数
 now(), current_timestamp();     -- 当前日期时间
 current_date();                    -- 当前日期
 current_time();                    -- 当前时间
 date('yyyy-mm-dd hh:ii:ss');    -- 获取日期部分
 time('yyyy-mm-dd hh:ii:ss');    -- 获取时间部分
 date_format('yyyy-mm-dd hh:ii:ss', '%d %y %a %d %m %b %j');    -- 格式化时间
 unix_timestamp();                -- 获得unix时间戳
 from_unixtime();                -- 从时间戳获得时间
 
 -- 字符串函数
 length(string)            -- string长度，字节
 char_length(string)        -- string的字符个数
 substring(str, position [,length])        -- 从str的position开始,取length个字符
 replace(str ,search_str ,replace_str)    -- 在str中用replace_str替换search_str
 instr(string ,substring)    -- 返回substring首次在string中出现的位置
 concat(string [,...])    -- 连接字串
 charset(str)            -- 返回字串字符集
 lcase(string)            -- 转换成小写
 left(string, length)    -- 从string2中的左边起取length个字符
 load_file(file_name)    -- 从文件读取内容
 locate(substring, string [,start_position])    -- 同instr,但可指定开始位置
 lpad(string, length, pad)    -- 重复用pad加在string开头,直到字串长度为length
 ltrim(string)            -- 去除前端空格
 repeat(string, count)    -- 重复count次
 rpad(string, length, pad)    --在str后用pad补充,直到长度为length
 rtrim(string)            -- 去除后端空格
 strcmp(string1 ,string2)    -- 逐字符比较两字串大小
 
 -- 聚合函数
 count()
 sum();
 max();
 min();
 avg();
 group_concat()
 
 -- 其他常用函数
 md5();
 default();
```



