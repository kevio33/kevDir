# Flutter--2

## 一、布局

### 1.Transform

可以对widget进行变换转变，

```dart
Container(
  color: Colors.black,
  child: Transform(
    alignment: Alignment.topRight,
    transform: Matrix4.skewY(0.3)..rotateZ(-math.pi / 12.0),
    child: Container(
      padding: const EdgeInsets.all(8.0),
      color: const Color(0xFFE8581C),
      child: const Text('Apartment for rent!'),
    ),
  ),
)
```



![image-20210624094052170](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210624094052170.png)

> API——https://api.flutter.dev/flutter/widgets/Transform-class.html

### 2.RotatedBox

将其child旋转整数的1/4，也就是旋转（n×90°）

```dart
const RotatedBox(
  quarterTurns: 3,//旋转3的1/4
  child: Text('Hello World!'),
)
```

![image-20210624095313938](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210624095313938.png)

### 3.LayoutBuilder

构建一个可以依赖于父小部件大小的小部件树。

```dart
 body: LayoutBuilder(
        builder: (BuildContext context, BoxConstraints constraints) {
          if (constraints.maxWidth > 600) {
            return _buildWideContainers();
          } else {
            return _buildNormalContainer();
          }
        },
      ),
    );
  }

  Widget _buildNormalContainer() {
    return Center(
      child: Container(
        height: 100.0,
        width: 100.0,
        color: Colors.red,
      ),
    );
  }

  Widget _buildWideContainers() {
    return Center(
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: <Widget>[
          Container(
            height: 100.0,
            width: 100.0,
            color: Colors.red,
          ),
          Container(
            height: 100.0,
            width: 100.0,
            color: Colors.yellow,
          ),
        ],
      ),
    );
```

![image-20210624095931046](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210624095931046.png)

> API——https://api.flutter.dev/flutter/widgets/LayoutBuilder-class.html

## 二、组件

### 1.RefreshIndicator——刷新组件

支持 Material“滑动刷新”的小部件。

```dart
 //下拉刷新组件
      body: RefreshIndicator(
        //圆圈进度颜色
        color: Colors.blue,
        //下拉停止的距离
        displacement: 44.0,
        //背景颜色
        backgroundColor: Colors.grey[200],
        onRefresh: () async {
          //模拟网络请求
          await Future.delayed(Duration(milliseconds: 2000));
          //结束刷新
          return Future.value(true);
        },
        //一个列表 
        child: ListView.builder(
          itemBuilder: (BuildContext context, int index) {
            return Container(
              height: 66,
              child: Text("测试数据"),
            );
          },
          //列表数据源数量
          itemCount: 100,
        ),
      ),
    );
```

![image-20210624102231223](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210624102231223.png)

> API——https://api.flutter.dev/flutter/material/RefreshIndicator-class.html

### 2.NotificationListener

> Notification是Flutter中一个重要的机制，在Widget树中，每一个节点都可以分发通知，通知会沿着当前节点（context）向上传递，所有父节点都可以通过NotificationListener来监听通知，Flutter中称这种通知由子向父的传递为“通知冒泡”（Notification Bubbling），这个和用户触摸事件冒泡是相似的，但有一点不同：通知冒泡可以中止，但用户触摸事件不行。

```dart
 child:  NotificationListener<ScrollNotification>(
     onNotification: (ScrollNotification scrolln){//监听滚动事件
         if(scrolln is ScrollStartNotification){
             print('start');
         }else if(scrolln is ScrollEndNotification){
             print('end');
         }
         return null;
     },
     child: ListView(//滚动
         children: List.generate(50, (index){
             return ListTile(title: Text('${index}'),subtitle: Text('$index'),);
         })
     ),
 )
```

自定义notification

```dart
class TestNotification extends Notification {
  TestNotification({
     @required this.count,
  });
  final int count;
}
```

>  **Notification有一个dispatch(context)方法，它是用于分发通知的，我们说过context实际上就是操作Element的一个接口，它与Element树上的节点是对应的，通知会从context对应的Element节点向上冒泡。**

### 3.ScrollBar(滚动条)

在滑动过程中的右侧的滚动条

```dart
Scrollbar(
      child: GridView.builder(
        itemCount: 120,
        gridDelegate:
        const SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 3),
        itemBuilder: (BuildContext context, int index) {
          return Center(
            child: Text('item $index'),
          );
        },
      ),
    );
```

### 4.NestedScrollView

支持嵌套滚动视图，其滚动位置是固有链接的

> API——https://api.flutter.dev/flutter/widgets/NestedScrollView-class.html

**(1)滚动隐藏AppBar**

比如实现如下场景，当列表滚动时，隐藏`AppBar`，用法如下：

```dart
NestedScrollView(
    //头部放置一个sliverheader
  headerSliverBuilder: (BuildContext context, bool innerBoxIsScrolled) {
    return <Widget>[SliverAppBar(
      title: Text('老孟'),
    )];
  },
  body: ListView.builder(itemBuilder: (BuildContext context,int index){
    return Container(
      height: 80,
      color: Colors.primaries[index % Colors.primaries.length],
      alignment: Alignment.center,
      child: Text(
        '$index',
        style: TextStyle(color: Colors.white, fontSize: 20),
      ),
    );
  },itemCount: 20,),
)
```

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/467322-20200508081454900-1354550916.gif)

### 5.SliverAppBar

> 与[CustomScrollView](https://api.flutter.dev/flutter/widgets/CustomScrollView-class.html)集成的材料设计应用栏。它让应用栏与滚动视图集成，以便它可以根据滚动偏移量改变高度或浮动在滚动视图中的其他内容之上。通常与CustomScrollView混合使用



> API——https://api.flutter.dev/flutter/material/SliverAppBar-class.html

```dart
CustomScrollView(
      slivers: <Widget>[
        SliverAppBar(
          pinned: false,
          snap: false,
          floating: true,
          expandedHeight: 160.0,
          flexibleSpace: const FlexibleSpaceBar(
            title: Text('SliverAppBar'),
            background: FlutterLogo(),
          ),
        ),
      ],
    );
```

![image-20210624115759047](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210624115759047.png)

> 三个常用属性：
>
> float:是否漂浮
>
> pinned:当往下滑时，appbar是否固定不会消失
>
> snap:当为true是自动获取焦点滑动出来，详情参考文档
>
> **当float为true时，snap必须为true**

## 



## 三、其他

### 1.网络请求

> 有时您可能需要处理大量数据，导致UI可能会挂起。
>
> 在这种情况下，与AsyncTask一样，在Flutter中，可以利用多个CPU内核来执行耗时或计算密集型任务。这是通过使用Isolates来完成的。
>
> 是一个独立的执行线程，它运行时不会与主线程共享任何内存。这意味着你不能从该线程访问变量或通过调用setState来更新你的UI。
>
> 我们来看一个简单的Isolate的例子，以及如何与主线程通信和共享数据以更新UI：

```dart
loadData() async {
    ReceivePort receivePort = new ReceivePort();
    await Isolate.spawn(dataLoader, receivePort.sendPort);

    // The 'echo' isolate sends it's SendPort as the first message
    SendPort sendPort = await receivePort.first;

    List msg = await sendReceive(sendPort, "https://jsonplaceholder.typicode.com/posts");

    setState(() {
      widgets = msg;
    });
  }

// the entry point for the isolate
  static dataLoader(SendPort sendPort) async {
    // Open the ReceivePort for incoming messages.
    ReceivePort port = new ReceivePort();

    // Notify any other isolates what port this isolate listens to.
    sendPort.send(port.sendPort);

    await for (var msg in port) {
      String data = msg[0];
      SendPort replyTo = msg[1];

      String dataURL = data;
      http.Response response = await http.get(dataURL);
      // Lots of JSON to parse
      replyTo.send(JSON.decode(response.body));
    }
  }

  Future sendReceive(SendPort port, msg) {
    ReceivePort response = new ReceivePort();
    port.send([msg, response.sendPort]);
    return response.first;
  }
```

“dataLoader”是在它自己的独立执行线程中运行的隔离区，您可以在其中执行CPU密集型任务，例如解析大于1万的JSON或执行计算密集型数学计算。

**显示进度条**

> 在Flutter中，这可以通过渲染Progress Indicator widget来实现。您可以通过编程方式显示Progress Indicator ， 通过布尔值通知Flutter在耗时任务发起之前更新其状态。
>
> 在下面的例子中，我们将build函数分解为三个不同的函数。如果showLoadingDialog为true（当widgets.length == 0时），那么我们展示ProgressIndicator，否则我们将展示包含所有数据的ListView。

```dart
void main() {
  runApp(new SampleApp());
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Sample App',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new SampleAppPage(),
    );
  }
}

class SampleAppPage extends StatefulWidget {
  SampleAppPage({Key key}) : super(key: key);

  @override
  _SampleAppPageState createState() => new _SampleAppPageState();
}

class _SampleAppPageState extends State<SampleAppPage> {
  List widgets = [];//返回请求到的数据

  @override
  void initState() {
    super.initState();
    loadData();
  }
    
    //加载数据
  loadData() async {
    String dataURL = "https://jsonplaceholder.typicode.com/posts";
    http.Response response = await http.get(dataURL);
    setState(() {
      widgets = JSON.decode(response.body);
    });
  }
    
  //判断是否请求到数据
  showLoadingDialog() {
    if (widgets.length == 0) {
      return true;
    }

    return false;
  }
  //判断是否还要继续加载
  getBody() {
    if (showLoadingDialog()) {
      return getProgressDialog();
    } else {
      return getListView();
    }
  }
  //获取加载条
  getProgressDialog() {
    return new Center(child: new CircularProgressIndicator());
  }
   //获取数据并且展示
    ListView getListView() => new ListView.builder(
        itemCount: widgets.length,
        itemBuilder: (BuildContext context, int position) {
            return getRow(position);
        });

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        appBar: new AppBar(
          title: new Text("Sample App"),
        ),
        body: getBody());
  }
  
  Widget getRow(int i) {
    return new Padding(padding: new EdgeInsets.all(10.0), child: new Text("Row ${widgets[i]["title"]}"));
  }
}
```

### 2.widget生命周期

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/1996162-b1e223789c7b9b75.png)

Flutter——Widget生命周期可以分为3个阶段：

1、实例化组件并添加到树， 即Navigator.push；

2、状态变化，即打开新的widget或者依赖的上级widget发生变化；

3、从树中移除,  即Navigator.pop。

```dart
/*构造函数：*/

同其它高级语言， 只执行一次；

/*initState：*/

插入到渲染树时调用，只执行一次。（类似Android Fragment的onCreateView函数）

/*didChangeDependencies：*/

1、在初始化initState后执行； 2、显示/关闭其它widget。 3、可执行多次；

/*1.didUpdateWidget：*/
 当你调用了 setState 将 Widget 的状态被改变时 didUpdateWidget 会被调用，Flutter 会创建一个新的 Widget 来绑定这个 State，并在这个方法中传递旧的 Widget ，因此如果你想比对新旧 Widget 并且对 State 做一些调整，你可以用它，另外如果你的某些 Widget 上涉及到 controller 的变更，要么一定要在这个回调方法中移除旧的 controller 并创建新的 controller 监听。

/*deative:*/

有点像Android的onStop函数， 在打开新的Widget或回到这个widget时会执行； 可执行多次；

/*dispose：*/

类似于Android的onDestroy，在执行Navigator.pop后会调用该办法，表示组件已销毁；某些情况下你的 Widget 被释放了，一个很经典的例子是 Navigator.pop 被调用，如果被释放的 Widget 中有一些监听或持久化的变量，你需要在 dispose 中进行释放，在我们学习 Bloc 或 Stream 时应该能了解 dispose ，在这个回调方法中去关闭 Stream。

/*reassemble：*/

点击闪电会执行，只用于调试时的hot reload。 release版本不会执行该函数。
```

> 常见业务场景：
>
> Widget A打开Widget B： Navigator.push(B)
>
> B构造函数--->B initState--->B didChangeDependencies--->B build--->A deactive--->A didChangeDependencies.
>
> Widget B退出： Navigator.pop
>
> A deactive--->A didChangeDependencies--->A build--->B deactive--->B dispose
>
> 可以看出， Flutter打开、关闭Widget时跟安卓、iOS的时序一样， 都是先处理即将显示的界面。

#### App生命周期

在Flutter中您可以通过挂接到WidgetsBinding观察并监听didChangeAppLifecycleState更改事件来监听生命周期事件

AppLifecycleState有4种状态：

- resumed - 应用程序可见并响应用户输入。这是来自Android的onResume
- inactive - 应用程序处于非活动状态，并且未接收用户输入。此事件在Android上未使用，仅适用于iOS
- paused - 应用程序当前对用户不可见，不响应用户输入，并在后台运行。通安卓的onStop
- suspending - 该应用程序将暂时中止。这在iOS上未使用

```dart
import 'package:flutter/widgets.dart';

class LifecycleWatcher extends StatefulWidget {
  @override
  _LifecycleWatcherState createState() => new _LifecycleWatcherState();
}

class _LifecycleWatcherState extends State<LifecycleWatcher> with WidgetsBindingObserver {
  AppLifecycleState _lastLifecyleState;

  /*插入到渲染树时调用，只执行一次。（类似Android Fragment的onCreateView函数）*/
  @override
  void initState() {
    super.initState();
    WidgetsBinding.instance.addObserver(this);
  }

  /*类似于Android的onDestroy， 在执行Navigator.pop后会调用该办法， 表示组件已销毁；*/
  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    super.dispose();
  }

  
  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    setState(() {
      _lastLifecyleState = state;
    });
  }

  @override
  Widget build(BuildContext context) {
    if (_lastLifecyleState == null)
      return new Text('This widget has not observed any lifecycle changes.', textDirection: TextDirection.ltr);
    return new Text('The most recent lifecycle state this widget observed was: $_lastLifecyleState.',
        textDirection: TextDirection.ltr);
  }
}

void main() {
  runApp(new Center(child: new LifecycleWatcher()));
}
```

### 3.手势监听—GestureDetector

使用GestureDetector，可以监听多种手势，例如

- Tap
  - `onTapDown`
  - `onTapUp`
  - `onTap`
  - `onTapCancel`
- Double tap
  - `onDoubleTap` 用户快速连续两次在同一位置轻敲屏幕.
- 长按
  - `onLongPress`
- 垂直拖动
  - `onVerticalDragStart`
  - `onVerticalDragUpdate`
  - `onVerticalDragEnd`
- 水平拖拽
  - `onHorizontalDragStart`
  - `onHorizontalDragUpdate`
  - `onHorizontalDragEnd`

例如，这里是一个GestureDetector，用于监听FlutterLogo的双击事件，双击时使其旋转:

```dart
AnimationController controller;
CurvedAnimation curve;

@override
void initState() {
  controller = new AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
  curve = new CurvedAnimation(parent: controller, curve: Curves.easeIn);
}

class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
        body: new Center(
          child: new GestureDetector(
            child: new RotationTransition(
                turns: curve,
                child: new FlutterLogo(
                  size: 200.0,
                )),
            onDoubleTap: () {
              if (controller.isCompleted) {
                controller.reverse();
              } else {
                controller.forward();
              }
            },
        ),
    ));
  }
}
```

可以通过gesturedetector回调来对ListView的item进行监听

```dart
  _getListData() {
    List<Widget> widgets = [];
    for (int i = 0; i < 100; i++) {
      widgets.add(new GestureDetector(
        child: new Padding(
            padding: new EdgeInsets.all(10.0),
            child: new Text("Row $i")),
        onTap: () {
          print('row tapped');
        },
      ));
    }
    return widgets;
  }
```

### 4.数据库和存储

#### (1)Shared preferences

> 可以用来存储一些简单的数据，可能是异步存储到磁盘，且不能保证数据持久化到磁盘，所以不建议使用来存储重要的数据

依赖

```yaml
dependencies:
  shared_preferences: ^2.0.6
```

导包

```dart
import 'package:shared_preferences/shared_preferences.dart';
```

example

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Center(
      child: RaisedButton(
        onPressed: _incrementCounter,
        child: Text('Increment Counter'),
        ),
      ),
    ),
  ));
}

_incrementCounter() async {
  SharedPreferences prefs = await SharedPreferences.getInstance();
  int counter = (prefs.getInt('counter') ?? 0) + 1;
  print('Pressed $counter times.');
  await prefs.setInt('counter', counter);
}
```

#### (2)SQFlite

```yaml

dependencies:
  sqflite: ^2.0.0+3
```

```dart
import 'package:sqflite/sqflite.dart';
```

> - 支持事务和批量
> - 打开时自动版本管理
> - 插入/查询/更新/删除查询的助手
> - 在 iOS 和 Android 上的后台线程中执行的 DB 操作



获取数据库路径，安卓和ios提供了获取数据库文件夹路径的方式，可以通过以下方式获得数据库路径

```dart
var databasesPath = await getDatabasesPath();
var path = join(databasesPath, dbName);

// Make sure the directory exists
try {
  await Directory(databasesPath).create(recursive: true);
} catch (_) {}
```



①打开数据库

```dart
var db = await openDatabase('my_db.db');

//以只读方式打开
// open the database in read-only mode
var db = await openReadOnlyDatabase(path);
```

②关闭数据库

```dart
await db.close();//往往在应用关闭时关闭
```

③**使用原生sql用例**

```dart
// 获取数据库位置
var databasesPath = await getDatabasesPath();
String path = join(databasesPath, 'demo.db');

// 删除数据库
await deleteDatabase(path);

// 打开数据库
Database database = await openDatabase(path, version: 1,
    onCreate: (Database db, int version) async {
  // 创建数据库同时创建表
  await db.execute(
      'CREATE TABLE Test (id INTEGER PRIMARY KEY, name TEXT, value INTEGER, num REAL)');
});

// 事务中插入记录
await database.transaction((txn) async {
  int id1 = await txn.rawInsert(
      'INSERT INTO Test(name, value, num) VALUES("some name", 1234, 456.789)');
  print('inserted1: $id1');
  int id2 = await txn.rawInsert(
      'INSERT INTO Test(name, value, num) VALUES(?, ?, ?)',
      ['another name', 12345678, 3.1416]);
  print('inserted2: $id2');
});

// 更新记录
int count = await database.rawUpdate(
    'UPDATE Test SET name = ?, value = ? WHERE name = ?',
    ['updated name', '9876', 'some name']);
print('updated: $count');

// 查询记录
List<Map> list = await database.rawQuery('SELECT * FROM Test');
List<Map> expectedList = [
  {'name': 'updated name', 'id': 1, 'value': 9876, 'num': 456.789},
  {'name': 'another name', 'id': 2, 'value': 12345678, 'num': 3.1416}
];
print(list);
print(expectedList);
assert(const DeepCollectionEquality().equals(list, expectedList));

// 查询记录数目
count = Sqflite
    .firstIntValue(await database.rawQuery('SELECT COUNT(*) FROM Test'));
assert(count == 2);

//删除记录
count = await database
    .rawDelete('DELETE FROM Test WHERE name = ?', ['another name']);
assert(count == 1);

// Close the database
await database.close();
```

④**使用sql助手**

```dart
SQL helpers 
Example using the helpers

final String tableTodo = 'todo';
final String columnId = '_id';
final String columnTitle = 'title';
final String columnDone = 'done';

class Todo {
  int id;
  String title;
  bool done;

  Map<String, Object?> toMap() {
    var map = <String, Object?>{
      columnTitle: title,
      columnDone: done == true ? 1 : 0
    };
    if (id != null) {
      map[columnId] = id;
    }
    return map;
  }

  Todo();

  Todo.fromMap(Map<String, Object?> map) {
    id = map[columnId];
    title = map[columnTitle];
    done = map[columnDone] == 1;
  }
}

class TodoProvider {
  Database db;

  Future open(String path) async {
    db = await openDatabase(path, version: 1,
        onCreate: (Database db, int version) async {
      await db.execute('''
create table $tableTodo ( 
  $columnId integer primary key autoincrement, 
  $columnTitle text not null,
  $columnDone integer not null)
''');
    });
  }

  Future<Todo> insert(Todo todo) async {
    todo.id = await db.insert(tableTodo, todo.toMap());
    return todo;
  }

  Future<Todo> getTodo(int id) async {
    List<Map> maps = await db.query(tableTodo,
        columns: [columnId, columnDone, columnTitle],
        where: '$columnId = ?',
        whereArgs: [id]);
    if (maps.length > 0) {
      return Todo.fromMap(maps.first);
    }
    return null;
  }

  Future<int> delete(int id) async {
    return await db.delete(tableTodo, where: '$columnId = ?', whereArgs: [id]);
  }

  Future<int> update(Todo todo) async {
    return await db.update(tableTodo, todo.toMap(),
        where: '$columnId = ?', whereArgs: [todo.id]);
  }

  Future close() async => db.close();
}
```

⑤**不使用数据库，只使用事物进行操作**

```dart
await database.transaction((txn) async {
  // Ok
  await txn.execute('CREATE TABLE Test1 (id INTEGER PRIMARY KEY)');
  
  // 不用在事物中使用数据库对象，否则会引起死锁
  //fales
  await database.execute('CREATE TABLE Test2 (id INTEGER PRIMARY KEY)');
});
```

⑥map对象的结果只允许读，若要修改，必须重新创建对象

```dart
List<Map<String, Object?>> records = await db.query('my_table');
// get the first record
Map<String, Object?> mapRead = records.first;
// Update it in memory...this will throw an exception
mapRead['my_column'] = 1;
// 错误，只允许读
// get the first record
Map<String, Object?> map = Map<String, Object?>.from(mapRead);
// Update it in memory now
map['my_column'] = 1;
```

⑦批量处理

使用批量处理对于大量数据操作效率更高

```dart
batch = db.batch();
batch.insert('Test', {'name': 'item'});
batch.update('Test', {'name': 'new_item'}, where: 'name = ?', whereArgs: ['item']);
batch.delete('Test', where: 'name = ?', whereArgs: ['item']);
results = await batch.commit();//要提交
```

例子

```dart
//原生的sql
void deleteBooks(List<BookShelf> bookList)async{
    var dbClient = await _sql.db;
    String in_book = "";
    for(int i=0;i<bookList.length;i++){
      in_book += "${bookList[i].bookId}";
      if(i != bookList.length-1){
        in_book+=",";
      }
    }
    dbClient.rawDelete("DELETE FROM ${ReadSQL.tableShelf} WHERE bookId IN ($in_book)");

  }

 //批量操作，该方法证明效率高
 void deleteBooks(List<BookShelf> bookList)async{
    var dbClient = await _sql.db;
    var batch = dbClient.batch();
    bookList.forEach((book){
      batch.rawDelete("DELETE FROM ${ReadSQL.tableShelf} WHERE bookId = ${book.bookId}");
    });
    batch.commit();//提交
  }
```

提交返回结果是有代价的，如果不关注提交返回的结果，则可以设置

```dart
await batch.commit(noResult: true);
```

默认情况下，批处理在遇到错误时立即停止（通常会还原未提交的更改）。您可以忽略错误，以便即使一个操作失败，也会运行并提交每个成功的操作：

```dart
await batch.commit(continueOnError: true);
```

⑧使用关键字作为库表名称

sqflite会自动转义为其添加双引号

```dart
db.query('table');
db.rawQuery('SELECT * FROM "table"');
  
```

⑨配置

```dart
_onConfigure(Database db) async {
  //添加支持级联删除
  await db.execute("PRAGMA foreign_keys = ON");
}

var db = await openDatabase(path, onConfigure: _onConfigure);
```

⑩预加载数据库

当第一次打开数据库时候可以选择调用onCreate回调来预加载数据

```dart
_onCreate(Database db, int version) async {
  // Database is created, create the table
  await db.execute(
    "CREATE TABLE Test (id INTEGER PRIMARY KEY, value TEXT)");
  // populate data
  await db.insert(...);
}

// Open the database, specifying a version and an onCreate callback
var db = await openDatabase(path,
    version: 1,
    onCreate: _onCreate);
```

##### 封装

往往只需要打开一次数据库即可，如果多次打开数据库会报错并且数据库被锁定，所以进行一个封装。

```dart
class Helper {
  final String path;
  Helper(this.path);
  Future<Database> _db;

  Future<Database> getDb() {
    _db ??= _initDb();
    return _db;
  }

  // Guaranteed to be called only once.
  Future<Database> _initDb() async {
    final db = await openDatabase(this.path);
    // do "tons of stuff in async mode"
    return db;
  }
}
```

