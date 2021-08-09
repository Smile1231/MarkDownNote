# Java API操作 ES

## 关于索引的API操作详解

> 具体的Api测试!

``restHighLevelClient.indices().xxx()``

> 创建索引


```java
// index create
@Test
public void create() throws IOException {
    // 1. 构建索引请求实体类 就是创建索引时的{}
    CreateIndexRequest springboot_index = new CreateIndexRequest("springboot_index");
   
    // 2.执行创建索引请求,返回响应
    CreateIndexResponse createIndexResponse =
            restHighLevelClient.indices().create(springboot_index, RequestOptions.DEFAULT);
   
    System.out.println(createIndexResponse);
}
```

> 判断索引是否存在


```java
@Test
 void getIndices() throws IOException {
     // 1. 创建请求体内容对象
     GetIndexRequest srpingboot_index = new GetIndexRequest("springboot_index");
   
     // 2. action
     boolean exists = restHighLevelClient.indices().exists(srpingboot_index,
             RequestOptions.DEFAULT);
     System.out.println(exists);
     GetIndexResponse getIndexResponse = restHighLevelClient.indices().get(srpingboot_index,
             RequestOptions.DEFAULT);
     System.out.println(getIndexResponse);
 }
```

> 删除索引


```java
@Test
void deleteIndices() throws IOException {
    // 1. 创建请求体内容对象,删除时索引需存在！
    DeleteIndexRequest springboot_index = new DeleteIndexRequest("springboot_index");
   
    // 2. action
    AcknowledgedResponse delete = restHighLevelClient.indices().delete(springboot_index,
            RequestOptions.DEFAULT);
    System.out.println(delete.isAcknowledged());
}
```

## API操作文档

> 创建文档

```java
@Test
void createDocument() throws IOException {
    // put /index/_create/id {...}
    // 1. 构建创建的文档的内容
    IndexRequest indexRequest = new IndexRequest("springboot_index");
    indexRequest.timeout("1s");
    indexRequest.id("1");
    // public IndexRequest source(String source, XContentType xContentType)
    indexRequest.source(JSON.toJSONString(new User("zxj", 18)), XContentType.JSON);
   
    // 2. 开始请求
    IndexResponse index = restHighLevelClient.index(indexRequest, RequestOptions.DEFAULT);
    System.out.println(index.status());
}
```
结果

这里的返回的全部内容和我们的命令是一样的
![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/image-20201018194243887.png)


> 更新文档信息


```java
@Test
   void updateDocument() throws IOException {
       // post /index/_update/id {"doc":{...}}
       // 1. 构建修改的文档的内容,String index, String id
       UpdateRequest updateRequest = new UpdateRequest("springboot_index", "1");
       updateRequest.timeout("1s");
       updateRequest.doc(JSON.toJSONString(new User("new Name", 18)), XContentType.JSON);

       // 2. 开始请求
       UpdateResponse updateResponse = restHighLevelClient.update(updateRequest, RequestOptions.DEFAULT);
       System.out.println(updateResponse.status());
       System.out.println(updateResponse.toString());
   }
```

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/image-20201018194653575.png)

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/image-20201018194726197.png)

> 删除文档记录


```java
@Test
   void dropDocument() throws IOException {
       // delete /index/_doc/id
       // 1. 构建删除文档的对象,String index, String id
       DeleteRequest deleteRequest = new DeleteRequest("springboot_index", "1");
       deleteRequest.timeout("1s");

       // 2. 开始请求
       DeleteResponse delete = restHighLevelClient.delete(deleteRequest, RequestOptions.DEFAULT);
       System.out.println(delete.status());
       System.out.println(delete.toString());
   }
```
![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/image-20201018195024638.png)

## 批量操作

``特殊的,真的项目一般都会批量插入数据``

批量操作bulkRequest有很多类型：例如下述。这里只展示批量增加

![image](https://zxj-typora.oss-cn-shanghai.aliyuncs.com/img/image-20201018195851648.png)

> 批量添加文档：

```java
@Test
void bulgRequest() throws IOException {
    // 1. 构建批量请求体
    BulkRequest bulkRequest = new BulkRequest();
    bulkRequest.timeout("10s");
    for (int i = 0; i < 10; i++) {
        IndexRequest indexRequest = new IndexRequest("springboot_index");
        indexRequest.source(JSON.toJSONString(new User("zxj"+i, 18)), XContentType.JSON);
        bulkRequest.add(indexRequest);
    }

    BulkResponse bulk = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
    System.out.println(!bulk.hasFailures());
    System.out.println(bulk.toString());
}
```

## 查询搜索🔺

```java
@GetMapping("search/{key}/{page}/{size}")
public List<Map<String, Object>> searchLists(@PathVariable("key") String keyword,
                                             @PathVariable("page") Long page, @PathVariable(
                                                     "size") Long size) throws IOException {
    // 构建搜索对象
    SearchRequest searchRequest = new SearchRequest("jd1");
    // 搜索条件构造
    SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
    searchSourceBuilder.timeout(new TimeValue(2L, TimeUnit.SECONDS));

    // 匹配查询
    MatchQueryBuilder title = QueryBuilders.matchQuery("title", keyword);
    searchSourceBuilder.query(title);

    // page
    if (page<=1L){
        page = 1L;
    }
    searchSourceBuilder.from((int) (page * size));
    searchSourceBuilder.size(Math.toIntExact(size));

    // highlight
    HighlightBuilder highlightBuilder = new HighlightBuilder();
    highlightBuilder.field("title"); // title字段高亮
    highlightBuilder.requireFieldMatch(false); // 关键字高亮一次
    highlightBuilder.preTags("<span style='color:red'>");
    highlightBuilder.postTags("</span>");
    searchSourceBuilder.highlighter(highlightBuilder);

    searchRequest.source(searchSourceBuilder);

    ArrayList<Map<String, Object>> objects = new ArrayList<>();

    SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
    for (SearchHit hit : searchResponse.getHits().getHits()) {
        // 查询结果
        Map<String, Object> sourceAsMap = hit.getSourceAsMap();

        // 将高亮的字段覆盖查询结果的该字段
        HighlightField highlightField = hit.getHighlightFields().get("title");
        if (highlightField != null) {
            Text[] fragments = highlightField.fragments();
            StringBuilder s = new StringBuilder();
            for (Text fragment : fragments) {
                s.append(fragment);
            }
            sourceAsMap.put("title", s.toString());
        }
        objects.add(sourceAsMap);
    }
    return objects;
}
```
