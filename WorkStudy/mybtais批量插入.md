# 使用``Mybatis``实现批量插入

> 什么是批量插入

一般的我们实现插入都是使用
```sql
INSERT INTO $table_name 
($column_name,...) 
VALUES(
    $column_value,
    ...
)
```

批量插入的话就是类似于
```SQL
INSERT INTO $table_name 
($column_name,...) 
VALUES
(
    $column_value,
    ...
),
(
    $column_value,
    ...
),
(
    $column_value,
    ...
)
```
> 在``Mybatis``中使用``xml``实现批量插入

- 创建``Java``实体类
    ```java
     public class Pojo{
         private String userName;
         private String password;
     }
    ```
- 创建接口
    ```java
     public interface InvoiceOrderMapper{
         void bulkInsert(List<Pojo> list);
     }
    ```

- ``xml``文件
    ```xml
     	<insert id="bulkInsert" parameterType="list" useGeneratedKeys="true" keyProperty="id">
            INSERT INTO $table_name 
            ('user_name','password') 
            VALUES
            <foreach collection="list" index="index" item="pojo" open="" close="" separator=",">
                (
                    #{pojo.userName,jdbcType=VARCHAR},
                    #{pojo.password,jdbcType=VARCHAR}
                )
            </foreach>
        </insert>
    ```
    ``item``是指遍历的每个项目,按照自己的命名就好,``separator``是分隔符









