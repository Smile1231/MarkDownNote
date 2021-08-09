# SpringCloud Rest学习环境搭建

## 介绍
- 我们会使用一个Dept部门模块做一个微服务通用案例Consumer消费者(Client)通过REST调用Provider提供者(Server)提供的服务。
- 回顾Spring，SpringMVC，Mybatis等以往学习的知识。
- Maven的分包分模块架构复习。

```
一个简单的Maven模块结构是这样的：

-- app-parent: 一个父项目(app-parent)聚合了很多子项目(app-util\app-dao\app-web...)
  |-- pom.xml
  |
  |-- app-core
  ||---- pom.xml
  |
  |-- app-web
  ||---- pom.xml
  ......

```
一个父工程带着多个Moudule子模块

- MicroServiceCloud父工程(Project)下初次带着3个子模块(Module)

- microservicecloud-api 【封装的整体entity/接口/公共配置等】
- microservicecloud-consumer-dept-80 【服务提供者】
- microservicecloud-provider-dept-8001 【服务消费者】

## SpringCloud版本选择
**大版本说明**

SpringBoot  | 	SpringCloud	| 关系
---|---|---
1.2.x |	Angel版本(天使) |	兼容SpringBoot1.2x
1.3.x |	Brixton版本(布里克斯顿) |	兼容SpringBoot1.3x，也兼容SpringBoot1.4x
1.4.x |	Camden版本(卡姆登) |	兼容SpringBoot1.4x，也兼容SpringBoot1.5x
1.5.x |	Dalston版本(多尔斯顿) |	兼容SpringBoot1.5x，不兼容SpringBoot2.0x
1.5.x |	Edgware版本(埃奇韦尔) |	兼容SpringBoot1.5x，不兼容SpringBoot2.0x
2.0.x |	Finchley版本(芬奇利) |	兼容SpringBoot2.0x，不兼容SpringBoot1.5x
2.1.x	 | Greenwich版本(格林威治)	


> 实际开发版本关系


spring-boot-starter-parent | _ | spring-cloud-dependencles	  | _
---|---|---|---
版本号 |	发布日期 |	版本号 |	发布日期
1.5.2.RELEASE |	2017-03 |	Dalston.RC1 |	2017-x
1.5.9.RELEASE |	2017-11 |	Edgware.RELEASE	  | 2017-11
1.5.16.RELEASE	 | 2018-04 |	Edgware.SR5 |	2018-10
1.5.20.RELEASE |	2018-09 |	Edgware.SR5 |	2018-10
2.0.2.RELEASE |	2018-05 |	Fomchiey.BULD-SNAPSHOT |	2018-x
2.0.6.RELEASE	 | 2018-10 |	Fomchiey-SR2	| 2018-10
2.1.4.RELEASE |	2019-04 |	Greenwich.SR1 |	2019-03

**使用后两个**

## 创建父工程
- 新建父工程项目springcloud，切记Packageing是pom模式
- 主要是定义POM文件，将后续各个子模块公用的jar包等统一提取出来，类似一个抽象父类
![image](https://img-blog.csdnimg.cn/20200521130052880.png#pic_center)

> pom.xml

