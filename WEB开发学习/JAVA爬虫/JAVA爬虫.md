# JAVA爬虫



## 一、准备



## 二、使用

### 1.一个简单的例子

```java
public class HttpClientUtil {
    public static void main(String[] args) throws IOException {
       //获取请求 https://search.jd.com/Search?keyword=java

        String url = "https://www.jianshu.com/p/8ff4652fabd0";
        //解析网页(Jsoup返回Document对象就是浏览器的Document对象)
        Document document = Jsoup.parse(new URL(url),30000);
        //所有在js中的方法都可以在这里使用
        Element element = document.getElementById("__next");

        Elements elements = element.getElementsByTag("span");
        
        System.out.print(elements.html());


    }
}
```



### 2.HttpClient进行网页爬取

#### 依赖配置

- HTTPClient依赖

**首先在maven仓库找到依赖**

![image-20220224170526309](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220224170526309.png)

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>

```



- Log4j依赖

**引入日志依赖**

![image-20220224170731788](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220224170731788.png)

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.25</version>
    <scope>test</scope>
</dependency>
```

**利用log4j.properties文件进行配置**

```properties
log4j.rootLogger=DEBUG,A1
log4j.logger.cn.insectTest = DEBUG

log4j.appender.A1 = org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout = org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern = %.d{yyyy-MM-dd HH:mm:ss,SSS} [%t] [%c]-[%p] %m%n 
```



#### HttpGet爬取资源

```java
//HttpClient进行爬取
        //创建HttpClient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();

        //创建HttpGet对象，设置url访问地址
        HttpGet httpGet = new HttpGet("http://www.itcast.cn");

		//创建一个HttpResponse对象，用于接收请求结果
        CloseableHttpResponse response = null;

        try {
            response = httpClient.execute(httpGet);
            if(response.getStatusLine().getStatusCode() == 200){
                String content = EntityUtils.toString(response.getEntity(),"utf8");
                System.out.println(content.length());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                //关闭资源
                httpClient.close();
                response.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
    }
```

![日志打印信息](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220224180032236.png)

![image-20220224180100337](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220224180100337.png)



##### 带参数的get请求

> 本质上还是没变化，只不过url通过`URIBuilder`进行创建，可以将参数进行拼接
>
> ```java
> //设置请求地址是：http://yun.itheima.com/search?keys=Java
>         //创建URIBuilder，可以将URL和蚕食拼接
>         URIBuilder uriBuilder = new URIBuilder("http://yun.itheima.com/search");
>         //设置参数
>         uriBuilder.setParameter("keys","Java");
> 
>         //创建HttpGet对象，设置url访问地址
>         HttpGet httpGet = new HttpGet(uriBuilder.build());
> ```
>
> 因为setParameter返回的是URIBuilder对象，所以如果设置多个参数，可以继续链式调用



#### HttpPost请求

使用方法只需要将HttpGet替换成HttpPost即可



##### 带参数的Post请求

> ```java
> //创建HttpPost对象
> HttpPost httpPost = new HttpPost("http://yun.itheima.com/search");
> 
> //声明List集合，封装表单中的参数
> List<NameValuePair> params = new ArrayList<NameValuePair>();
> 
> //设置参数
> params.add(new BasicNameValuePair("key","Java"));
> 
> //将参数转换为表单对象，并设置编码
> UrlEncodedFormEntity formEntity = new UrlEncodedFormEntity(params,"utf8"); 
> 
> //将表单数据放进HttpPost里面
> httpPost.setEntity(formEntity);
> ```



#### 连接池

如果每次进行网络请求都创建一个HttpClient，那么必定消耗的资源是非常多的，因此我们可以通建立连接池(数据库的连接也是此原理)来创建Http连接，这样我们每次从连接池里面去获取连接

```java
public static void main(String[] args) {

    //创建连接池管理器
    PoolingHttpClientConnectionManager cm = new PoolingHttpClientConnectionManager();

    //设置能够发出的最大连接数
    cm.setMaxTotal(100);

    //设置每个目的主机能够接收到的最大连接数
    cm.setDefaultMaxPerRoute(10);

    //使用连接池管理发起请求
    doGet(cm);
    doGet(cm);
}

private static void doGet(PoolingHttpClientConnectionManager cm) {
    //不是每次创建新的HttpClient，而是从连接池中获取HttpClient对象
    CloseableHttpClient httpClient = HttpClients.custom().setConnectionManager(cm).build();

    HttpGet httpGet = new HttpGet("http://www.itcast.cn");

    CloseableHttpResponse httpResponse = null;

    try {
        httpResponse = httpClient.execute(httpGet);

        if(httpResponse.getStatusLine().getStatusCode() == 200){
            String content = EntityUtils.toString(httpResponse.getEntity(),"utf8");

        }
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        try {
            httpResponse.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        //此时不再需要关闭httpClient,因为以及交给连接池进行管理
        //httpClient.close();
    }
```



#### 设置请求信息

设置请求超时时间等等

```java
RequestConfig requestConfig = RequestConfig.custom()
    .setConnectTimeout(1000)//设置连接的最长时间
    .setConnectionRequestTimeout(500)//设置获取连接的最长时间
    .setSocketTimeout(10*1000)//设置数据传输的最长时间
    .build();

httpGet.setConfig(requestConfig);
```



### 3.Jsoup

抓取到网页之后，需要**对页面进行解析**，可以使用字符串处理工具解析页面，也可以使用正则表达式，但是这些方法都会带来很大开发成本。



Jsoup是一款Java的HTML解析器，可以直接解析某个URL地址、HTML文本内容，提供了一套非常简单API，可通过DOM，CSS以及类似jQuery的操作方法对HTML数据进行操作



#### 依赖

```xml
<dependency>
	<groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.10.2</version>
</dependency>
```

> 如果想要爬取视频、音乐等元素需要使用
>
> **tika**



> Commons-io 一个可以对文件进行操作的工具包
>
> ```xml
> <dependency>
>     <groupId>commons-io</groupId>
>     <artifactId>commons-io</artifactId>
>     <version>2.11.0</version>
> </dependency>
> ```



> commons-lang3
>
> 包含一些常用的工具：StringUtils
>
> ```xml
> <dependency>
>         <groupId>org.apache.commons</groupId>
>         <artifactId>commons-lang3</artifactId>
>         <version>3.9</version>
> </dependency>
> ```
>
> 



#### 使用

##### 解析URL

> ```java
> //解析url地址，第一个参数是访问url，第二个参数是访问时的超时时间
> Document document = Jsoup.parse(new URL("http://www.itcast.cn"),1000);
> 
> //使用标签选择器，获取title的内容
> String title = document.getElementsByTag("title").first().text();
> 
> //打印
> System.out.println(title);
> ```
>
> ![image-20220225142338690](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220225142338690.png)
>
> ![image-20220225174916374](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220225174916374.png)



##### 解析字符串

> Jsoup同样可以解析字符串，通过读取html文件方式
>
> ```java
> //使用工具类读取文件，获取字符串
> String content = FileUtils.readFileToString(new File("C:\\User\\test.html"),"utf8");
> 
> //解析字符串
> Document document = Jsoup.parse(content);
> ```
>
> ```java
> //如果是Response就需要使用EntityUtils转换成String
> String content = Entityutils.toString(response.getEntity(),"utf8");
> ```
>
> 



##### 解析文件

> Jsoup也可以直接读取文件
>
> ```java
> Document document = Jsoup.parse(new File("C:\\user\\test.html"),"utf8");
> ```



##### 获取DOM元素、元素数据



###### 使用DOM

> 方法和JavaScript中操作dom的方法一致，不再赘述

###### 使用Selector选择器

> - 通过标签查找
>
>   ```java
>   Document doc = Jsoup.parse(new File(""),"utf8");
>   
>   Elements eles = doc.select("span");
>   
>   for(Element element : eles){
>       //code
>   }
>   ```
>
> - 通过ID查找
>
>   ```java
>   Document doc = Jsoup.parse(new File(""),"utf8");
>   
>   //记得加上#
>   Elements eles = doc.select("#city_bj");
>   ```
>
> - 通过class查找
>
>   ```java
>   Document doc = Jsoup.parse(new File(""),"utf8");
>   
>   //记得加上.
>   Elements eles = doc.select(".city_bj");
>   ```
>
> - 通过属性查找
>
>   ```java
>   Document doc = Jsoup.parse(new File(""),"utf8");
>   
>   //记得加上[]
>   Elements eles = doc.select("[name]");
>   ```
>
> - 通过属性—属性值查找
>
>   ```java
>   Document doc = Jsoup.parse(new File(""),"utf8");
>   
>   //记得加上[]
>   Elements eles = doc.select("[class=a1]");
>   ```



###### 使用组合选择器

> - 元素+ID
>
>   ```java
>   //返回的结果可能多个
>   Elements eles = doc.select("h3#cith_bj");
>   ```
>
> - 元素+class
>
>   ```java
>   Elements eles = doc.select("h3.cith_bj");
>   ```
>
>   
>
> - 元素+属性名
>
>   ```java
>   Elements eles = doc.select("span[abc]");
>   ```
>
>   > 任意组合
>   >
>   > ```java
>   > //找到具有abc属性且class为"s_name"的span标签
>   > Elements eles = doc.select("span[abc].s_name");
>   > ```
>
> - ancestor child：查找某个元素下子元素
>
>   ```java
>   //找到class为.city_con下所以li标签
>   Elements eles = doc.select(".city_com li");
>   ```
>
>   
>
> - parent > child：查找某个父元素下直接子元素
>
>   ```java
>   Elements eles = doc.select(".city_com > ul > li");
>   ```
>
>   
>
> - parent > * :查找某个父元素下所以直接子元素





##### 案例



### 4.WebMagic

该框架底层使用的是HttpClient和Jsoup，让我们能够更方便开发爬虫。

WebMagic项目代码分为核心和拓展两部分。核心部分(WebMagic-core)是一个精简的、模块化的爬虫实现，而扩展部分则包括一些便利的、实用性的功能。



#### 构成

WebMagic的结构分为Downloader、PageProcessor、Scheduler、Pipeline四大组件，并由Spider将它们彼此组织起来。四大组件对应爬虫生命周期中的下载、处理、管理和持久化等功能。

> 1. Downloader：Downloader负责从互联网上下载页面，以便后续处理。WebMagic默认使用了Apache HttpClient作为下载工具。
> 2. PageProcessor：PageProcessor负责解析页面，抽取有用信息，以及发现新的链接。WebMagic使用Jsoup作为HTML解析工具，并基于其开发了解析XPath的工具Xsoup。**在这四个组件中，`PageProcessor`对于每个站点每个页面都不一样，是需要使用者定制的部分。**
> 3. Scheduler：Scheduler负责管理待抓取的URL，以及一些去重的工作。WebMagic默认提供了JDK的内存队列来管理URL，并用集合来进行去重。也支持使用Redis进行分布式管理。除非项目有一些特殊的分布式需求，否则无需自己定制Scheduler。
> 4. Pipeline：Pipeline负责抽取结果的处理，包括计算、持久化到文件、数据库等。WebMagic默认提供了“输出到控制台”和“保存到文件”两种结果处理方案。`Pipeline`定义了结果保存的方式，**如果你要保存到指定数据库，则需要编写对应的Pipeline**。对于一类需求一般只需编写一个`Pipeline`。



#### 数据流转对象

> 1. Request：`Request`是对URL地址的一层封装，一个Request对应一个URL地址。它是PageProcessor与Downloader交互的载体，也是PageProcessor控制Downloader唯一方式。除了URL本身外，它还包含一个Key-Value结构的字段`extra`。你可以在extra中保存一些特殊的属性，然后在其他地方读取，以完成不同的功能。例如附加上一个页面的一些信息等。
> 2. Page：`Page`代表了从Downloader下载到的一个页面——可能是HTML，也可能是JSON或者其他文本格式的内容。Page是WebMagic抽取过程的核心对象，它提供一些方法可供抽取、结果保存等。
> 3. ResultItems：`ResultItems`相当于一个Map，它保存PageProcessor处理的结果，供Pipeline使用。它的API与Map很类似，值得注意的是它有一个字段`skip`，若设置为true，则不应被Pipeline处理。



#### 依赖

```xml
 <!-- WebMagic-core依赖 -->
<dependency>
    <groupId>us.codecraft</groupId>
    <artifactId>webmagic-core</artifactId>
    <version>0.7.4</version>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>


<!-- WebMagic-extension依赖-->
<dependency>
    <groupId>us.codecraft</groupId>
    <artifactId>webmagic-extension</artifactId>
    <version>0.7.4</version>
</dependency>
```

> webmagic依赖注入的时候自动就加入了log4j工具包，所以需要排除





#### 使用

##### 简单的案例

> ```java
> public class JobProcessor implements PageProcessor {
>     //解析页面
>     public void process(Page page) {
>         //解析返回的数据page，并且把解析的结果放到ResultItems中,resultItems是一个类似Map的数据结构
>         page.putField("div",page.getHtml().css("div#smoothmenu1 li").all());
>     }
> 
>     private Site site = Site.me();
>     public Site getSite() {
>         return site;
>     }
> 
>     //主函数执行爬虫
>     public static void main(String[] args) {
>         Spider.create(new JobProcessor())
>             .addUrl("https://www.ctbu.edu.cn/jsdh/bkxs.htm")
>             .run();
>     }
> }
> ```
>
> ![image-20220226162855462](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220226162855462.png)



##### 实现PageProcessor

> 抽取元素Selectable
>
> WebMagic主要使用三种抽取技术：
>
> - XPath
>
>   > 对于这样一个html结构：
>   >
>   > ![image-20220226163421049](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220226163421049.png)
>   >
>   > ```java
>   > page.putField("div",page.getHtml().xpath("//div[@id=news_div]/ul/li/div/a"));
>   > ```
>
> - 正则表达式
>
>   > 正则表达式可以在css或者Xpath基础上进一步筛选内容
>   >
>   > ```java
>   > page.putField("div",page.getHtml().css("div#smoothmenu1 li").regex(".*江苏.*").all());
>   > ```
>
> - CSS选择器
>
> 



**webMagic选择器方法**

> | 方法                           | 说明               | 实例           |
> | ------------------------------ | ------------------ | -------------- |
> | xpath(String xpath)            | 使用XPath选择      | html.xpath("") |
> | $(Strig selector)              | 使用CSS选择器选择  |                |
> | $(String selector,String attr) | 使用CSS选择器选择  |                |
> | css(String selector)           | 功能与$想通        |                |
> | links()                        | 选择所有连接       |                |
> | regex(String regex)            | 使用正则表达式抽取 |                |



**webMagic抽取数据方法**

| 方法       | 说明                            | 实例                                  |
| ---------- | ------------------------------- | ------------------------------------- |
| get()      | 返回一条String类型结果          | String link = html.links().get()      |
| toString() | 通get()，返回一条String类型结果 | String link = html.links().toString() |
| all()      | 返回所有抽取结果                | List links = html.links().all()       |



###### 获取连接，

并对链接目的地址网页元素进行爬取

```java
//获取连接:a标签的url中以9结尾的所以连接
page.addTargetRequests(page.getHtml().css("div#news_div").links().regex(".*9$").all());
//对连接地址进行爬取
page.putField("url",page.getHtml().css("div.mt h1").all());
```



##### 实现Pipeline保存结果

> ```java
> Spider.create(new JobProcessor())
>             .addUrl("https://www.ctbu.edu.cn/jsdh/bkxs.htm")
>     		.addPipeline(new FilePipeline("C\\Users\\..."))//输出结果保存的文件夹
>             .thread(5)//开启5个线程
>             .run();
> ```



##### Spider

> Spider是爬虫启动入口。在启动爬虫之前，我们需要使用一个PageProcessor创建一个Spider对象，然后使用run()启动
>
> 同时Spider其他最近(Download/Scheduler/Pipeline)都可以通过set方法设置
>
> | **方法**                  | **说明**                                         | **示例**                                                     |
> | ------------------------- | ------------------------------------------------ | ------------------------------------------------------------ |
> | create(PageProcessor)     | 创建Spider                                       | Spider.create(new GithubRepoProcessor())                     |
> | addUrl(String…)           | 添加初始的URL                                    | spider .addUrl("http://webmagic.io/docs/")                   |
> | thread(n)                 | 开启n个线程                                      | spider.thread(5)                                             |
> | run()                     | 启动，会阻塞当前线程执行                         | spider.run()                                                 |
> | start()/runAsync()        | 异步启动，当前线程继续执行                       | spider.start()                                               |
> | stop()                    | 停止爬虫                                         | spider.stop()                                                |
> | addPipeline(Pipeline)     | 添加一个Pipeline，一个Spider可以有多个Pipeline   | spider .addPipeline(new ConsolePipeline())                   |
> | setScheduler(Scheduler)   | 设置Scheduler，一个Spider只能有个一个Scheduler   | spider.setScheduler(new RedisScheduler())                    |
> | setDownloader(Downloader) | 设置Downloader，一个Spider只能有个一个Downloader | spider .setDownloader(new SeleniumDownloader())              |
> | get(String)               | 同步调用，并直接取得结果                         | ResultItems result = spider.get("http://webmagic.io/docs/")  |
> | getAll(String…)           | 同步调用，并直接取得一堆结果                     | List results = spider .getAll("http://webmagic.io/docs/", "http://webmagic.io/xxx") |



##### 爬虫配置Site

Site.me()可以对爬虫进行一些配置，包括编码、抓取间隔、超过时间、重复次数等

```java
private Site site = Site.me()
            .setCharset("utf8")
            .setTimeOut(10000)//超时时间
            .setRetrySleepTime(3000)//设置重试的间隔时间
            .setSleepTime(3)//设置重试次数
            ;
```

| **方法**                 | **说明**                                  | **示例**                                                     |
| ------------------------ | ----------------------------------------- | ------------------------------------------------------------ |
| setCharset(String)       | 设置编码                                  | site.setCharset("utf-8")                                     |
| setUserAgent(String)     | 设置UserAgent                             | site.setUserAgent("Spider")                                  |
| setTimeOut(int)          | 设置超时时间，单位是毫秒                  | site.setTimeOut(3000)                                        |
| setRetryTimes(int)       | 设置重试次数                              | site.setRetryTimes(3)                                        |
| setCycleRetryTimes(int)  | 设置循环重试次数                          | site.setCycleRetryTimes(3)                                   |
| addCookie(String,String) | 添加一条cookie                            | site.addCookie("dotcomt_user","code4craft")                  |
| setDomain(String)        | 设置域名，需设置域名后，addCookie才可生效 | site.setDomain("github.com")                                 |
| addHeader(String,String) | 添加一条addHeader                         | site.addHeader("Referer","[https://github.com](https://github.com/)") |
| setHttpProxy(HttpHost)   | 设置Http代理                              | site.setHttpProxy(new HttpHost("127.0.0.1",8080))            |



##### 实现Scheduler(对URL进行管理)

> Scheduler是对RUL进行管理的组件，包括两个作用：
>
> - 对待抓取的URL队列进行管理
> - 对已抓取的URL进行去重
>
> 如果只是本地规模较小的爬虫，无序定制Scheduler
>
> | 类                        | 说明                                                         | 备注                                                         |
> | ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | DuplicateRemovedScheduler | 抽象基类，提供一些模板方法                                   | 继承它可以实现自己功能                                       |
> | QueueScheduler            | 使用内存队列保存待抓取URL                                    |                                                              |
> | PriorityScheduler         | 使用带有优先级的内存队列保存待抓取URL                        | 耗费内存较QueueScheduler更大，但是设置request.priority之后，只能使用PriorityScheduler才可以使优先级生效 |
> | FileCacheQueueScheduler   | 使用文件保存抓取URL，可以在关闭程序并下次启动时，从之前抓取到的URL继续抓取 | 需要指定路径，会建立`.urls.txt`和`.cursor.txt`两个文件       |
> | RedisScheduler            | 使用Redis保存抓取队列，可进行多台机器同时合作抓取            | 需要安装并启动redis                                          |
>
> 
>
> 
>
> **去重**
>
> 去重部分被单独抽象成一个接口：DuplicateRemover，从而可以为同一个Scheduler选择不同的去重方式，适应不同需求，目前提供两种去重方式
>
> | 类                            | 说明                                                    |
> | ----------------------------- | ------------------------------------------------------- |
> | HashSetDuplicateRemover(默认) | 使用HashSet来进行去重，占用内存大                       |
> | BloomFilterDuplicateRemover   | 使用BloomFilter来进行去重，占用内存小，但是可能漏抓页面 |
>
> > 如果要是有BloonFilter需要添加依赖
> >
> > ```xml
> > <dependency>
> > 	<groupId>com.google.guava</groupId>
> >     	<artifactId>guava</artifactId>
> >     	<version>16.0</version>
> > </dependency>
> > ```
> >
> > ```java
> > Spider.create(new JobProcessor())
> >             .setScheduler(new QueueScheduler().setDuplicateRemover(new BloomFilterDuplicateRemover(10000000))) //参数设置需要对多少条数据去重
> >             .run();
> > ```





#### 定时任务

> 在开发中可以使用Spring内置的Spring Task，这是Spring3.0加入的定时任务功能。我们使用注解的方式定时启动爬虫进行数据爬取。
>
> 我们使用的是@Scheduled注解，其属性如下：
>
> 1）<font color="red">**cron:**</font> cron表达式，指定任务在特定时间执行；
>
> 2）<font color="red">**fixedDelay:**</font> 上一次任务执行完后多久再执行，参数类型为long，单位ms
>
> 3）**fixedDelayString:** 与fixedDelay含义一样，只是参数类型变为String
>
> 4）**fixedRate:** 按一定的频率执行任务，参数类型为long，单位ms
>
> 5）**fixedRateString:** 与fixedRate的含义一样，只是将参数类型变为String
>
> 6）**initialDelay:** 延迟多久再第一次执行任务，参数类型为long，单位ms
>
> 7）**initialDelayString:** 与initialDelay的含义一样，只是将参数类型变为String
>
> 8）**zone:** 时区，默认为当前时区，一般没有用到
>
> 
>
> 如果是比较复杂的业务：例如在工作日的晚上八点执行，就需要Cron表达式



##### Cron表达式

> cron的表达式是字符串，实际上是由七子表达式，描述个别细节的时间表。这些子表达式是分开的空白，代表
>
> ```
> 1.  Seconds
> 2.  Minutes
> 3.  Hours
> 4.  Day-of-Month
> 5.  Month
> 6.  Day-of-Week
> 7.  Year (可选字段)
> 
> 
> 例 "0 0 12 ? * WED" 在每星期三下午12:00 执行,
> ```
>
> > 每一个字段都有一套可以指定有效值，如
> >
> > Seconds (秒) ：可以用数字0－59 表示，
> >
> > Minutes(分) ：可以用数字0－59 表示，
> >
> > Hours(时) ：可以用数字0-23表示,
> >
> > Day-of-Month(天) ：可以用数字1-31 中的任一一个值，但要注意一些特别的月份
> >
> > Month(月) ：可以用0-11 或用字符串:
> >
> > JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC
> >
> > Day-of-Week(天) ：可以用数字1-7表示（1 ＝ 星期日）或用字符口串:
> >
> > SUN, MON, TUE, WED, THU, FRI, SAT
> >
> > “/”：为特别单位，表示为“每”如“0/15”表示每隔15分钟执行一次,“0”表示为从“0”分开始, “3/20”表示表示每隔20分钟执行一次，“3”表示从第3分钟开始执行
> >
> > “?”：表示每月的某一天，或第周的某一天
> >
> > “L”：用于每月，或每周，表示为每月的最后一天，或每个月的最后星期几如“6L”表示“每月的最后一个星期五”
>
> 可以使用CronExpress.jar生成Cron表达式



##### 定时任务使用

> 首先要引入Spring的依赖，然后注解配置
>
> ```java
> @SpringBootApplication
> // 使用定时任务, 需要先开启定时任务, 需要添加注解
> @EnableScheduling
> public class Application {
> 
>     public static void main(String[] args) {
>         SpringApplication.run(Application.class, args);
>     }
> }
> ```
>
> ```java
> //测试
> public class TaskTest {
> 
>     @Scheduled(cron = "0/5 * * * * *")
>     public void test() {
>         System.out.println("定时任务执行了");
>     }
> }
> ```



#### 内容去重

> - BloomFilter
>
>   > 如果两个内容只有几个标点符号不同，那就不会被认为重复
>
> - KMP算法
>
>   > 字符串匹配算法，时空复杂度太高，不适合大量数据重复对比
>
> - simgash算法
>
>   > 通过算法产生签名，然后进行对比去重
>   >
>   > 导入途径参考:
>   >
>   > > https://www.ayulong.cn/blog/19
>   > >
>   > > 在我们的项目中引入依赖引入依赖
>   > >
>   > > ![image-20220226195610767](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220226195610767.png)
>   > >
>   > > ```xml
>   > > <groupId>com.lou</groupId>
>   > > <artifactId>simhasher</artifactId>
>   > > <version>0.0.1-SNAPSHOT</version>
>   > > ```
>   > >
>   > > 然后在我们项目中就可以使用了
>   > >
>   > > ```java
>   > > SimHasher simHasher;
>   > > ```
>   >
>   > 
>
>   



#### 代理：

**1)爬虫存在问题**

有些网站为了预防爬虫，通过ip+时间进行鉴别，因为正常人不能短时间开启太多页面，发起太多请求。

为了防止我们ip被禁，我们需要代理服务器来爬取数据



**2)什么是代理服务器**

代理(proxy)，是一种特殊的网络服务，允许一个网络终端通过这个服务与另一个网络终端进行非直接连接。

> 一个完整的代理请求过程为：
>
> - 客户端先与代理服务器创建连接，
> - 接着根据代理服务器所使用的代理协议，对目标服务器创建连接
> - 获得目标服务器指定资源





**3)代理服务器提供**

网上许多代理服务器提供商，但是大多是免费且不好用

> 米扑代理
>
> https://proxy.mimvp.com/free.php



**4)代理服务器使用**

> 由HttpClientDownloader设置：
>
> | **API**                                                      | **说明** |
> | ------------------------------------------------------------ | -------- |
> | HttpClientDownloader.setProxyProvider(ProxyProvider proxyProvider) | 设置代理 |
>
> ProxyProvider有一个默认实现：SimpleProxyProvider。它是一个基于简单Round-Robin的、没有失败检查的ProxyProvider。可以配置任意个候选代理，每次会按顺序挑选一个代理使用。它适合用在自己搭建的比较稳定的代理的场景。
>
> > 如果需要根据实际使用情况对代理服务器进行管理（例如校验是否可用，定期清理、添加代理服务器等），只需要自己实现APIProxyProvider即可。
>
> ```java
>  // 创建下载器 Downloader
> HttpClientDownloader httpClientDownloader = new HttpClientDownloader();
> // 给下载器设置代理服务器信息,ip地址和端口号是从代理商那儿获取的
> httpClientDownloader.setProxyProvider(SimpleProxyProvider.from(new Proxy("221.122.91.65", 80)));
> 
> Spider.create(new ProxyTest())
>     .addUrl("https://api.myip.com/")
>     .setDownloader(httpClientDownloader)// 设置下载器
>     .run();
> ```
>
> ![image-20220227151709137](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220227151709137.png)





#### 案例：

> 案例一：
>
> **爬取安居客房价信息实例**
>
> ```java
> public class AnjukeProcessor implements PageProcessor {
>     //抓取网站的相关配置，包括编码、抓取间隔、重试次数等
>     private Site site = Site.me().setRetryTimes(3).setSleepTime(100);
>     private static int count = 0;
>     private static List<String> urlList = new ArrayList<>();
> 
>     @Override
>     public void process(Page page) {
>         //从页面发现后续的url地址来抓取
>         page.addTargetRequests(
>                 page.getHtml().xpath("//div[@class='page-content']/div[@class='multi-page']/a/@href").all());
> 
>         //判断链接是否符合"https://qd.anjuke.com/community/p任意个数字"格式
>         if (page.getUrl().regex("https://qd.anjuke.com/community/p[0-9]+").match()) {
>             //定义如何抽取页面信息，并保存下来
>             List<Selectable> selectableList = page.getHtml().xpath("//div[@class='list-content']/div[@class='li-itemmod']").nodes();
>             List<HousePrice> list = new ArrayList<>();
>             for(Selectable selectable : selectableList){
>                 String name = selectable.xpath("//div[@class='li-info']/h3/a/text()").toString();
>                 String price = selectable.xpath("//div[@class='li-side']/p[1]/strong/text()").toString();
>                 HousePrice housePrice = new HousePrice();
>                 housePrice.setName(name.trim());
>                 housePrice.setPriceStr(price.trim());
>                 list.add(housePrice);
>             }
> 
>             page.putField("housePriceList",list);
>             urlList.add(page.getUrl().toString());
>             count++;
>         }
>     }
> 
>     @Override
>     public Site getSite() {
>         return site;
>     }
> 
>     public static void main(String[] args) {
>         Spider.create(new AnjukeProcessor())
>                 .addUrl("https://qd.anjuke.com/community/") //从https://qd.anjuke.com/community/开始爬取
>                 .addPipeline(new HousePricePipeline())  //使用自定义的Pipeline
>                 .thread(5)
>                 .run();
>         System.out.println("----------抓取了"+count+"条记录");
>     }
> }
> ```
>
> ```java
> public class HousePricePipeline implements Pipeline {
>     @Override
>     public void process(ResultItems resultItems, Task task) {
>         System.out.println("----------get page: " + resultItems.getRequest().getUrl());
>         List<HousePrice> list = resultItems.get("housePriceList");
>         System.out.println("----------list size:" + list.size());
>      }
> }
> ```





### *5.Elastic Search



#### (1)安装

首先下载包，然后解压启动

> 截图有误，应该是端口号为9200

![image-20220303161341079](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220303161341079.png)



然后解压ES图形工具

![image-20220303162426006](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220303162426006.png)

安装grunt

```shell
npm install grunt
```



在图形界面安装目录下面启动cmd

```shell
#启动grunt
grunt server
```



启动失败，因为还需要一些组件

![image-20220303162715566](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220303162715566.png)

```shell
#通过npm install命令进行安装
#选择组件名，右键就可以复制，再次右键就可以粘贴
npm install grunt-contrib-clean grunt-contrib-concat grunt-contrib-watch grunt-contrib-connect grunt-contrib-copy grunt-contrib-jasmine
```



再次启动，可以启动成功

然后通过网页访问

![image-20220303163413725](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220303163413725.png)



**安装分词器**

解压ik文件夹

![image-20220303163530162](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220303163530162.png)

复制到ES的plugin目录，并且更名ik

![image-20220303163609200](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220303163609200.png)

之后重启就可以了

> 进行分词测试
>
> ![image-20220303163736305](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220303163736305.png)
>
> > **ik_max_word**: 会将文本做最细粒度的拆分，比如会将“中华人民共和国人民大会堂”拆分为“中华人民共和国、中华人民、中华、华人、人民共和国、人民、共和国、大会堂、大会、会堂等词语。
> >
> > **ik_smart**: 会做最粗粒度的拆分，比如会将“中华人民共和国人民大会堂”拆分为中华人民共和国、人民大会堂。
> >
> > **pretty=true**: 美化输出



#### (2)开发中用到ES

```xml
<dependency>
    <groupId>org.elasticsearch</groupId>
    <artifactId>elasticsearch</artifactId>
    <version>5.6.8</version>
</dependency>

<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>transport</artifactId>
    <version>5.6.8</version>
</dependency>
```



### 6.selenium

对于特定需求：需要自动提交表单然后提交，然后爬取目标网页数据

可以使用selenium来实现



- 首先下载Chrome Driver，用以驱动打开浏览器

> 参考——https://blog.csdn.net/zhoukeguai/article/details/113247342

- 然后添加selenium依赖包，并且进行应用

  > 参考——
  >
  > https://blog.csdn.net/qq_22003641/article/details/79137327
  >
  > 