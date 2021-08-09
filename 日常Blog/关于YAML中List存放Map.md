# 关于``YAML``中``List``存放``Map``

## ``YAML``如何书写

> ``YAML``语法
```
  大小写敏感
  使用缩进表示层级关系
  缩进时不允许使用Tab键，只允许使用空格。
  缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
```
> 支持的数据结构
```
对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
基本类型：单个的、不可再分的值
```
> 基本语法
- ``k:``(空格)``v：``表示一对键值对
- ``v:``包含上面举的数据
- ``k;string``类型

> ``value``的使用说明

- 基本类型：数字，字符串，布尔

    - ``k: v：``字面直接来写；
    - 字符串默认不用加上单引号或者双引号；
- ``""``：双引号:不会转义字符串里面的特殊字符；

     - 特殊字符会作为本身想表示的意思；
     - 如 ：``name: "zhangsan \n lisi"：``输出；``zhangsan`` 换行 ``lisi``
- ``''``：单引号:转义特殊字符

     - 特殊字符终只是一个普通的字符串数据 ，
     - 如： ``name: ‘zhangsan \n lisi’：输出；zhangsan \n lisi ``

> 对象、Map（属性和值）（键值对）：
```yml
 k: v：在下一行来写对象的属性和值的关系；注意缩进对象还是k: v的方式

friends: 
 	lastName: zhangsan          
 	age: 20
 ## 行内写法：friends: {lastName: zhangsan,age: 18}
```

> 数组（``List、Set``）：

用- 值表示数组中的一个元素
```yml
  pets:  
 	‐ cat  
 	‐ dog  
 	‐ pig
 # 行内写法 pets: [cat,dog,pig]
```

## ``yml``的使用

- 简单类型使用 ``@Value``

- 复杂类型使用 ：``@ConfigurationProperties``

  1. 如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用``@Value；`` 
  2. 如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用``@ConﬁgurationProperties；``

``yml``文件：
```yml
spring:
  test: 小猪
  #list<map>
  testList:
    -
      name: 小王
      age: 12
    -
      name: 小李
      age: 13
  # map<String,String>
  testMap:
    name: 小朱
    age: 14
testname: 小明
```
## 使用``@Value``
```java
/**
* value 的使用
*/
@Component
@Data
public class ValueTest {
     // 直接赋值

     @Value("小李")
     private  String name;

     @Value("1")
     private int num;

     //读取配置文件的信息
     @Value("${spring.testMap.name}")
     private String name2;

     @Value("${spring.test}")
     private String name1;
     //报错
     //@Value("${spring.testMap}")
     private Map<String,String> map;

     //使用SpEL
     @Value("#{1+1}")
     private int num1;
}
```
进行测试``Test``:

```java
@Autowired
private ValueTest valueTest;
@Test
public void test111(){
     System.out.println(valueTest);
}
// ValueTest{name='小李', num=1, name2='小朱', name1='小猪', map=null, num1=2}
```
## 使用``@ConfigurationProperties``
```xml
<dependency>         
     <groupId>org.springframework.boot</groupId>              					
     <artifactId>spring‐boot‐configuration‐processor</artifactId>              
     <optional>true</optional>              
</dependency>
```
```java
@Data
@Component
@ConfigurationProperties(prefix = "spring")
@Validated
public class TestConfigurationProperties {
    private List<Map> testList;
    private Map<String,String> testMap;

    private String name;

    private String test;
}
```
```java

@Autowired
    private TestConfigurationProperties testConfigurationProperties;

    @Test
    public void test8(){
        System.out.println(testConfigurationProperties.getTestMap());
        System.out.println(testConfigurationProperties.getTestList());
        System.out.println(testConfigurationProperties.getName());
        System.out.println(testConfigurationProperties.getTest());

    }
//{name=小朱, age=14}
//[{name=小王, age=12}, {name=小李, age=13}]
//null
//小猪
```





