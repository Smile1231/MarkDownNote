# `Scrapy`爬虫实例

## 1. `Scrapy`的简单介绍

- 架构

    官方解析：`Scrapy`是一个为了爬取网站数据，提取结构性数据而编写的应用框架。可以应用在包括数据挖掘，信息处理或存储历史数据等一系列的程序中。

    其最初是为了页面抓取（更确切来说,网络抓取）所设计的，也可以应用在获取`API`所返回的数据或者通用的网络爬虫。

    ![图 5](../../images/6eb35de1a98e94a0a7461e012bb7a144926229ef31b92e55fa378e621df64558.png)  

- 架构分析：

    - `Scrapy Engine：Scrapy`引擎。负责控制数据流在系统中所有组件中流动，并在相应动作发生时触发事件。
    - `Scheduler：`调度器。从`Scrapy Engine`接受请求（`requests`）并排序列入队列，并在引擎再次请求时返回。用它来决定下一个抓取的网址是什么，同时去除重复的网址。
    - `Downloader：`下载器。抓取网页并将网页内容返还给`Spiders`。建立在`twisted`异步模型。
    - `Spiders：`爬虫。用户自定义的类，主要用来解析网页，提取`Items`，发送`url`跟进等新请求等。
    - `Item Pipelines：`管道。主要用来处理`Spider`解析出来的`Items`，进行按规则过滤，验证，持久化存储（如数据库存储）等
    - `Downloader Middlewares：`下载中间件。位于`Scrapy Engine`和`Downloader`之间，主要是处理`Scrapy`引擎与下载器之间的请求及响应。
    - `Spider Middlewares：`爬虫中间件。位于`Scrapy Engine`和`Spiders`之间，主要工作是处理`Spiders`的响应输入和请求输出。
    - `Scheduler Middlewares：`调度中间件。位于`Scrapy Engine`和`Scheduler`之间。主要工作是处理从`Scrapy Engine`发送到`Scheduler`的请求和响应。

- 数据处理流程：
    1. 引擎打开一个网站(`open a domain`)，找到处理该网站的`Spider`并向该`Spider`请求要爬取的第一个`start_urls`。
    2. 引擎从`Spider`中获取到第一个要爬取的`URL`并在调度器(`Scheduler`)以`Request`调度。
    3. 引擎向调度器请求下一个要爬取的`URL`。
    4. 调度器返回下一个要爬取的`URL`给引擎，引擎将`URL`通过`Downloader Middlewares（request）`转发给下载器`(Downloader)`。
    5. 一旦页面下载完毕，`Downloader`生成一个该页面的`Response`，并将其通过`Downloader Middlewares(response)`发送给引擎。
    6. 引擎从`Downloader`中接收到`Response`并通过`Spider Middlewares（request）`发送给`Spider`处理。
    7. `Spider`处理`Response`并返回爬取到的`Item`及(跟进的)新的`Request`给引擎。
    8. 引擎将(`Spider`返回的)爬取到的`Item`给`Item Pipeline`，将(`Spider`返回的)`Request`给调度器。
    9. 系统重复2-9的操作，直到调度中没有更多地`request`，然后断开引擎与网站之间的联系。

## 2. `Scrapy`安装

我一般都会使用`conda`来构建自己所需要的环境,所以这次也采取`Conda`来安装`Scrapy`,

```python
# 查看环境列表
conda info list 
#  使用想使用的conda环境
conda acticate myenv
# 安装scrapy包
conda install scrapy
```
## 3.实例

### 1. 创建项目
```pyhton
scrapy startproject [项目名字]
```

![图 6](../../images/4929d3bedce54be182b6096f9e38c68ad1fbdd29f3a3dbb6bd072ba646cba7f6.png)  

同时也可以看到出现了提示的两行提示
```
cd spider

scrapy genspider example example.com
```

在任意文件夹运行都可以，如果提示权限问题，可以加`sudo`运行。这个命令将会创建一个名字为`tutorial`的文件夹，文件夹结构如下：

```python
|____scrapy.cfg     # Scrapy部署时的配置文件
|____tutorial         # 项目的模块，引入的时候需要从这里引入
| |______init__.py    
| |______pycache__
| |____items.py     # Items的定义，定义爬取的数据结构
| |____middlewares.py   # Middlewares的定义，定义爬取时的中间件
| |____pipelines.py       # Pipelines的定义，定义数据管道
| |____settings.py       # 配置文件
| |____spiders         # 放置Spiders的文件夹
| | |______init__.py
| | |______pycache__
```


我们进入到项目中
```shell
cd .\spider\
````

### 2. 创建`Spider`
`Spider`是由你来定义的`Class`，`Scrapy`用它来从网页里抓取内容，并将抓取的结果解析。不过这个`Class`必须要继承`Scrapy`提供的`Spider`类`scrapy.Spider`，并且你还要定义`Spider`的名称和起始请求以及怎样处理爬取后的结果的方法。

创建一个`Spider`也可以用命令生成，比如要生成`Quotes`这个`Spider`，可以执行命令。

根据上面的语法提示,生成

```python
cd .\spider\
scrapy genspider quotes quotes.toscrape.com
```
![图 7](../../images/b5d3be8bfa9b99e6d8c4a0bf649a968e40ab98f3462d047c28168b33636d4188.png)  

执行`genspider`这个命令，第一个参数是`Spider`的名称，第二个参数是网站域名。执行完毕之后，你会发现在`spiders`文件夹中多了一个`quotes.py`，这就是你刚刚创建的`Spider`，内容如下：
```python
# -*- coding: utf-8 -*-
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"
    allowed_domains = ["quotes.toscrape.com"]
    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        pass
```

可以看到有三个属性，`name`，`allowed_domains`，`start_urls`，另外还有一个方法`parse`

- `name`，每个项目里名字是唯一的，用来区分不同的`Spider`。
- `allowed_domains`允许爬取的域名，如果初始或后续的请求链接不是这个域名下的，就会被过滤掉。
- `start_urls`，包含了`Spider`在启动时爬取的`url`列表，初始请求是由它来定义的。
- `parse`，是`Spider`的一个方法，默认情况下，被调用时`start_urls`里面的链接构成的请求完成下载后，返回的`response`就会作为唯一的参数传递给这个函数，该方法负责解析返回的`response`，提取数据或者进一步生成要处理的请求。

### 3. 创建`Item`

`Item`是保存爬取数据的容器，它的使用方法和字典类似，虽然你可以用字典来表示，不过`Item`相比字典多了额外的保护机制，可以避免拼写错误或者为定义字段错误。

创建`Item`需要继承`scrapy.Item`类，并且定义类型为`scrapy.Field`的类属性来定义一个`Item`。观察目标网站，我们可以获取到到内容有`text, author, tags`

所以可以定义如下的`Item`，修改`items.py`如下：
```python
import scrapy

class QuoteItem(scrapy.Item):

    text = scrapy.Field()
    author = scrapy.Field()
    tags = scrapy.Field()
```
定义了三个`Field`，接下来爬取时我们会使用它。

### 4. 解析`Response`

在上文中说明了`parse`方法的参数`resposne`是`start_urls`里面的链接爬取后的结果。所以在`parse`方法中，我们可以直接对`response`包含的内容进行解析，比如看看请求结果的网页源代码，或者进一步分析源代码里面包含什么，或者找出结果中的链接进一步得到下一个请求。

观察网站，我们可以看到网页中既有我们想要的结果，又有下一页的链接，所以两部分我们都要进行处理。

首先看一下网页结构，每一页都有多个`class`为`quote`的区块，每个区块内都包含`text，author，tags，`所以第一部需要找出所有的`quote`，然后对每一个`quote`进一步提取其中的内容。

![图 8](../../images/be34faff4f37aab245f4f50c008f30e7dd644c7825cbc0bcd4297db90b275927.png)  


提取的方式可以选用`CSS`选择器或`XPath`选择器，在这里我们使用`CSS`选择器进行选择，`parse`方法改写如下：

```python
def parse(self, response):
    quotes = response.css('.quote')
    for quote in quotes:
        text = quote.css('.text::text').extract_first()
        author = quote.css('.author::text').extract_first()
        tags = quote.css('.tags .tag::text').extract()
```

在这里使用了`CSS`选择器的语法，首先利用选择器选取所有的`quote`赋值为`quotes`变量。
然后利用`for`循环对每个`quote`遍历，解析每个`quote`的内容。

对`text`来说，观察到它的`class`为`text`，所以可以用`.text`来选取，这个结果实际上是整个带有标签的元素，要获取它的内容，可以加`::text`来得到。这时的结果是大小为`1`的数组，所以还需要用`extract_first`方法来获取第一个元素，而对于`tags`来说，由于我们要获取所有的标签，所以用`extract`方法获取即可。

以第一个quote的结果为例，各个选择方法及结果归类如下：

- 源码
    ```html
    <div class="quote" itemscope="" itemtype="http://schema.org/CreativeWork">
            <span class="text" itemprop="text">“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”</span>
            <span>by <small class="author" itemprop="author">Albert Einstein</small>
            <a href="/author/Albert-Einstein">(about)</a>
            </span>
            <div class="tags">
                Tags:
                <meta class="keywords" itemprop="keywords" content="change,deep-thoughts,thinking,world"> 
                
                <a class="tag" href="/tag/change/page/1/">change</a>
                
                <a class="tag" href="/tag/deep-thoughts/page/1/">deep-thoughts</a>
                
                <a class="tag" href="/tag/thinking/page/1/">thinking</a>
                
                <a class="tag" href="/tag/world/page/1/">world</a>
            
            </div>
        </div>
    ```
- `quote.css(.text)`
   ```html
    [<Selector xpath="descendant-or-self::*[@class and contains(concat(' ', normalize-space(@class), ' '), ' text ')]" data='<span class="text" itemprop="text">“The '>]

   ```

- `quote.css('.text::text')`
  ```html
    [<Selector xpath="descendant-or-self::*[@class and contains(concat(' ', normalize-space(@class), ' '), ' text ')]/text()" data='“The world as we have created it is a pr'>]
  ```

- `quote.css('.text').extract()`
  ```html
    ['<span class="text" itemprop="text">“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”</span>']
  ```
- `quote.css('.text::text').extract()`
  ```html
   ['“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”']
  ```
- `quote.css('.text::text').extract_first()
   ```html
    “The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”
   ```
所以，对于`text`，要获取第一个元素即可，所以使用`extract_first()`方法，对于`tags`，要获取所有元素，使用`extract()`方法。

### 5. 使用`Item`

刚才定义了`Item`，接下来就要轮到使用它了，你可以把它理解为一个字典，不过在声明的时候需要实例化。然后依次对刚才解析的结果赋值，返回即可。

接下来`QuotesSpider`改写如下：
```python
import scrapy
from spider.items import QuoteItem


class QuotesSpider(scrapy.Spider):
    name = 'quotes'
    allowed_domains = ['quotes.toscrape.com']
    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        queues = response.css('.quote')
        for quote in queues:
            item = QuoteItem()
            item['text'] = quote.css('.text::text').extract_first()
            item['author'] = quote.css('.author::text').extract_first()
            item['tags'] = quote.css('.tags .tag::text').extract()
            yield item
```

如此一来，首页的所有内容就解析出来了，并赋值成了一个个`QuoteItem`。

### 6. 后续`Request`
如上的操作实现了从初始页面抓取内容，不过下一页的内容怎样继续抓取？这就需要我们从该页面中找到信息来生成下一个请求，然后下一个请求的页面里找到信息再构造下一个请求，这样循环往复迭代，从而实现整站的爬取。

![图 9](../../images/d7cf2a6b6443c3ad2f0295f1c3ef21d67d90575dd63b9021edc91f20a83dee53.png)  


观察到刚才的页面拉到最下方，有一个Next按钮，查看一下源代码，可以发现它的链接是`/page/2/`，实际上全链接就是`http://quotes.toscrape.com/page/2`，通过这个链接我们就可以构造下一个请求。

构造请求时需要用到`scrapy.Request`，在这里我们传递两个参数，`url`和`callback`。

- `url，`请求链接
- `callback，`回调函数，当这个请求完成之后，获取到`response，`会将`response`作为参数传递给这个回调函数，回调函数进行解析或生成下一个请求，如上文的`parse`方法

在这里，由于`parse`就是用来解析`text，author，tags`的方法，而下一页的结构和刚才已经解析的页面结构是一样的，所以我们还可以再次使用`parse`方法来做页面解析。

好，接下来我们要做的就是利用选择器得到下一页链接并生成请求，在`parse`方法后追加下面的代码。
```python
next = response.css('.pager .next a::attr("href")').extract_first()
url = response.urljoin(next)
yield scrapy.Request(url=url , callback=self.parse)
```
第一句代码是通过`CSS`选择器获取下一个页面的链接，需要获取`<a>`超链接中的`href`属性，在这里用到了`::attr(href)`操作，通过`::attr`加属性名称我们可以获取属性的值。然后再调用`extract_first`方法获取内容。

第二句是调用了`urljoin`方法，它可以将相对`url`构造成一个绝对的`url`，例如获取到的下一页的地址是`/page/2`，通过`urljoin`方法处理后得到的结果就是`http://quotes.toscrape.com/page/2/`

第三句是通过`url`和`callback`构造了一个新的请求，回调函数`callback`依然使用的`parse`方法。这样在完成这个请求后，`response`会重新经过`parse`方法处理，处理之后，得到第二页的解析结果，然后生成第二页的下一页，也就是第三页的请求。这样就进入了一个循环，直到最后一页。

通过几行代码，我们就轻松地实现了一个抓取循环，将每个页面的结果抓取下来了。

现在改写之后整个`Spider`类是这样的：

```python
import scrapy
from tutorial.items import QuoteItem

class QuotesSpider(scrapy.Spider):
    name = "quotes"
    allowed_domains = ["quotes.toscrape.com"]
    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        quotes = response.css('.quote')
        for quote in quotes:
            item = QuoteItem()
            item['text'] = quote.css('.text::text').extract_first()
            item['author'] = quote.css('.author::text').extract_first()
            item['tags'] = quote.css('.tags .tag::text').extract()
            yield item

        next = response.css('.pager .next a::attr("href")').extract_first()
        url = response.urljoin(next)
        yield scrapy.Request(url=url, callback=self.parse)
```

接下来让我们试着运行一下看看结果，进入目录，运行如下命令：

```python
scrapy crawl quotes
```

此时新版本的`scrapy`可能会报错

![图 13](../../images/faa3184b9bdb02c48675651c71a458beb017a702e597b625e3ed4cfc463c1ea5.png)  

则需要在环境安装提示的包即可

```python
conda install protego
```


然后在运行就可以看到`Scrapy`的运行结果了。

```python
2021-10-21 16:33:56 [scrapy.utils.log] INFO: Scrapy 2.4.1 started (bot: spider)
2021-10-21 16:33:56 [scrapy.utils.log] INFO: Versions: lxml 4.6.3.0, libxml2 2.9.12, cssselect 1.1.0, parsel 1.5.2, w3lib 1.21.0, Twisted 21.2.0, Python 3.7.11 (default, Jul 27 2021, 09:42:29) [MSC v.1916 64 bit (AMD64)], pyOpenSSL 20.0.1 (OpenSSL 1.1.1l  24 Aug 202
1), cryptography 3.4.8, Platform Windows-10-10.0.18362-SP0
2021-10-21 16:33:56 [scrapy.utils.log] DEBUG: Using reactor: twisted.internet.selectreactor.SelectReactor
2021-10-21 16:33:56 [scrapy.crawler] INFO: Overridden settings:
{'BOT_NAME': 'spider',
 'NEWSPIDER_MODULE': 'spider.spiders',
 'ROBOTSTXT_OBEY': True,
 'SPIDER_MODULES': ['spider.spiders']}
2021-10-21 16:33:56 [scrapy.extensions.telnet] INFO: Telnet Password: 746d72fb7492469c
2021-10-21 16:33:57 [scrapy.middleware] INFO: Enabled extensions:
['scrapy.extensions.corestats.CoreStats',
 'scrapy.extensions.telnet.TelnetConsole',
 'scrapy.extensions.logstats.LogStats']
2021-10-21 16:33:57 [scrapy.middleware] INFO: Enabled downloader middlewares:
['scrapy.downloadermiddlewares.robotstxt.RobotsTxtMiddleware',
 'scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware',
 'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware',
 'scrapy.downloadermiddlewares.defaultheaders.DefaultHeadersMiddleware',
 'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware',
 'scrapy.downloadermiddlewares.retry.RetryMiddleware',
 'scrapy.downloadermiddlewares.redirect.MetaRefreshMiddleware',
 'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware',
 'scrapy.downloadermiddlewares.redirect.RedirectMiddleware',
 'scrapy.downloadermiddlewares.cookies.CookiesMiddleware',
 'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware',
 'scrapy.downloadermiddlewares.stats.DownloaderStats']
2021-10-21 16:33:57 [scrapy.middleware] INFO: Enabled spider middlewares:
['scrapy.spidermiddlewares.httperror.HttpErrorMiddleware',
 'scrapy.spidermiddlewares.offsite.OffsiteMiddleware',
 'scrapy.spidermiddlewares.referer.RefererMiddleware',
 'scrapy.spidermiddlewares.urllength.UrlLengthMiddleware',
 'scrapy.spidermiddlewares.depth.DepthMiddleware']
2021-10-21 16:33:57 [scrapy.middleware] INFO: Enabled item pipelines:
[]
2021-10-21 16:33:57 [scrapy.core.engine] INFO: Spider opened
2021-10-21 16:33:57 [scrapy.extensions.logstats] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)
2021-10-21 16:33:57 [scrapy.extensions.telnet] INFO: Telnet console listening on 127.0.0.1:6023
2021-10-21 16:33:58 [scrapy.core.engine] DEBUG: Crawled (404) <GET http://quotes.toscrape.com/robots.txt> (referer: None)
2021-10-21 16:33:58 [scrapy.core.engine] DEBUG: Crawled (200) <GET http://quotes.toscrape.com/> (referer: None)
2021-10-21 16:33:58 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'author': 'Albert Einstein',
 'tags': ['change', 'deep-thoughts', 'thinking', 'world'],
 'text': '“The world as we have created it is a process of our thinking. It '
         'cannot be changed without changing our thinking.”'}
2021-10-21 16:33:58 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'author': 'J.K. Rowling',
 'tags': ['abilities', 'choices'],
 'text': '“It is our choices, Harry, that show what we truly are, far more '


...

2021-10-21 16:34:01 [scrapy.dupefilters] DEBUG: Filtered duplicate request: <GET http://quotes.toscrape.com/page/10/> - no more duplicates will be shown (see DUPEFILTER_DEBUG to show all duplicates)
2021-10-21 16:34:01 [scrapy.core.engine] INFO: Closing spider (finished)
2021-10-21 16:34:01 [scrapy.statscollectors] INFO: Dumping Scrapy stats:
{'downloader/request_bytes': 2881,
 'downloader/request_count': 11,
 'downloader/request_method_count/GET': 11,
 'downloader/response_bytes': 23440,
 'downloader/response_count': 11,
 'downloader/response_status_count/200': 10,
 'downloader/response_status_count/404': 1,
 'dupefilter/filtered': 1,
 'elapsed_time_seconds': 3.804354,
 'finish_reason': 'finished',
 'finish_time': datetime.datetime(2021, 10, 21, 8, 34, 1, 614908),
 'item_scraped_count': 100,
 'log_count/DEBUG': 112,
 'log_count/INFO': 10,
 'request_depth_max': 10,
 'response_received_count': 11,
 'robotstxt/request_count': 1,
 'robotstxt/response_count': 1,
 'robotstxt/response_status_count/404': 1,
 'scheduler/dequeued': 10,
 'scheduler/dequeued/memory': 10,
 'scheduler/enqueued': 10,
 'scheduler/enqueued/memory': 10,
 'start_time': datetime.datetime(2021, 10, 21, 8, 33, 57, 810554)}
2021-10-21 16:34:01 [scrapy.core.engine] INFO: Spider closed (finished)
```

在这里贴出部分运行结果，中间的一些抓取结果输出已省略。

首先`Scrapy`输出了当前的版本号，启动的项目。其次输出了当前在`settings.py`中的一些重写后的配置。然后输出了当前所应用的`middlewares`和`pipelines，middlewares`是默认启用的，如果要修改，我们可以在`settings.py`中修改，`pipelines`默认是空，同样也可以在`settings.py`中配置，后面会进行讲解。

再接下来就是输出各个页面的抓取结果了，可以看到它一边解析，一边翻页，直至将所有内容抓取完毕，然后终止。

在最后`Scrapy`输出了整个抓取过程的统计信息，如请求的字节数，请求次数，响应次数，完成原因等等。

这样整个`Scrapy`程序就成功运行完毕了。

可以发现我们通过非常简单的一些代码就完成了一个网站内容的爬取，相比之前自己一点点写程序是不是简洁太多了？

### 7. 保存到文件

刚才运行完`Scrapy`后，我们只在控制台看到了输出结果，如果想将结果保存该怎么办呢？

比如最简单的形式，将结果保存成`Json`文件。

要完成这个其实不需要你写任何额外的代码，`Scrapy`提供了`Feed Exports`可以轻松地将抓取结果输出，例如我们想将上面的结果保存成`Json`文件，可以执行如下命令：
```python
scrapy crawl quotes -o quotes.json
```

运行后发现项目内就会多了一个`quotes.json`文件，里面包含的就是刚才抓取的所有内容，是一个`Json`格式，多个项目由中括号包围，是一个合法的`Json`格式。

![图 10](../../images/4a76090a2afa7d8975a81f593258aa637037eba6cf5f34840342094e493a6a39.png)  


另外你还可以每一个`Item`一个`Json`，最后的结果没有中括号包围，一行对应一个`Item`，命令如下：

```python
scrapy crawl quotes -o quotes.jl
```
或
```python
scrapy crawl quotes -o quotes.jsonlines
```

![图 11](../../images/008dc7d76e2756f61e39a511d42005338fed0bea4f7cf04ab42fb9a32f371eee.png)  

![图 12](../../images/d8243aa2d461270b87964cfce16ab51f01ee3458c6c0358ecb6ba5a5b3dcf2b3.png)  

另外还支持很多格式输出，例如`csv，xml，pickle，marshal`等等，还支持`ftp，s3`等远程输出，另外还可以通过自定义`ItemExporter`来实现其他的输出。

例如如下命令分别对应输出为`csv，xml，pickle，marshal，`格式以及`ftp`远程输出：

```python
scrapy crawl quotes -o quotes.csv
scrapy crawl quotes -o quotes.xml
scrapy crawl quotes -o quotes.pickle
scrapy crawl quotes -o quotes.marshal
scrapy crawl quotes -o ftp://user:pass@ftp.example.com/path/to/quotes.csv
```
其中`ftp`输出需要你正确配置好你的用户名，密码，地址，输出路径，否则会报错。

通过`Scrapy`提供的`Feed Exports`我们可以轻松地输出抓取结果到文件，对于一些小型项目这应该是足够了，不过如果想要更复杂的输出，如输出到数据库等等，你可以使用`Item Pileline`更方便地实现。

### 8. 使用`Item Pipeline`

至此，你已经可以成功地完成抓取并将结果保存了，如果你想进行更复杂的操作，如将结果保存到数据库，如`MongoDB`，或者筛选某些有用的`Item`，可以定义`Item Pileline`来实现。

`Item Pipeline`意为项目管道，当生成`Item`后，它会自动被送到`Item Pipeline`进行处理，我们常用它来做如下操作：

- 清理`HTML`数据
- 验证爬取数据，检查爬取字段
- 查重并丢弃重复内容
- 将爬取结果储存到数据库


要实现一个`Item Pipeline`很简单，只需要定义一个类并实现`process_item`方法即可，启用后，`Item Pipeline`会自动调用这个方法，这个方法必须返回包含数据的字典或是`Item`对象，或者抛出`DropItem`异常。

这个方法由两个参数，一个是`item`，每次`Spider`生成的`Item`都会作为参数传递过来，另一个是`spider`，就是`Spider`的实例。

好，接下来我们实现一个`Item Pipeline`，筛掉`text`长度大于`50`的`Item`并将结果保存到`MongoDB`。

修改项目里的`pipelines.py`文件，之前自动生成的可以删掉，增加一个`TextPipeline`类，内容如下：
```python
from scrapy.exceptions import DropItem

class TextPipeline(object):
    def __init__(self):
        self.limit = 50

    def process_item(self, item, spider):
        if item['text']:
            if len(item['text']) > self.limit:
                item['text'] = item['text'][0:self.limit].rstrip() + '...'
            return item
        else:
            return DropItem('Missing Text')
```
在构造方法里面定义了限制长度，长度限制为`50`，然后实现了`process_item`方法，参数是`item`和`spider`，首先判断`item`的`text`属性是否存在，如果不存在，那就跑出`DropItem`异常，如果存在，再判断长度是否大于`50`，如果大于，那就截断然后拼接省略号，再将`item`返回即可。

接下来，我们再将处理后的`item`存入`MongoDB`，如果你还没有安装，请先安装好`MongoDB`。

另外还需要安装一个`MongoDB`开发包`pymongo`，利用`pip`安装即可：(`pip3 install pymongo
`)
```
conda install pymongo
```

接下来定义另外一个`Pipeline`，同样在`pipelines.py`中，实现另一个类`MongoPipeline`，内容如下：
```python
import pymongo

class MongoPipeline(object):
    def __init__(self, mongo_uri, mongo_db):
        self.mongo_uri = mongo_uri
        self.mongo_db = mongo_db

    @classmethod
    def from_crawler(cls, crawler):
        return cls(
            mongo_uri=crawler.settings.get('MONGO_URI'),
            mongo_db=crawler.settings.get('MONGO_DB')
        )

    def open_spider(self, spider):
        self.client = pymongo.MongoClient(self.mongo_uri)
        self.db = self.client[self.mongo_db]

    def process_item(self, item, spider):
        name = item.__class__.__name__
        self.db[name].insert(dict(item))
        return item

    def close_spider(self, spider):
        self.client.close()
```
在这个类中，实现了`API`定义的另外几个方法。

- `from_crawler`，这是一个类方法，用`@classmethod`标识，是一种依赖注入的方式，方法的参数就是`crawler`，通过`crawler`这个我们可以拿到全局配置的每个配置信息，在全局配置`settings.py`中我们可以定义`MONGO_URI`和`MONGO_DB`来指定`MongoDB`连接需要的地址和数据库名称，拿到配置信息之后返回类对象即可。所以这个方法的定义主要是用来获取`settings.py`中的配置的。
- `open_spider`，当`spider`被开启时，这个方法被调用。在这里主要进行了一些初始化操作。
- `close_spider`，当`spider`被关闭时，这个方法会调用，在这里将数据库连接关闭。


那么最主要的`process_item`方法则执行了数据插入操作。

好，定义好这两个类后，我们需要在`settings.py`中使用他们，还需要定义`MongoDB`的连接信息。

在`settings.py`中加入如下内容：

```python
ITEM_PIPELINES = {
   'tutorial.pipelines.TextPipeline': 300,
   'tutorial.pipelines.MongoPipeline': 400,
}
MONGO_URI='localhost'
MONGO_DB='tutorial'
```

赋值`ITEM_PIPELINES`字典，键名是`Pipeline`的类名称，键值是调用优先级，数字越小越先被调用。

定义好了之后，再重新执行爬取，命令如下：
```python
scrapy crawl quotes
```
爬取结束后，可以观察到`MongoDB`中创建了一个`tutorial`的数据库，`QuoteItem`的表。

到现在，我们就通过抓取`quotes`完成了整个`Scrapy`的简单入门，但这只是冰山一角，还有很多内容等待我们去探索，后面会进行讲解。

