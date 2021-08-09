# ElasticSearch安装
一定要在服务器上面搭建

下载地址:https://www.elastic.co/cn/downloads/elasticsearch

官网下载巨慢,翻墙,网盘中下载即可

华为云: https://mirrors.huaweicloud.com/elasticsearch/7.6.2/

我们学习的话Window和Linux都可以学习 ==我们这里现在window下学习==

ELK三剑客,解压即用!(web项目! 前端环境! npm 下载依赖)

Node.js python2


> window下安装!

- elasticSearch
- elasticSearch Head (一个前端项目)
- kibana

> windows下下载好了 es的项目


```
安装：
     1 解压，
     2 bin
     3 启动
目录：
     bin 启动文件
     config 配置文件
         log4j2 日志配置文件
         jvm.options java 虚拟机相关的配置
         elasticsearch.yml elasticsearch 的配置文件！ 默认 9200 端口！ 跨域！
         lib 相关jar包
         logs 日志！
         modules 功能模块
         plugins 插件！

```
> 安装可视化插件


```
安装：
          1 elasticsearch-head-master，解压
          2 npm install   npm run start ， 需要node环境
          3 解决跨域问题，elasticsearch->config->elasticsearch.yml配置：
             http.cors.enabled: true
             http.cors.allow-origin: "*"
```

进入到bin目录下 .bat文件
![image.png](https://i.loli.net/2020/12/30/FZs76G4dncHt9kz.png)

双击运行


> Kibana 只是一个开发工具

启动只需要bin目录下的 .bat文件即可

```
# 如需汉化
  \x-pack\plugins\translations\translations 下有 zh-CN.json 文件
  
 # 只需要在 kibana.yml 文件中 加入 
  i18n.locale: "zh-CN"
 
```
