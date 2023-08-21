# Flutter学习--3
@[toc]
## 一、组件

### 1.AzListView



#### (1)概述

- 轻松创建城市列表或联系人列表界面。
- 列表项可以按 AZ 分组。
- 带有浮动选项的粘性标题。
- 支持自定义标题。
- 支持索引联动。
- IndexBar 支持自定义样式。
- IndexBar 支持本地图像。
- 允许滚动到列表中的特定项目。



#### (2)使用

```yaml
dependencies:
  azlistview: ^1.1.1
```

![image-20210719180948850](https://img-blog.csdnimg.cn/img_convert/f7472548ed51c55bdcfc8464abaf3f01.png)

```dart
AzListView(
    data: dataList,
    itemCount: dataList.length,
    physics: BouncingScrollPhysics(),
    itemBuilder: (_,index)=>item(index),
    susItemBuilder: (_,index)=>susItem(index),
    indexBarOptions: IndexBarOptions(
        needRebuild: true,
        ignoreDragCancel: true,
        downTextStyle: TextStyle(fontSize: 12, color: Colors.white),
        downItemDecoration://装饰右侧字母栏
        	BoxDecoration(shape: BoxShape.circle, color: Colors.green),
        indexHintWidth: 27,//右侧字母栏点击之后出现的box的宽度
        indexHintHeight: 27,
        indexHintTextStyle: TextStyle(fontSize: 13,fontWeight: FontWeight.w500,color: Colors.white),
        indexHintDecoration: BoxDecoration(//装饰右侧字母点击之后的box
            shape: BoxShape.rectangle,
            borderRadius: BorderRadius.circular(7),
            color: HexColor("#3B45BC"),
        ),
        indexHintAlignment: Alignment.centerRight,//右侧字母点击之后字母框的位置
        indexHintChildAlignment: Alignment(-0.0, 0.0),//右侧字母框中子组件的偏移量
        indexHintOffset: Offset(-15, 0),//右侧字母框中的偏移量
    ),
)
```

构建item

```dart
/*加载每条数据*/
  Widget item(int index) {
      return Container(
          child: Row(
              children: <Widget>[
                  Padding(
                      padding: EdgeInsets.only(left:29,top: 8,bottom: 8),
                      child: Text('${dataList[index].studentName}',style: TextStyle(color: HexColor('#999999'),fontWeight: FontWeight.w400),),
                  ),
                  Padding(
                      padding: dataList[index].studentName.length==3?EdgeInsets.only(left: 70):EdgeInsets.only(left: 84),
                      child: Text('${dataList[index].phone}',style: TextStyle(color: HexColor('#333333'),fontWeight: FontWeight.w400),)
                  ),
                  Padding(
                      padding: EdgeInsets.only(left: 84),
                      child: IconButton(icon:Icon(Icons.phone),color: HexColor('#5434A3'),iconSize: 15,onPressed: (){
                          //do somthing
                      })
                  )
              ],
          ),
      );
  }
```

排序的字母表

```dart
/*名字首字母排序*/
  Widget susItem(int index) {
    return Container(
      height: 35,
      width: MediaQuery.of(context).size.width,
      padding: EdgeInsets.only(left: 16),
      color: Color(0xFFF3F4F5),
      alignment: Alignment.centerLeft,
      child: Text(
        '${dataList[index].tagIndex}',
        softWrap: false,
        style: TextStyle(
          fontSize: 14.0,
          color: Color(0xFF666666),
        ),
      ),
    );
  }
```

![image-20210719181348351](https://img-blog.csdnimg.cn/img_convert/d7be95795f4b6b62aa25f66ad9e684ac.png)

将每条记录的首字母进行分类以便查找

```dart
/*将每条记录的姓名首字母拼音进行配对，以便右方字母快速搜索*/
  void handleList(List<DataBean> list) {
    if (list.isEmpty) return;
    for (int i = 0, length = list.length; i < length; i++) {
      String pinyin = PinyinHelper.getPinyinE(list[i].studentName);
      String tag = pinyin.substring(0, 1).toUpperCase();
      list[i].namePingyin = pinyin;
      if (RegExp("[A-Z]").hasMatch(tag)) {
        list[i].tagIndex = tag;
      } else {
        list[i].tagIndex = "#";
      }
    }
    // A-Z sort.
    SuspensionUtil.sortListBySuspensionTag(dataList);//根据tag从A-Z排序
    // show sus tag.
    SuspensionUtil.setShowSuspensionStatus(dataList);//设置显示悬停Header状态
  }
```

调用

```dart
void loadData() async {
    //加载联系人列表
    rootBundle.loadString('assets/data/car_models.json').then((value) {
      List list = json.decode(value);
      list.forEach((v) {
        contactList.add(ContactInfo.fromJson(v));
      });
      _handleList(contactList);//对于数据要调用处理拼音排序方法
    });
  }
```





上述代码运用到Flutter的汉字转拼音插件

```yaml
dependencies:
  lpinyin: ^2.0.2  #latest version
```

使用

```dart
import 'package:lpinyin/lpinyin.dart';

String text = "天府广场";

//字符串拼音首字符
PinyinHelper.getShortPinyin(str); // tfgc

//字符串首字拼音
PinyinHelper.getFirstWordPinyin(str); // tian

//无法转换拼音会 throw PinyinException
PinyinHelper.getPinyin(text);
PinyinHelper.getPinyin(text, separator: " ", format: PinyinFormat.WITHOUT_TONE);//tian fu guang chang

//无法转换拼音 默认用' '替代
PinyinHelper.getPinyinE(text);
PinyinHelper.getPinyinE(text, separator: " ", defPinyin: '#', format: PinyinFormat.WITHOUT_TONE);//tian fu guang chang

//添加用户自定义字典
List<String> dict1 = ['耀=yào','老=lǎo'];
PinyinHelper.addPinyinDict(dict1);//拼音字典
List<String> dict2 = ['奇偶=jī,ǒu','成都=chéng,dū'];
PinyinHelper.addMultiPinyinDict(dict2);//多音字词组字典
List<String> dict3 = ['倆=俩','們=们'];
ChineseHelper.addChineseDict(dict3);//繁体字字典
```

### 2.PageView

#### (1)概述

pageView控件的作用和android的ViewPager类似，能够做轮播图，当然也经常和bottomNavigationbar以及TabBar混合使用

#### (2)使用

构造方法

```dart
PageView({Key? key, 
          Axis scrollDirection,//滑动方向 
          bool reverse, //page滚动方向是否相反，false默认，true取反
          PageController? controller, //用于控制pageview
          ScrollPhysics? physics, //滑动的方式:BouncingScrollPhysics 阻尼效果;ClampingScrollPhysics 水波纹效果
          bool pageSnapping,//是否具有回弹效果，默认为true 
          ValueChanged<int>? onPageChanged, //监听页面是否改变                  
          List<Widget> children = const <Widget>[],           DragStartBehavior dragStartBehavior, 
          bool allowImplicitScrolling, 
          String? restorationId, 
          Clip clipBehavior, //裁剪children
          ScrollBehavior? scrollBehavior})
```



##### 结合底部导航栏

```dart
class PageViewDemo extends StatefulWidget {
  @override
  State<StatefulWidget> createState() => _PageViewDemoState();
}

class _PageViewDemoState extends State<PageViewDemo>
    with SingleTickerProviderStateMixin {
  TabController _tabController;
  PageController _pageController = PageController();
  List<String> titles = ["首页", "账单", "我的"];

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: titles.length, vsync: this);
  }

  @override
  void dispose() {
    super.dispose();
    _tabController.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // 底部
      bottomNavigationBar: Material(
        color: Colors.white,
        child: TabBar(tabs: <Tab>[
          Tab(
            text: titles[0],
          ),
          Tab(
            text: titles[1],
          ),
          Tab(
            text: titles[2],
          ),
        ],
          indicatorColor: Colors.blue,
          indicatorWeight: 5.0,
          controller: _tabController,
          labelColor: Colors.red,
          unselectedLabelColor: Colors.grey,
          onTap: (index){
            _pageController.jumpToPage(index);//跳到对应的页面
          },
        ),
      ),
      body: PageView(
        controller: _pageController,
        children: <Widget>[
          Text(titles[0]),
          Text(titles[1]),
          Text(titles[2])

        ],
        onPageChanged: (index){
          _tabController.animateTo(index);//tabbar选中状态
        },
      ),
    );
  }
}

```

```dart
class TopPageViewDemo extends StatefulWidget{
  @override
  State<StatefulWidget> createState() => TopPageViewDemoState();
}

class TopPageViewDemoState extends State<TopPageViewDemo> with SingleTickerProviderStateMixin{
  TabController _tabController;
  PageController _pageController = PageController();
  List<String> titles = ["首页", "我的"];

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: titles.length, vsync: this);
  }

  @override
  void dispose() {
    super.dispose();
    _tabController.dispose();
  }


  @override
  Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: Text("AppBarTitle",style: TextStyle(color: Colors.black),),
          backgroundColor: Colors.white,
          bottom: TabBar(tabs: <Tab>[
            Tab(
              text: titles[0],
            ),
            Tab(
              text: titles[1],
            ),
          ],
          controller: _tabController,
          indicatorColor: Colors.red,
          labelColor: Colors.black,
          unselectedLabelColor: Colors.grey,
          ),
        ),
        body: PageView(
          controller: _pageController,
          children: <Widget>[
            Text(titles[0]),
            Text(titles[1]),
          ],
          onPageChanged: (index){
            _tabController.animateTo(index);
          },
        ),
      );
  }
}
```

##### 结合顶部导航栏

```dart
class TopPageViewDemo extends StatefulWidget{
  @override
  State<StatefulWidget> createState() => TopPageViewDemoState();
}

class TopPageViewDemoState extends State<TopPageViewDemo> with SingleTickerProviderStateMixin{
  TabController _tabController;
  PageController _pageController = PageController();
  List<String> titles = ["首页", "我的"];

  @override
  void initState() {
    super.initState();
    _tabController = TabController(length: titles.length, vsync: this);
  }

  @override
  void dispose() {
    super.dispose();
    _tabController.dispose();
  }


  @override
  Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: Text("AppBarTitle",style: TextStyle(color: Colors.black),),
          backgroundColor: Colors.white,
          bottom: TabBar(tabs: <Tab>[
            Tab(
              text: titles[0],
            ),
            Tab(
              text: titles[1],
            ),
          ],
          controller: _tabController,
          indicatorColor: Colors.red,
          labelColor: Colors.black,
          unselectedLabelColor: Colors.grey,
          ),
        ),
        body: PageView(
          controller: _pageController,
          children: <Widget>[
            Text(titles[0]),
            Text(titles[1]),
          ],
          onPageChanged: (index){
            _tabController.animateTo(index);
          },
        ),
      );
  }
}
```

#### (3)pageController

```dart
//构造方法
PageController({int initialPage, bool keepPage, double viewportFraction})
```

**常用参数**

指定初始页面

```dart
initialPage->int
```

当前正在展示的page

```dart
page → double?
```

保留当前页面，当重建时默认恢复当前页面，而忽略initialpage，如果为false，总是从initialpage开始

**常用方法**

从当前页面转换的指定页面(**带有动画**)

```dart
animateToPage(int page, {required Duration duration, required Curve curve}) → Future<void>
```

添加监听

```dart
addListener(VoidCallback listener) → void
```

从当前页面跳转到指定页面

```dart
jumpToPage(int page) → void
```

跳转到下一个页面(有动画)

```dart
nextPage({required Duration duration, required Curve curve}) → Future<void>
```

跳转到上一个界面

```dart
previousPage({required Duration duration, required Curve curve}) → Future<void>
```

### 3.ConstrainedBox(限定宽高布局)

> ConstrainedBox的作用是限定子元素child的最大宽度、最大高度、最小宽度和最小高度。

```dart
ConstrainedBox(
          constraints: BoxConstraints(
             maxHeight: 200,//最大高度
             maxWidth: 200,//最大宽度
             minHeight: 150,//最小高度
             minWidth: 150,//最小宽度
          ),
         child: Container(
            width: 400,
            height: 400,
            color: Colors.red,
         ),
      ),
```

### 4.LimitedBox(限定最大宽高布局)

LimitedBox组件是限制类型的组件，可对最大宽高进行限制。和ConstrainedBox类似，不过没有最小宽高限制。

LimitedBox是将child限制在其设定的最大宽高中的，但这个限定是有条件的。**当LimitedBox最大宽度不受限制的，child的宽度就会受到这个最大宽度的限制**，高度同理。

```dart
const LimitedBox({
    Key key,
    this.maxWidth = double.infinity,
    this.maxHeight = double.infinity,
    Widget child,
  })
```

### 5.FractionallySizedBox(百分比布局)

FractionallySizedBox(百分比布局)组件会根据现有空间来调整child的尺寸，**所以就算为child设置了具体的尺寸数值，也不起作用。**当需要在一个区域里面取百分比尺寸时，可以使用这个组件，**比如需要一个高度30%宽度60%的区域。**

FractionallySizedBox的布局主要跟它的宽高因子（两个参数）有关，当参数为null或者有具体数值的时候，布局表现不一样。还有一个alignment参数，作为对齐方式进行布局。具体可分为以下两种情况：

> 1.设置了具体的宽高因子，具体的宽高则根据现有空间宽高*因子，当宽高因子大于1的时候，有可能会超出父组件的范围。
>
> 2.没有设置宽高因子，则填满可用区域。

```dart
FractionallySizedBox({
    Key key,
    this.alignment = Alignment.center,
    this.widthFactor,
    this.heightFactor,
    Widget child,
  })
```

例子：

```dart
Container(
         color: Colors.green,
         width: 200.0,
         height: 100.0,
        child: FractionallySizedBox(
           alignment: Alignment.topLeft,
           widthFactor: 1.5, //宽度因子
           heightFactor: 0.5, //高度因子
           child: Container(
              color: Colors.red,
           ),
        ),
      )
```

![image-20210804165626353](https://img-blog.csdnimg.cn/img_convert/daf3fb8753449bfef617cbf6a648e1b7.png)



### 6.CustomeScrollView

> **CustomScrollView是可以使用sliver自定义滚动模型（效果）的widget**。其中sliver包括SliverList、SliverFixedExtentList、SliverGrid、SliverPadding、SliverAppBar，对于大多数Sliver来说，它们和可滚动Widget最主要的区别是Sliver不会包含Scrollable Widget，也就是说Sliver本身不包含滚动交互模型。因此，CustomScrollView可以将它们组合起来，统一控制滚动。

```dart
 const CustomScrollView({
    Key key,
    Axis scrollDirection = Axis.vertical, //滑动的方向
    bool reverse = false, //控制 CustomScrollView 里列表项的排列顺序
    ScrollController controller, //可以控制 CustomScrollView 滚动的位置
     bool primary,
    ScrollPhysics physics,
    bool shrinkWrap = false,
     
    Key center, 
    //放在 CustomScrollView 中间的子Widget 的 key ,center前面的sliveritem会向上增长，center之后的sliveritem向下增长
     
    double anchor = 0.0, //CustomScrollView 开始滑动的偏移量
     //如果 anchor 为 0.0，则 CustomScrollView 的 子Widget 从头开始排列
     //如果 anchor 为 0.5，则 CustomScrollView 的 子Widget 从中间开始排列
     //如果 anchor 为 1.0，则 CustomScrollView 的 子Widget 从底部开始排列
     
    double cacheExtent,//预加载区域
    this.slivers = const <Widget>[],//sliverItem
    int semanticChildCount,//可见子元素的总数
    DragStartBehavior dragStartBehavior = DragStartBehavior.start,
     //确定处理拖动开始行为的方式。
     //如果设置为[DragStartBehavior.start]，则在检测到拖动手势时将开始滚动拖动行为
     //如果设置为[DragStartBehavior.down]，它将在首次检测到向下事件时开始
```

#### Sliver系列的控件

**(1)SliverAppBar**

SliverAppBar 控件，一个 MD 的 AppBar 。属性和 AppBar 类似，但做的效果比 AppBar 更加强大。

- **forceElevated**

结合 elevation 使用，当elevation 不为 0 的时候，是否显示阴影

- **expandedHeight**

AppBar 展开时候的高度

- **floating**

true 的时候下滑AppBar优先滑动展示，展示完成后才给滑动控件滑动

- **snap**

snap 为 true, 则 floating 也要为 true 。true 的时候根据手指松开的位置展开或者收缩AppBar

- **pinned**

appBar 收缩到最小高度的时候 appBar 是否可见

- **flexibleSpace**

appbar里面的内容(常用FlexibleSpaceBar()来创建内容)



> 三个常用属性：
>
> float:是否漂浮
>
> pinned:当往下滑时，appbar是否固定不会消失
>
> snap:当为true是自动获取焦点滑动出来，详情参考文档
>
> **当float为true时，snap必须为true**

```dart
 new CustomScrollView(
        //滚动方向
        scrollDirection: Axis.vertical,
        //是否反转滚动方向
        reverse: false,
        //监听事件等等控制器
        controller: _scrollController,
        //true 的话 controller 一定要为null
        primary: false,
        //滑动效果，如阻尼效果等等
        physics: const BouncingScrollPhysics(),
        //滑动控件是否在头部上面滑过去
        shrinkWrap: false,
        //0到1之间，到顶部的距离
        anchor: 0.0,
        //“预加载”的区域,0.0 为关闭预加载
        cacheExtent: 0.0,
        slivers: <Widget>[
          SliverAppBar(
            elevation: 5,
            forceElevated: true,
            expandedHeight: 250.0,
            floating: true,
            snap: true,
            pinned: true,
            flexibleSpace: FlexibleSpaceBar(
              title: const Text('SliverAppBar'),
              background: Image.network(
                'https://cn.bing.com/th?id=OIP.xq1C2fmnSw5DEoRMC86vJwD6D6&pid=Api&rs=1',
                fit: BoxFit.fill,
              ),
              //标题是否居中
              centerTitle: true,
              //标题间距
              titlePadding: EdgeInsetsDirectional.only(start: 0, bottom: 16),
              collapseMode: CollapseMode.none,
            ),
          ),
        ],
        semanticChildCount: 6,//可见子元素的总数
      ),
    );
  }
}
```



**(2)SliverPadding**

为 Sliver 系列控件添加一个 padding 。如给上面 SliverAppBar 添加一个 Padding 。

```dart
SliverPadding(
    padding: EdgeInsets.all(10.0),
    sliver: SliverAppBar(...),
),
```



**(3)SliverGrid**

多行多列的列表控件，相当于 Android 的 GridView，有两个属性

- **delegate**创建元素

通过SliverChildDelegate()或者SliverChildBuilderDelegate ()创建

①SliverChildListDelegate 一般用来构 item 不多的列表，效率更低。

```dart
List<String> _gridLists = ['grid item 1','grid item 2','grid item 3','grid item 4','grid item 5'];

delegate: SliverChildListDelegate(
                  _gridLists.map((name){
                    return new Container(
                      alignment: Alignment.center,
                      color: Colors.blue,
                      child: new Text(name),
                    );
                  }).toList()
              ),
```

②SliverChildBuilderDelegate 一般用来构建 item 更多的列表，效率更高。

```dart
delegate: new SliverChildBuilderDelegate(
                    (BuildContext context, int index) {
                  //创建子widget
                  return new Container(
                    alignment: Alignment.center,
                    color: Colors.blue,
                    child: new Text('grid item $index'),
                  );
                },
                childCount: 20,
              ),
```



- **gridDelegate**创建表格样式

也是可以通过两种方式创建

> SliverGridDelegateWithMaxCrossAxisExtent，根据给的 maxCrossAxisExtent 最大宽度自动分配一列展示多少个。

```dart
gridDelegate: new SliverGridDelegateWithMaxCrossAxisExtent(
                maxCrossAxisExtent: 150,
                mainAxisSpacing: 10.0, //主轴中间间距
                crossAxisSpacing: 10.0, //副轴中间间距
                childAspectRatio: 2.0, //item 宽高比
              ),
```



> SliverGridDelegateWithFixedCrossAxisCount，固定展示多少列。

```dart
gridDelegate: new SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 4,
                mainAxisSpacing: 10.0, //主轴中间间距
                crossAxisSpacing: 10.0, //副轴中间间距
                childAspectRatio: 2.0, //item 宽高比
              )
```



**(4)SliverList**

和上面 delegate 属性一样，需要创建一个 SliverChildDelegate 。

```dart
new SliverList(
    delegate: new SliverChildBuilderDelegate(
        (BuildContext context, int index) {
            //创建列表项
            return new Container(
                height: 50,
                alignment: Alignment.center,
                color: Colors.lightBlue[100 * ((index % 9) + 1)],
                child: new Text('list item $index'),
            );
        }, childCount: 30 //30个列表项
    ),
),
```

**(5)SliverFixedExtentList**

比 SliverList 多一个 itemExtent 属性，设置 item 的高度 。item 里面的子控件无法再改动高度。

```dart
 const SliverFixedExtentList({
    Key key,
    @required SliverChildDelegate delegate,
    @required this.itemExtent,
  }) : 
```





**(6)SliverPersistentHeader**

上面 SliverAppBar 就是结合 SliverPersistentHeader 实现的效果，SliverPersistentHeader 需要一个 SliverPersistentHeaderDelegate 。 实现  SliverPersistentHeaderDelegate 有 4 个方法需要重写

```dart
class SliverAppBarDelegate extends SliverPersistentHeaderDelegate {
  @override
  Widget build(BuildContext context, double shrinkOffset, bool overlapsContent) {
    // TODO: implement build
    return null;
  }
  @override
  // TODO: implement maxExtent
  double get maxExtent => null;

  @override
  // TODO: implement minExtent
  double get minExtent => null;

  @override
  bool shouldRebuild(SliverPersistentHeaderDelegate oldDelegate) {
    // TODO: implement shouldRebuild
    return null;
  }
}
```



**(7)SliverToBoxAdapter**

```dart
const SliverToBoxAdapter({
    Key key,
    Widget child,
  })
```

里面可以设置不是**sliver**类型的widget。如下图添加的青色容器

![image-20210805165218574](https://img-blog.csdnimg.cn/img_convert/cdcedc01b2f7d3738c623a4fc65360df.png)



**(8)SliverFillViewport**

占满一屏或者比一屏更多的布局，

```dart
new SliverFillViewport(
    delegate:new SliverChildBuilderDelegate(
        (BuildContext context, int index) {
            //创建列表项
            return new Container(
                alignment: Alignment.center,
                color: Colors.lightBlue,
                child: new Text('SliverFillViewport'),
            );
        }, childCount: 1
    ),
    viewportFraction:1.0,//占屏幕的比例
),
```





### 7.SingleChildScrollView

在Flutter中我们通常使用SingleChildScrollView处理滑动，这里需要注意的是，通常SingleChildScrollView只应在期望的内容不会超过屏幕太多时使用，这是因为SingleChildScrollView不支持基于Sliver的延迟实例化模式，所以如果预计视口可能包含超出屏幕尺寸太多的内容时使用SingleChildScrollView将会导致性能差的问题，此时应该使用一些支持Sliver延迟加载的可滚动组件，如ListView。

> **基于Sliver的延迟构建模式：**
>
> 通常可滚动组件的子组件可能会非常多，占用的总高度也会非常大，如果要一次性将子组件全部构建出将会导致性能差的问题出现，为此，Flutter中提出一个Sliver（中文为"薄片"的意思）概念，如果一个可滚动组件支持Sliver模型，那么该滚动组件可以将子组件分成好多个薄片（Sliver），只有当Sliver出现在视口时才会去构建它，这种模型也成为"基于Sliver的延迟构建模型"。可滚动组件中有很多都支持基于Sliver的延迟构建模型，如ListView、GridView，但是也有不支持该模型的，如SingleChildScrollView

```js
SingleChildScrollView({Key? key, 
                       Axis scrollDirection, //滑动方向，默认垂直
                       bool reverse, 
                       EdgeInsetsGeometry? padding, 
                       bool? primary, 
                       
                       /*此属性接受一个ScrollPhysics类型的对象，它决定可以滚动如何响应用户操作，比如用户滑动完抬起手指后，                        继续执行动画，或者滑动到边界时，如何显示。
                       //默认情况下，Flutter会根据具体平台分别使用不同的ScrollPhysics对象，对应不同的显示效果，如当滑动到                        边界时，继续拖动的话，在iOS上会出现弹性效果，
                       //而在Android上会出现微光效果。如果你想在所有平台下使用同一种效果，可以显示指定一个固定的                                  ScrollPhysics。
                       //Flutter SDK包含两个ScrollPhysics的子类。1.ClampingScrollPhysics：Android下微光效果，                             2.BouncingScrollPhysics：iOS下弹性效果*/
                       ScrollPhysics? physics, 
                       
                       //此属性接收一个ScrollController对象，ScrollController的主要作用是控制滚动位置和监听滚动事                             件。
                       //默认情况下，Widget树中会有一个默认的PrimaryScrollController，如果子树中的可滚动组件没有显示的                        指定controller，并且primary属性值为true时，可滚动组件会使用这个默认的ScrollController。
                       //这种机制带来的好处是父组件可以控制子树中可滚动的滚动行为，例：scaffold正是使用这种机制在iOS中实现                        了点击导航回到顶部的功能。
                       ScrollController? controller, 
                       
                       Widget? child, 
                       DragStartBehavior dragStartBehavior, 
                       Clip clipBehavior, 
                       String? restorationId, 
                       
                       /*定义了此ScrollView如何自动关闭键盘。*/
                       ScrollViewKeyboardDismissBehavior keyboardDismissBehavior})
```



demo

```js
return new MaterialApp(
      title: 'SingleChildScrollView Demo',
      home: new Scaffold(
        appBar: AppBar(
          title: new Text('SingleChildScrollView Demo'),
        ),
        body: new SingleChildScrollView(
          physics: BouncingScrollPhysics(),
          child: new Center(
            child: new Column(
              children: <Widget>[
                Container(
                  width: 300.0,
                  height: 200.0,
                  color: Colors.blue,
                ),
                Container(
                  width: 300.0,
                  height: 200.0,
                  color: Colors.yellow,
                ),
                Container(
                  width: 300.0,
                  height: 200.0,
                  color: Colors.pink,
                ),
                Container(
                  width: 300.0,
                  height: 200.0,
                  color: Colors.blue,
                ),
                Container(
                  width: 300.0,
                  height: 200.0,
                  color: Colors.yellow,
                ),
                Container(
                  width: 300.0,
                  height: 200.0,
                  color: Colors.pink,
                ),
                Container(
                  width: 300.0,
                  height: 200.0,
                  color: Colors.blue,
                ),
              ],
            ),
          ),
        ),
      ),
    );
```



通过SingleChildScrollView会和Column一起搭配使用时会出现与ListView和Column搭配使用出现的相同的结果，Column会尽可能去适应子内容，二SingleChildScrollView会尝试无限扩大，两者这一特性会互相冲突，所以以下提供两种解决办法（这里只详细介绍一种）

**IntrensicHeight + ConstrainedBox**

- LayoutBuilder拥有视图口（viewport）的尺寸
- ConstrainedBox能够限制最小的尺寸
- IntrensicHeight能够让Column强制自适应子组件的高度，所以可以使用Expand，这样就能恰好占满屏幕

```js
Widget build(BuildContext context) {
   return DefaultTextStyle(
     style: Theme.of(context).textTheme.bodyText2!,
     child: LayoutBuilder(
       builder: (BuildContext context, BoxConstraints viewportConstraints) {
         return SingleChildScrollView(
           child: ConstrainedBox(
             constraints: BoxConstraints(
               minHeight: viewportConstraints.maxHeight,
             ),
             child: IntrinsicHeight(
               child: Column(
                 children: <Widget>[
                   Container(
                     // A fixed-height child.
                     color: const Color(0xffeeee00), // Yellow
                     height: 120.0,
                     alignment: Alignment.center,
                     child: const Text('Fixed Height Content'),
                   ),
                   Expanded(
                     // A flexible child that will grow to fit the viewport but
                     // still be at least as big as necessary to fit its contents.
                     child: Container(
                       color: const Color(0xffee0000), // Red
                       height: 120.0,
                       alignment: Alignment.center,
                       child: const Text('Flexible Content'),
                     ),
                   ),
                 ],
               ),
             ),
           ),
         );
       },
     ),
   );
 }
```









### 8.DropdownButton

**和安卓Spinner类似的下拉选框**

```js
DropdownButton({
    Key key,
    @required this.items,       // 下拉选项列表
    this.selectedItemBuilder,   // 选项 item 构造器
    this.value,                 // 选中内容
    this.hint,                  // 启动状态下默认内容
    this.disabledHint,          // 禁用状态下默认内容
    @required this.onChanged,   // 选择 item 回调，其中 items 中 value 是必须参数，且不相同；回调返回的内容是 DropdownMenuItem 中 child 内                                      容；
    this.elevation = 8,         // 阴影高度，只能是 1 / 2 / 3 / 4 / 6 / 8 / 9 / 12 / 16 / 24，
    this.style,                 // 选项列表 item 样式，但下拉列表 item 设置文本样式后，以 item 设置为准
    this.underline,             // 按钮下划线
    this.icon,                  // 下拉按钮图标
    this.iconDisabledColor,     // 禁用状态下图标颜色
    this.iconEnabledColor,      // 启动时图标颜色
    this.iconSize = 24.0,       // 图标尺寸
    this.isDense = false,       // 是否降低按钮高度,true 时将按钮高度缩小
    this.isExpanded = false,    // isExpanded 用于是否填充按钮宽度到父控件，true 为填充，false 为默认不填充；
})

const DropdownMenuItem({
    Key key,
    this.value,             // 对应选中状态内容，必须参数
    @required this.child,   // 下拉列表 item 内容
})
```

> **items** 和 **onChanged** 回调是必须参数，且在不同状态下，展示的效果不同；**items** 为下拉选项列表，**onChanged** 为选中回调；两者其中一个为 **null** 时为按钮禁用状态，不可点击，默认下拉 **icon** 为灰色；**items** 不为空时，需为相同类型的 **DropdownMenuItem** 类型列表；



```js
DropdownButton(items: [
  DropdownMenuItem(child: Text('北京')),
  DropdownMenuItem(child: Text('天津')),
  DropdownMenuItem(child: Text('河北'))
], onChanged: (value) {});
```



### 9.RawChip

Material风格标签控件，此控件是其他标签控件的基类，通常情况下，不会直接创建此控件，而是使用如下控件：

- Chip
- InputChip
- ChoiceChip
- FilterChip
- ActionChip

RawChip可以通过设置`onSelected`被选中，设置`onDeleted`被删除，也可以通过设置`onPressed`而像一个按钮，它有一个`label`属性，有一个前置（`avatar`）和后置图标（`deleteIcon`）。

![image-20210829133758682](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210829133758682.png)



```js
RawChip({
    Key? key,
    this.avatar,//设置左侧控件，一般是图标
    required this.label,
    this.labelStyle,
    this.padding,
    this.visualDensity,
    this.labelPadding,
    Widget? deleteIcon,
    this.onDeleted,
    this.deleteIconColor,
    this.useDeleteButtonTooltip = true,
    this.deleteButtonTooltipMessage,
    this.onPressed,
    this.onSelected,
    this.pressElevation,
    this.tapEnabled = true,
    this.selected = false,
    this.isEnabled = true,
    this.disabledColor,
    this.selectedColor,
    this.tooltip,
    this.side,
    this.shape,
    this.clipBehavior = Clip.none,
    this.focusNode,
    this.autofocus = false,
    this.backgroundColor,
    this.materialTapTargetSize,//控制最小点击区域
    this.elevation,
    this.shadowColor,
    this.selectedShadowColor,
    this.showCheckmark = true,
    this.checkmarkColor,
    this.avatarBorder = const CircleBorder(),
  })
```



设置删除相关属性

```js
RawChip(
  label: Text('老孟'),
  onDeleted: (){
    print('onDeleted');
  },
  deleteIcon: Icon(Icons.delete),
  deleteIconColor: Colors.red,
  deleteButtonTooltipMessage: '删除',
)
```

![image-20210829134412134](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210829134412134.png)



设置阴影：

```js
RawChip(
  label: Text('老孟'),
  elevation: 8,
  shadowColor: Colors.blue,
)
```

![image-20210829134452192](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210829134452192.png)

设置选中状态下“前置对勾”图标：

```js
RawChip(
  label: Text('老孟'),
  selected: true,
  showCheckmark: true,
  checkmarkColor: Colors.red,
)
```

![image-20210829134644017](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210829134644017.png)

##### ChoiceChip

允许从一组选项中进行单个选择，创建一个类似于单选按钮的标签，本质上ChoiceChip也是一个RawChip，ChoiceChip本身不具备单选属性。

单选demo如下：

```js
int _selectIndex = 0;
Wrap(
  spacing: 15,
  children: List.generate(10, (index) {
    return ChoiceChip(
      label: Text('老孟 $index'),
      selected: _selectIndex == index,
      onSelected: (v) {
        setState(() {
          _selectIndex = index;
        });
      },
    );
  }).toList(),
)
```

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/20200510092116818.png)

##### FilterChip

FilterChip可以作为过滤标签，本质上也是一个RawChip，用法如下：

```js
List<String> _filters = [];

Column(
  children: <Widget>[
    Wrap(
      spacing: 15,
      children: List.generate(10, (index) {
        return FilterChip(
          label: Text('老孟 $index'),
          selected: _filters.contains('$index'),
          onSelected: (v) {
            setState(() {
              if(v){
                _filters.add('$index');
              }else{
                _filters.removeWhere((f){
                  return f == '$index';
                });
              }
            });
          },
        );
      }).toList(),
    ),
    Text('选中：${_filters.join(',')}'),
  ],
)
```

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/2020051009211745.png)

##### Chip

Chip是一个简单的标签控件，仅显示信息和`删除`相关属性，是一个简化版的RawChip，用法和RawChip一样。源代码如下：

```js
@override
Widget build(BuildContext context) {
  assert(debugCheckHasMaterial(context));
  return RawChip(
    avatar: avatar,
    label: label,
    labelStyle: labelStyle,
    labelPadding: labelPadding,
    deleteIcon: deleteIcon,
    onDeleted: onDeleted,
    deleteIconColor: deleteIconColor,
    deleteButtonTooltipMessage: deleteButtonTooltipMessage,
    tapEnabled: false,
    shape: shape,
    clipBehavior: clipBehavior,
    focusNode: focusNode,
    autofocus: autofocus,
    backgroundColor: backgroundColor,
    padding: padding,
    materialTapTargetSize: materialTapTargetSize,
    elevation: elevation,
    shadowColor: shadowColor,
    isEnabled: true,
  );
}
```







### 10.SmartRefresher

```yaml
dependencies:
  pull_to_refresh: ^1.5.7
```

```js
import 'package:pull_to_refresh/pull_to_refresh.dart';
```





## 二、其他

### 1.切换保持页面

在flutter中经常遇到切换页面之后返回上一个界面会出现界面重构了，之前的状态不能保存。在以下有三种实现界面状态保存的方法

#### (1)通过IndexedStack实现

IndexedStack作用是显示第第index个child,其它child在页面上是不可见的，但所有child状态都被保持：

```dart
/// home.dart
class _MyHomePageState extends State<MyHomePage> {
  ...
  ...
  ...
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('demo'),
        ),
        bottomNavigationBar: BottomNavigationBar(
            items: items, currentIndex: currentIndex, onTap: onTap),
        // body: bodyList[currentIndex]
        body: IndexedStack(
          index: currentIndex,
          children: bodyList,
        ));
  }
```

#### (2)通过offstage实现

offstage实现也和第一个方法类似：

```dart
/// home.dart
class _MyHomePageState extends State<MyHomePage> {
  ...
  ...
  ...
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('demo'),
        ),
        bottomNavigationBar: BottomNavigationBar(
            items: items, currentIndex: currentIndex, onTap: onTap),
        // body: bodyList[currentIndex],
        body: Stack(
          children: [
            Offstage(
              offstage: currentIndex != 0,
              child: bodyList[0],
            ),
            Offstage(
              offstage: currentIndex != 1,
              child: bodyList[1],
            ),
            Offstage(
              offstage: currentIndex != 2,
              child: bodyList[2],
            ),
          ],
        ));
  }
}
```

> 上述两种方式都有一个缺点，那就是在第一次记载的时候吧左右child都实例化了，有点吃存储。

#### (3)继承`AutomaticKeepAliveClientMixin`

##### 实现顶部导航栏

> 这种方式一般是配合着TabBar+TabBarView使用，实现顶部导航栏切换页面的保存。

实现过程很简单，只保持界面状态的子页State中，继承`AutomaticKeepAliveClientMixin`并重写`wantKeepAlive`为`true`即可。

```dart
class RecommendPage extends StatefulWidget {
  @override
  _RecommendPageState createState() => _RecommendPageState();
}

class _RecommendPageState extends State<RecommendPage>
    with AutomaticKeepAliveClientMixin {
  int count = 0;

  void add() {
    setState(() {
      count++;
    });
  }

  @override
  bool get wantKeepAlive => true;
    
  ......
```

##### 实现底部导航栏

> `PageView`+`AutomaticKeepAliveClientMixin`重写之前的底部导航

```dart
class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  final items = [
    BottomNavigationBarItem(icon: Icon(Icons.home), title: Text('首页')),
    BottomNavigationBarItem(icon: Icon(Icons.music_video), title: Text('听')),
    BottomNavigationBarItem(icon: Icon(Icons.message), title: Text('消息'))
  ];

  final bodyList = [FirstPage(), SecondPage(), ThirdPage()];

  final pageController = PageController();

  int currentIndex = 0;

  void onTap(int index) {
    pageController.jumpToPage(index);
  }

  void onPageChanged(int index) {
    setState(() {
      currentIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        bottomNavigationBar: BottomNavigationBar(
            items: items, currentIndex: currentIndex, onTap: onTap),
        // body: bodyList[currentIndex],
        body: PageView(
          controller: pageController,
          onPageChanged: onPageChanged,
          children: bodyList,
          physics: NeverScrollableScrollPhysics(), // 禁止滑动
        ));
  }
}
```



在bodyList子页State中继承`AutomaticKeepAliveClientMixin`并重写`wantKeepAlive`，以`second_page.dart`为例：

```dart
/// second_page.dart
import 'package:flutter/material.dart';

class SecondPage extends StatefulWidget {
  @override
  _SecondPageState createState() => _SecondPageState();
}

class _SecondPageState extends State<SecondPage>
    with AutomaticKeepAliveClientMixin {
  int count = 0;

  void add() {
    setState(() {
      count++;
    });
  }

  @override
  bool get wantKeepAlive => true;

  @override
  void initState() {
    super.initState();
    print('second initState');
  }

  @override
  Widget build(BuildContext context) {
    super.build(context);
    return Scaffold(
        body: Center(
          child: Text('Second: $count', style: TextStyle(fontSize: 30))
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: add,
          child: Icon(Icons.add),
        ));
  }
}
```

该方法实现之后不会在第一次加载时全部实例化child

### 2.double.infinity

作用可以总结为：和父母一样大，一些小部件可以让孩子长到自己想要的大小

> Column，ListView，OverflowBox 在这种情况下，使用 double.infinity

根据文档，这些是分配给各个double常数的值

```dart
static const double infinity = 1.0 / 0.0;
static const double negativeInfinity = -infinity;
static const double minPositive = 5e-324;
static const double maxFinite = 1.7976931348623157e+308;
```

### 3.ListView嵌套Column和Row

在Flutter的ListView的子View中，你可以在Row中使用Expanded填充水平方向的剩余空间，而无法在Column中使用Expanded填充垂直方向的剩余空间。

> 原因是ListView垂直方向的计算是包裹子View的，**也就是说子View必须有一个明确的高度，或者尽可能小的高度，而不能是无限高。**
> Row是横向排列，在Row中使用Expanded是填充水平方向的剩余空间，这和ListView的这一特性没有冲突，可以使用。
>
> 
>
> 而Column是竖直排列，在Column中使用Expanded是填充竖直方向的剩余空间，这将和ListView的这一特性发生冲突，因为ListView将无法计算自己的子View的高度。

解决办法：

> IntrinsicHeight可以根据子控件的高度，智能调整自身高度
>
> 把这个控件加在itemVIew的最外层，把Row给包括起来。
>
> IntrinsicHeight可以根据子控件（左侧文案和右侧图片），智能选择一个合适的高度调整自身，使得ListView的子View的高度是确切的，这样ListView便不会因为子View想要无限高，而自己又要包括子View而报错了。

```dart
 ListView(
        children: [
          IntrinsicHeight(
            child: Container(
              width: double.infinity,
              color: Colors.deepPurple,
              child: Column(
                children: [
                  Expanded(
                      flex: 35,
                      child: Container(
                        width: double.infinity,
                        color: Colors.red,
                        child: Text('sdsdsdsds'),
                      )
                  ),
                  Expanded(
                      flex: 1,
                      child: Container(
                        width: double.infinity,
                        color: Colors.greenAccent,
                        child: Text('33333333'),
                      )
                  )
                ],
              ),
            ),
          )
        ],
      )
```

![image-20210804140247735](https://img-blog.csdnimg.cn/img_convert/901796d9054c566bae6df598502fc318.png)

### 4.MediaQuery

> **MediaQuery** 一直存在于 **WidgetsApp** 和 **MaterialApp** 中，**MediaQuery** 继承自 **InheritedWidget** 是一个单独的 **Widget**，但一般通过 **MediaQuery.of(context)** 来获取相关信息；
>
> 当相关信息发生变化，例如屏幕旋转等时，屏幕中 **Widget** 会重新构建，以保持最新状态；我们可以通过 **MediaQuery** 构造函数和提供的静态方法手动设置对应的相关信息；

```dart
const MediaQuery({
    Key key,
    @required this.data,
    @required Widget child,
})
```

**MediaQuery.removePadding() 删除内边距**

```dart
factory MediaQuery.removePadding({
    Key key,
    @required BuildContext context,
    bool removeLeft = false,
    bool removeTop = false,
    bool removeRight = false,
    bool removeBottom = false,
    @required Widget child,
})
```

**MediaQuery.removeViewInsets() 删除视图内边距**

```dart
factory MediaQuery.removeViewInsets({
    Key key,
    @required BuildContext context,
    bool removeLeft = false,
    bool removeTop = false,
    bool removeRight = false,
    bool removeBottom = false,
    @required Widget child,
})
```

#### MediaQueryData

**MediaQueryData** 包含关于媒介的相关信息；一般通过 **MediaQuery.of(context)** 获取；

```dart
const MediaQueryData({
    this.size = Size.zero,
    this.devicePixelRatio = 1.0,
    this.textScaleFactor = 1.0,
    this.platformBrightness = Brightness.light,
    this.padding = EdgeInsets.zero,
    this.viewInsets = EdgeInsets.zero,
    this.systemGestureInsets = EdgeInsets.zero,//为手势边距，如 Android Q 之后添加的向左滑动关闭页面等；
    this.viewPadding = EdgeInsets.zero,
    this.physicalDepth = double.maxFinite,
    this.alwaysUse24HourFormat = false,//当前设备是否为24小时制
    this.accessibleNavigation = false,//是否使用 TalkBack 或 VoiceOver 之类的辅助功能与应用程序进行交互，用以辅助视力障碍人群；
    this.invertColors = false,//invertColors 为是否使用颜色反转，主要用于 iOS 设备；
    this.highContrast = false,//为用户是否要求前景与背景之间的对比度高，主要用于 iOS 设备；
    this.disableAnimations = false,//为平台是否要求禁用或减少动画；
    this.boldText = false,// 为平台是否要求使用粗体；
});
```

(1)size

 **size** 为媒介的尺寸大小，以逻辑像素为单位；

```dart
print('屏幕 Size -> ${MediaQuery.of(context).size}');
```



(2)devicePixelRatio

   **devicePixelRatio** 为像素密度；与设备物理像素有关，与横竖屏等无关；



(3)orientation

   **orientation** 为横竖屏，**Orientation.landscape** 为横屏，**Orientation.portrait** 为竖屏；

```dart
print('横竖屏 -> ${MediaQuery.of(context).orientation}');
```



(4)textScaleFactor

   **textScaleFactor** 为每个逻辑像素的字体像素数，可以理解为字体的像素比；

```dart
print('字体像素比 -> ${MediaQuery.of(context).textScaleFactor}');

MediaQuery(data: MediaQuery.of(context).copyWith(textScaleFactor: 1.2),
    child: Text('字体像素比 * 1.2', style: TextStyle(color: Colors.white, fontSize: 16.0));
print('字体像素比 * 1.2 -> ${MediaQuery.of(context).copyWith(textScaleFactor: 1.2).textScaleFactor}');
```



(5)platformBrightness

   **platformBrightness** 为当前设备的亮度模式；注意调整屏幕亮度并不会改变该模式，与当前系统支持的黑暗模式和明亮模式相关；

```dart
print('亮度模式 -> ${MediaQuery.of(context).platformBrightness}');
```



(6)padding

   **padding** 为屏幕内边距，一般是刘海儿屏或异形屏中被系统遮挡部分边距；

```dart
print('内边距 -> ${MediaQuery.of(context).padding}');
```



(7)viewInsets

   **viewInsets** 为键盘弹出时等遮挡屏幕边距，其中 **viewInsets.bottom** 为键盘高度；

```dart
print('键盘遮挡内边距 -> ${MediaQuery.of(context).viewInsets}');
```



#### 注意

在尝试获取其他子 **Widget Size** 时，有两点需要注意，首先要设置一个全局的 **GlobalKey** 来获取当前位置，**key** 需要为唯一的；第二通过 **GlobalKey().currentContext** 获取 **BuildContext** 上下文环境，从而获取对应尺寸；

```jsx
var _itemExpandedKey = GlobalKey();
var _itemTextKey = GlobalKey();

Expanded(
    key: _itemExpandedKey,
    child: FlatButton(
        onPressed: () => _itemClick(2),
        child: Center(child: Text('按钮 Size', key: _itemTextKey, style: TextStyle(color: Colors.white, fontSize: 16.0))),
        color: Colors.purpleAccent.withOpacity(0.4)))
```

 

### 5.Flutter中的Key

#### 概述

什么是key

> 您可以使用key来控制框架将在widget重建时与哪些其他widget匹配。默认情况下，框架根据它们的runtimeType和它们的显示顺序来匹配。**使用key时，框架要求两个widget具有相同的key和runtimeType。**

> **Widget 只是一个配置且无法修改，而 Element 才是真正被使用的对象**，并可以修改。当新的 Widget 到来时将会调用 canUpdate 方法，来确定这个 Element是否需要更新。返回true时可以更新对element索引
> canUpdate 对两个（新老） Widget 的 runtimeType 和 key 进行比较，从而判断出当前的 Element 是否需要更新





**key之间的关系**

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/795b6eec6a6eda58e5a6b7b4d3aae549.png)

```dart
@immutable
abstract class Key {
  const factory Key(String value) = ValueKey<String>;

  @protected
  const Key.empty();
}
```

默认创建 Key 将会通过工厂方法根据传入的 value 创建一个 ValueKey。Key 派生出两种不同用途的 Key：LocalKey 和 GlobalKey。

#### LocalKey

LocalKey 直接继承至 Key，它应用于拥有相同父 Element 的小部件进行比较的情况，也就是上述例子中，有一个多子 Widget 中需要对它的子 widget 进行移动处理，这时候你应该使用Localkey。

Localkey 派生出了许多子类 key：

- ValueKey : ValueKey('String')
- ObjectKey : ObjectKey(Object)
- UniqueKey : UniqueKey()

Valuekey 又派生出了 PageStorageKey : PageStorageKey('value')

#### GlobalKey

```dart
@optionalTypeArgs
abstract class GlobalKey<T extends State<StatefulWidget>> extends Key {
···
static final Map<GlobalKey, Element> _registry = <GlobalKey, Element>{};
static final Set<Element> _debugIllFatedElements = HashSet<Element>();
static final Map<GlobalKey, Element> _debugReservations = <GlobalKey, Element>{};
···
BuildContext get currentContext ···
Widget get currentWidget ···
T get currentState ···

```

GlobalKey 使用了一个静态常量 Map 来保存它对应的 Element。

你可以通过 GlobalKey 找到持有该GlobalKey的 **Widget**，**State** 和 **Element**。

注意：GlobalKey 是非常昂贵的，需要谨慎使用。

#### 什么时候需要使用key

**ValueKey**

如果您有一个 Todo List 应用程序，它将会记录你需要完成的事情。我们假设每个 Todo 事情都各不相同，而你想要对每个 Todo 进行滑动删除操作。

这时候就需要使用 ValueKey！

```dart
return TodoItem(
    key: ValueKey(todo.task),
    todo: todo,
    onDismissed: (direction){
        _removeTodo(context, todo);
    },
);
```

**ObjectKey**

如果你有一个生日应用，它可以记录某个人的生日，并用列表显示出来，同样的还是需要有一个滑动删除操作。

我们知道人名可能会重复，这时候你无法保证给 Key 的值每次都会不同。但是，当人名和生日组合起来的 Object 将具有唯一性。

这时候你需要使用 ObjectKey！

**UniqueKey**

如果组合的 Object 都无法满足唯一性的时候，你想要确保每一个 Key 都具有唯一性。那么，你可以使用 UniqueKey。它将会通过该对象生成一个具有唯一性的 hash 码。

不过这样做，每次 Widget 被构建时都会去重新生成一个新的 UniqueKey，失去了一致性。也就是说你的小部件还是会改变。（还不如不用?）

**PageStorageKey**

当你有一个滑动列表，你通过某一个 Item 跳转到了一个新的页面，当你返回之前的列表页面时，你发现滑动的距离回到了顶部。这时候，给 Sliver 一个 PageStorageKey！它将能够保持 Sliver 的滚动状态。

**GlobalKey**

GlobalKey 能够跨 Widget 访问状态。 在这里我们有一个 Switcher 小部件，它可以通过 changeState 改变它的状态。

```dart
class SwitcherScreenState extends State<SwitcherScreen> {
  bool isActive = false;
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Switch.adaptive(
            value: isActive,
            onChanged: (bool currentStatus) {
              isActive = currentStatus;
              setState(() {});
            }),
      ),
    );
  }
 
  changeState() {
    isActive = !isActive;
    setState(() {});
  }
}
```

但是我们想要在外部改变该状态，这时候就需要使用 GlobalKey。

```dart
class _ScreenState extends State<Screen> {
  final GlobalKey<SwitcherScreenState> key = GlobalKey<SwitcherScreenState>();
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SwitcherScreen(
        key: key,
      ),
      floatingActionButton: FloatingActionButton(onPressed: () {
        key.currentState.changeState();
      }),
    );
  }
}
```

这里我们通过定义了一个 GlobalKey 并传递给 SwitcherScreen。然后我们便可以通过这个 key 拿到它所绑定的 SwitcherState 并在外部调用 changeState 改变状态了。

### 6.Timer

timer可以实现延时任务

```dart
//回调一次的任务
Timer timer = new Timer(Duration(seconds:5),(){
    //code
});

//回调多次的任务,要切记取消掉timer
Timer timer = new Timer.periodic(Duration(seconds:5),(timer){
    //code
    
    timer.cancel();
});

@override
void dispose() {
    // 组件销毁时判断Timer是否仍然处于激活状态，是则取消
    if(_timer.isActive){
		_timer.cancel();
	}
    super.dispose();
}
```

### 7.静态分析

静态分析是指在不允许代码前提下进行的代码安全分析，把一些编译阶段可能不会出现的错误打印出来，增加了代码安全性。

例如：

我们在使用完StreamController的时候忘记了关闭流，造成了内存泄漏，这时候编译阶段是不会报错的，所有就需要进行静态分析：

```js
var controller = StreamController<String>();
```

```js
//控制台警告
info - example.dart:9:19 - Avoid empty statements. - empty_statements
```

#### 静态分析方式：

##### (1)命令行操作

例如我要静态分析lib目录下面的代码

```bash
dartanalyzer lib
```

获取更多帮助

```bash
dartanalyzer --help
```

##### (2)自定义静态分析文件

具体参考:

> 官网介绍：https://dart.cn/guides/language/analysis-options
>
> pub包：https://pub.flutter-io.cn/packages/analyzer

但是我们想要在外部改变该状态，这时候就需要使用 GlobalKey。

```dart
class _ScreenState extends State<Screen> {
  final GlobalKey<SwitcherScreenState> key = GlobalKey<SwitcherScreenState>();
 
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SwitcherScreen(
        key: key,
      ),
      floatingActionButton: FloatingActionButton(onPressed: () {
        key.currentState.changeState();
      }),
    );
  }
}
```

这里我们通过定义了一个 GlobalKey 并传递给 SwitcherScreen。然后我们便可以通过这个 key 拿到它所绑定的 SwitcherState 并在外部调用 changeState 改变状态了。



### 8.FocusScope/FocusNode



