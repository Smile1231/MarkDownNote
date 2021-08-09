# 配置YAML文件

## Yaml配置

> 基本语法

```yaml
- key: value；kv之间有空格
- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
- 字符串无需加引号，如果要加，''与""表示字符串内容 会 被 转义/不转义
```
> 数据类型

- 字面量：单个的、不可再分的值。date、boolean、string、number、null
  
  ```yaml
   k: v
  ```
- 对象：键值对的集合。map、hash、set、object
  
  ```yaml
    行内写法：  k: {k1:v1,k2:v2,k3:v3}
    #或
    k: 
    	k1: v1
      k2: v2
      k3: v3
  ```
- 数组：一组按次序排列的值。array、list、queue
  
  ```yaml
    行内写法：  k: [v1,v2,v3]
    #或者
    k:
     - v1
     - v2
     - v3
  ```
### 配置提示
自定义的类和配置文件绑定一般没有提示。
```yaml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-configuration-processor</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

