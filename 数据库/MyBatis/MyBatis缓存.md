# Mybatis 缓存

## MyBatis 缓存
> 为什么使用缓存？ 

当用户频繁查询某些固定的数据时，第一次将这些数据从数据库中查询出来，保存在缓存中。当用户再次查询这些数据时，不用再通过数据库查询，而是去缓存里面查询。减少网络连接和数据库查询带来的损耗，从而提高我们的查询效率，减少高并发访问带来的系统性能问题。

一句话概括：经常查询一些不经常发生变化的数据，使用缓存来提高查询效率。

像大多数的持久化框架一样，``MyBatis`` 也提供了缓存策略，通过缓存策略来减少数据库的查询次数，从而提高性能。 ``MyBatis`` 中缓存分为一级缓存，二级缓存。

### 一级缓存
> 介绍

一级缓存是 ``SqlSession`` 级别的缓存，是默认开启的。

所以在参数和 ``SQL``完全一样的情况下，我们使用同一个 ``SqlSession`` 对象调用一个 ``Mapper`` 方法，往往只执行一次 ``SQL``，因为使用 ``SqlSession`` 第一次查询后，``MyBatis`` 会将其放在缓存中，以后再查询的时候，如果没有声明需要刷新，并且缓存没有超时的情况下，``SqlSession`` 都会取出当前缓存的数据，而不会再次发送 ``SQL`` 到数据库。

验证
资源目录 ``resources`` 下增加 ``log4j.properties``：
```properties
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
​
### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=./resources/mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
​
### set log levels - for more verbose logging change 'info' to 'debug' ###
​
log4j.rootLogger=debug, stdout
```

``pom.xml``中导入 ``log4j`` 的依赖，从而可以查看底层调用 ``JDBC`` 的 ``log``：
```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
​
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.7</version>
</dependency>
```
编写代码验证 ``MyBatis`` 中的一级缓存：
```java
@Test
public void testOneCache() throws IOException {
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
​
    // 根据 id 查询用户信息
    // 第一次查询，查询的数据库
    User user1 = userMapper.findById(1);
    System.out.println(user1);
​
    // 第二次查询，查询的是一级缓存
    User user2 = userMapper.findById(1);
    System.out.println(user2);
}
```
可以发现，虽然在上面的代码中查询了两次，但最后只执行了一次数据库操作，这就是 ``MyBatis`` 提供的一级缓存在起作用了。因为一级缓存的存在，导致第二次查询 ``id`` 为 ``1`` 的记录时，并没有发出 ``SQL`` 语句从数据库中查询数据，而是从一级缓存中查询。


> 分析

一级缓存是 ``SqlSession``范围的缓存，执行 ``SqlSession`` 的 ``C``（增加）``U``（更新）``D``（删除）操作，或者调用 ``clearCache()、commit()、close()`` 方法，都会清空缓存。

第一次发起查询用户 ``id ``为 41 的用户信息，先去找缓存中是否有 ``id`` 为 41 的用户信息，如果没有，从数据库查询用户信息。
得到用户信息，将用户信息存储到一级缓存中。
如果 ``sqlSession`` 去执行 ``commit`` 操作（执行插入、更新、删除），清空 ``SqlSession`` 中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读。
第二次发起查询用户 ``id`` 为 41 的用户信息，先去找缓存中是否有 ``id`` 为 41 的用户信息，缓存中有，直接从缓存中获取用户信息。
清除
``sqlSession.clearCache()`` 手动清空一级缓存：
```java
@Test
public void testOneCache() throws IOException {
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

    // 根据 id 查询用户信息
    // 第一次查询，查询的数据库
    User user1 = userMapper.findById(1);
    System.out.println(user1);

    // clearCache: 手动清空缓存
    sqlSession.clearCache();

    // 第二次查询，查询的是一级缓存
    User user2 = userMapper.findById(1);
    System.out.println(user2);
}
```
``flushCache="true"`` 自动清空一级缓存：
```xml
<select id="findById" resultType="com.renda.domain.User" parameterType="int" flushCache="true">
    SELECT * FROM `user` WHERE id = #{id}
</select>
```

###二级缓存

> 介绍

二级缓存是 ``namspace`` 级别（跨 ``sqlSession``）的缓存，是默认不开启的。

实现二级缓存的时候，``MyBatis`` 要求返回的 ``POJO`` 必须是可序列化的，也就是要求实现 ``Serializable`` 接口。

二级缓存的开启需要进行配置，配置方法很简单，只需要在映射 ``XML`` 文件配置 ``<cache/>`` 就可以开启二级缓存了。

> 验证
配置核心配置文件
```xml
<settings>
    ...
    
    <!--
            cacheEnabled 的取值默认为 true，所以这一步可以省略不配置。
            为 true 代表支持二级缓存；为 false 代表不支持二级缓存。
        -->
    <setting name="cacheEnabled" value="true"/>
</settings>
```
``yaml``中的配置

```yaml
mybatis
  configuration
    cacheEnabled: true
```

配置 ``UserMapper.xml`` 映射
```xml
<mapper namespace="com.renda.mapper.UserMapper">
    <!-- 当前映射开启二级缓存 -->
    <cache></cache>
    <!--
        根据 id 查询用户
        useCache="true" 代表当前这个 statement 是使用二级缓存
    -->
    <select id="findById" resultType="com.renda.domain.User" parameterType="int" useCache="true">
        SELECT * FROM `user` WHERE id = #{id}
    </select>
</mapper>
```

修改 ``User`` 实体
```java
public class User implements Serializable {
    private static final long serialVersionUID = 7898016747305399302L;
​
    private Integer id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;
​
    private List<Orders> ordersList;
    private List<Role> roleList;
    
    ...   
}
```

> 测试结果
```java
@Test
public void testTwoCache() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
​
    SqlSession sqlSession1 = sqlSessionFactory.openSession();
    UserMapper userMapper1 = sqlSession1.getMapper(UserMapper.class);
    // 第一次查询
    User user = userMapper1.findById(1);
    System.out.println(user);
    // 只有执行 sqlSession.commit 或者 sqlSession.close，那么一级缓存中内容才会刷新到二级缓存
    sqlSession1.close();
​
    SqlSession sqlSession2 = sqlSessionFactory.openSession();
    UserMapper userMapper2 = sqlSession2.getMapper(UserMapper.class);
    // 第二次查询
    User user2 = userMapper2.findById(1);
    System.out.println(user2);
    sqlSession2.close();
}
```
> 分析

二级缓存是 ``mapper ``映射级别的缓存，多个 ``SqlSession`` 去操作同一个 ``Mapper`` 映射的 ``sql ``语句，多个 ``SqlSession`` 可以共用二级缓存，二级缓存是跨 ``SqlSession`` 的。

映射语句文件中的所有 ``select`` 语句将会被缓存。
映射语句文件中的所有 ``insert、update`` 和 ``delete`` 语句会刷新缓存。
注意问题：``MyBatis`` 的二级缓存因为是 ``namespace`` 级别，某个 ``namespace`` 的增删改只会刷新它自己的缓存，会导致不同 ``namespace`` 缓存了别的 ``namespace`` 的旧值，所以在进行多表查询时会产生脏读问题。

> 小结

``MyBatis`` 的缓存，都不需要手动存储和获取数据，是 ``MyBatis`` 自动维护的。
``MyBatis`` 开启了二级缓存后，那么查询顺序：``二级缓存 --> 一级缓存 --> 数据库``。
注意：因为 ``MyBatis`` 的二级缓存会存在脏读问题，所以实际开发中会使用第三方的缓存技术 ``Redis`` 解决问题。





