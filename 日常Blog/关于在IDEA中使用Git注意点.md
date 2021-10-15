# 关于在`IDEA`中使用`Git`的一些操作

今天在项目中使用`Git`想撤销已`Commit`但是没有`Push`的版本


一不小心点了`Drop`,导致代码全部丢失,瞬间人就没了,于是在紧急中我决定寻找救济方案

<br/>


首先执行`git reflog`查看本地记录,记录都是从最新的翻页显示到历史的,


![图 1](../images/be62f7e4b29b6e27e8e758b80873d4f73facb0edf41b015a04c14a718ebe99e7.png)  

退出浏览是在英文状态下按`Q`键

找到想回退的那一行操作,最前面是`HASH`值

然后在 `Terminal`中输入

`git reset --hard (hashValue)`

![图 2](../images/de7a5125ea8fb624b91f6b8f87b85195bac581e95b19cb292def013e48d88847.png)  

就可以了

吓死我了


同时如果在`IDEA`中想撤销上一次`Commit`,

![图 3](../images/5f183aa249b7e77bf589e71bc2dc29ca62593d24b34a93d77f411075267375c8.png)  

在输入框中输入`HEAD~1`即可撤销上一次`Commit`

![图 4](../images/edb5cb27eccde06ba9466be601cf6021856af3d3d52107d2734241e66ac0d41f.png)  




有惊无险!!!