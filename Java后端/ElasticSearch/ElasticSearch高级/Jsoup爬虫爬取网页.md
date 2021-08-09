# Jsoup 爬虫爬取网页内容

> springboot导入maven依赖


```xml
<!--解析网页jsoup-->
<dependency>
	<groupId>org.jsoup</groupId>
	<artifactId>jsoup</artifactId>
	<version>1.10.2</version>
</dependency>
```

> 选择需要爬取的网页
![image.png](https://i.loli.net/2020/12/30/hC9ubzWQK3M6xrI.png)


**java操作**

```java
public List<Content> parseJD(String keywords) throws IOException {
        //获取请求 https://search.jd.com/Search?keyword=java&enc=utf-8&pvid=5452d8c0790c4c6fb86b61a5c8e9b880
        //前提,需要联网

        String url = "https://search.jd.com/Search?keyword="+ keywords+"&enc=utf-8";

        //解析网页(Jsoup 返回的就是Document 浏览器Doc对象)
        Document document = Jsoup.parse(new URL(url), 30000);
        Element element = document.getElementById("J_goodsList");


        Elements elements = element.getElementsByTag("li");

        ArrayList<Content> goodsList = new ArrayList<>();
        // System.out.println(elements.html());
        for (Element e1 : elements) {
            String img = e1.getElementsByTag("img").eq(0).attr("data-lazy-img");

            String price = e1.getElementsByClass("p-price").eq(0).text();

            String title = e1.getElementsByClass("p-name").eq(0).text();

            goodsList.add(new Content(title, img, price));

//            System.out.println("==========================");
//            System.out.println(img);
//            System.out.println(price);
//            System.out.println(title);

        }
        return goodsList;
    }
```
结果:
![image.png](https://i.loli.net/2020/12/30/U7Sf5Xr8HPQ4kYV.png)




