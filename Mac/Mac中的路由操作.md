# 路由操作

## 指令
```shell
route -n add -net 192.168.0.0（需进入的网段） -netmask 255.255.255.0 （掩码）192.168.5.254 （进该网段的网关）
```
这只是静态添加，不算永久，重启后应该会消失，网上有人说这是从内核的ip路由表中添加的。我总感觉某人在嘲笑我的``linux``内核学的很差。。所以不相信。
## 路由删除
```shell
route -v delete -net 10.10.12.0（某网段） -gateway 10.10.12.1（某网关）
```

## 要是删除失败---

```shell
route -n delete -net 10.10.12.0
```

## 路由查看

```shell
为了看下添加和删除的效果，直接用netstat -r命令
```

<hr/>

> 打开终端，删除默认路由
```shell
sudo route -n delete 0.0.0.0
```
> 增加默认路由到无线网络
```shell
sudo route -n add 0.0.0.0 192.168.43.1
其中192.168.43.1是wifi的ip网关,网关就是路由器
```
> 关闭wifi，连接网线,增加有线的路由
```shell
sudo route -n add 172.16.154.xxx 172.16.154.254
其中172.16.154.xxx是你的有线网卡ip，172.16.154.254是网关
```
> 打开wifi，在使用两个网卡的时候，内外网均可访问
```shell
附加：
删除路由：route -v delete -net 172.16.154.0 -gateway 172.16.154.254
其中 172.16.154.0（某网段），172.16.154.254（某网关）
```

> 路由查看：
```shell
netstat -r
```

