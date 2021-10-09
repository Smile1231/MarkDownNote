# `MyBatis`之分页查询：`MyBatis PageHelper`

## 1. 添加依赖

```xml
<!--MyBatis 分页插件: MyBatis PageHelper-->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.5</version>
</dependency>
```
## 2. 添加配置

在`application.properties`配置文件中添加`MyBatis PageHelper`的配置项
```properties
# PageHelper 分页插件配置
pagehelper.helperDialect=mysql
pagehelper.reasonable=true
pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql
```

## 3. 分页查询

通过 `MyBatis PageHelper` 进行分页查询实际上非常简单，只需在`service(或mapper)`方法执行查询前，调用一次 `PageHelper.startPage(pageNum,pageSize)` `来设置分页查询参数即可，其中pageNum` 为记录页数，`pageSize` 为单页记录数量。此时`service(或mapper)`方法的查询结果就是分页后的结果了。如果期望获得相关的分页信息，还可以将查询结果封装到`PageInfo`对象中，以获得总页数、总记录数、当前页数等相关分页信息

现在通过一个实际示例，来具体演示操作，这里我们提供了一个分页查询的`Controller`

```java
/**
 * 分页查询
 * @param pageNum 记录页数
 * @param pageSize 单页记录数量
 * @return
 */
@ResponseBody
@RequestMapping("/findPage")
public List<Student> findPage(@RequestParam int pageNum, @RequestParam int pageSize) {
    // 设置分页查询参数
    PageHelper.startPage(pageNum,pageSize);
    List<Student> studentList = studentService.findList();

    for(Student student : studentList) {
        System.out.println("element : " + student);
    }

    // 封装分页查询结果到 PageInfo 对象中以获取相关分页信息
    PageInfo pageInfo = new PageInfo( studentList );
    System.out.println("总页数: " + pageInfo.getPages());
    System.out.println("总记录数: " + pageInfo.getTotal());
    System.out.println("当前页数: " + pageInfo.getPageNum());
    System.out.println("当前页面记录数量: " + pageInfo.getSize());

    return pageInfo.getList();
}
```

`service`方法中所调用的查询`SQL`如下所示，可以看到，`SQL`中无需使用`limit`语句

```xml
...
<resultMap id="studentResultMap" type="com.aaron.springbootdemo.pojo.Student">
    <id property="id" column="id" jdbcType="INTEGER"/>
    <result property="username" column="username" jdbcType="VARCHAR"/>
    <result property="sex" column="sex" jdbcType="VARCHAR"/>
    <result property="address" column="address" jdbcType="VARCHAR"/>
</resultMap>

<select id="findList" parameterType="String" resultMap="studentResultMap">    
    SELECT * FROM user
</select>
...
```

> `NOTE:` 


`PageHelper.startPage(pageNum,pageSize)` 只对其后的第一次`SQL`查询进行分页。故若需进行分页查询，必须每次在`service（或mapper）`方法执行`SQL`查询前调用`PageHelper.startPage(pageNum,pageSize) `方法




