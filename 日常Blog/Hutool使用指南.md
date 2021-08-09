# ``Hutool``使用指南

[官网参考文档地址：https://hutool.cn/docs/#/](https://hutool.cn/docs/#/)


``Hutool``是一个``Java``工具包，它帮助我们简化每一行代码，避免重复造轮子。如果你有需要用到某些工具方法的时候，不妨在``Hutool``里面找找，可能就有。本文将对``Hutool``中的常用工具类和方法进行介绍。
##一、简介
一个``Java``基础工具类，对文件、流、加密解密、转码、正则、线程、XML等JDK方法进行封装，组成各种``Util``工具类，同时提供以下组件：

- ``hutool-core`` 核心，包括``Bean``操作、日期、各种``Util``等
- ``hutool-aop`` ``JDK``动态代理封装，提供非``IOC``下的切面支持
- ``hutool-bloomFilter`` 布隆过滤，提供一些``Hash``算法的布隆过滤
- ``hutool-cache`` 缓存
- ``hutool-cron`` 定时任务模块，提供类``Crontab``表达式的定时任务
- ``hutool-crypto`` 加密解密模块
- ``hutool-db`` ``JDBC``封装后的数据操作，基于``ActiveRecord``思想
- ``hutool-dfa`` 基于``DFA``模型的多关键字查找
- ``hutool-extra`` 扩展模块，对第三方封装（模板引擎、邮件等）
- ``hutool-http`` 基于``HttpUrlConnection``的``Http``客户端封装
- ``hutool-log`` 自动识别日志实现的日志门面
- ``hutool-script`` 脚本执行封装，例如``Javascript``
- ``hutool-setting`` 功能更强大的``Setting``配置文件和``Properties``封装
- ``hutool-system`` 系统参数调用封装（``JVM``信息等）
- ``hutool-json`` ``JSON``实现
- ``hutool-captcha`` 图片验证码实现
- ``hutool-poi``	针对``POI``中``Excel``和``Word``的封装
- ``hutool-socket``	基于``Java``的``NIO``和``AIO``的``Socket``封装

## 二、安装

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.6.5</version>
</dependency>
```







