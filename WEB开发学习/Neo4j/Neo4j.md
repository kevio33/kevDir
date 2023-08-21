# 	Neo4j

## 一.知识图谱

首先看看什么是知识图谱

> 知识图谱（Knowledge Graph），在图书情报界称为知识域可视化或知识领域映射地图，是显示知识发展进程与结构关系的一系列各种不同的图形，用 可视化技术描述知识资源及其载体，挖掘、分析、 构建、绘制和显示知识及它们之间的相互联系。 知识图谱是通过将应用数学、 图形学、信息可视化技术、 信息科学等学科的理论与方法与计量学引文分析、共现分析等方法结合，并利用可视化的图谱形象地展示学科的核心结构、发展历史、 前沿领域以及整体知识架构达到多学科融合目的的现代理论。它能为学科研究提供切实的、有价值的参考。



## 二.Neo4j简介

知识图谱由于数据包含实体、属性、关系等，常见的关系型数据库不能很好的体现数据的这些特点，所以一般存储在图数据库当中



## 三.安装

> 参考---https://blog.csdn.net/Appleyk/article/details/79091898

> 因为有些版本需要jdk11支持，所以要下载jdk11，并且配置：
>
> https://blog.csdn.net/weixin_39830588/article/details/110708697?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ELandingCtr%7EHighlightScore-2.queryctrv2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ELandingCtr%7EHighlightScore-2.queryctrv2&utm_relevant_index=4



**安装后启动neo4j:**

> neo4j.bat console



**通过浏览器访问：**

> http://localhost:7474
>
> 默认账户密码:neo4j



**注册服务：**

> ```shell
> neo4j install-service
> ```
>
> 然后以后通过命令进行neo4j启动和停止等命令
>
> > - neo4j start：启动服务
> > - neo4j stop：停止服务
> > - neo4j restart：重启服务
> > - neo4j status：neo4j运行情况
> >
> > ![image-20220220195227478](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220220195227478.png)



## 四.使用

![image-20220221165633315](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220221165633315.png)



### 1.相关概念

- **节点：**

　　节点是图数据库中的一个基本元素，表示一个具体的实体，可以类比成关系数据库中的记录

- **关系：**

　　关系是图数据库中节点之间的连接，也表示成节点之间的边，neo4j中关系是双向的

- **属性：**

　　属性是对节点以及关系的一种说明，可以类比成关系数据库中的字段或者列，这里需要强调的是，在图数据库中关系也是可以设计属性的。

- **标签：**

　　标签（label）是同一类节点的组合，但是并不是在同一标签下的节点一定包含相同属性。

- **路径：**

　　路径是图数据库建立之后，任意两个节点之间的连通管理，是关系的组合。



### 2.**创建一个基本节点**

> 姓名 ：鱼暖暖（节点的名称即节点的变量名,下面我们用简写的英文字母表示）
>
> 性别 ：男
>
> 职业 ：程序员（节点的类别，可作为节点属性的data）
>
> 爱好 ：NBA
>
> ```cypher
> Create(Yu:Coder{name:'yht',sex:'男',hobby:'vollyball'})
> ```
>
> 

![img](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/20180118103104454)





## 五、Cypher

### 1.简介

cypher是一种非常简洁的图查询语言，可以在shell与浏览器端直接执行。其基本语法包含以下四个部分：

>  start：设定查找的起始节点
>
> match：匹配图形模式， 可以定位感兴趣数据的子图形
>
> where：对查询数据进行过滤
>
> limit：限制查询数量
>
> return：返回结果
>
> 注意：在新版本的neo4j中start可以省略



### 2.CREATE创建节点

> ```cypher
> CREATE (ee:Person { name: "yht", sex:"男", age:"23" })
>                      
> Merge (..)//采用merge可以避免重复创建
> ```
>
> - `CREATE` 创建数据的子句
> - `()` 圆括号表示一个节点
> - `ee:Person` 变量 'ee' 和 标签 'Person' 表示新的 节点(ee表示变量名，Person表示节点名，也就是标签)
> - 花括号添加属性到节点



![image-20220221175705483](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220221175705483.png)



### 3.Match查询节点

> ```cypher
> MATCH (ee:Person) WHERE ee.name = "Emil" RETURN ee;
> ```
>
> - `MATCH` 指定节点和关系的模式的子句
> - `(ee:Person)` 带有标签 "Person" 的单节点模式, 将匹配项赋给变量 "ee"
> - `WHERE` 约束结果的子句
> - `ee.name = "Emil"` 匹配name是Email的Person类节点
> - `RETURN` 用于请求特定结果的子句
>
> 
>
> ee是变量，通过where查询相匹配的节点，并返回该变量ee

> **IN**：多条件查找
>
> ![image-20220316201130936](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220316201130936.png)



### 4.Delete删除结点

> 删除label为Person的节点
>
> ```cypher
> Match (n:Person)
> Delete n//注意，换行通过Shift+Enter
> ```
>
> **如果删除的节点有关系，那么要加上`detach`关键字**
>
> ```cypher
> Match(n:Coder) Detach
> Delete n
> ```

### 5.更新结点

> 先match出结点，然后在set结点





### 6.节点和关系

Cypher 采用一对**圆括号** () 来表示节点，如 (n:角色) 表示一个 **角色** 节点，n 是**变量名**，供命令执行时用 n 来访问这个节点，在命令执行完毕后就无法使用了。同时单独的 () 表示一个**匿名节点**，在匹配时表示**匹配**所有节点。

> - `--`表示无方向的关系
> - `-->` 表示有方向的关系
> - `-[r]->` 则给关系赋予一个变量名，方便对这个关系进行操作
> - `-[r:配偶]->` 匹配关系为 `配偶` 的类型



例子：

> ```cypher
> Match(n:Coder{name:'鱼暖暖'})
> Match(m:Photographer{name:'凉暖暖'})
> Merge(n)-[:LOVE{years:'five years more'}]->(m)
> ```
>
> - 查到节点n和m
> - 关系连接
> - 防止Match的节点不存在，关系连接的时候用Merge：如果节点存在创建
> - 关系：[:关系data]



![image-20220221193805122](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220221193805122.png)



### 7.添加索引

![image-20220316201218420](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220316201218420.png)

> ```cypher
> create index on :Person(name)//Person是label名称，name是对应属性
> ```
>



### 8.获取所有关系和结点

![image-20220317225158002](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220317225158002.png)

## 参考

> - https://www.cnblogs.com/1314h/p/15092899.html
>
> - https://blog.csdn.net/Appleyk/article/details/79091898
>
>   语法规范
>
> - https://www.jianshu.com/p/2180b68da482