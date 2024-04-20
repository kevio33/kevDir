> https://www.finclip.com/news/f/44170.html

## 1.Flutter自定义视图

> [自定义视图](https://juejin.cn/post/6968655659373953061)

Flutter中的自定义视图可以通过`CustomPaint`实现，

**可以分为以下几步：**

- 创建继承`CustomPainter`的类。
- 在类中的`paint`方法绘制想要的内容：在这个方法中会得到画布canvas以及控件的尺寸Size，只需要利用画笔（paint）在画布（canvas）绘制相应的内容即可。
- 在类中的`shouldRepaint`方法返回需要重绘的条件：如果返回true则会在相应的值变化时会触发重绘。
- 如果需要做动画那么还需要在自定义类的构造函数中添加:`super(repaint:xxx)`就能自动的添加监听器。这样当值变化时就能自动的触发重绘。

### (1)CustomPaint

CustomPaint是一个能够自定义视图的类，通过传入CustomPainter作为子Widget实现视图绘制

```dart
const CustomPaint({
    Key? key,
    this.painter,
    this.foregroundPainter,
    this.size = Size.zero,
    this.isComplex = false,
    this.willChange = false,
    Widget? child,
})
```

- `painter`以及`foregroundPainter`都是`CustomPainter`对象，分别表示child的`背景`和`前景 `

### (2)CustomPainter

绘制流程主要是在CustomPainter中进行，继承CustomPainter后绘制图形主要用到三个对象：

- `paint`：画笔，主要是设置绘制图形的样式，颜色、渐变等等
- `canvas`：画布，主要可以设定绘制的图形， 如矩形、圆形、路径等 。还可以进行变换，如裁剪、平移、选择。
- `path`： 可以通过Path对象来**定义各种复杂的图形路径**，然后通过Canvas对象的drawPath方法来绘制该路径。 



**下面来一步步实现：**

#### ①继承CustomPainter

```dart
class ProgressRingPainter extends CustomPainter {
    //画笔
    Paint _paint;
    //进度
    final Animation<double> progress;
    //路径测量
    PathMetric pathMetric;

    ProgressRingPainter(this.progress) : super(repaint: progress) {
        Path  _path = Path();
        _path.addOval(Rect.fromCenter(center: Offset(0, 0), width: 90, height: 90));
        pathMetric = _path.computeMetrics().first;

        _paint = Paint()
            ..color = Colors.black38
            ..strokeWidth = 10
            ..style = PaintingStyle.stroke;
    }

    @override
    void paint(Canvas canvas, Size size) {
        ...
    }

    @override
    bool shouldRepaint(covariant ProgressRingPainter oldDelegate) {
        ...
    }
}
```

> - 构造方法中的` super:(repaint:progress) `，这个repaint需要是一个继承了`Listenable`的对象，以监听实现视图更新

#### ②绘制方法paint

```dart
@override
void paint(Canvas canvas, Size size) {
    canvas.clipRect(Offset.zero & size);
    canvas.translate(size.width / 2, size.height / 2);
    canvas.drawCircle(Offset(0, 0), size.width / 2 - 5, _paint);

    canvas.drawPath(
        pathMetric.extractPath(
            0,
            pathMetric.length * progress.value,
        ),
        Paint()
        ..color = Colors.pinkAccent
        ..strokeWidth = 10
        ..style = PaintingStyle.stroke);
}
```

#### ③触发重绘流程

就是当前进度条的值与之前不一致的时候，进行重绘

```dart
@override
bool shouldRepaint(covariant ProgressRingPainter oldDelegate) {
    return progress.value != oldDelegate.progress.value;
}
```



**自定义加载视图的完整代码**

```dart
class ProgressRingPainter extends CustomPainter {
    //画笔
    Paint _paint;
    //进度
    final Animation<double> progress;
    //路径测量
    PathMetric pathMetric;

    ProgressRingPainter(this.progress) : super(repaint: progress) {
        Path  _path = Path();
        _path.addOval(Rect.fromCenter(center: Offset(0, 0), width: 90, height: 90));
        pathMetric = _path.computeMetrics().first;

        _paint = Paint()
            ..color = Colors.black38
            ..strokeWidth = 10
            ..style = PaintingStyle.stroke;
    }

    @override
    void paint(Canvas canvas, Size size) {
        canvas.clipRect(Offset.zero & size);
        canvas.translate(size.width / 2, size.height / 2);
        canvas.drawCircle(Offset(0, 0), size.width / 2 - 5, _paint);
        canvas.drawPath(
            pathMetric.extractPath(
                0,
                pathMetric.length * progress.value,
            ),
            Paint()
            ..color = Colors.pinkAccent
            ..strokeWidth = 10
            ..style = PaintingStyle.stroke);
    }

    @override
    bool shouldRepaint(covariant ProgressRingPainter oldDelegate) {
        return progress.value != oldDelegate.progress.value;
    }
}
```



#### ④调用自定义视图

进度条文字是不断刷新的，因此可以通过两种方式刷新控件：

- setState重建
- 文字自身刷新，通过 `ValueListenableBuilder `，可以传入一个Listenable变量监听值，还可以传入builder来构建新的视图

**调用的完整代码**

```dart
class ProgressRing extends StatefulWidget {
    @override
    State<StatefulWidget> createState() => _StateProgressRing();
}

class _StateProgressRing extends State<ProgressRing>
    with SingleTickerProviderStateMixin {
    AnimationController controller;

    @override
    void initState() {
        super.initState();
        controller = AnimationController(//一个10s的动画效果
            vsync: this,
            duration: Duration(seconds: 10),
        )..repeat(reverse: true);
    }

    @override
    Widget build(BuildContext context) {
        return Scaffold(
            appBar: AppBar(
                title: Text('CustomPaint'),
            ),
            body: Center(
                child: CustomPaint(
                    size: Size(100, 100),
                    painter: ProgressRingPainter(controller),
                    child: Container(
                        width: 100,
                        height: 100,
                        child: Center(
                            child: ValueListenableBuilder(
                                valueListenable: controller,
                                builder: buildText,
                            ),
                        ),
                    ),
                ),
            ),
        );
    }

    @override
    void dispose() {
        controller.dispose();
        super.dispose();
    }

    Widget buildText(BuildContext context, double value, Widget child) {
        return Text("${(value * 100).toInt()}%");
    }
}
```





## 2.Flutter渲染

目前的一些跨平台方案，从前端渲染的角度来分类的话，大致可以分为以下几种方案。 

**WebView 渲染**

这种方案就很好理解，现在很多项目都会嵌入 H5 的页面。就是用 JavaScript 等前端技术进行开发，在客户端上用 WebView 来进行渲染。微信小程序目前使用的就是这种方案。

- 它的优点很明显，使用成熟的前端技术进行开发，学习成本低，开发效率高，并且支持动态发布代码。

- 但缺点也很明显，在性能体验上，和原生还是存在较大差距的。

**原生控件渲染**

既然 WebView 的性能不够好，于是就有了使用原生控件进行渲染的方案。这种方案，同样也是使用 JavaScript 开发，区别是它最终是调用原生控件进行渲染的。这种方案的代表是 Facebook 的 **React Native**。

- 由于使用原生控件进行渲染，性能体验也会更接近原生。但也只是更接近，和原生还是有差距的，因为它需要**频繁的进行 JavaScript 和原生之间的通信，这个通信效率是比较低**的。

- 另外，由于需要**适配各个平台的控件，那就有可能出现，系统控件更新了，而框架本身还没有更新**，由此产生了一些问题。换句话说，这种方案是**受到原生控件限制**的。

**绘图引擎渲染**

在前端，如果完全不使用原生控件，我们可以**通过系统的绘图 API 绘制出一个用户界面**。从这个角度出发，可以在各个平台使用一个统一接口的绘图引擎来进行界面绘制，这个引擎最终调用的是系统的 API 绘制的。这样的话，它的性能可以做到接近原生，并且又不受原生控件的限制，在不同平台上能够做到 UI 统一。 

 **下图主要将三种渲染方式进行介绍**

![a66d44759a5e14fbc1f7f03cbfec6729.png](FlutterView.assets/333d0a309faa4555ae0c904e5897674e.png) 

> **绘图引擎 Skia**
>
> Flutter 使用 Skia 作为它的绘图引擎。Skia 已经被 Google 收购，目前很多 Google 旗下的产品都是用 Skia 绘制的，包括 Android。
>
> > Android 内置了 Skia，但 iOS 没有，所以在打 iOS 安装包的时候，会把 Skia 一起打进去。这就导致了，用同一份 Flutter 代码打包之后，iOS 的包要比 Android 的包大一些。

### (1)Flutter如何实现跨平台的

> https://developer.aliyun.com/article/914437

Flutter 是由 Google 开发的，一个跨平台 UI 解决方案。换句话说，它原则上只管 UI 的问题，如果涉及到平台本身的一些功能，比如调用蓝牙、摄像头，一般还是需要原生代码去操作。但现在也会有一些第三方库帮我们解决这些问题。 





## 3.Flutter打包

和原生打包有什么区别？