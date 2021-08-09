# Docker安装tomcat


```
# 官方的使用
docker run -it --rm tomcat:9.0
# 之前的启动都是后台，停止了容器，容器还是可以查到， docker run -it --rm image 一般是用来测试，用完就删除
--rm       Automatically remove the container when it exits
#下载
docker pull tomcat
#启动运行
docker run -d -p 8080:8080 --name tomcat01 tomcat
#测试访问有没有问题
curl localhost:8080

#进入容器
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
db09851cf82e        tomcat              "catalina.sh run"   28 seconds ago      Up 27 seconds       0.0.0.0:8080->8080/tcp   tomcat01
➜  ~ docker exec -it db09851cf82e /bin/bash             
root@db09851cf82e:/usr/local/tomcat# 
# 发现问题：1、linux命令少了。 2.没有webapps

```

> 思考问题：我们以后要部署项目，如果每次都要进入容器是不是十分麻烦？要是可以在容器外部提供一个映射路径，webapps，我们在外部放置项目，就自动同步内部就好了！

