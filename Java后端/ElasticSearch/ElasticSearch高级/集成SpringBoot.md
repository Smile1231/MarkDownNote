# 集成SpringBoot

找文档 : [https://www.elastic.co/guide/en/elasticsearch/client/index.html](https://www.elastic.co/guide/en/elasticsearch/client/index.html)
![image.png](https://i.loli.net/2020/12/30/gDipUcHC5swhxdQ.png)

![image](https://img-blog.csdnimg.cn/20200808113921437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FkbWluNzQxYWRtaW4=,size_16,color_FFFFFF,t_70)

> Maven依赖

```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.10.1</version>
</dependency>
```
找对象
![image.png](https://i.loli.net/2020/12/30/4LcF7VCTGiXIZR5.png)

> 配置基本的项目

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1597907258818.png)

> 注意: 问题:一定要保证我们导入的依赖和我们的ES版本一致

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1597907493787.png)


按照官网的操作我们要构建一个对象

![image.png](https://i.loli.net/2020/12/30/b6QsCrMo91SZJLT.png)


```java
@Configuration
public class ElasticSearchClientConfig {

    @Bean
    public RestHighLevelClient restHighLevelClient(){
        RestHighLevelClient client = new RestHighLevelClient(
                RestClient.builder(
                        new HttpHost("localhost", 9200, "http")));
        return  client;
    }
}
```
分析源码

狂神的Spring步骤:

- 找对象

- 放到spring中待用

- 如果是springboot,那就先分析源码

- xxxAutoConfiguration,xxxProperties

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1597908504743.png)

源码中提供的对象

虽然这里导入了3个类,静态内部类,核心类就一个



