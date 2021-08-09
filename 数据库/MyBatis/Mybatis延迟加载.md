
# Mybatis 延迟加载（懒加载）和立即加载

> 什么是延迟加载？

实际开发过程中很多时候并不需要总是在加载用户信息时就一定要加载他的订单信息。此时就是我们所说的延迟加载。

在 ``一对多``中，当有一个用户，它有个100个订单；在查询用户时，用户下的订单应该是，什么时候用，什么时候查询；在查询订单时，订单所属的用户信息应该是随着订单一起查询出来。

**延迟加载**：就是在需要用到数据时才进行加载，不需要用到数据时就不加载数据。延迟加载也称懒加载。

优点：先从单表查询，需要时再从关联表去关联查询，大大提高数据库性能，因为查询单表要比关联查询多张表速度要快。
缺点：因为只有当需要用到数据时，才会进行数据库查询，这样在大批量数据查询时，因为查询工作也要消耗时间，所以可能造成用户等待时间变长，造成用户体验下降。

- ``一对多，多对多``：通常情况下采用延迟加载。

- ``一对一（多对一)``：通常情况下采用立即加载。

**注意**：延迟加载是基于嵌套查询来实现的。

## 实现

### 局部延迟加载

在 ``association`` 和 ``collection`` 标签中都有一个 ``fetchType`` 属性，通过修改它的值，可以修改局部的加载策略。

``OrderMapper.xml``

```xml
<!--
        fetchType="lazy" : 延迟加载策略
        fetchType="eager": 立即加载策略
    -->
<resultMap id="orderMap2" type="com.renda.domain.Orders">
    <id property="id" column="id"/>
    <result property="ordertime" column="ordertime"/>
    <result property="total" column="total"/>
    <result property="uid" column="uid"/>
    <association property="user" javaType="com.renda.domain.User"
                 select="com.renda.mapper.UserMapper.findById" column="uid" fetchType="lazy"/>
</resultMap>
​
<select id="findAllWithUser2" resultMap="orderMap2">
    SELECT * FROM orders
</select>
```
### 设置触发延迟加载的方法

在配置了延迟加载策略后，发现即使没有调用关联对象的任何方法，在调用当前对象的 ``equals、clone、hashCode、toString`` 方法时也会触发关联对象的查询。
```java
OrderMapper orderMapper = sqlSession.getMapper(OrderMapper.class);
List<Orders> allWithUser2 = orderMapper.findAllWithUser2();
for (Orders orders : allWithUser2) {
    //因为 Orders 的 toString 没有开启延迟加载
    //配置了延迟加载的关联对象 User 还是被打印出来
    System.out.println(orders);
}
```

可以在配置文件 ``sqlMapConfig.xml`` 中使用 ``lazyLoadTriggerMethods`` 配置项覆盖掉上面四个方法。
```xml
<settings>
    <!-- 所有 toString 方法都会触发延迟加载 -->
    <setting name="lazyLoadTriggerMethods" value="toString()"/>
</settings>
```
如果是在 ``yaml``文件中配置:
```yaml
mybatis
  configuration
    lazy-load-trigger-methods: toString()
```

### 全局延迟加载

在 ``MyBatis`` 的核心配置文件中可以使用 ``setting`` 标签修改全局的加载策略。
```xml
<settings>
    <!-- 开启全局延迟加载功能 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 所有 toString 方法都会触发延迟加载 -->
    <setting name="lazyLoadTriggerMethods" value="toString()"/>
</settings>
```

配置完全局延迟加载功能后，需要加载关联的对象就需要调用它的 ```toString``` 方法：
```java
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
List<User> allWithOrder2 = userMapper.findAllWithOrder2();
​
for (User user : allWithOrder2) {
    System.out.println(user);
    // 需要用到用户关联的订单
    System.out.println(user.getOrdersList());
}
```
注意：局部的加载策略优先级高于全局的加载策略；

所以，在开启全局延迟加载后，为了实现订单能立即加载关联的用户信息，就可以在局部开启立即加载策略：
```xml
<!--
        fetchType="lazy" : 延迟加载策略
        fetchType="eager": 立即加载策略
    -->
<resultMap id="orderMap2" type="com.renda.domain.Orders">
    <id property="id" column="id"/>
    <result property="ordertime" column="ordertime"/>
    <result property="total" column="total"/>
    <result property="uid" column="uid"/>
    <association property="user" javaType="com.renda.domain.User"
                 select="com.renda.mapper.UserMapper.findById" column="uid" fetchType="eager"/>
</resultMap>
​
<select id="findAllWithUser2" resultMap="orderMap2">
    SELECT * FROM orders
</select>
```






