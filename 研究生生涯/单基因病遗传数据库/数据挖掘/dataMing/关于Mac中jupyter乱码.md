# 关于`Mac`中`jupyter`乱码

## 步骤一 查看自己的字体中有哪种中文字体

```python
import matplotlib
a=sorted([f.name for f in matplotlib.font_manager.fontManager.ttflist])
for i in a:
	print(i)
```
![图 1](../../../../../images/273bbef7dd633fffa434dede7a813f0c90a10d9fa4fe6cc1d471e029cb93ca41.png)  

找到支持中文的字体
![图 2](../../../../../images/ef1580d6a85f7d5fb77045889af9cab6a80c79438de410a423065e5faf736070.png)  


## 选择相应的字体

```python
plt.rcParams['font.sans-serif']='Heiti TC'
plt.rcParams['axes.unicode_minus'] = False  # 负号正常显示
```
![图 3](../../../../../images/35f1d741d922f7a88d6542e063aeeaa66e95b1a1b14a33ea5517a2ba69794074.png)  





