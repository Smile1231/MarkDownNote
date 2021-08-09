## IK分词器插件

### 什么是分词器

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596637415868.png)

==如果使用中文,建议使用ik分词器==

IK体用了两个分词算法: ik_smart 和,其中 ik_smart 为最少(ˉ▽￣～) 切~~分, ik_max_word 为最细粒度划分! 一会我们测试

> 安装


> 下载完毕之后,放入到我们的elasticsearch插件即可

![image](http://victorfengming.gitee.io/course/elasticsearch/base/08-elasticsearch-IK%E5%88%86%E8%AF%8D%E5%99%A8%E6%8F%92%E4%BB%B6.assets/1596637767841.png)

 - 重启观察ES
    ![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596674648793.png)
 - elasticsearch-plugin 可以通过这个命令来查看加载的插件
  ![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596674703775.png)
- 使用kibana测试!
    ![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596674979545.png)

## ik分词器添加我们自己的配置

这种自己需要的词,需要自己加到我们的分词器字典中!

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596675349989.png)

重启ES,看细节
![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/1596675426305.png)