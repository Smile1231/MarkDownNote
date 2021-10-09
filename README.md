# MarkDownNote
- [MarkDownNote](#markdownnote)
  - [介绍](#介绍)
  - [目录](#目录)
    - [SpringBoot](#springboot)
      - [SpringBoot使用log4j2](#springboot使用log4j2)
      - [SpringBoot使用Druid连接池](#springboot使用druid连接池)
      - [SpringBoot使用定时任务](#springboot使用定时任务)
      - [SpringBoot一些插件](#springboot一些插件)
      - [SpringBoot使用异步任务](#springboot使用异步任务)
      - [SpringBoot使用邮件任务](#springboot使用邮件任务)
      - [SpringBoot Web开发源码探究](#springboot-web开发源码探究)
      - [SpringBootYaml文件配置](#springbootyaml文件配置)
      - [SpringBoot中常见Controller注解](#springboot中常见controller注解)
      - [SpringBoot反射](#springboot反射)
      - [SpringBoot注解](#springboot注解)
      - [SprinBoot自动配置原理入门](#sprinboot自动配置原理入门)
      - [SprinBoot整合Swagger2](#sprinboot整合swagger2)
      - [SpringBoot文件上传下载](#springboot文件上传下载)
      - [SpringBoot实现多线程](#springboot实现多线程)
      - [springBoot获取ip,端口等](#springboot获取ip端口等)
      - [SpringBoot使用JWT完成Token验证](#springboot使用jwt完成token验证)
      - [SpringBoot手把手教你写出优雅的后端接口](#springboot手把手教你写出优雅的后端接口)
      - [关于``springboot``中``yaml``提示的问题](#关于springboot中yaml提示的问题)
      - [``SpringBoot``常见方式解决跨域问题](#springboot常见方式解决跨域问题)
      - [``SpringBoot javax``获取邮件内容，删除邮件、根据时间段筛选邮件，筛选时间段+未读邮件](#springboot-javax获取邮件内容删除邮件根据时间段筛选邮件筛选时间段未读邮件)
    - [SpringSecurity](#springsecurity)
      - [``SpringBoot`` + ``SpringSecurity``示例](#springboot--springsecurity示例)
    - [SpringData](#springdata)
      - [Jpa 的简单实用](#jpa-的简单实用)
    - [多线程知识](#多线程知识)
    - [Git的使用](#git的使用)
      - [关于本地``Git``传送``Blog``的注意点](#关于本地git传送blog的注意点)
    - [ElasticSearch相关](#elasticsearch相关)
      - [🌟ES知识以及整合SpringBoot](#es知识以及整合springboot)
      - [ES基础（狂神版本）](#es基础狂神版本)
      - [ES进阶（狂神版本）](#es进阶狂神版本)
      - [Jsoup爬取网页](#jsoup爬取网页)
      - [ES安装](#es安装)
      - [IK分词器](#ik分词器)
    - [Nginx](#nginx)
    - [Linux](#linux)
      - [linux--Docker 安装](#linux--docker-安装)
      - [Linux--Docker 常用命令](#linux--docker-常用命令)
      - [Linux--Docker 安装Tomcat](#linux--docker-安装tomcat)
      - [Linux--Docker 安装Nginx](#linux--docker-安装nginx)
      - [Linux--Docker 镜像讲解](#linux--docker-镜像讲解)
      - [Linux--Docker 可视化](#linux--docker-可视化)
      - [Linux--Docker 部署es+kibana](#linux--docker-部署eskibana)
      - [Linux--Docker -- 高级](#linux--docker----高级)
      - [Linux 处理目录的基本指令](#linux-处理目录的基本指令)
      - [Linux 目录结构以及关机指令](#linux-目录结构以及关机指令)
      - [Linux Vim 使用](#linux-vim-使用)
      - [Linux解压缩](#linux解压缩)
      - [Linux 基本权限属性](#linux-基本权限属性)
      - [Linux 三种安装方式示例](#linux-三种安装方式示例)
      - [Linux 软硬链接](#linux-软硬链接)
      - [Linux 文件内容查看指令](#linux-文件内容查看指令)
      - [Linux 账号管理](#linux-账号管理)
    - [前端](#前端)
      - [Vue基本语法](#vue基本语法)
      - [Vue-Cli 的使用](#vue-cli-的使用)
      - [TS整合Webpack](#ts整合webpack)
      - [Vue 属性](#vue-属性)
      - [Vue 3](#vue-3)
      - [CSS](#css)
      - [``js``中的``some、find、findindex、includes、filter``的使用](#js中的somefindfindindexincludesfilter的使用)
      - [``html``页面插入地图](#html页面插入地图)
    - [Mysql](#mysql)
      - [Mysql 安装](#mysql-安装)
      - [DML 操作](#dml-操作)
      - [DQL 查询](#dql-查询)
      - [Mysql 指令](#mysql-指令)
      - [外键](#外键)
      - [MD 5 加密以及内置函数](#md-5-加密以及内置函数)
      - [常用函数示例](#常用函数示例)
      - [Mysql备份](#mysql备份)
      - [Mysql 事务](#mysql-事务)
      - [权限以及表维护](#权限以及表维护)
      - [Mysql 索引](#mysql-索引)
    - [Mybatis](#mybatis)
      - [SpringBoot整合Mybatis 基本使用](#springboot整合mybatis-基本使用)
      - [Mybatis实现多表联查](#mybatis实现多表联查)
      - [Mybatis 延迟加载](#mybatis-延迟加载)
      - [Mybatis 缓存](#mybatis-缓存)
      - [``Mybatis``中``#``号和``$``号的区别](#mybatis中号和号的区别)
      - [Plus 的使用](#plus-的使用)
      - [常见Java 包 解释](#常见java-包-解释)
      - [``MyBatis``实现批量插入](#mybatis实现批量插入)
      - [``MBP``实现分页查询](#mbp实现分页查询)
      - [`Mybatis`使用`Sql`进行模糊查询](#mybatis使用sql进行模糊查询)
      - [`MyBatis`之分页查询-`MyBatis PageHelper`](#mybatis之分页查询-mybatis-pagehelper)
      - [`Mybatis`中大于小于等于](#mybatis中大于小于等于)
      - [`Mybtais`封装成`Map`结果](#mybtais封装成map结果)
    - [Redis](#redis)
      - [Redis基本安装知识](#redis基本安装知识)
      - [Redis五大数据类型和三种特殊的数据类型](#redis五大数据类型和三种特殊的数据类型)
      - [Redis缓存穿透和雪崩](#redis缓存穿透和雪崩)
      - [Redis 事务](#redis-事务)
      - [Redis 哨兵模式](#redis-哨兵模式)
      - [Redis redis.conf详解](#redis-redisconf详解)
      - [Redis 主从复制](#redis-主从复制)
      - [Redis 发布订阅](#redis-发布订阅)
      - [Redis 持久化](#redis-持久化)
      - [Java 操作Redis](#java-操作redis)
      - [Springboot整合Redis实现缓存](#springboot整合redis实现缓存)
    - [``JAVA``设计模式(补充中...)](#java设计模式补充中)
    - [Oracle 数据库](#oracle-数据库)
    - [面试](#面试)
      - [多线程](#多线程)
      - [JDBC](#jdbc)
      - [SOA 模式](#soa-模式)
      - [FastJson使用](#fastjson使用)
      - [面试集锦](#面试集锦)
      - [``Spring Bean``的生命周期](#spring-bean的生命周期)
      - [汉得笔试](#汉得笔试)
      - [Java面试小册子](#java面试小册子)
      - [享道出行面经](#享道出行面经)
    - [``Java``相关](#java相关)
      - [``Java``学习线路](#java学习线路)
      - [``Java8`` 内置函数式接口](#java8-内置函数式接口)
      - [``Stream``流的使用](#stream流的使用)
      - [``UML``图关系介绍](#uml图关系介绍)
      - [``StringFormat``的使用](#stringformat的使用)
      - [过滤器和拦截器的区别](#过滤器和拦截器的区别)
      - [路径的讲解](#路径的讲解)
      - [位，字节，字符讲解](#位字节字符讲解)
      - [Cache、Cookie、Session、Token](#cachecookiesessiontoken)
      - [``equals``与 ``==``区别](#equals与-区别)
      - [正则表达式中的特殊字符](#正则表达式中的特殊字符)
      - [Apache Commons Lang 3.10使用简介](#apache-commons-lang-310使用简介)
      - [``Jackson`` 使用方法总结](#jackson-使用方法总结)
      - [二分查找](#二分查找)
      - [``JVM``的第一次探究](#jvm的第一次探究)
      - [如何保证接口的幂等性](#如何保证接口的幂等性)
      - [``Hutool``使用指南](#hutool使用指南)
      - [超级好用的``StringJoiner``](#超级好用的stringjoiner)
      - [关于 ``Try-With-Source`` 的使用以及注意点](#关于-try-with-source-的使用以及注意点)
      - [关于``YAML``中``List``存放``Map``](#关于yaml中list存放map)
      - [``Corn``表达式](#corn表达式)
      - [关于``Exception``的一些小心得](#关于exception的一些小心得)
      - [``Assert``断言](#assert断言)
      - [关于本地``Git``传送``Blog``的注意点](#关于本地git传送blog的注意点-1)
      - [`EasyExcel`的使用](#easyexcel的使用)
      - [布隆过滤器](#布隆过滤器)
      - [`java stream`中`Collectors`的用法](#java-stream中collectors的用法)
      - [`Spring`手动回滚事务](#spring手动回滚事务)
      - [`stream`中`Collectors`的用法](#stream中collectors的用法)
    - [小程序项目杂记](#小程序项目杂记)
      - [小程序获取``openId``](#小程序获取openid)
      - [小程序使用``weUI``](#小程序使用weui)
      - [``Ngrok``的使用(内网穿透)](#ngrok的使用内网穿透)
      - [``Java``中的可以使用的工具类,很方便](#java中的可以使用的工具类很方便)
  - [面试资料pdf下载](#面试资料pdf下载)
  - [Mac](#mac)
  - [``IDEA``快捷键](#idea快捷键)
  - [``VsCode``快捷键](#vscode快捷键)
  - [``UML``图讲解](#uml图讲解)
  - [``IDEA``小图标含义](#idea小图标含义)
  - [``DateUse``](#dateuse)
    - [``Anaconda``的基本使用](#anaconda的基本使用)
    - [`numpy`的使用](#numpy的使用)
    - [`UbUntu`中的数据库安装](#ubuntu中的数据库安装)
  - [Algorithm](#algorithm)
    - [二叉树遍历算法](#二叉树遍历算法)
    - [分治算法](#分治算法)
    - [贪心算法](#贪心算法)
    - [dp算法](#dp算法)
  - [数据挖掘](#数据挖掘)
    - [关于使用`Jupyter`时`Mac`乱码](#关于使用jupyter时mac乱码)



## 介绍
茂茂的个人技术笔记，主要是发现自己好容易忘记，就把学的东西都记下来吧

## 目录

### [SpringBoot](./Java后端/SpringBoot)

#### [SpringBoot使用log4j2](./Java后端/SpringBoot/启动器/Springboot使用log4记录日志.md)

#### [SpringBoot使用Druid连接池](./Java后端/SpringBoot/启动器/使用Druid连接池.md)

#### [SpringBoot使用定时任务](./Java后端/SpringBoot/异步,定时,邮件任务/定时任务.md)

#### [SpringBoot一些插件](./Java后端/SpringBoot/小工具或者插件使用/开发技巧.md)

#### [SpringBoot使用异步任务](./Java后端/SpringBoot/异步,定时,邮件任务/异步任务.md)

#### [SpringBoot使用邮件任务](./Java后端/SpringBoot/异步,定时,邮件任务/邮件任务.md)

#### [SpringBoot Web开发源码探究](./Java后端/SpringBoot/核心技术以及核心功能/Web开发.md)

#### [SpringBootYaml文件配置](./Java后端/SpringBoot/核心技术以及核心功能/配置YAML文件.md)

#### [SpringBoot中常见Controller注解](./Java后端/SpringBoot/注解与反射/Controller中注解汇总.md)

#### [SpringBoot反射](./Java后端/SpringBoot/注解与反射/反射笔记.md)

#### [SpringBoot注解](./Java后端/SpringBoot/注解与反射/注解.md)

#### [SprinBoot自动配置原理入门](./Java后端/SpringBoot/自动配置原理/自动配置原理入门.md)

#### [SprinBoot整合Swagger2](./Java后端/Swagger/Swagger2使用.md)

#### [SpringBoot文件上传下载](./面试之旅/SpringBoot文件上传下载.md)

#### [SpringBoot实现多线程](./面试之旅/springboot实现多线程.md)

#### [springBoot获取ip,端口等](./项目学习/关于获取IP，端口等等.md)

#### [SpringBoot使用JWT完成Token验证](./项目学习/SpringBoot使用jwt验证.md)

#### [SpringBoot手把手教你写出优雅的后端接口](./Java后端/标准式的后端接口格式.md)

#### [关于``springboot``中``yaml``提示的问题](./Java后端/关于springboot中yaml提示.md)

#### [``SpringBoot``常见方式解决跨域问题](./日常Blog/SpringBoot常见解决跨域方式.md)

#### [``SpringBoot javax``获取邮件内容，删除邮件、根据时间段筛选邮件，筛选时间段+未读邮件](./WorkStudy/javax获取邮件并筛选.md)

<hr>

### SpringSecurity

#### [``SpringBoot`` + ``SpringSecurity``示例](./Java后端/SpringSecurity/SpringBoot+SpringSecurity示例.md)


<hr>

### SpringData

#### [Jpa 的简单实用](./Java后端/SpringData/SpringDataJpa.md)

<hr>

### [多线程知识](./Java后端/多线程/多线程笔记.md)

<hr>

### [Git的使用](./Java后端/Git笔记/Git常用指令.md)

#### [关于本地``Git``传送``Blog``的注意点](./日常Blog/关于Git写Blog的注意点.md)

<hr>

### [ElasticSearch相关](./Java后端/ElasticSearch)

#### [🌟ES知识以及整合SpringBoot](./Java后端/ElasticSearch/ES资料.md)

####  [ES基础（狂神版本）](./Java后端/ElasticSearch/ElasticSearch基础)

#### [ES进阶（狂神版本）](./Java后端/ElasticSearch/ElasticSearch高级)

####  [Jsoup爬取网页](./Java后端/ElasticSearch/ElasticSearch高级/Jsoup爬虫爬取网页.md)

####  [ES安装](./Java后端/ElasticSearch/ElasticSearch基础/ElasticSearch安装.md)

####  [IK分词器](./Java后端/ElasticSearch/ElasticSearch基础/IK%20分词器.md)

<hr>

### [Nginx](./Java后端/nginx/nginx尚硅谷.md) 




<hr>


###  [Linux](./Linux)

#### [linux--Docker 安装](./Linux/Docker/Docker基础/Docker安装.md)

#### [Linux--Docker 常用命令](./Linux/Docker/Docker基础/Docker的常用命令.md)

####  [Linux--Docker 安装Tomcat](./Linux/Docker/Docker基础/Docker安装tomcat.md)

####  [Linux--Docker 安装Nginx](./Linux/Docker/Docker基础/Docker%20安装Nginx.md)

####  [Linux--Docker 镜像讲解](./Linux/Docker/Docker基础/Docker镜像讲解.md)

####  [Linux--Docker 可视化](./Linux/Docker/Docker基础/可视化.md)

####  [Linux--Docker 部署es+kibana](./Linux/Docker/Docker基础/部署es+kibana.md)

#### [Linux--Docker -- 高级](./Linux/Docker/Docker高级)

####  [Linux 处理目录的基本指令](./Linux/linux相关/常见Linux指令.md)

####  [Linux 目录结构以及关机指令](./Linux/linux相关/常见关机指令以及目录解释.md)

#### [Linux Vim 使用](./Linux/linux相关/Vim使用及账号用户管理.md)

####  [Linux解压缩](./Linux/linux相关/Linux解压缩.md)

####  [Linux 基本权限属性](./Linux/linux相关/linux基本属性.md)

####  [Linux 三种安装方式示例](./Linux/linux相关/三种软件安装方式及服务器基本环境搭建.md)

####  [Linux 软硬链接](./Linux/linux相关/拓展：Linux%20链接概念.md)

####  [Linux 文件内容查看指令](./Linux/linux相关/文件内容查看.md)

####  [Linux 账号管理](./Linux/linux相关/账号管理.md)

<hr>


### [前端](./前端)

####  [Vue基本语法](./前端/TS+Vue/Vue2语法相关笔记.md)

#### [Vue-Cli 的使用](./前端/TS+Vue/Vue-Cli.md)

#### [TS整合Webpack](./前端/TS+Vue/Ts配置Webpack笔记.md)

#### [Vue 属性](./前端/TS+Vue/Vue属性笔记.md)

#### [Vue 3](./前端/TS+Vue/Vue3项目的创建.md)

#### [CSS](./前端/css)

#### [``js``中的``some、find、findindex、includes、filter``的使用](./前端/js/js中的some、find、findindex、includes、filter的使用.md)

#### [``html``页面插入地图](./前端/html页面插入地图.md)

<hr>


### [Mysql](./数据库/MySql)

#### [Mysql 安装](./数据库/MySql/MySql基础/Mysql安装.md)

#### [DML 操作](./数据库/MySql/MySql基础/DML语言.md)

#### [DQL 查询](./数据库/MySql/MySql基础/DQL语言.md)

#### [Mysql 指令](./数据库/MySql/MySql基础/MySql基本指令.md)

#### [外键](./数据库/MySql/MySql基础/外键.md)

#### [MD 5 加密以及内置函数](./数据库/MySql/MySql高级/MD5%20加密.md)

#### [常用函数示例](./数据库/MySql/MySql高级/MySQL函数.md)

#### [Mysql备份](./数据库/MySql/MySql高级/Mysql备份.md)

#### [Mysql 事务](./数据库/MySql/MySql高级/事务.md)

#### [权限以及表维护](./数据库/MySql/MySql高级/权限及如何设计数据库.md)

#### [Mysql 索引](./数据库/MySql/MySql高级/索引.md)


<hr>


### [Mybatis](./数据库/MyBatis)


#### [SpringBoot整合Mybatis 基本使用](./数据库/MyBatis/SpringBoot整合Mybatis.md)

#### [Mybatis实现多表联查](./数据库/MyBatis/MyBatis多表联查.md)

#### [Mybatis 延迟加载](./数据库/MyBatis/Mybatis延迟加载.md)

#### [Mybatis 缓存](./数据库/MyBatis/MyBatis缓存.md)

#### [``Mybatis``中``#``号和``$``号的区别](./面试之旅/Mybatis中%23号和%24号的区别.md)

#### [Plus 的使用](./数据库/MyBatis/基本使用Plus.md)

#### [常见Java 包 解释](./数据库/MyBatis/常见包的理解.md)

#### [``MyBatis``实现批量插入](./WorkStudy/mybtais批量插入.md)

#### [``MBP``实现分页查询](./日常Blog/MybatisPlus实现分页查询.md)

#### [`Mybatis`使用`Sql`进行模糊查询](./日常Blog/Mybatis使用Sql进行模糊查询.md)

#### [`MyBatis`之分页查询-`MyBatis PageHelper`](./日常Blog/MyBatis之分页查询-MyBatis_PageHelper.md)

#### [`Mybatis`中大于小于等于](./日常Blog/Mybatis中大于小于等于.md)

#### [`Mybtais`封装成`Map`结果](./日常Blog/Mybtais封装成Map结果.md)

<hr>

### [Redis](./数据库/Redis/Redis基础)

#### [Redis基本安装知识](./数据库/Redis/Redis基础/Redis入门以及安装还有一些基本知识.md)

#### [Redis五大数据类型和三种特殊的数据类型](./数据库/Redis/Redis基础)

#### [Redis缓存穿透和雪崩](./数据库/Redis/Redis高级/Redis缓存穿透和雪崩.md)

#### [Redis 事务](./数据库/Redis/Redis高级/事务.md)

#### [Redis 哨兵模式](./数据库/Redis/Redis高级/哨兵模式.md)

#### [Redis redis.conf详解](./数据库/Redis/Redis高级/Redis.conf详解.md)

#### [Redis 主从复制](./数据库/Redis/Redis高级/Redis主从复制%20.md)

#### [Redis 发布订阅](./数据库/Redis/Redis高级/Redis发布订阅.md)

#### [Redis 持久化](./数据库/Redis/Redis高级/Redis持久化.md)

#### [Java 操作Redis](./数据库/Redis/Redis高级/SpringBoot整合.md)

#### [Springboot整合Redis实现缓存](./数据库/Redis/Springboot整合Redis.md)

<hr>

### [``JAVA``设计模式(补充中...)](./日常Blog/23设计模式.md)


<hr>

### [Oracle 数据库](./数据库/Oracle/PLSql基本语法.md)

<hr>

### 面试

#### [多线程](./面试之旅/多线程.md)

#### [JDBC](./面试之旅/JDBC.md)

#### [SOA 模式](./面试之旅/SOA模式.md)

#### [FastJson使用](./面试之旅/FastJson.md)

#### [面试集锦](./面试之旅/面试集锦)

#### [``Spring Bean``的生命周期](./面试之旅/Spring%20Bean的生命周期.md)

#### [汉得笔试](./面试之旅/汉得笔试总结.md)

#### [Java面试小册子](./面试之旅/Java面试小手册.md)

#### [享道出行面经](./面试之旅/赛可出行.md)



<hr>

### ``Java``相关

#### [``Java``学习线路](./日常Blog/Java线路.md)

#### [``Java8`` 内置函数式接口](./日常Blog/Java8内置四大函数式接口.md)

#### [``Stream``流的使用](./日常Blog/Stream流的使用.md)

#### [``UML``图关系介绍](./日常Blog/UML图关系介绍.md)

#### [``StringFormat``的使用](./Java后端/StringFormat的使用.md)

#### [过滤器和拦截器的区别](./面试之旅/过滤器和拦截器的区别.md)

#### [路径的讲解](./面试之旅/路径讲解.md) 

#### [位，字节，字符讲解](./面试之旅/位，字节，字符等讲解.md)

#### [Cache、Cookie、Session、Token](./面试之旅/Cache、Cookie、Session、Token.md)

#### [``equals``与 ``==``区别](./面试之旅/equals与==区别.md)

#### [正则表达式中的特殊字符](./Java后端/正则表达式中的特殊字符.md)

#### [Apache Commons Lang 3.10使用简介](./项目学习/Apache%20Commons%20Lang使用.md)

#### [``Jackson`` 使用方法总结](./项目学习/Jackson%20使用方法总结.md)

#### [二分查找](./面试之旅/查找与排序/二分查找.md)

#### [``JVM``的第一次探究](./面试之旅/JVM原理探究第一次.md)

#### [如何保证接口的幂等性](./面试之旅/如何保证接口的幂等性.md)

#### [``Hutool``使用指南](./日常Blog/Hutool使用指南.md)

#### [超级好用的``StringJoiner``](./日常Blog/超级好用的StringJoiner.md)

#### [关于 ``Try-With-Source`` 的使用以及注意点](./日常Blog/关于TryWithSource.md)

#### [关于``YAML``中``List``存放``Map``](./日常Blog/关于YAML中List存放Map.md)

#### [``Corn``表达式](./日常Blog/corn表达式.md)

#### [关于``Exception``的一些小心得](./日常Blog/关于Exception的处理细节.md)

#### [``Assert``断言](./日常Blog/Assert断言.md)

#### [关于本地``Git``传送``Blog``的注意点](./日常Blog/关于Git写Blog的注意点.md)

#### [`EasyExcel`的使用](./日常Blog/EasyExcel的使用.md)

#### [布隆过滤器](./日常Blog/布隆过滤器.md)

#### [`java stream`中`Collectors`的用法](./日常Blog/stream中Collectors的用法.md)


#### [`Spring`手动回滚事务](./日常Blog/Spring手动回滚事务.md)

#### [`stream`中`Collectors`的用法](./日常Blog/stream中Collectors的用法.md)

<hr>

### [小程序项目杂记](./项目学习/小程序/小程序数据相关踩坑.md)

#### [小程序获取``openId``](./项目学习/小程序/小程序关于获取openID与用户信息.md)

#### [小程序使用``weUI``](./项目学习/小程序/小程序使用weiUI.md)

#### [``Ngrok``的使用(内网穿透)](./Java后端/Ngrok.md)

#### [``Java``中的可以使用的工具类,很方便](./日常Blog/Java自带的好用的工具类.md)

<hr>

## [面试资料pdf下载](./面试之旅/面试资料下载pdf)

## [Mac](./Mac)

## [``IDEA``快捷键](./日常Blog/IDEA快捷键.md)

## [``VsCode``快捷键](./日常Blog/VSCode快捷键.md)

## [``UML``图讲解](./日常Blog/UML图关系介绍.md)

## [``IDEA``小图标含义](./日常Blog/IDEA小图标含义.md)

## [``DateUse``](./研究生生涯/单基因病遗传数据库/数据挖掘)

### [``Anaconda``的基本使用](./研究生生涯/单基因病遗传数据库/数据挖掘/Anaconda基本.md)

### [`numpy`的使用](./研究生生涯/单基因病遗传数据库/数据挖掘/数据分析/)

### [`UbUntu`中的数据库安装](./日常Blog/UbUntu中的数据库安装.md)


<hr/>

## [Algorithm](./algorithm/)

### [二叉树遍历算法](./algorithm/二叉树/二叉树遍历.md)

### [分治算法](./algorithm/dp算法/动态规划.md)

### [贪心算法](./algorithm/贪心算法/贪心算法.md)

### [dp算法](./algorithm/dp算法/动态规划.md)

<hr/>

## [数据挖掘](./研究生生涯/单基因病遗传数据库/数据挖掘/dataMing/)

### [关于使用`Jupyter`时`Mac`乱码](./研究生生涯/单基因病遗传数据库/数据挖掘/dataMing/关于Mac中jupyter乱码.md)


<div style="height:200px;"></div>


如果有错误的地方请指出，个人邮箱 71205901060@stu.ecnu.edu.cn

