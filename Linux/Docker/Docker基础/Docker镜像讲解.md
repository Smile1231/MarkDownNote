# Docker镜像讲解

## 镜像是什么
> 镜像就是一个轻量级的,可执行的独立软件包,用来打包软件运行环境和基于运行环境开发的软件,它包含运行某个软件所需的所有内容,包括代码,运行时,库,环境变量和配置文件。所有的应用,直接打包docker镜像,就可以直接跑起来!


### 如何得到镜像:

- 从远程仓库下载
- 朋友拷贝给你
- 自己制作一个镜像 DockerFile

### commit镜像
```
docker commit 提交容器成为一个新的镜像

#命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器ID 目标镜像名:[tag]


```


```
[root@CZP ~]# docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
db186da947d7        portainer/portainer   "/portainer"             16 hours ago        Up 16 hours         0.0.0.0:8088->9000/tcp                           interesting_shockley
bd4094db247f        elasticsearch:7.6.2   "/usr/local/bin/dock…"   17 hours ago        Up 17 hours         0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   elasticsearch
94b00b6f6172        tomcat:9.0            "catalina.sh run"        17 hours ago        Up 17 hours         0.0.0.0:8080->8080/tcp                           tomcat
d458bc50a808        nginx                 "/docker-entrypoint.…"   18 hours ago        Up 18 hours         0.0.0.0:80->80/tcp                               nginx01
63d4c4115212        36304d3b4540          "docker-entrypoint.s…"   22 hours ago        Up 22 hours         0.0.0.0:6379->6379/tcp                           redis
[root@CZP ~]# docker commit -a="czp" -m="add basic webapps app" 94b00b6f6172 tomcat_9.0:1.0
sha256:75e6ea173695b146c9ddf9d5865e7bdeb78e69c84d2d3520e516cd9f498a1e9a
[root@CZP ~]# docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
tomcat_9.0            1.0                 75e6ea173695        8 seconds ago       652MB
nginx                 latest              2622e6cca7eb        33 hours ago        132MB
portainer/portainer   latest              cd645f5a4769        9 days ago          79.1MB
redis                 latest              36304d3b4540        13 days ago         104MB
mysql                 latest              30f937e841c8        2 weeks ago         541MB
tomcat                9.0                 1b6b1fe7261e        3 weeks ago         647MB
elasticsearch         7.6.2               f29a1ee41030        2 months ago        791MB
elasticsearch         latest              5acf0e8da90b        20 months ago       486MB

```




