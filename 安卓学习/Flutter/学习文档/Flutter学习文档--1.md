



## 一、Flutter生态情况

```
gitgub地址:https://github.com/Solido/awesome-flutter 里面包含许多插件

showcase: https://flutter.io/showcase/  flutter官网
```

**material design：**

[Material](https://material.io/guidelines/)是一种标准的移动端和web端的视觉设计语言

## 二、第一个flutter程序

改写第一个flutter程序

```dart
import 'package:flutter/material.dart';

void main()=>runApp(MyApp());//将一个widget传给runApp函数即可

//自定义组件：实质是创建一个继承自statelesswidget的类
class MyApp extends StatelessWidget{
    @override
    Widget build(BuildContext context) {
        return MaterialApp(//widget需要位于MaterialApp内才能正常显示， 因此我们使用MaterialApp来运行该应用
            title: 'Welcome to flutter',
            home: Scaffold(
                appBar: AppBar(//设置应用顶部bar的内容
                    title: Text("welcomr to flutter")
                ),
                body: Center(//设置应用中心的内容
                    child: Text("hello world"),
                ),
            ),
        );
    } 
}
```

> 在编写应用程序时，通常会创建新的widget:
>
> - 无状态的`StatelessWidget`
>
> - 有状态的`StatefulWidget`
>
> 具体的选择取决于您的widget是否需要管理一些状态。widget的主要工作是实现一个build函数，用以构建自身。一个widget通常由一些较低级别widget组成。

#### 	(1)MaterialApp

Flutter的wdiget里，MaterialApp 代表使用（Material Design）风格的应用。里面包含了纸墨设计风格应用所需要的基本控件。

**主要属性**

- `title` ： 在任务管理窗口中所显示的应用名字
- `theme` ： 应用各种 UI 所使用的主题颜色
- `color` ： 应用的主要颜色值（primary color），也就是安卓任务管理窗口中所显示的应用颜色
- `home` ： 应用默认所显示的界面 Widget
- `routes` ： 应用的顶级导航表格，这个是多页面应用用来控制页面跳转的，类似于网页的网址
- `initialRoute` ：第一个显示的路由名字，默认值为 Window.defaultRouteName
- `onGenerateRoute` ： 生成路由的回调函数，当导航的命名路由的时候，会使用这个来生成界面
- `onLocaleChanged` ： 当系统修改语言的时候，会触发å这个回调
- `navigatorObservers` ： 应用 Navigator 的监听器
- `debugShowMaterialGrid` ： 是否显示 纸墨设计 基础布局网格，用来调试 UI 的工具
- `showPerformanceOverlay` ： [显示性能标签](https://links.jianshu.com/go?to=https%3A%2F%2Fflutter.io%2Fdebugging%2F%23performanceoverlay)
- `checkerboardRasterCacheImages 、showSemanticsDebugger、debugShowCheckedModeBanner(关闭右上角debug图标)` 各种调试开关

**常见字段及其属性**

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210609133445817.png" alt="image-20210609133445817" style="zoom:67%;" />



#### (2)widget:

Flutter有一套丰富、强大的基础widget，其中以下是很常用的：

- [`Text`](https://docs.flutter.io/flutter/widgets/Text-class.html)：该 widget 可让创建一个带格式的文本。
- [`Row`](https://docs.flutter.io/flutter/widgets/Row-class.html)、 [`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html)： 这些具有弹性空间的布局类Widget可让您在水平（Row）和垂直（Column）方向上创建灵活的布局。其设计是基于web开发中的Flexbox布局模型。
- [`Stack`](https://docs.flutter.io/flutter/widgets/Stack-class.html)： **取代线性布局** (译者语：和Android中的LinearLayout相似)，[`Stack`](https://docs.flutter.io/flutter/widgets/Stack-class.html)允许子 widget 堆叠， 可以使用 [`Positioned`](https://docs.flutter.io/flutter/widgets/Positioned-class.html) 来定位他们相对于`Stack`的上下左右四条边的位置。Stacks是基于Web开发中的绝度定位（absolute positioning )布局模型设计的。
- [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)： [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html) 可让您创建矩形视觉元素。container 可以装饰为一个[`BoxDecoration`](https://docs.flutter.io/flutter/painting/BoxDecoration-class.html), 如 background、一个边框、或者一个阴影。 [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html) 也可以具有边距（margins）、填充(padding)和应用于其大小的约束(constraints)。另外， [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)可以使用矩阵在三维空间中对其进行变换。



#### (3)Scaffold：脚手架

**简介**：`Scaffold` 实现了基本的 `Material` 布局。只要是在 `Material` 中定义了的单个界面显示的布局控件元素，都可以使用 `Scaffold` 来绘制。可以**将 Scaffold 理解为一个布局的容器**。可以在这个容器中绘制我们的用户界面。

一般来说，**总是定义一个 Scaffold 当作实参传入到 `MaterialApp` 的 home 属性**。换句话说，一个 MaterialApp 总是绑定一个 Scaffold。

**Scaffold定义了一个 UI 框架**，这个框架包含： 头部导航栏，body，右下角浮动按钮，底部导航栏等。

**主要属性**：

```dart
class Scaffold extends StatefulWidget {
    /// Creates a visual scaffold for material design widgets.
    const Scaffold({
        Key key,
        this.appBar, //横向水平布局，通常显示在顶部（*）
        this.body, // 内容（*）
        this.floatingActionButton, //悬浮按钮，就是上图右下角按钮（*）
        this.floatingActionButtonLocation, //悬浮按钮位置
        //悬浮按钮在[floatingActionButtonLocation]出现/消失动画
        this.floatingActionButtonAnimator, 
        //在底部呈现一组button，显示于[bottomNavigationBar]之上，[body]之下
        this.persistentFooterButtons,
        //一个垂直面板，显示于左侧，初始处于隐藏状态（*）
        this.drawer,
        this.endDrawer,
        //出现于底部的一系列水平按钮（*）
        this.bottomNavigationBar,
        //底部持久化提示框
        this.bottomSheet,
        //内容背景颜色
        this.backgroundColor,
        //弃用，使用[resizeToAvoidBottomInset]
        this.resizeToAvoidBottomPadding,
        //重新计算布局空间大小
        this.resizeToAvoidBottomInset,
        //是否显示到底部，默认为true将显示到顶部状态栏
        this.primary = true,
        //
        this.drawerDragStartBehavior = DragStartBehavior.down,
    }) : assert(primary != null),
    assert(drawerDragStartBehavior != null),
    super(key: key);
```

**下面举例一些常用的scaffold属性**

##### ①bottomNavigationBar(底部导航栏)

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210613152118493.png" alt="image-20210613152118493" style="zoom:67%;" />

```dart
class ContentY extends StatefulWidget {
    @override
    _ContentYState createState() => _ContentYState();
}

//由于需要动态渲染，所以自定义为stateful组件
class _ContentYState extends State<ContentY> {
    int _index = 0;
    @override
    Widget build(BuildContext context) {
        return Scaffold(
            appBar: AppBar(
                title: Text('大家好吗'),
            ),
            bottomNavigationBar: BottomNavigationBar(
                currentIndex: this._index,//默认显示点亮的图标
                /*底部导航栏的图标*/
                items: [
                    BottomNavigationBarItem(
                        icon: Icon(Icons.home_sharp),
                        label: '主页键'
                    ),
                    BottomNavigationBarItem(
                        icon: Icon(Icons.category),
                        label: '分类'
                    ),
                    BottomNavigationBarItem(
                        icon: Icon(Icons.add_a_photo),
                        label: 'camera'
                    )
                ],
                onTap: (int index){
                    setState(() {
                        this._index = index;//点击按钮后点亮显示图标
                    });
                },
            ),
        );
    }
}
```

> 当图标数量超过四个的时候，需要设置type:BottomNavigationBarType.fix

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210613154039072.png" alt="image-20210613154039072" style="zoom:67%;" />

**点击不同按钮跳转不同界面：**

首先将不同页面抽离出去

![image-20210613161403517](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210613161403517.png)

```dart
class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text('home page'),
    );
  }
}
```

然后Tab.dart引入依赖

![image-20210613161505311](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210613161505311.png)

由于每个页面也是一个组件，所以将其放在list中

```dart
List _pageList = [
    HomePage(),
    CategoryPage(),
    CameraPage(),
    PamphletPage()
  ];
```

最后在scoffold中的body属性调用即可

```dart
 body: this._pageList[this._index],
```

Tabs.dart完整代码

```dart
class ContentY extends StatefulWidget {
  @override
  _ContentYState createState() => _ContentYState();
}

class _ContentYState extends State<ContentY> {
  List _pageList = [
    HomePage(),
    CategoryPage(),
    CameraPage(),
    PamphletPage()
  ];
  int _index = 0;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('大家好吗'),
      ),
      body: this._pageList[this._index],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: this._index,//默认显示点亮的图标
        /*底部导航栏的图标*/
        items: [
          ......
        ],
        onTap: (int index){
          setState(() {
            this._index = index;//点击按钮后更改显示图标
          });
        },
      ),
    );
  }
}
```

**结合floatingactionbar形成一个镶嵌的效果：**

```dart
bottomNavigationBar: BottomAppBar(
        color: Colors.white,
        shape: CircularNotchedRectangle(), // 底部导航栏打一个圆形的洞
       ......
 floatingActionButton: FloatingActionButton(
     onPressed: () => _onFabClick,
     child: Icon(Icons.add),
 ),
 floatingActionButtonLocation:
    FloatingActionButtonLocation.centerDocked, //设置FloatingActionButton的位置
);
```

![悬浮按钮镶嵌在Bar上](https://gitee.com/kevinyong/kevin-gallery/raw/master/16ac48963dfdc164)

##### ②AppBar(顶部导航栏)

![image-20210615143205145](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615143205145.png)

**Leading：**

```dart
//一般leading里面设置一个IconButton
appBar:AppBar(
	leading:IconButton(
        icon:Icon(Icons.menu),
        onPressd:(){
            
        },
    )
)
```

![image-20210615144131278](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615144131278.png)

**actions：**

```dart
appBar:AppBar(
	actions: <Widget>[
          IconButton(
              icon: Icon(Icons.search),
              onPressed:(){
              })
        ]
    )
)
```

![image-20210615144418496](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615144418496.png)

**bottom**

主要涉及TabBar的定义

①通过tabbar类来实现

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615144923461.png" alt="image-20210615144923461" style="zoom:67%;" />

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615144827352.png" alt="image-20210615144827352" style="zoom:67%;" />

```dart
return DefaultTabController(//外层要嵌套一个controller
    length: 2,
    child: Scaffold(
        appBar: AppBar(
            title: Text('Main Screen'),
            bottom: TabBar(//声明TabBar
                tabs: <Widget>[
                    Tab(text:'首页'),
                    Tab(text: '个人信息',)
                ]
            )
        )
    )
);
```



![image-20210615145709238](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615145709238.png)

通过点击tabbar显示不同页面：

```dart
body: TabBarView(
    children: <Widget>[//点击第一个tab就显示list第一个元素
        ListView(
            children: <Widget>[
                Text('首页 1'),
                Text('首页 2'),
                Text('首页 3')
            ],
        ),
        ListView(//点击第二个tab显示第二个元素
            children: <Widget>[
                Text('个人 1'),
                Text('个人 2'),
                Text('个人 3')
            ],
        )

        //也可以在前边onTap定义点击事件
        TabBar(
            tabs: <Widget>[
                Tab(child: Text('yht')),
                Tab(child: Text('hty'))
            ],
            onTap: (index){
                if(index==1){
                    print('1');
                }else{
                    print('0');
                }
            },
        ),
```

**如果顶部嵌套太多的scaffold则会导致有多个appbar，这时候可以把tabbar定义在title属性里面**

更改之前：

![image-20210615151349323](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615151349323.png)

更改之后:

![image-20210615151435113](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615151435113.png)



②通过tabcontroller实现:

```dart
//1.必须通过stateful Widget来实现
class TabBarControllerPage extends StatefulWidget {
  @override
  _TabBarControllerPageState createState() => _TabBarControllerPageState();
}

//2.混合SingleTickerProviderStateMixin
class _TabBarControllerPageState extends State<TabBarControllerPage> with SingleTickerProviderStateMixin{
   TabController _tabController;//3.声明tabcontroller

    //4.初始化tabcontroller
   @override
  void initState() {
    // TODO: implement initState
    super.initState();
    _tabController = TabController(length: 2, vsync: this);
  }

    //5.声明tabbar
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('tabbarcontroll'),
        bottom: TabBar(
          controller: _tabController,
          tabs: <Widget>[
            Text('hello'),
            Text('你好')
          ],//调用已经初始化的tabBarcontroller
        ),
      ),
      body: TabBarView(
        controller: _tabController,
        children: <Widget>[
          Center(child: Text('首页1')),
          Center(child: Text('首页2'))
        ],
      ),
    );
  }
}
```

**使用tabcontroller好处是:**

能够在外部监听tabbar和进行一系列自定义操作

```dart
 @override
  void initState() {
    // TODO: implement initState
    super.initState();
    _tabController = TabController(length: 2, vsync: this);
     
      //监听tabbar
    _tabController.addListener((){
        print(_tabController.index);
    });
  }
```



##### ④drawer侧滑栏：

```dart
//左侧侧滑栏
drawer:Drawer(
	child:....
)
//右侧侧滑栏
endDrawer:Drawer(
	child:...
)   
```



###### 配置header

往往在drawerheader外部包裹一层row或者container，来占满整个drawer宽度

①Drawerheader--添加头部

![image-20210615155640957](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615155640957.png)

```dart
drawer:Drawer(
	child:Column(
    	children:<Widget>[
          Row(
          	children:<Widget>[
                Expanded(
                	child:DrawerHeader(//声明一个drawerheader
                    .....
                    )
                )
            ]
          )  
        ],
    )
)
```

②UserAccountsDrawerHeader

格式被局限，一般作为用户信息界面

![image-20210615160112479](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615160112479.png)

```dart
drawer:Drawer(
	child:Column(
    	children:<Widget>[
          Row(
          	children:<Widget>[
                Expanded(
                	child:UserAccountsDrawerHeader(//声明一个drawerheader
                    .....
                    )
                )
            ]
          )  
        ],
    )
)
```





**drawer的跳转可以通过图片或者listtile的ontap属性来实现点击**



#### (4)Stateless widgets 和  Stateful widgets

- `Stateless widgets` 是不可变的, 这意味着它们的属性不能改变 - 所有的值都是最终的.
- `State*ful* widgets` 持有的状态可能在widget生命周期中发生变化. 实现一个 stateful widget 至少需要两个类:
  1. 一个 StatefulWidget类。
  2. 一个 State类。 StatefulWidget类本身是不变的，但是 State类在widget生命周期中始终存在.



##### ①StatelessWidget

```dart
class ContentY extends StatelessWidget {
  int countN = 0;//用于计数
  @override
  Widget build(BuildContext context) {
    return Container(
      child: ListView(
        children: <Widget>[
          Text('${this.countN}'),//显示计数
          ElevatedButton(
              onPressed: (){
                countN++;//点击按钮，计数增加
                print(countN);
              },
              child: Text('按钮')
          )
        ],
      ),
    );
  }
}
```

![image-20210613145008874](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210613145008874.png)

**以上结果表明，无论怎么点击按钮，显示数字都不会改变，证明StateLessWidget中组件不会发生变化**

##### ②StatefulWidget

```dart
class ContentY extends StatefulWidget {//继承statefulWidget
  @override
  _ContentYState createState() => _ContentYState();//重写方法
}

class _ContentYState extends State<ContentY> {//继承State
  List _list = new List();
  @override
  Widget build(BuildContext context) {
    return Container(
      child: ListView(
        children: <Widget>[
          Column(
            children: _list.map((value){//column里面的元素为_list里边的value
               return ListTile(
                 title: Text(value),
              );
            }).toList(),
          ),
          ElevatedButton(
              onPressed: (){//设置点击事件
                setState(() {//必须调用该方法，flutter才会更新页面数据
                  _list.add('new1');
                  _list.add('new2');
                });
          },
              child: Text('增加')
          )
        ],
      ),
    );
  }
}
```

![image-20210613151530155](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210613151530155.png)



###### `setState(){}`

`setState` 方法会触发当前 `State` 对象关联的 `StatefulWidget` 以及它的子树中所有依赖于此状态的 widget 进行重建，因此可以认为是局部刷新。 

> **注意**
>
> 如果 `StatefulWidget` 的子树很大，或者有很多依赖于此状态的 widget，那么调用 `setState` 方法可能会导致大量的 widget 重建，从而影响应用程序的性能。因此，在使用 `setState` 方法时，需要尽可能地减少依赖于此状态的 widget 的数量，以提高应用程序的性能。 





#### (5)pubspec.yaml

pubspec文件管理Flutter应用程序的assets(资源，如图片、package等)

##### ①通过pubspec.yaml引入第三方库

在pubspec.yaml中，将english_words（3.1.0或更高版本）添加到依赖项列表

```yaml
dependencies:
  flutter:
    sdk: flutter

  english_words: ^3.1.0 #引入的依赖库；english_words开源软件包 ，其中包含数千个最常用的英文单词以及一些实用功能.
```

在Android Studio的编辑器视图中查看pubspec时，单击右上角的 **Packages get**，这会将依赖包安装到您的项目。您可以在控制台中看到以下内容：

```dart
flutter packages get
Running "flutter packages get" in startup_namer...
Process finished with exit code 0
```

然后在dart文件中引入

```dart
import 'package:english_words/english_words.dart';
```





##### ②静态资源加载

![image-20210419212148908](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210419212148908.png)

![image-20210419212205542](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210419212205542.png)

![image-20210609174257109](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210609174257109.png)

![image-20210419212225967](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210419212225967.png)





## 三、基本组件：

### 1.TextWighet

textwidget

```dart
child: Text("hello world,我是yht，来自。。。哈哈哈哈哈哈哈哈哈哈lalallalalllallal",
          textAlign: TextAlign.center,//居中对齐,textAlign.center
          maxLines: 1,//最大的显示行数
          overflow: TextOverflow.ellipsis,//溢出行数的文字用省略号省略
          //TextOverflow.fade：产生渐变效果
          softWrap: false//文本超过边界是否换行
          //设置文字样式
          style: TextStyle(
            fontSize: 25.0,
            color: Color.fromARGB(255, 0, 0, 255),
            decoration: TextDecoration.underline,//下划线
            decorationStyle: TextDecorationStyle.solid//下划线样式
            fontStyle: FontStyle.italic,//斜体
            fontWeight: FontWeight.bold//粗体
           
          ),
          ),
```

RichText:

控件RichText包含TextSpan子组件，TextSpan子组件可包含一组的TextSpan子组件，TextSpan子组件通过stye设置不同文本样式，通过recognizer实现点击等各种事件

```dart
 RichText(
    text: TextSpan(
        text: '本人同意领取赠险并许可保险公司沟通赠险生效事宜,已阅读',
        style: TextStyle(color: Colors.black, fontSize: 13),
        children: <TextSpan>[
             TextSpan(
                 text: '《活动规则》',
                 style:TextStyle(color: Color(0xFF008EFF), fontSize: 13),
                        recognizer: TapGestureRecognizer()
                          ..onTap = () async {
                            AgreementDialog.showActivityDialog(context);
                      }),
              TextSpan(
                  text: '及',
                  style: TextStyle(color: Colors.black, fontSize: 13),
                    ),
               TextSpan(
                   text: '《信息安全说明》',
                   style:TextStyle(color: Color(0xFF008EFF), fontSize: 13),
                        recognizer: TapGestureRecognizer()
                          ..onTap = () async {
                            AgreementDialog.showSecurityDialog(context);
                       }),
                  ],
                ),
          )
```

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210609153302436.png" alt="image-20210609153302436" style="zoom:67%;" />

**TextField:输入框**

```dart
//Row 不允许直接嵌套 TextField
//解决方案：在 TextField 外面套一层 Expanded 即可

Row(
  children: [
    Expanded(child:TextField(
      decoration: InputDecoration(
        hintText: "请输入用户名",
      ),
    )),
  ],
),
```

![image-20210423170649511](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210423170649511.png)



可以通过在container里面放RowWidget实现不同布局和widget嵌套

```dart
 body: new Container(//主要的容器
        child: new Row(//里面放一个row widget
          children: <Widget>[//然后在多个widget
            Expanded(child:
              new TextField(
                decoration: new InputDecoration(
                  icon: new Icon(Icons.person)
                ) ,
              ),
            )],
        ),
      ),
```





### 2.ContainerWidget

#### 核心点：

- Container具有绘制、定位、调整大小功能。
- 通常用来装载其它子控件，**假如Container没有子控件，它将自动填充整个屏幕；反之，会根据子控件大小，调整自身大小，从而达到自适应效果。**
  - Container只能容纳一个子控件，如果想容纳更多的子控件,可以将子控件设置为Row、Column、Stack(这三个子控件都有一个children属性)
- 使用Container时，通常要有一个父控件，常用的父控件有Center、Padding、Column、Row、Scaffold。

![image-20210609140015478](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210609140015478.png)

container1：

```dart
body: Center(//设置应用中心的内容
          child: Container(
            child: Text(
              "hahahahhaaahaha",
              style: TextStyle(
                fontSize: 16.0,
              ),
            ),
            alignment: Alignment.center,//container中元素水平对齐方式为居中
            width: 500,//设置容器宽高
            height: 400,
            color: Colors.blue,//设置背景色
          ),
        ),
```



container2：

```dart
body: Center(//设置应用中心的内容
          child: Container(
            child: Text(
              "hahahahhaaahaha",
              style: TextStyle(
                fontSize: 16.0,
              ),
            ),
            alignment: Alignment.center,//container中元素水平对齐方式为居中
            width: 500,//设置容器宽高
            height: 400,
            /*color: Colors.blue,设置背景色*/
            padding: const EdgeInsets.fromLTRB(10, 100, 0, 0),//设置padding
            margin: const EdgeInsets.all(10.0),
            decoration: new BoxDecoration(//装饰container
              gradient: const LinearGradient(
                  colors: [Colors.lightBlue,Colors.green,Colors.purple]),//线性渐变
              border: Border.all(width: 2.0,color: Colors.yellow),
                
              borderRadius: BorderRadius.circular(150)//更改边框的弧度
              /*borderRadius: BorderRadius.all(
                 //Radius.circular(150)
            	)*///也可以起到更改边框弧度作用
            ),
          ),
        ),
```



#### **注意：**

①当存在decoration属性时，外部不能够存在color属性

```dart
child: Container(
        width: 300.0,
        height: 300.0,
        decoration: BoxDecoration(
          color: Colors.deepPurple,//必须在decoration里面进行color属性声明
          borderRadius: BorderRadius.only(
            topRight: Radius.circular(30),
            topLeft: Radius.circular(50)
          )
        )
      )
```



### 3.ImageWidget



```dart
body: Center(//设置应用中心的内容
          child: Container(
            child: new Image.network(
              'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Ffile.fh21.com.cn%2Ffhfile1%2FM00%2F38%2FC3%2Fo4YBAFcdqPmAMNTPAADIXz-20so63.jpeg&refer=http%3A%2F%2Ffile.fh21.com.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1617788425&t=3fc6e4e7324e36fc9b2591149c588925',//加载网络图片
              fit: BoxFit.contain,//原比例显示图片，不改变图片的比例
              //fit: BoxFit.fill//填满整个容器，会拉伸
              //fit: BoxFit.cover//图片充满整个容器，但是不改变比例
                
              //color: Colors.purple,//为图片设置颜色
              //colorBlendMode: BlendMode.darken,设置图片颜色模式
              //repeat: ImageRepeat.repeat,//图片重复Y纵向(repeatY)，X横向(repeatX)
            ),
            width: 300,
            height: 300,
            color: Colors.yellow,
          ),
        ),
```

#### 圆形图片

**(1)通过container里面的decoration来制作**

```dart
child:Container(
    ......
    decoration: BoxDecoration(
        borderRadius: BorderRadius.circular(150),//更改边框的弧度
        image:DecorationImage(
            image:NetWorkImage("...."),
            fit:BoxFit.cover
        )
    )
),
```

![image-20210609164119313](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210609164119313.png)



**(2)通过组件ClipOval**

```dart
return Center(
      child: Container(
       child: ClipOval(//clipoval
         child: Image.network("...",
           width: 300.0,//指定宽高
           height: 300.0,
           fit: BoxFit.cover,
         ),
```



### 4.ColumnWidget

垂直布局组件，它根据子元素的长宽来自适应，而不像ListView直接占满

![image-20210610160630716](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610160630716.png)

```dart
body: Center(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.center,//相对于所有元素中最长的哪一个的中心
            mainAxisAlignment: MainAxisAlignment.center,//主轴（纵轴）的分布为中心,
            children: <Widget>[
              Text("哈哈哈"),
              Text("llala"),
              Text("babab")
            ],
          ),
        )
```



![1615379126839](https://gitee.com/kevinyong/kevin-gallery/raw/master/1615379126839.png)

#### 注意：

①**在Flutter 的 Column/Row 内使用ListView.builder()需要对该ListView的大小进行指定.**
具体的解决办法就是 在该ListView.builder()外嵌套一个SizeBox或者Container设置高度即可。

```dart
Container(
    height: 200.0,
    child: ListView.builder(
        itemCount: 3,
        itemBuilder: _cellForRow,
    ),
),
```

### 5.RowWidget

![image-20210610160608783](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610160608783.png)

```dart
 body: new Row(
          children: <Widget>[
            Expanded(child: new RaisedButton(//expanded指灵活的布局，如果所有元素都这样这是会均分
              onPressed: (){},//按压事件
              color: Colors.redAccent,
              child: Text("shit 1"),
            ),),
            new RaisedButton(
              onPressed: (){},
              color: Colors.lightBlue,
              child: Text("shit2"),
            )
          ],
        ),
```

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1615378534394.png" alt="1615378534394" style="zoom:67%;" />

```dart
Row(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,//实现均分
        children: <Widget>[
          Text('one'),
          Text('two'),
          Text('three')
        ],
     )
```

![image-20210610162737348](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610162737348.png)

```dart
MainAxisAlignment.center//紧凑居中
```

![image-20210610162856551](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610162856551.png)

```dart
mainAxisAlignment: MainAxisAlignment.end//排列在最后面
```

![image-20210610162935072](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610162935072.png)

```dart
mainAxisAlignment: MainAxisAlignment.spaceAround
```

![image-20210610163013917](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610163013917.png)

```dart
mainAxisAlignment: MainAxisAlignment.spaceBetween
```

![image-20210610163056893](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610163056893.png)

### 6.ListView

#### 垂直列表

格式化代码：shift+tab左，tab右

ListView里面元素放ListFile相当于一个一个卡片

```dart
 body: ListView(
          children: <Widget>[//list数组，每个元素是一个对象
            new ListTile(
              leading: new Icon(Icons.ac_unit),//图标放在最前面
              title: new Text('first shit'),//图标文字
              subtitle: Text('sdsds')//二级标题
              //trailing:Icon(Icons.ac_unit)//图标放在title后面
            ),
            new ListTile(
              leading: new Icon(Icons.ac_unit_outlined),
              title: new Text('second shit'),
            ),
            new ListTile(
              leading: new Icon(Icons.ac_unit_rounded),
              title: new Text('third shit'),
            ),
          ],
        ),
```

![image-20210419210602164](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210419210602164.png)



元素放Image

```dart
//listview里面以image作为元素,插入图片
body: ListView(
          children: <Widget>[
            new Image.network('...'),
            new Image.network('...'),
            new Image.network('...')
          ],
        ),
```



![1615367494804](https://gitee.com/kevinyong/kevin-gallery/raw/master/1615367494804.png)





#### 水平列表

```dart
scrollDirection: Axis.horizontal,//设置listview的方向为水平
```



```dart
body: Center(//body里面放一个container
          child: Container(
            height: 200,
            child: ListView(//container里面放一个listview
                scrollDirection: Axis.horizontal,//设置listview的方向为水平
                children: <Widget>[//listview放一个container数组
                  new Container(
                    color: Colors.purple,
                    width: 150,
                  ),
                  new Container(
                    color: Colors.yellow,
                    width: 150,
                  ),
                  new Container(
                    color: Colors.green,
                    width: 150,
                  )
                ]
            ),
          ),
        ),
```



![1615368332353](https://gitee.com/kevinyong/kevin-gallery/raw/master/1615368332353.png)



#### ListView动态获取元素

①自定义方法

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610141350536.png" alt="image-20210610141350536" style="zoom:67%;" />





②通过构造函数传参，**ListBuilder遍历数据**

```dart
void main()=>runApp(MyApp(
  items:new List<String>.generate(1000, (index) => "item $index")//传递参数，通过generate方法构造一个长度为1000，内容为"item $index的集合"
));

class MyApp extends StatelessWidget{
  final List<String> items;//用于接收参数
  MyApp({Key key,@required this.items}):super(key:key);//构造方法，且需要调用父类构造方法
		........
 		........
            child: new ListView.builder(
              itemCount: items.length,//获取items长度
              itemBuilder: (context,index){//动态构建items
                return ListTile(
                  title: new Text('${items[index]}'),//将items[index]的值取出来
                );
              },
            ), 
  .............
}
```



<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1615370080172.png" alt="1615370080172" style="zoom:67%;" />

③读取文件数据，然后遍历展示到listview中

创建数据文件

![image-20210610143355772](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610143355772.png)

引用数据文件

![image-20210610143423079](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610143423079.png)

在类中创建遍历数据集的方法

```dart
//遍历ListData.dart中的数据
  List<Widget> _trans(){
    var tempList = ListData.map((e){//遍历元素，e为list中每个元素，后面方法体返回对每个e的操作
        return ListTile(title: Text(
            '${e['name']}',
          )
        );
    });
    return tempList.toList();//返回的还是一个可迭代对象，要转换成List
  }
```

ListView调用

```dart
child: ListView(
    children: _trans(),//调用内部方法
),    
```

![image-20210610143713582](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610143713582.png)



#### 注意：

​				**①ListView里边不能嵌套ListView**

### 7.GridView

主要通过两种方式创建：

- GridView.count
- GridView.Builder

![image-20210610151256795](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610151256795.png)

①GridView.count

```dart
body: GridView.count(
          padding: EdgeInsets.all(20),
          crossAxisCount: 2,//每行列数
          crossAxisSpacing: 10,//每个元素间的间距
          children: <Widget>[
            const Text("first shit"),
            const Text("first shit"),
            const Text("first shit"),
            const Text("first shit"),
            const Text("first shit"),
            const Text("first shit"),
          ],
        ),
```



<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210419210752315.png" alt="image-20210419210752315" style="zoom:67%;" />



②通过GridView.Builder

用法与ListView.Builder类似

```dart
child: GridView.builder(
        itemCount: ListData.length,
        itemBuilder: (context,index){
          return Text(ListData[index]['name']);
        },
      )
```

如果想要设置gridview相应的属性(例如每行多少列，每行间距)，应该在gridDelegate里面设置

```dart
 gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisSpacing: 10,//每列元素间距
          crossAxisCount: 2//每行列数
        ),
```

完整代码

```dart
return Container(
      child: GridView.builder(
        itemCount: ListData.length,
        itemBuilder: (context,index){
          return Text(ListData[index]['name'],
          textAlign: TextAlign.center,
          style: TextStyle(
            fontSize: 20
            ),
          );
        },
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisSpacing: 10,//每列元素间距
          crossAxisCount: 2//每行列数
        ),
      )
    );
```

![image-20210610153609382](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610153609382.png)

③通过GridView创建

```dart
 body: GridView(
          gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
            crossAxisCount: 2,//列数
            mainAxisSpacing: 2,//行间距
            crossAxisSpacing: 2,//列间距
            childAspectRatio: 1,//横宽比?
          ),
          children: <Widget>[
            new Image.network('...',fit: BoxFit.cover,),
            new Image.network('...',fit: BoxFit.cover,),
            new Image.network('...',fit: BoxFit.cover,),
          ],
        ),
```



<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210419210806053.png" alt="image-20210419210806053" style="zoom:67%;" />

### 8.StackWidget

![image-20210610165014157](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610165014157.png)

```dart
class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    var stack = Stack(//声明层叠布局
 /**
FractionalOffset的两个参数dx和dy是一个倍数而不是距离值。
dx=1表示子控件从父容器的最左边移动到最右边的距离，也就是width=parentWidtth-childWidth，而不是parentWidth
dy=1表示子控件从父容器最顶部移动到最底部的距离，也就是height=parentHeight-childHeight，而不是parentHeight
**/

      alignment: const FractionalOffset(0.5, 0.8),
      children: <Widget>[
        new CircleAvatar(
          backgroundImage: NetworkImage('https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwx2.sinaimg.cn%2Forj360%2Fea15d2fcly1gfdoivv22qj20sg0lcgnv.jpg&refer=http%3A%2F%2Fwx2.sinaimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1617971934&t=85630b4c441735fb462e389bd0d5d1d8'),
          radius: 100,
        ),
        new Container(
          decoration: BoxDecoration(//矩形色块
            color: Colors.lightBlue
          ),
          padding: EdgeInsets.all(5),
          child: Text('kevin'),
        )
      ],
    );
    return MaterialApp(
      title: "hello",
      home: Scaffold(
        appBar: AppBar(
          title: Text("nihaoma"),
        ),
        body: Center(
          child: stack//调用stack布局
        )
      ),
    );
  }
}
```



![1615380167220](https://gitee.com/kevinyong/kevin-gallery/raw/master/1615380167220.png)

#### (1)stack 和 Align

```dart
child:Stack(
	alignment:Alignment.center,//stack内容居中
    alignment:Alignment(double x,double y),//还可以指定x，y
)
```

Alignment的坐标图

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1083096-bda59650b4ab2e8c.png" alt="img" style="zoom: 33%;" />



由于Stack的Alignment属性是把内部全部元素同时定位，在有多个元素时就变得没效果，所以使用Align组件搭配进行定位

```dart
Align(Alignment alignment,Widget child);
```



#### (2)Stack 和 Positioned

![image-20210610170804069](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610170804069.png)

**由于stackwidget超过三个元素以上就会出现布局问题，所以，超过两个元素的stackwidget建议元素使用positionwidget**

![1615434038914](https://gitee.com/kevinyong/kevin-gallery/raw/master/1615434038914.png)

### 10.CardView

Card是卡片组件块，内容可以由大多数widget组成，Card具有圆角和阴影，这让它看起来有立体感

![image-20210612162022620](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210612162022620.png)

```dart
var card = new Card(
      child: Column(
        children: <Widget>[
          ListTile(
            title: Text("hello", style: TextStyle(fontWeight: FontWeight.bold),),//设置字体样式
            subtitle: Text('电话：1111111111'),
            leading: Icon(Icons.account_box),
          ),
          new Divider(),//分割线
          ListTile(
            title: Text("haode",style: TextStyle(fontWeight: FontWeight.bold),),
            subtitle: Text('hahahahha'),
            leading: Icon(Icons.account_box),
          )
        ],
      ),
    );
```



<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1615436559715.png" alt="1615436559715" style="zoom:67%;" />

```dart
return ListView(
      children:<Widget>[
        Card(
          child: Column(
            children: <Widget>[
              AspectRatio(
                aspectRatio: 3.0/1.0,
                child: Image.network("...",
                fit: BoxFit.cover,)
              ),
              ListTile(
                leading: CircleAvatar(
                  backgroundImage: NetworkImage("..."),
                ),
                title: Text('Loki'
                ),
                subtitle: Text('brother of Thor'),
              )
            ],
          ),
        ),
      ],
    );
```

![image-20210612165018633](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210612165018633.png)

#### CircleAvatar:

```dart
 //为一个专门设置头像的组件
CircleAvatar(
                  backgroundImage: NetworkImage("..."),
                ),
```



### 11.SizeBox

通常用来做widget与widget的间距

```dart
SizeBox(height:..,width:...),
```

### 12.Padding

在flutter中，很多组件不存在padding属性，这时候可以使用Padding组件处理容器与子元素之间的间隔

![image-20210610155939320](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610155939320.png)

```dart
return GridView.count(
	crossAxisCount: 2,
    childAspectRatio:1.7,
    children:<Widget>[
        Padding(
        	padding: EdgeInsets.fromLTRB(),
            child: Image.network('...')
        ),
        Padding(
        	...
        )
    ]

);
```

### 13.Expanded

Expanded组件是flutter中使用率很高的一个组件，它可以动态调整child组件沿主轴的尺寸，比如填充剩余空间，比如设置尺寸比例。它常常和Row或Column组合起来使用。

当row中不加expanded组件时：

![在这里插入图片描述](https://gitee.com/kevinyong/kevin-gallery/raw/master/20200502165044239.png)

当第一个图片加入expanded组建时

![在这里插入图片描述](https://gitee.com/kevinyong/kevin-gallery/raw/master/20200502165153212.png)

```dart
第一个组件使用Expanded包裹后，第一个组件设置的width会无效，并且它在主轴方向变大，直到填充剩余空间，但在纵轴方向上并没有变大，依然维持自身的height。
```



主要属性：

![image-20210610163246086](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610163246086.png)

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610163504260.png" alt="image-20210610163504260" style="zoom:67%;" />

![image-20210610163520450](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210610163520450.png)

### 14.AspectRatio—图片元素适配屏幕

该组件的作用是设置调整子元素的宽高比

```dart
return Container(
      child: AspectRatio(
        aspectRatio: 3.0/1.0,//宽高比：3:1
        child: Container(
          color: Colors.deepPurple,
        ),
      )
    );
```

宽高比：3：1

![image-20210612162312347](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210612162312347.png)

### 15.Wrap组件

​	wrap组件可以实现流布局，单行的wrap跟row表现一致，单列的则跟column表现一致，但是当mianAxis上空间不足时，则向crossAxis上拓展显示

![image-20210612170422252](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210612170422252.png)

### 16.divider

添加一条分割线

```dart
Divider();
```





## 四、路由

Flutter中通过Navigator组件管理路由导航，Flutter中给我们提供了两种配置路由跳转的方法：

- 基本路由
- 命名路由



### (1)基本路由

```dart
ElevatedButton(
    onPressed: (){
        Navigator.of(context).push(//通过Navigator组件
            MaterialPageRoute(
                builder: (context)=>NavigatorPage()
            )
        );
    }, 
    child: Text('路由跳转'))
```

> **传值，可以通过构造函数进行传值**
>
> ```dart
> Navigator.of(context).push(//通过Navigator组件
>     MaterialPageRoute(
>         builder: (context)=>NavigatorPage('hello')
>     )
> );
> ```

跳转到新的页面之后，可以通过顶部appbar的返回按键，或者通过自定义返回

```dart
Navigator.of(context).pop();

//也可以传值
Navigator.pop(context,'hello');
```

### (2)命名路由

MaterialApp里面的属性——routes，可以进行路由的命名

```dart
return MaterialApp(
    home:Tabs(),
    routes:{
        '/form':(context)=>Form(),//对路由进行命名，采用键值对的方式
        '/search':(context)=>Search()
    }
);
```

通过指定名称进行跳转

```dart
Navigator.pushNamed(context,'/form');
```

**命名路由传值**

①使用 `ModalRoute.of()` 

创建实体类

```dart
class ScreenArguments {
  final String title;
  final String message;

  ScreenArguments(this.title, this.message);
}
```

创建提取值的widget

```dart
class ExtractArgumentsScreen extends StatelessWidget {
    static const routeName = '/extractArguments';

    @override
    Widget build(BuildContext context) {
        // Extract the arguments from the current ModalRoute
        // settings and cast them as ScreenArguments.
        final args = ModalRoute.of(context)!.settings.arguments as ScreenArguments;//提取参数，并且与实体类配对

        return Scaffold(
            appBar: AppBar(
                title: Text(args.title),
            ),
            body: Center(
                child: Text(args.message),
            ),
        );
    }
}
```

注册命名路由

```dart
MaterialApp(
  routes: {
    ExtractArgumentsScreen.routeName: (context) => ExtractArgumentsScreen(),
  },
)
```

进行路由跳转

```dart
// A button that navigates to a named route.
// The named route extracts the arguments
// by itself.
ElevatedButton(
  onPressed: () {
    // When the user taps the button,
    // navigate to a named route and
    // provide the arguments as an optional
    // parameter.
    Navigator.pushNamed(
      context,
      ExtractArgumentsScreen.routeName,//路由名称
      arguments: ScreenArguments(//参数
        'Extract Arguments Screen',
        'This message is extracted in the build method.',
      ),
    );
  },
  child: Text('Navigate to screen that extracts arguments'),
),
```

②使用**`onGenerateRoute`**

下述代码还可以进行更改，让其遍历material的routes

```dart
MaterialApp(
 //通过onGenerateRoute属性
  onGenerateRoute: (settings) {
    // If you push the PassArguments route
    if (settings.name == PassArgumentsScreen.routeName) {
      // Cast the arguments to the correct
      // type: ScreenArguments.
      final args = settings.arguments as ScreenArguments;

      // Then, extract the required data from
      // the arguments and pass the data to the
      // correct screen.
      return MaterialPageRoute(
        builder: (context) {
          return PassArgumentsScreen(
            title: args.title,
            message: args.message,
          );
        },
      );
    }
    // The code only supports
    // PassArgumentsScreen.routeName right now.
    // Other values need to be implemented if we
    // add them. The assertion here will help remind
    // us of that higher up in the call stack, since
    // this assertion would otherwise fire somewhere
    // in the framework.
    assert(false, 'Need to implement ${settings.name}');
    return null;
  },
)
```

传值

```dart
ElevatedButton(
              onPressed: () {
                // When the user taps the button, navigate
                // to a named route and provide the arguments
                // as an optional parameter.
                Navigator.pushNamed(
                  context,
                  PassArgumentsScreen.routeName,
                  arguments: ScreenArguments(
                    'Accept Arguments Screen',
                    'This message is extracted in the onGenerateRoute function.',
                  ),
                );
              },
```

#### 对于StatefulWidget的传值：

![image-20210613203522310](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210613203522310.png)

### (3)替换路由

可以将当前页面进行替换，从而实现路由替换，而不是路由跳转新的界面

```dart
Navigator.of(context).pushReplacementNamed('/register');
//或者
Navigator.pushReplacementNamed(context,'/register');
```

这样可以实现多个路由在同一个页面的操作，最后操作完毕调用pop方法直接返回根界面



**此外，如果没有使用替换路由，则可以使用另一种方法返回根**：

```dart
Navigator.pushAndRemoveUntil(context,'/',(Route router)=>false);
//或者
Navigator.of(context).pushAndRemoveUntil(
	new MaterialPageRoute(builder:(context)=>new FirstPage()),(route)=>route == null
);
```

上述方法虽然能够返回，但是不能保证前边widget状态，所以通过以下方式

```dart
Navigator.of(context).popUntil(ModalRoute.withName('/'));
```

### (4)路由进行动画切换

①对于同一个图片，点击以后放大，通过路由跳转

```dart
import 'package:flutter/material.dart';

class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
        child: Image.network(
          'https://picsum.photos/250?image=9',
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: Center(
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}
```

②添加Hero组件，绑定两个屏幕，从而可以实现动画

```dart
class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Main Screen'),
      ),
      body: GestureDetector(
        onTap: () {
          Navigator.push(context, MaterialPageRoute(builder: (_) {
            return DetailScreen();
          }));
        },
        child: Hero(//hero组件
          tag: 'imageHero',
          child: Image.network(
            'https://picsum.photos/250?image=9',
          ),
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          Navigator.pop(context);
        },
        child: Center(
          child: Hero(//hero组件
            tag: 'imageHero',
            child: Image.network(
              'https://picsum.photos/250?image=9',
            ),
          ),
        ),
      ),
    );
  }
}
```

## 五、Button

- RasiedButton(已弃用)，凸起的Button，现已被ElevatedButton取代
- FlatButton(已弃用)，扁平的按钮，被TextButton取代
- **ElevatedButton**
- **IconButton，图标按钮**
- ButtonBar，按钮组
- **TextButton,相当于链接**
- **FloatingActionButton，浮动按钮**

### 1.RasiedButton

![image-20210615185414911](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210615185414911.png)E

### 2.ElevatedButton

构造器

```dart
//普通构造函数
ElevatedButton({Key? key, required VoidCallback? onPressed, VoidCallback? onLongPress, ButtonStyle? style, FocusNode? focusNode, bool autofocus = false, Clip clipBehavior = Clip.none, required Widget? child})

//命名构造函数
ElevatedButton.icon({Key? key, required VoidCallback? onPressed, VoidCallback? onLongPress, ButtonStyle? style, FocusNode? focusNode, bool? autofocus, Clip? clipBehavior, required Widget icon, required Widget label})
//通过icon和label构建一个elevatedbutton
```



常用属性

```dart
//true则按钮默认聚焦
autofocus → bool
//button的child,一般是button中的文字
child → Widget?
//The content will be clipped (or not) according to this option
clipBehavior → Clip
//button是否启用，如果onPressed活着onLongPressed为null，活着enabled=false,则按钮被禁用
enabled → bool
//An optional focus node to use as the focus node for this widget
focusNode → FocusNode?
//hash码
hashCode → int
//Controls how one widget replaces another widget in the tree
key → Key?
//长按按钮的回调
onLongPress → VoidCallback?
//按钮点击回调
onPressed → VoidCallback?

runtimeType → Type
//设置按钮样式
style → ButtonStyle?
```

静态方法

```dart
styleFrom({
    Color? primary, 
    Color? onPrimary, 
    Color? onSurface, 
    Color? shadowColor, 
    double? elevation, 
    TextStyle? textStyle, 
    EdgeInsetsGeometry? padding, 
    Size? minimumSize, 
    Size? fixedSize, 
    BorderSide? side, 
    OutlinedBorder? shape,
    MouseCursor? enabledMouseCursor, 
    MouseCursor? disabledMouseCursor, 
    VisualDensity? visualDensity,
    MaterialTapTargetSize? tapTargetSize, 
    Duration? animationDuration, 
    bool? enableFeedback,
    AlignmentGeometry? alignment, 
    InteractiveInkFeatureFactory? splashFactory}) → ButtonStyle
```

可以通过静态方法来提前为button设置样式

```dart
final ButtonStyle style =
    ElevatedButton.styleFrom(textStyle: const TextStyle(fontSize: 20));

ElevatedButton(
    style: style,//调用样式
    onPressed: null,
    child: const Text('Disabled'),
),
```

可以通过style里的background属性来设置点击以后透明度

```dart
style: ButtonStyle(
    alignment: Alignment.center,
    backgroundColor: MaterialStateProperty.resolveWith<Color>(//监听点击事件，然后切换不透明度
        (Set<MaterialState> states) {
            if (states.contains(MaterialState.pressed))
                return Colors.pink.withOpacity(0.5);
            return Colors.pink; // Use the component's default.
        },
    )
),


//如果只想设置颜色，则
    ElevatedButton(
    style: ButtonStyle(
        backgroundColor: MaterialStateProperty.all(Colors.pink)
    ))
```

### 3.ButtonBar

```dart
//将元素中的button作为一组
ButtonBar(
	RaisedButton(
    	...
    ),
    RaisedButton(
    	.....
    )
)
```

### 4.floatingactionbutton

构造函数

```dart
//构造一个圆形的浮动按钮
FloatingActionButton({Key? key, Widget? child, String? tooltip, Color? foregroundColor, Color? backgroundColor, Color? focusColor, Color? hoverColor, Color? splashColor, Object? heroTag, double? elevation, double? focusElevation, double? hoverElevation, double? highlightElevation, double? disabledElevation, required VoidCallback? onPressed, MouseCursor? mouseCursor, bool mini, ShapeBorder? shape, Clip clipBehavior, FocusNode? focusNode, bool autofocus, MaterialTapTargetSize? materialTapTargetSize, bool isExtended})
//构造一个拓展的按钮，里面可以添加label和icon
FloatingActionButton.extended({Key? key, 
                               String? tooltip, 
                               Color? foregroundColor, 
                               Color? backgroundColor, 
                               Color? focusColor, 
                               Color? hoverColor, 
                               Object? heroTag, 
                               double? elevation, 
                               double? focusElevation, 
                               double? hoverElevation, 
                               Color? splashColor, 
                               double? highlightElevation, 
                               double? disabledElevation,
                               required VoidCallback? onPressed, 
                               MouseCursor? mouseCursor, 
                               ShapeBorder? shape, 
                               bool isExtended,
                               MaterialTapTargetSize? materialTapTargetSize,
                               Clip clipBehavior, 
                               FocusNode? focusNode,
                               bool autofocus, 
                               Widget? icon, //icon
                               required Widget label})//label

```

## 六、flutter中的表单

### (1)TextField

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617140906670.png" alt="image-20210617140906670" style="zoom:67%;" />

```dart
TextField(
              decoration: InputDecoration(
                border: OutlineInputBorder(),//设置边框
                hintText: 'sdsds'//设置占位符
              ),
            ),
            TextField(
              obscureText: true,//密码框
              decoration: InputDecoration(
                border: OutlineInputBorder(),
                labelText: '密码'//输入的时候左上角会有提示
              ),
            )
```

![image-20210617142658654](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617142658654.png)

#### 配合controller来赋初始值

首先初始化controller

```dart
var _username = new TextEditingController();

@override
void initState(){
    super.initState();
    _username.text='初始值';
}


/*然后在textfield的controller属性里面调用controller*/
TextField(
controller:_username
)
```

**此时controller已经和textfield形成双向绑定，所以要获取文本框内容，直接获取_username的值即可**

```dart
ElevatedButton(
                  onPressed: (){
                    print("value:"+_username.value.text);//获取值
                  },
                  child: Text('登录'))
```

### (2)CheckBox（多选框）

![image-20210617144945605](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617144945605.png)

checkbox初始是没有状态的，所以需要给他进行动态赋值状态

```dart
class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  bool isChecked = false;//设置选中状态

  @override
  Widget build(BuildContext context) {
    return Checkbox(
      checkColor: Colors.white,//当选中是里面图标颜色
      value: isChecked,//默认选中状态
      onChanged: (value) {//进行监听
        setState(() {
          isChecked = value!;//更新状态
        });
      },
    );
  }
}

```



#### checkBoxListTile

**不能放在Row里面**

![image-20210617144924618](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617144924618.png)

![image-20210617151543030](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617151543030.png)

### (3)Radio（单选框）

```dart
var _sex = 1;

Radio(
    value: 1,
    groupValue: this._sex,//规定组
    onChanged: (v){
        setState(() {
            this._sex = v;
        });

    }),
Radio(
    value: 2,
    groupValue: this._sex,//规定组
    onChanged: (v){
        setState(() {
            this._sex = v;
        });

    }),
```

![image-20210617155458065](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617155458065.png)

#### radioListTile

![image-20210617155718686](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617155718686.png)

**selected属性**

```dart
selected:this.sex==1//当选中该radio的时候，整个tile将会被高亮。
```

![image-20210617160125483](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617160125483.png)

### (4)switch

```dart
Switch(
	value:this.flag,
    onChanged:(v){
        setState((){
            print(v),
            this.flag = v;
        })
    }
)
```

![image-20210617160445999](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617160445999.png)

## 七、日期和时间戳

```dart
var time = new DateTime.now();//获取当前时间
print(time.millsecondsSinceEpoch);//转换为时间戳，13位，ms为单位

//时间戳转换为时间
var a = time.millisecondsSinceEpoch;
print(DateTime.fromMillisecondsSinceEpoch(a));
```

### (1)dataformat三方库

```dart
https://pub.dev/packages/date_format
```

```yaml
dependencies:
	date_format: ^2.0.2
```

详情参考文档

```dart
https://pub.dev/packages/date_format
```

### (2)制作一个下拉时间组件

```dart
InkWell(//inkwell是一个点击以后会类似于墨水阴影的样式
    child:Row(
        children: <Widget>[
            Text('2021-6-17'),
            Icon(Icons.arrow_drop_down)
        ],
    ),
    onTap: (){

    },
```

![image-20210617165842699](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617165842699.png)

##### 配置日期选项表

```dart
var _nowDate = DateTime.now();
//封装函数
_showDatePicker(){
    showDatePicker(context: context, initialDate: _nowDate, firstDate: DateTime(1999), lastDate: DateTime(2022));
  }

//inkwell
onTap: _showDatePicker
```

点击之后

![image-20210617170746983](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617170746983.png)

因为showDatePicker是一个future类型的函数，所以可以获取结果

```dart
showDatePicker(context: context, initialDate: _nowDate, firstDate: DateTime(1999), lastDate: DateTime(2022)).then((value) => print(value));
```

**也可以用async+await**

```dart
_showDatePicker() async{
    var result = await showDatePicker(context: context, initialDate: _nowDate, firstDate: DateTime(1999), lastDate: DateTime(2022));
    print(result);
  }
```

##### 配置时间选项表

```dart
_showTimePicker(){
    showTimePicker(context: context, initialTime: TimeOfDay(hour: 15, minute: 30)).then((value) => print(value));
  }
```

![image-20210617173225461](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617173225461.png)

##### 配置flutter国际化库

1、配置flutter_localizations依赖

 找到pubspec.yaml配置flutter_localizations

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter
```

2、导入国际化的包 flutter_localizations

```dart
import 'package:flutter_localizations/flutter_localizations.dart'; 
```

3、设置国际化

```dart
void main() {
  runApp(
    new MaterialApp(
      title: 'app',
      theme: new ThemeData(
        primaryColor: Colors.white,
      ),
      home: new MyLoginWidget(),
      localizationsDelegates: [
        //此处
        GlobalMaterialLocalizations.delegate,
        GlobalWidgetsLocalizations.delegate,
      ],
      supportedLocales: [
        //此处
        const Locale('zh', 'CH'),
        const Locale('en', 'US'),
      ],
    ),
  );
}
```

4、要显示中文的控件设置：

```dart
_showDatePicker() async{
    var date =await showDatePicker(
      context: context,
      initialDate: _datetime,
      firstDate:DateTime(1900),
      lastDate:DateTime(2050),
      locale: Locale('zh'),    
    );
    if(date==null) return;
    print(date);
    setState(() {
       _datetime=date;
    });
  }
```

![image-20210617175156389](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617175156389.png)

### (3)三方组件时间选择表

```dart
https://pub.dev/packages/flutter_cupertino_date_picker/install
```

导入依赖

```yaml
dependencies:
  flutter_cupertino_date_picker: ^1.0.26+2
```

引入库

```dart
import 'package:flutter_cupertino_date_picker/flutter_cupertino_date_picker.dart';
 
```

应用代码：

```dart
const String MIN_DATETIME = '2010-05-12';
const String MAX_DATETIME = '2021-11-25';
const String INIT_DATETIME = '2019-05-17';

/// Display date picker.
  void _showDatePicker() {
    DatePicker.showDatePicker(
      context,
      onMonthChangeStartWithFirstDate: true,
      pickerTheme: DateTimePickerTheme(
        showTitle: true,
        confirm: Text('确定', style: TextStyle(color: Colors.red)),
        cancel: Text('取消',style: TextStyle(color: Colors.cyan))
      ),
      minDateTime: DateTime.parse(MIN_DATETIME),
      maxDateTime: DateTime.parse(MAX_DATETIME),
      initialDateTime: DateTime.now(),
      dateFormat: "yyyy-MMMM-dd",
      locale: DateTimePickerLocale.zh_cn,
      onClose: () => print("----- onClose -----"),
      onCancel: () => print('onCancel'),
      onChange: (dateTime, List<int> index) {
        setState(() {
          _dateTime = dateTime;
        });
      },
      onConfirm: (dateTime, List<int> index) {
        setState(() {
          _dateTime = dateTime;
        });
      },
    );
  }
```



![image-20210617185628801](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210617185628801.png)

## 八、flutter_swiper轮播图

依赖

```yaml
dependencies:
  flutter_swiper: ^1.1.6
```

导包

```dart
import 'package:flutter_swiper/flutter_swiper.dart';
```

使用

```dart
var _pictureList = [
    {
  	  'URL':'...'
    },
    {
      'URL':'...'
    },
    {
      'URL':'...'
    }
  ];


new Swiper(
        itemBuilder: (BuildContext context,int index){
          return new Image.network(_pictureList[index]['URL'],fit: BoxFit.fill,);//通过index来遍历
        },
        itemCount: _pictureList.length,//长度为_picturelist的元素个数
        pagination: new SwiperPagination(),//配置分页器
        control: new SwiperControl(),//配置左右箭头
      ),
```

更多参考文档

```dart
https://pub.dev/packages/flutter_swiper
```

## 九、dialog

### (1)AlertDialog

需要在showDialog中返回一个alertdialog

```dart
showDialog/*<String>*/(
        context: context,
        builder: (context) => AlertDialog(
          title: const Text('AlertDialog Title'),
          content: const Text('AlertDialog description'),
          actions: <Widget>[//按钮
            TextButton(
              onPressed: () => Navigator.pop(context, 'Cancel'),
              child: const Text('Cancel'),
            ),
            TextButton(
              onPressed: () => Navigator.pop(context, 'OK'),
              child: const Text('OK'),
            ),
          ],
        ),
    
    
    /*也可以在外部定义异步方法*/
    _showDialog() async{
        var result = await showDialog(//等待执行结束，并且获取结果
        context: context,
        builder: (context) => AlertDialog(
          title: const Text('AlertDialog Title'),
          content: const Text('AlertDialog description'),
          actions: <Widget>[//按钮
            TextButton(
              onPressed: () => Navigator.pop(context, 'Cancel'),
              child: const Text('Cancel'),
            ),
            TextButton(
              onPressed: () => Navigator.pop(context, 'OK'),
              child: const Text('OK'),
            ),
          ]
        );
    }
            
   /*也可以通过switch-case来进行跳转*/
```

![image-20210620115716204](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210620115716204.png)

### (2)SimpleDialog

与alertdialog相似，都是在showDialog里面返回simpledialog类型

```dart
showDialog<Department>(
    context: context,
    builder: (BuildContext context) {
      return SimpleDialog(
        title: const Text('Select assignment'),
        children: <Widget>[
          SimpleDialogOption(
            onPressed: () { Navigator.pop(context, Department.treasury); },
            child: const Text('Treasury department'),
          ),
          SimpleDialogOption(
            onPressed: () { Navigator.pop(context, Department.state); },
            child: const Text('State department'),
          ),
        ],
      );
    }
  )
```

![image-20210620121247204](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210620121247204.png)

```dart
/*可以通过在switch-case里面进行点击事件的选择*/
Future<void> _askedToLead() async {
  switch (await showDialog<Department>(
    context: context,
    builder: (BuildContext context) {
      return SimpleDialog(
        title: const Text('Select assignment'),
        children: <Widget>[
          SimpleDialogOption(
            onPressed: () { Navigator.pop(context, Department.treasury); },
            child: const Text('Treasury department'),
          ),
          SimpleDialogOption(
            onPressed: () { Navigator.pop(context, Department.state); },
            child: const Text('State department'),
          ),
        ],
      );
    }
  )) {
    case Department.treasury:
      // Let's go.
      // ...
    break;
    case Department.state:
      // ...
    break;
    case null:
      // dialog dismissed
    break;
  }
}
```

### (3)showModalBottomSheet

```dart
showModalBottomSheet<void>(
          context: context,
          builder: (BuildContext context) {
            return Container(
              ...
              child: Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  mainAxisSize: MainAxisSize.min,
                  children: <Widget>[
                    const Text('Modal BottomSheet'),
                    ElevatedButton(
                      child: const Text('Close BottomSheet'),
                      onPressed: () => Navigator.pop(context),
                    )
                  ],
                ),
              ),
            );
          },
        );
```

### (4)Toast

依赖

```yaml
fluttertoast: ^8.0.7
```

导包

```dart
import 'package:fluttertoast/fluttertoast.dart';
```

使用

```dart
Fluttertoast.showToast(
        msg: "This is Center Short Toast",
        toastLength: Toast.LENGTH_SHORT,
        gravity: ToastGravity.CENTER,
        timeInSecForIosWeb: 1,
        backgroundColor: Colors.red,
        textColor: Colors.white,
        fontSize: 16.0
    );
```

### (5)自定义dialog

继承dialog

```dart
class MannualDiaglog extends Dialog{
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Material(
      type: MaterialType.transparency,//设置为透明
      child: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Padding(
              padding: EdgeInsets.all(10),
              child: Container(
                height: 200,
                width: 200,
                color: Colors.white,
              ),
            )
          ],
        ),
      ),
    );
  }
}
```

调用

```dart
ElevatedButton(
    onPressed: (){
        showDialog(
            context: context,
            builder: (context)=> MannualDiaglog());
    },
    child: Text('登录')
),
```

![image-20210620151808446](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210620151808446.png)

然后自己在dialog里面添加组件布局



**定时器（可以结合dialog自动关闭）,但是有些小问题（如果自动关闭前手动关闭会报错）**

![image-20210620145315581](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210620145315581.png)

## 十、网络请求

### 	(1)http库

依赖

```yaml
dependencies:
  http: ^0.13.3
```

导入

```dart
import 'package:http/http.dart';
```

使用：

**最简单的一次性请求**

```dart
import 'package:http/http.dart' as http;

//一次性请求
var url = Uri.parse('https://example.com/whatsit/create');//解析地址
var response = await http.post(url, body: {'name': 'doodle', 'color': 'blue'});//请求数据，post
print('Response status: ${response.statusCode}');
print('Response body: ${response.body}');

print(await http.read('https://example.com/foobar.txt'));
```

**如果要多次访问服务器可以通过client代理，最后关闭client即可**

```dart
var client = http.Client();
try {
    var uriResponse = await client.post(Uri.parse('https://example.com/whatsit/create'),//post请求
                                        body: {'name': 'doodle', 'color': 'blue'});
    print(await client.get(uriResponse.bodyFields['uri']));//get请求
} finally {
    client.close();
}
```

**还可以封装一个请求的类：**

可以通过自己创建[Request](https://pub.dev/documentation/http/latest/http/Request-class.html)或[StreamedRequest](https://pub.dev/documentation/http/latest/http/StreamedRequest-class.html)对象并将它们传递给[Client.send](https://pub.dev/documentation/http/latest/http/Client/send.html)来 对您的请求和响应进行更细粒度的控制。

```dart
class UserAgentClient extends http.BaseClient {
  final String userAgent;
  final http.Client _inner;

  UserAgentClient(this.userAgent, this._inner);

  Future<http.StreamedResponse> send(http.BaseRequest request) {
    request.headers['user-agent'] = userAgent;
    return _inner.send(request);
  }
}
```

#### 请求重试：

默认情况下，这会重试任何响应状态代码为 503 Temporary Failure 的请求，最多重试 3 次。它在第一次重试前等待 500 毫秒，每次将延迟增加 1.5 倍。

```dart
import 'package:http/http.dart' as http;
import 'package:http/retry.dart';

Future<void> main() async {
  final client = RetryClient(http.Client());
  try {
    print(await client.read('http://example.org'));
  } finally {
    client.close();
  }
}
```

### (2)dio

dio是一个强大的网络请求库，支持拦截器，RestfulApi，FormData，请求取消，Cookie管理，文件上传/下载，超时，自定义适配器等等

```dart
https://pub.dev/packages/dio
```

依赖

```yaml
dependencies:
  dio: ^4.0.0
```

#### 使用

get请求

```dart
import 'package:dio/dio.dart';
void getHttp() async {
  try {
    var response = await Dio().get('http://www.google.com');
    print(response);
  } catch (e) {
    print(e);
  }
}

//带参数
var dio = Dio();
response = await dio.get('/test', queryParameters: {'id': 12, 'name': 'wendu'});
print(response.data.toString());
```

post请求：

```dart
response = await dio.post('/test', data: {'id': 12, 'name': 'wendu'});
```

多个请求

```dart
response = await Future.wait([dio.post('/info'), dio.get('/token')]);
```

下载文件

```dart
response = await dio.download('https://www.google.com/', './xx.html');
```

发送表单

```dart
var formData = FormData.fromMap({
  'name': 'wendux',
  'age': 25,
});
var response = await dio.post('/info', data: formData);
```

监听上传进度

```dart
response = await dio.post(
  'http://www.dtworkroom.com/doris/1/2.0.0/test',
  data: {'aa': 'bb' * 22},
  onSendProgress: (int sent, int total) {
    print('$sent $total');
  },
);
```

#### 配置

配置可以使用BaseOptions、Options、RequestOptions优先级别递增，且可以根据优先级别覆盖参数（所以我们可以在设计两个及以上域名地址的时候通过覆盖来动态更换URL）

```dart 
BaseOptions 基类请求配置
Options	单次请求配置
RequestOptions 实际请求配置
```



```dart
var dio = Dio(); // with default Options

// 使用默认配置方式
dio.options.baseUrl = 'https://www.xx.com/api';
dio.options.connectTimeout = 5000; //5s
dio.options.receiveTimeout = 3000;

// 通过BaseOptions实例来配置.
var options = BaseOptions(
  baseUrl: 'https://www.xx.com/api',
  connectTimeout: 5000,
  receiveTimeout: 3000,
);
Dio dio = Dio(options);

//请求
response = await dio.request(
  '/test',
  data: {'id':12,'name':'xx'},
  options: Options(method:'GET'),
);
```

可以通过Options类来配置dio的选项

```dart
{
  /// Http method.
  String method;

  /// Request base url, it can contain sub path, like: 'https://www.google.com/api/'.
  String baseUrl;

  /// Http request headers.
  Map<String, dynamic> headers;

   /// Timeout in milliseconds for opening  url.
  int connectTimeout;

   ///  Whenever more than [receiveTimeout] (in milliseconds) passes between two events from response stream,
  ///  [Dio] will throw the [DioError] with [DioErrorType.RECEIVE_TIMEOUT].
  ///  Note: This is not the receiving time limitation.
  int receiveTimeout;

  /// Request data, can be any type.
  T data;

  /// If the `path` starts with 'http(s)', the `baseURL` will be ignored, otherwise,
  /// it will be combined and then resolved with the baseUrl.
  String path='';

  /// The request Content-Type. The default value is 'application/json; charset=utf-8'.
  /// If you want to encode request body with 'application/x-www-form-urlencoded',
  /// you can set [Headers.formUrlEncodedContentType], and [Dio]
  /// will automatically encode the request body.
  String contentType;

  /// [responseType] indicates the type of data that the server will respond with
  /// options which defined in [ResponseType] are `JSON`, `STREAM`, `PLAIN`.
  ///
  /// The default value is `JSON`, dio will parse response string to json object automatically
  /// when the content-type of response is 'application/json'.
  ///
  /// If you want to receive response data with binary bytes, for example,
  /// downloading a image, use `STREAM`.
  ///
  /// If you want to receive the response data with String, use `PLAIN`.
  ResponseType responseType;

  /// `validateStatus` defines whether the request is successful for a given
  /// HTTP response status code. If `validateStatus` returns `true` ,
  /// the request will be perceived as successful; otherwise, considered as failed.
  ValidateStatus validateStatus;

  /// Custom field that you can retrieve it later in [Interceptor]、[Transformer] and the   [Response] object.
  Map<String, dynamic> extra;
  
  /// Common query parameters
  Map<String, dynamic /*String|Iterable<String>*/ > queryParameters;  
  
   /// [collectionFormat] indicates the format of collection data in request
  /// options which defined in [CollectionFormat] are `csv`, `ssv`, `tsv`, `pipes`, `multi`,`multiCompatible`.
  /// The default value is `multiCompatible`
  late CollectionFormat collectionFormat;

}
```

**使用 application/x-www-form-urlencoded 格式**

默认情况下，Dio 将请求数据（String 类型除外）序列化为`JSON`. 要以该`application/x-www-form-urlencoded`格式发送数据，您可以：

```dart
//Instance level
dio.options.contentType= Headers.formUrlEncodedContentType;
//or works once
dio.post(
  '/info',
  data: {'id': 5},
  options: Options(contentType: Headers.formUrlEncodedContentType),
);
```

**发送 FormData** 

你也可以用Dio发送FormData，它会在.中发送数据`multipart/form-data`，并且支持上传文件。

```dart
var formData = FormData.fromMap({
  'name': 'wendux',
  'age': 25,
  'file': await MultipartFile.fromFile('./text.txt',filename: 'upload.txt')//上传文件信息
});
response = await dio.post('/info', data: formData);
```

**多个文件上传**

添加多个文件有两种方式`FormData`，唯一的区别是数组类型的上传key不同。

```dart
FormData.fromMap({
  'files': [
    MultipartFile.fromFileSync('./example/upload.txt', filename: 'upload.txt'),
    MultipartFile.fromFileSync('./example/upload.txt', filename: 'upload.txt'),
  ]
});

//最终key变为'files[]'
```

上传的**key最终变成了'files[]'**，这是因为很多后端服务在获取文件数组的时候都会给key加一个中括号。**如果你不想要 "[]"**，你应该创建 FormData 如下（不要使用`FormData.fromMap`）：

```dart
var formData = FormData();
formData.files.addAll([
  MapEntry('files',
    MultipartFile.fromFileSync('./example/upload.txt',filename: 'upload.txt'),
  ),
  MapEntry('files',
    MultipartFile.fromFileSync('./example/upload.txt',filename: 'upload.txt'),
  ),
]);
```

#### 拦截器

```dart
dio.interceptors.add(InterceptorsWrapper(
    onRequest:(options, handler){
     // 在请求发送之前进行的事
     return handler.next(options); //continue，开始请求
    },
    onResponse:(response,handler) {
     //在响应之前做事
     return handler.next(response); // continue，开始响应
    },
    onError: (DioError e, handler) {
     // Do something with response error
     return  handler.next(e);//continue
    }
));
```

##### 解决并拒绝请求

在所有拦截器中，您都可以干扰它们的执行流程。如果你想用一些自定义数据来解析请求/响应，你可以返回一个`Response`对象或返回`dio.resolve(data)`. 如果您想拒绝带有错误消息的请求/响应，您可以返回一个`DioError`对象或 return `dio.reject(errMsg)`。

```dart
dio.interceptors.add(InterceptorsWrapper(
  onRequest:(options, handler) {
   return handler.resolve(Response(requestOptions:options,data:'fake data'));
  },
));
Response response = await dio.get('/test');
print(response.data);//'fake data'
```



##### 锁定/解锁拦截器

您可以通过调用拦截器的`lock()`/`unlock`方法来锁定/解锁拦截器。一旦请求/响应拦截器被锁定，传入的请求/响应将在进入拦截器之前被添加到队列中，直到拦截器解锁才会继续。

**例如：**

出于安全考虑，我们需要所有的请求在header中设置一个csrfToken，如果csrfToken不存在，我们需要先请求一个csrfToken，然后再进行网络请求，**因为请求csrfToken的进度是异步的，所以我们需要在请求拦截器中执行这个异步请求**。代码如下：

```dart
dio.interceptors.add(InterceptorsWrapper(
  onRequest: (Options options, handler) async {
    print('send request：path:${options.path}，baseURL:${options.baseUrl}');
    if (csrfToken == null) {//没有token
      print('no token，request token firstly...');
      //lock the dio.
      dio.lock();
      tokenDio.get('/token').then((d) {
        options.headers['csrfToken'] = csrfToken = d.data['data']['token'];
        print('request token succeed, value: ' + d.data['data']['token']);
        print( 'continue to perform request：path:${options.path}，baseURL:${options.path}');
        handler.next(options);
      }).catchError((error, stackTrace) {
        handler.reject(error, true);
      }) .whenComplete(() => dio.unlock()); // unlock the dio
    } else {
      options.headers['csrfToken'] = csrfToken;
      handler.next(options);
    }
  }
));
```

**可以通过调用清理等待队列`clear()`；**

#### 封装：

**创建一个HTTPutil基类**

```dart
class HttpUtil {
    static HttpUtil instance;
    Dio dio;
    BaseOptions options;

    CancelToken cancelToken = new CancelToken();

    static HttpUtil getInstance() {
        if (null == instance) instance = new HttpUtil();
        return instance;
    }


    /*
   * config it and create
   */
    HttpUtil() {
        //BaseOptions、Options、RequestOptions 都可以配置参数，优先级别依次递增，且可以根据优先级别覆盖参数
        options = new BaseOptions(
            //请求基地址,可以包含子路径
            baseUrl: "http://www.google.com",
            //连接服务器超时时间，单位是毫秒.
            connectTimeout: 10000,
            //响应流上前后两次接受到数据的间隔，单位为毫秒。
            receiveTimeout: 5000,
            //Http请求头.
            headers: {
                //do something
                "version": "1.0.0"
            },
            //请求的Content-Type，默认值是"application/json; charset=utf-8",Headers.formUrlEncodedContentType会自动编码请求体.
            contentType: Headers.formUrlEncodedContentType,
            //表示期望以那种格式(方式)接受响应数据。接受4种类型 `json`, `stream`, `plain`, `bytes`. 默认值是 `json`,
            responseType: ResponseType.json,
        );

        dio = new Dio(options);
    }


}
```

**get请求**

```dart
/*
   * get请求
   */
get(url, {data, options, cancelToken}) async {
    Response response;
    try {
        response = await dio.get(url, queryParameters: data, options: options, cancelToken: cancelToken);
        print('get success---------${response.statusCode}');
        print('get success---------${response.data}');

        //      response.data; 响应体
        //      response.headers; 响应头
        //      response.request; 请求体
        //      response.statusCode; 状态码

    } on DioError catch (e) {
        print('get error---------$e');
        formatError(e);
    }
    return response.data;
}
```

```dart
url:请求地址
data：请求参数
options：请求配置
cancelToken:取消标识
```



**Post请求**

```dart
  /*
   * post请求
   */
  post(url, {data, options, cancelToken}) async {
    Response response;
    try {
      response = await dio.post(url, queryParameters: data, options: options, cancelToken: cancelToken);
      print('post success---------${response.data}');
    } on DioError catch (e) {
      print('post error---------$e');
      formatError(e);
    }
    return response.data;
  }
```

**异常处理**

```dart
  /*
   * error统一处理
   */
  void formatError(DioError e) {
    if (e.type == DioErrorType.CONNECT_TIMEOUT) {
      // It occurs when url is opened timeout.
      print("连接超时");
    } else if (e.type == DioErrorType.SEND_TIMEOUT) {
      // It occurs when url is sent timeout.
      print("请求超时");
    } else if (e.type == DioErrorType.RECEIVE_TIMEOUT) {
      //It occurs when receiving timeout
      print("响应超时");
    } else if (e.type == DioErrorType.RESPONSE) {
      // When the server response, but with a incorrect status, such as 404, 503...
      print("出现异常");
    } else if (e.type == DioErrorType.CANCEL) {
      // When the request is cancelled, dio will throw a error with this type.
      print("请求取消");
    } else {
      //DEFAULT Default error type, Some other Error. In this case, you can read the DioError.error if it is not null.
      print("未知错误");
    }
  }
```

取消请求

```dart
  /*
   * 取消请求
   * 同一个cancel token 可以用于多个请求，当一个cancel token取消时，所有使用该cancel token的请求都会被取消。
   * 所以参数可选
   */
  void cancelRequests(CancelToken token) {
    token.cancel("cancelled");
  }
```

#### cookie_manager

```yaml
dependencies:
  dio_cookie_manager: ^2.0.0  #latest version
```

```dart
import 'package:cookie_jar/cookie_jar.dart';
```

简单使用

```dart
import 'package:dio/dio.dart';
import 'package:dio_cookie_manager/dio_cookie_manager.dart';
import 'package:cookie_jar/cookie_jar.dart';

main() async {
  var dio =  Dio();
  var cookieJar=CookieJar();
  dio.interceptors.add(CookieManager(cookieJar));
  
  // 第一次请求，cookiemanager进行cookie保存
  await dio.get("https://baidu.com/");
  
  // Print cookies
  print(await cookieJar.loadForRequest(Uri.parse("https://baidu.com/")));

  // 第二次带cookie请求
  await dio.get("https://baidu.com/");
  ... 
}
```

可以通过创建CookieJar` or `PersistCookieJar实例进行cookie管理

- CookieJar——将cookie保存在RAM
- PersistCookieJar——将cookie保存在文件

```dart
dio.interceptors.add(CookieManager(PersistCookieJar()))
```

> Note:在flutter里面，  `PersistCookieJar` 的路径必须合法化(存在在手机且可写). 可以使用 [path_provider](https://pub.dartlang.org/packages/path_provider) 来获取正确的路径

```dart
//path_provider依赖
dependencies:
  path_provider: ^2.0.2

import 'package:path_provider/path_provider.dart';
Directory appDocDir = await getApplicationDocumentsDirectory();
String appDocPath = appDocDir.path;

var cj = PersistCookieJar(ignoreExpires: true, storage: FileStorage(appDocPath +"/.cookies/" ));
dio.interceptors.add(CookieManager(cj));
...
```





## 十一、状态管理

### GETX

> https://juejin.cn/post/7020598013986865182




