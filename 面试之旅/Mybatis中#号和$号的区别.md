# ``Mybatis``中``#``号和``$``号的区别

## ``#{}`` 默认会用引号将参数引起来

## ``${}``单纯替代

示例：

之前的写法，``select * from product_tree_v pv where pv.product_code in(#{product})``

预编译出来的结果：``select * from product_tree_v pv where pv.product_code in ？``;

``#{}``被当作一个占位符了，而参数前后也会被加上引号。

运行时的``sql``是：``select * from product_tree_v pv where pv.product_code in('‘p001','p002','p003'')``;

所以无论如何都是查不到数据的。

换成``${}``：``select * from product_tree_v pv where pv.product_code in($product})``;

预编译出来的结果 ：``select * from product_tree_v pv where pv.product_code in (‘p001','p002','p003')``;

这样就是纯粹的将参数传进去，没有做任何的转义操作。这才是我们真正想要的。

## 总结

``mybatis``作为``ORM``框架，从性能，系统维护性，实用性上来说，都是非常优秀的，

其所有的``sql``在执行前都会通过数据库驱动进行预编译，这样``DBMS``就可以不用编译直接接收参数运行，

而``#``和``$``号的区别在预编译后就能看出来了，``#{}``预编译完是占位符``?``，而``${}``预编译完就是传进来的参数。

## ``#{}``的优点：

  使用``#{}``可以预防``sql``攻击，而``${}``却不能

例如 ``select * from ${tablename} ``  如果传入的是 ``product; drop product;``

那么你的表数据就会被无声无息的干掉了。

使用``${}``的场景：

1. 作为``in``条件时，

2. 参数为``int``类型并且数据库中字段的类型是``number``，

3. 表名

4. ``order by ${}``，排序字段


``--------------------------ps----------------------------``

``<![CDATA[]]>``的用法，在该符号内的语句，将不会被当成字符串来处理，而是直接当成``sql``语句，比如有大于，小于号，要执行一个存储过程都需要加上这个。




