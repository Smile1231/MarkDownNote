# `java stream`中`Collectors`的用法

- [`java stream`中`Collectors`的用法](#java-stream中collectors的用法)
  - [<span id="common">简介</span>](#简介)
  - [<span id="toList">`Collectors.toList()`</span>](#collectorstolist)
  - [<span id="toSet">`Collectors.toSet()`</span>](#collectorstoset)
  - [<span id="toColl">`Collectors.toCollection()`</span>](#collectorstocollection)
  - [<span id="toMap">`Collectors.toMap()`</span>](#collectorstomap)
  - [<span id="toCollAndThen">Collectors.collectingAndThen()</span>](#collectorscollectingandthen)
  - [<span id="join">`Collectors.joining()`</span>](#collectorsjoining)
  - [<span id="count">`Collectors.counting()`</span>](#collectorscounting)
  - [<span id="sum">`Collectors.summarizingDouble/Long/Int()`</span>](#collectorssummarizingdoublelongint)
  - [<span id="average">`Collectors.averagingDouble/Long/Int()`</span>](#collectorsaveragingdoublelongint)
  - [<span id="summing">`Collectors.summingDouble/Long/Int()`</span>](#collectorssummingdoublelongint)
  - [<span id="max">`Collectors.maxBy()/minBy()`</span>](#collectorsmaxbyminby)
  - [<span id="group">`Collectors.groupingBy()`</span>](#collectorsgroupingby)
  - [<span id="par">`Collectors.partitioningBy()`</span>](#collectorspartitioningby)

## <span id="common">简介</span>

在`java stream`中，我们通常需要将处理后的`stream`转换成集合类，这个时候就需要用到`stream.collect`方法。`collect`方法需要传入一个`Collector`类型，要实现`Collector`还是很麻烦的，需要实现好几个接口。

于是`java`提供了更简单的`Collectors`工具类来方便我们构建`Collector`。

下面我们将会具体讲解`Collectors`的用法。

假如我们有这样两个`list：`

```java
List<String> list = Arrays.asList("jack", "bob", "alice", "mark");
List<String> duplicateList = Arrays.asList("jack", "jack", "alice", "mark");
```
上面一个是无重复的`list`，一个是带重复数据的`list`。接下来的例子我们会用上面的两个`list`来讲解`Collectors`的用法。

## <span id="toList">`Collectors.toList()`</span>
```java
List<String> listResult = list.stream().collect(Collectors.toList());
        log.info("{}",listResult);
```
![图 3](../images/377c0dc6b78b952986fcdbda47c00e0de5541feda71f8f3b7be708a2b82ebb99.png)  

将`stream`转换为`list`。这里转换的`list`是`ArrayList`，如果想要转换成特定的`list`，需要使用`toCollection`方法。

## <span id="toSet">`Collectors.toSet()`</span>

```java
Set<String> setResult = list.stream().collect(Collectors.toSet());
        log.info("{}",setResult);
```
![图 4](../images/18288bdae22ec17290aa99d124621bd66dfe2215523189573c1d6c95447dd8a2.png)  

## <span id="toColl">`Collectors.toCollection()`</span>

上面的`toMap,toSet`转换出来的都是特定的类型，如果我们需要自定义，则可以使用`toCollection()`

```java
List<String> custListResult = list.stream().collect(Collectors.toCollection(LinkedList::new));
        log.info("{}",custListResult);
```
![图 5](../images/f3d2e616bf4355bd3de63fc6a0658ce23a22fc1d1de1b8e7c319c243b210e6b9.png)  

## <span id="toMap">`Collectors.toMap()`</span>

`toMap`接收两个参数，第一个参数是`keyMapper`，第二个参数是`valueMapper`:
```java
Map<String, Integer> mapResult = list.stream()
                .collect(Collectors.toMap(Function.identity(), String::length));
        log.info("{}",mapResult);
```
如果`stream`中有重复的值，则转换会报`IllegalStateException`异常：

```java
Map<String, Integer> duplicateMapResult = duplicateList.stream()
                .collect(Collectors.toMap(Function.identity(), String::length));
```
![图 6](../images/e79210b1bad80da9dd204bfbe09f8a555cdfb1fb5ea37f2293c598c4028c5605.png)  

解决方案:

```java
Map<String, Integer> duplicateMapResult2 = duplicateList.stream()
                .collect(Collectors.toMap(Function.identity(), String::length, (item, identicalItem) -> item));
        log.info("{}",duplicateMapResult2);
```
在`toMap`中添加第三个参数`mergeFunction`，来解决冲突的问题。

![图 7](../images/6ebc362b84d88b04d2f7bab5e412c00ba8c07bc0230a854750a5e45ff40b0d19.png)  

## <span id="toCollAndThen">Collectors.collectingAndThen()</span>

`collectingAndThen`允许我们对生成的集合再做一次操作。

```java
List<String> collectAndThenResult = list.stream()
                .collect(Collectors.collectingAndThen(Collectors.toList(), l -> {return new ArrayList<>(l);}));
        log.info("{}",collectAndThenResult);
```
![图 8](../images/ccc4748c1e6bc74f847395237b9f4792a105646d9ad72247fe63978d27e581d6.png)  


## <span id="join">`Collectors.joining()`</span>

`Joining`用来连接`stream`中的元素：

```java
String joinResult = list.stream().collect(Collectors.joining());
        log.info("{}",joinResult);
        String joinResult1 = list.stream().collect(Collectors.joining(" "));
        log.info("{}",joinResult1);
        String joinResult2 = list.stream().collect(Collectors.joining(" ", "prefix","suffix"));
        log.info("{}",joinResult2);
```
![图 9](../images/d387b3e828e21c8d34a028f885871517a596463285c8f0eb19d2d2325677fb46.png)  

可以不带参数，也可以带一个参数，也可以带三个参数，根据我们的需要进行选择。

## <span id="count">`Collectors.counting()`</span>

`counting`主要用来统计`stream`中元素的个数：

```java
Long countResult = list.stream().collect(Collectors.counting());
        log.info("{}",countResult);
```

![图 10](../images/e8c54338b72fcf05fb725384bec2e80fefd3ad1108a6a9db86c418cf750d01c5.png)  


## <span id="sum">`Collectors.summarizingDouble/Long/Int()`</span>

`SummarizingDouble/Long/Int`为`stream`中的元素生成了统计信息，返回的结果是一个统计类：

```java
IntSummaryStatistics intResult = list.stream()
                .collect(Collectors.summarizingInt(String::length));
        log.info("{}",intResult);
```
![图 11](../images/8752ee993a9d11103833ec7b353bd3fdfe7b42e249aef4debd2e8d8587a4f7b8.png)  

## <span id="average">`Collectors.averagingDouble/Long/Int()`</span>

`averagingDouble/Long/Int()`对`stream`中的元素做平均：

```java
Double averageResult = list.stream().collect(Collectors.averagingInt(String::length));
        log.info("{}",averageResult);
```
![图 12](../images/590dbc6f7e4b43e611699340fd3af0700148a03e357357d808ae36a7586c9834.png)  


## <span id="summing">`Collectors.summingDouble/Long/Int()`</span>

`summingDouble/Long/Int()`对`stream`中的元素做`sum`操作：

```java
Double summingResult = list.stream().collect(Collectors.summingDouble(String::length));
        log.info("{}",summingResult);
```

![图 13](../images/34229a149d90b4a4ce507df5e364a80b55dba1048b316306ff34ad3a8829ac34.png)  


## <span id="max">`Collectors.maxBy()/minBy()`</span>

`maxBy()/minBy()`根据提供的`Comparator`，返回`stream`中的最大或者最小值：
```java
Optional<String> maxByResult = list.stream().collect(Collectors.maxBy(Comparator.naturalOrder()));
        log.info("{}",maxByResult);
```

![图 14](../images/85aa6015319f64920aa49ff597ed122ba206019157ed21fad8a6292a39f4349c.png)  


## <span id="group">`Collectors.groupingBy()`</span>

`GroupingBy`根据某些属性进行分组，并返回一个`Map`：

```java
Map<Integer, Set<String>> groupByResult = list.stream()
                .collect(Collectors.groupingBy(String::length, Collectors.toSet()));
        log.info("{}",groupByResult);
```
![图 15](../images/7b9cae76f9bb07bf4b0f09c9cedb0d502058a9d90127951474acbd36455bf84a.png)  


## <span id="par">`Collectors.partitioningBy()`</span>

`PartitioningBy`是一个特别的`groupingBy，PartitioningBy`返回一个`Map`，这个`Map`是以`boolean`值为`key`，从而将`stream`分成两部分，一部分是匹配`PartitioningBy`条件的，一部分是不满足条件的：

![图 16](../images/8080f87394427e6b4b1b7c13f7c6dde168ee6ff12a5c3a9df0f7877d9833c0d2.png)  

