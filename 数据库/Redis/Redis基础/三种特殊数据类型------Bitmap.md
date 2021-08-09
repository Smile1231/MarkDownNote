# Bitmap

为什么其他教程都不喜欢讲这些？这些在生活中或者开发中，都有十分多的应用场景，学习了，就是就
是多一个思路！


> 位存储

统计用户信息，活跃，不活跃！ 登录 、 未登录！ 打卡，365打卡！ 两个状态的，都可以使用
Bitmaps！
Bitmap 位图，数据结构！ 都是操作二进制位来进行记录，就只有0 和 1 两个状态！

365 天 = 365 bit

1字节 = 8bit 

46 个字节左右！

> 测试

![image.png](https://i.loli.net/2020/12/10/oOzwbPAxHYp5v3W.png)

使用bitmap 来记录 周一到周日的打卡！

周一：1

周二：0

周三：0

周四：1 ......

![image.png](https://i.loli.net/2020/12/10/jJN5mQIWo8UEkbu.png)

查看某一天是否有打卡！

```sh
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0

```

统计操作，统计 打卡的天数！

```sh
127.0.0.1:6379> bitcount sign # 统计这周的打卡记录，就可以看到是否有全勤！
(integer) 3

```


