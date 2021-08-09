# Dockerfile
DockerFile是用来构建docker镜像的文件!命令参数脚本!

构建步骤:

- 编写一个dockerfile脚本
- docker build 构建成为一个镜像
- docker run 运行镜像
- docker push发布镜像(Docker hub , 阿里云镜像仓库! )
 
![image](https://img-blog.csdnimg.cn/20200619183412937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)

![image](https://img-blog.csdnimg.cn/20200619183425960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)
很多官方镜像都是基础包, 很多功能没有,我们通常会自己搭建自己的镜像!

官方可以制作镜像,那么我们也可以!

## Dockerfile构建过程
很多指令:

- 每个保留关键字(指令)都是必须要大写
- 执行从上到下顺序执行
- ‘#’ 表示注释
- 每一个指令都会创建提交一个新的镜像层,并提交 !

![image](https://img-blog.csdnimg.cn/20200619183442209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)

dockerfile是面向开发的,我们以后要发布项目,做镜像就需要编写dockerfile文件,这个文件十分简单

Docker镜像 逐渐成为了一个企业交付的标准,必须要掌握 !

步骤: 开发,部署,上线,运维…缺一不可

1. DockerFIle: 构建文件,定义了一切的步骤 ,源代码

2. DockerImages: 通过DockerFile构建生成的镜像,最终发布运行的产品,原来是一个jar,war

3. Docker容器: 容器就是镜像运行起来提供服务的

## DockerFile的指令
以前的话我们是使用的别人的,现在我们知道了这些指令后,我们来练习自己写一个镜像!

```shell
FROM 	# 基础镜像, 一切从这里开始构建
MANTAINER # 镜像是谁写的, 姓名+邮箱
RUN #镜像构建的时候需要运行的命令
ADD # 步骤, tomcat镜像,压缩包! 添加内容
WORKDIR # 镜像的工作目录
VOLUME # 挂载的目录
EXPOSE # 暴露端口配置
RUN #运行
CMD # 指定这个容器启动的时候要运行的命令,只有最后一个会生效,可被替代
ENTRYPOINT # 指定这个容器启动的时候要运行的命令,可以追加命令
ONBUILD # 当构建一个被继承 DockerFile 这个时候就会运行ONBUILD的指令,触发指令
COPY	#	类似ADD,将我们文件拷贝到镜像中
ENV 	# 构建的时候设置环境变量! 

```
![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zczAuYmRzdGF0aWMuY29tLzcwY0Z1SFNoX1ExWW54R2twb1dLMUhGNmhoeS9pdC91PTI2ODk3NDY0OSwyNjA3MDE5OTExJmZtPTI2JmdwPTAuanBn?x-oss-process=image/format,png)
## 实战测试
Docker Hub 中99%镜像都是从centos基础镜像过来的,然后配置需要的软件

![image](https://img-blog.csdnimg.cn/20200619184350120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)
```shell
创建一个自己的centos
```
```shell
# 1 编写一个DOckerfile的文件

FROM centos

MAINTAINER czp<2432688105@qq.com>

ENV MYPATH /usr/local

WORKDIR $MYPATH

RUN yum -y  install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"


# 2. 通过这个文件构建镜像
# 命令 docker build -f dockerfile文件路径 -t
Successfully built 5ebc296aad5a
Successfully tagged mycentos:1.0

# 3. 测试运行
```

增强之后的镜像

![image](https://img-blog.csdnimg.cn/20200619184405292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)

我们可以列出本地进程的历史

![image](https://img-blog.csdnimg.cn/20200619184416447.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)

我们平时拿到一个镜像,可以研究一下它是怎么做的

### CMD 和ENTRYPOINT的区别

```shell
CMD # 指定这个容器启动的时候要运行的命令,只有最后一个会生效,可被替代
ENTRYPOINT # 指定这个容器启动的时候要运行的命令,可以追加命令

```
测试cmd命令

```shell

# 编写
[root@CZP dockerfile]# cat dockerfile-centos-test 
FROM centos
CMD ["ls","-a"]


# 构建镜像
[root@CZP dockerfile]# docker build -f dockerfile-centos-test -t centostest .

# 想要追加一个命令 -l  ls -al
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\": executable file not found in $PATH": unknown.
ERRO[0000] error waiting for container: context canceled 

# cmd的情况下 替换了CMD["ls","-a"]命令,-不是命令追加

```

ENTRYPOINT是往命令之后追加

## 实战:Tomcat镜像
1. 准备镜像文件. tomcat压缩包, jdk压缩包!
2. ![image](https://img-blog.csdnimg.cn/20200619184436678.png#pic_center)
3. 编写Dockerfile文件, 官方命名 Dockerfile, build会自动寻找这个文件,就不需要 -f 指定了!

```shell

FROM centos
MAINTAINER czp<2432688105@qq.com>

COPY readme.txt /usr/local/readme.txt

ADD apache-tomcat-9.0.33.tar.gz /usr/local/

ADD jdk-8u221-linux-x64.rpm /usr/local/


RUN yum -y install vim
 
ENV MYPATH /usr/local
 
WORKDIR $MYPATH
 
ENV JAVA_HOME /usr/local/jdk1.8.0_11
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.33

ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.33

# 配置环境变量
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:/CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-9.0.33/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.33/bin/logs/catalina.out
```
4. 构建镜像
```shell
# docker build -t diytomcat .

```
5. 本地测试

```shell
curl localhost:9090
```

## 发布自己的镜像
```
DockerHub
```
1. 地址hub.docker.com 注册自己的账号!
2. 确定这个账号可以登录
3. 在服务器上提交自己的镜像


```
[root@CZP ~]# docker login --help

Usage:	docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username

```
4. 登录完毕就可以提交镜像了,就是一步 docker push


```
#push自己的镜像到服务器上一定要带上版本号
[root@CZP ~]# docker push czp/centos:1.0


docker tag [id] [tag] 为容器添加一个版本

```

==提交到阿里云镜像仓库==
1. 登录阿里云
2. 找到容器镜像服务
3. 创建命名空间

![image](https://img-blog.csdnimg.cn/20200619184533739.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)

4. 创建容器镜像
![image](https://img-blog.csdnimg.cn/20200619184546684.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)

5. 浏览阿里云
![image](https://img-blog.csdnimg.cn/2020061918455938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)

## 小结
![image](https://img-blog.csdnimg.cn/20200619184610970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDUwMjUwOQ==,size_16,color_FFFFFF,t_70#pic_center)


