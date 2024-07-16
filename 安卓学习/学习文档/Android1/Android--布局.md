# Android学习文档--2



## 布局

### 概述

布局定义了应用中界面的结构，布局的所有元素都使用`View`和`ViewGroup`对象层次结构进行构建:

> **View**--通常用于绘制用户可看到并与之交互的内容。View对象称为`“微件”`，例如Button或TextView等。
>
> **ViewGroup**--则是不可见的容器，用于定义View和其他ViewGroup对象的布局结构，ViewGroup对象通常称为“布局”，例如LinearLayout，ConstraintLayout



> **可以通过两种方式声明界面元素:**
>
> - XML文件中
> - 运行时实例化布局元素

> **Android中常用的5大布局方式有以下几种：**
>
> - 线性布局（LinearLayout）：按照垂直或者水平方向布局的组件。
> - 帧布局（FrameLayout）：组件从屏幕左上方布局组件。
> - 表格布局（TableLayout）：按照行列方式布局组件。
> - 相对布局（RelativeLayout）：相对其它组件的布局方式。
> - 绝对布局（AbsoluteLayout）：按照绝对坐标来布局组件。



activity文件加载绑定XML中的布局

![1606984464879](Android--布局.assets/1606984464879.png)





values目录下存放的是一些属性变量，可以在布局时候引用

![1606998617100](Android--布局.assets/1606998617100.png)

例如：应用String.xml文件里面name=tv_1的值作为text内容

![1606998661266](Android--布局.assets/1606998661266.png)

#### **①ID属性：**

尽量为每一个View对象声明创建一个id，因为在后续会通过搜索结构树来查找该组件。声明ID的格式有两种：

- > ```xml
  > android:id="@+id/my_button"
  > 字符串开头处的 @ 符号指示 XML 解析器应解析并展开 ID 字符串的其余部分，并将其标识为 ID 资源。加号 (+) 表示这是一个新的资源名称，必须创建该名称并将其添加到项目的资源（在 R.java 文件中）内.
  > ```

- > ```xml
  > android:id="@android:id/empty"
  > Android 框架还提供许多其他 ID 资源。引用 Android 资源 ID 时，不需要加号，但必须添加 android 软件包命名空间。添加 android 软件包命名空间后，我们现在将从 android.R 资源类而非本地资源类引用 ID。
  > ```

#### ②viewbinding

在Android Studio3.6及更高版本中**通过使用viewbinding来替代`findViewById`**。

**启用绑定**

要在某个模块中启用视图绑定，将viewBinding元素添加到gradle中：

```groovy
android {
    ...
        viewBinding {
            enabled = true
        }
}
```

> `gradle 8.2`更换了写法
>
> ```groovy
> buildFeatures {
>        viewBinding true
> }
> ```

如果您希望在生成绑定类时**忽略某个布局文件**，请将 `tools:viewBindingIgnore="true"` 属性添加到相应布局文件的根视图中：

```xml
<LinearLayout
    ...
    tools:viewBindingIgnore="true" >
    ...
</LinearLayout>
```



**绑定过程：**

为某个模块**`启用视图绑定功能后`**，系统会为该模块中包含的**`每个 XML 布局文件生成一个绑定类`**。每个绑定类均包含对根视图以及**`具有 ID`** 的所有视图的**引用**。系统会通过以下方式生成绑定类的名称：将 XML 文件的名称转换为驼峰式大小写，并在末尾添加“Binding”一词。

例如某个布局文件`result_profile.xml`:

```xml
<LinearLayout ... >
    <TextView android:id="@+id/name" />
    <ImageView android:cropToPadding="true" />
    <Button android:id="@+id/button"
            android:background="@drawable/rounded_button" />
</LinearLayout>
```

> - 所生成的绑定类的名称就为 **`ResultProfileBinding`**。此类具有两个字段：一个是名为 `name` 的 `TextView`，另一个是名为 `button` 的 `Button`。该布局中的 `ImageView` 没有 ID，因此绑定类中不存在对它的引用。
> - 每个绑定类还包含一个 `getRoot()` 方法，用于为相应布局文件的根视图提供直接引用。在此示例中，`ResultProfileBinding` 类中的 `getRoot()` 方法会返回 `LinearLayout` 根视图。

##### Activity使用视图绑定

> 在 Activity 的 `onCreate()` 方法中执行以下步骤：
>
> 1. 调用生成的绑定类中包含的静态 `inflate()` 方法。此操作会创建该绑定类的实例以供 Activity 使用。
>
> 2. 通过调用 `getRoot()` 方法获取对根视图的引用。
>
> 3. 将根视图传递到 `setContentView()`，使其成为屏幕上的活动视图。
>
>    ```java
>     private ResultProfileBinding binding;//绑定类的名称
>    
>        @Override
>        protected void onCreate(Bundle savedInstanceState) {
>            super.onCreate(savedInstanceState);
>            binding = ResultProfileBinding.inflate(getLayoutInflater());
>            View view = binding.getRoot();
>            setContentView(view);
>        }
>    ```
>
>    然后可以使用该绑定类实例来引用任何视图：
>
>    ```java
>    binding.getName().setText(viewModel.getName());
>    binding.button.setOnClickListener(new View.OnClickListener() {
>        viewModel.userClicked()
>    });
>    ```

##### Fragment使用View绑定

```java
private ResultProfileBinding binding;

    @Override
    public View onCreateView (LayoutInflater inflater,
                              ViewGroup container,
                              Bundle savedInstanceState) {
        binding = ResultProfileBinding.inflate(inflater, container, false);
        View view = binding.getRoot();
        return view;
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        binding = null;
    }
```

##### 与findViewById区别：

- **Null 安全**：由于视图绑定会创建对视图的直接引用，因此不存在因视图 ID 无效而引发 Null 指针异常的风险。此外，如果视图仅出现在布局的某些配置中，则绑定类中包含其引用的字段会使用 `@Nullable` 标记。
- **类型安全**：每个绑定类中的字段均具有与它们在 XML 文件中引用的视图相匹配的类型。这意味着不存在发生类转换异常的风险。



#### ③databinding

> [databinding详解](https://juejin.cn/post/6844904085800353805)

`DataBinding`是一种更为高级的视图绑定框架，允许你在布局文件中直接绑定数据。于Android Studio 1.3引入，相较于`ViewBinding`：

- 支持双向数据绑定，即当数据变化时，视图也会相应地更新，反之亦然。 
- 具有更强大的表达式语言，支持一些逻辑和运算符。

**使用**

```groovy
android {
    ...
        dataBinding {
            enabled = true
        }
}
```



`DataBinding`使用布局文件中的数据绑定表达式将数据直接与视图绑定。 

```xml
<!--示例代码-->
<!--布局文件 activity_main.xml-->
<!--要以layout开头-->
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="user"
            type="com.example.User" />
    </data>

    <LinearLayout>
    	<TextView
            android:id="@+id/myTextView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.name}" />
        <EditText
            android:id="@+id/password"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:inputType="textPassword"
            android:text="@={user.password}"
            app:layout_constraintBottom_toTopOf="@+id/login"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/userName" />
        <Button
            android:id="@+id/login"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="@{() -> user.login()}"
            android:text="Login"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/password" />
    </LinearLayout>
    
    
</layout>
```

> 如果**@后面不加等号，那就只是单向绑定**，只能由 ViewModel 将数据变化通知到界面。
>
> **@加了等号，才是双向绑定**，即界面上的数据改变才能传递给到 ViewModel。 
>
> Button的`android:onClick`属性值，设置为了**@{() -> user.login()}**，这就是将该按钮的点击事件绑定到User的`login()`方法  

```java
// 在 Activity 中使用 DataBinding
ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
User user = new User("John Doe");
binding.setUser(user);
```







#### **布局位置**

可以通过调用 `getLeft()` 方法和 `getTop()` 方法来检索视图的位置。例如：***如果 `getLeft()` 返回 20，则表示视图位于其直接父项左边缘向右 20 个像素处。***，此外系统还提供了getRight()和getBottom来计算右/下边缘坐标，例如：***调用 `getRight()` 类似于进行以下计算：`getLeft() + getWidth()`。也就是该View最右边距离父视图最左边的距离，而不是视图右边距离父视图右边的距离***



#### **尺寸、内边距和外边距**

View尺寸拥有两对宽度和高度值：

- `getMeasuredWidth()`和`getMeasuredHeight()`来获得定义视图希望在其父项内具有的大小
- `getWidth`和`getHeight`来获得视图在屏幕上的实际尺寸。这些值可以（但不必）与测量宽度和测量高度不同。

**设置、获取内边距**

- `setPadding(int,int,int,int);`设置内边距
- `getPadding(Left/Right/Top/Bottom)`来获取内边距

### 1.LinearLayout

安卓中，一般距离单位使用**dp**（为了自适应），文字大小使用**sp**

![1606986131604](Android--布局.assets/1606986131604.png)

![1606987066204](Android--布局.assets/1606987066204.png)

**！！！！layout_width 和 width区别**

- layout_width：是相对于父容器的
- width：是相对于自身的



**常用线性布局的信息**

```xml
<LinearLayout
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical"  #排列方式，线性布局特有
              >
    <View
          android:layout_width="match_parent" #宽度
          android:layout_height="200dp"  #高度
          android:background="#0000FF"  
          android:layout_margin="30dp"  
          android:layout_gravity="center"  #设置显示方式，center为在容器正中央
          android:layout_weight="1" #权重，对两个元素的占比进行等比切分（在父容器减去两个容器占比和剩下                                        的部分进行平分）
          />
</LinearLayout>
```

![1606988449398](Android--布局.assets/1606988449398.png)

![1606988460415](Android--布局.assets/1606988460415.png)

> 1、 `android :orientation="vertical"`表示该布局下的元素垂直排列；
> 2、`android:layout_gravity="center_horizontal"`表示该布局在父布局里水平居中，此时其父布局必须拥有`android:orientation="vertical"`属性；
>
> 3、`android:layout_gravity="center_vertical"`表示该布局在父布局里垂直居中，此时其父布局必须应设置成 `android:orientation="horizontal"`属性（默认为该属性），且其父布局的高度应设置为 android:layout_height="fill_parent"属性；
>
> 4、`android:gravity="center_horizontal"`表示该布局下的统一水平线的元素在垂直方向居中（**对于图片和文字水平线上面对齐很有帮助**）；
>
> **5、内部元素居中：在LinearLayout中写属性` android:gravity=”center”  `即可**

### 2.RelativeLayout

> 参考——https://blog.csdn.net/csdnbian/article/details/108818528

> **常用属性**
>
> - **相对于父控件**：
>
>      `android:layout_alignParentTop `     控件的顶部与父控件的顶部对齐;
>
>      `android:layout_alignParentBottom`  控件的底部与父控件的底部对齐;
>
>      `android:layout_alignParentLeft`      控件的左部与父控件的左部对齐;
>
>      `android:layout_alignParentRight  `   控件的右部与父控件的右部对齐;
>
> - **相对给定Id控件**
>
>      `android:layout_above` 控件的底部置于给定ID的控件之上;
>
>      `android:layout_below `    控件的顶部置于给定ID的控件之下;
>
>      `android:layout_toLeftOf `   控件的右边缘与给定ID的控件左边缘对齐;
>
>      `android:layout_toRightOf` 控件的左边缘与给定ID的控件右边缘对齐;
>
>      `android:layout_alignBaseline`  控件的baseline与给定ID的baseline对齐;
>
>      `android:layout_alignTop `       控件的顶部边缘与给定ID的顶部边缘对齐;
>
>      `android:layout_alignBottom  ` 控件的底部边缘与给定ID的底部边缘对齐;
>
>      `android:layout_alignLeft   `    控件的左边缘与给定ID的左边缘对齐;
>
>      `android:layout_alignRight  `    控件的右边缘与给定ID的右边缘对齐;
>
> - **居中**：
>
>      `android:layout_centerHorizontal `水平居中;
>      
>      `android:layout_centerVertical `   垂直居中;
>      
>      `android:layout_centerInParent  `父控件的中央;
>

相对其他元素进行布局

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <View
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:id="@+id/view_1"  
        android:background="#000000"/>

    <View
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:background="#ff0000"
        android:id="@+id/view_2"  
        android:layout_below="@id/view_1"  
        android:layout_toLeftOf="@id/view_1"/>  <!--相对于view1左边布局，view2的右边缘与view1的左边缘对齐-->
     

</RelativeLayout>
```



### 3.ConstraintLayout

ConstraintLayout与RelativeLayout相似，都是根据同级视图与布局之间的关系进行布局，但灵活性要高于RelativeLayout

**约束条件**

使用ConstraintLayout，**必须要为该视图添加至少一个水平约束条件和一个垂直约束条件**。例如下面这个例子

![image-20210926181752865](Android--布局.assets/image-20210926181752865.png)

> **图1的C没有垂直方向上的约束，所以在绘制布局时会显示在顶部**



**添加依赖**

```groovy
dependencies {
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
    // To use constraintlayout in compose
    implementation "androidx.constraintlayout:constraintlayout-compose:1.0.0-beta02"
}
```

**constraintLayout常用属性:**

![image-20210927140803132](Android--布局.assets/image-20210927140803132.png)

> 上面比较有意思的是：
>
> **`layout_constraintBaseline_toBaselineOf`** ***用于文本对齐***
>
> **Baseline**指的是文本基线，举个例子：
>
> ![image-20210926182917452](Android--布局.assets/image-20210926182917452.png)
>
> 如图所示，两个TextView的高度不一致，但是又希望他们**文本对齐**，这个时候就可以使用该属性，代码如下：
>
> ```xml
>  <TextView
>         android:id="@+id/TextView1"
>         .../>
> 
>     <TextView
>         android:id="@+id/TextView2"
>         ...
>         app:layout_constraintLeft_toRightOf="@+id/TextView1" 
>         app:layout_constraintBaseline_toBaselineOf="@+id/TextView1"/>
> ```
>
> ![image-20210926182953195](Android--布局.assets/image-20210926182953195.png)



使用ConstraintLayout**必须要约定每一个view的位置**，下面就是一个最简单的例子

```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ConstriantLayoutDemo">


    <Button
        android:id="@+id/cons_btn1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="btn1"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/cons_btn2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="btn2"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/cons_btn1" />

</androidx.constraintlayout.widget.ConstraintLayout>
```



**全部约束属性设置为parent，会达到居中的效果**

```xml
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ImageView
        android:id="@+id/iv_beauty"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        android:src="@drawable/beauty"
        android:layout_width="200dp"
        android:layout_height="200dp" />

</android.support.constraint.ConstraintLayout>
```

![img](Android--布局.assets/9271486-3de42b3303493acc.jpg)



#### **bias(偏移量)**

![image-20210927141201353](Android--布局.assets/image-20210927141201353.png)

如果我想`ImageView`相对于父布局左边间隔`1/10`个父布局长度，相对于父布局上边间隔`2/10`个父布局高度，我们该怎实现呢？看代码：

```xml
<ImageView
        android:id="@+id/iv_beauty"
        app:layout_constraintHorizontal_bias="0.1"//左偏移
        app:layout_constraintVertical_bias="0.2"//下偏移
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        ... />
```

> 这里需要注意的是：
>
> - 想要设置偏移，必须先将控件设置父布局居中。
> - 偏移的长度，如横向的偏移，是父布局宽度减去`ImageView`宽度的剩下的10%



#### **Circular positioning**

**圆形定位**：以一个控件为圆心设置角度和半径定位

![image-20210927141237163](Android--布局.assets/image-20210927141237163.png)

圆形定位，就是以目标控件为圆心，通过设置角度和半径确定我们当前控件的位置，如官方图：

![img](Android--布局.assets/9271486-5e66d03342f5ce55.jpg)

```xml
<ImageView
        android:id="@+id/iv_biu"
        ...../>

    <ImageView
        android:id="@+id/iv_girl"
        ....
        app:layout_constraintCircle="@+id/iv_biu"
        app:layout_constraintCircleRadius="120dp"
        app:layout_constraintCircleAngle="35"
        .../>
```

该属性可以设置一些动画

![img](Android--布局.assets/9271486-9b04c9d60b693c88.gif)



#### **Percent dimension**

**百分比布局**

![image-20210927141401936](Android--布局.assets/image-20210927141401936.png)

如果我们想将`ImageView`的宽度设置为父布局宽度的70%，这个时候我们就可以考虑使用百分比属性了，代码如下：

```xml
 <ImageView
        android:id="@+id/iv_beauty"
        ....
        app:layout_constraintWidth_default="percent"//先设置为百分比
        app:layout_constraintWidth_percent="0.7"//在设置百分比参数
        .... />
```

![img](Android--布局.assets/9271486-d34c0dc2add176ca.jpg)



#### **Ration比例**

![image-20210927141428928](Android--布局.assets/image-20210927141428928.png)

将控件比如`ImageView`设置长:宽=2:1

```xml
 <ImageView
        android:id="@+id/iv_beauty"
        android:layout_width="200dp"
        android:layout_height="0dp"
        app:layout_constraintDimensionRatio="2:1"//宽高比
       ..../>
```

> 如果需要按照比例设置控件，这里需要先将`layout_width`或者`layout_height`一方设置为`0dp`，另一个设置为正常比例，接着给`layout_constraintDimensionRatio`设置比例



#### **Chain Style(约束链类型)**

![image-20210927141502486](Android--布局.assets/image-20210927141502486.png)

约束链声明了在某一个方向上的布局方式(和Flutter中Row组件的MainAxisAlignment属性类似)

![img](Android--布局.assets/9271486-42765eab507b1a6b.gif)

> - `spread`：视图均匀分布。
> - `spread_inside`：除了约束链的头部和尾部贴在两边的约束上，其余均匀分布。
> - `packed`：将视图打包在一起，默认居中。

```xml
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- 在第一个控件设置chain_style，第一个和最后一个ImageView也要设置
    好约束，上下控件也要互为约束 -->
    <ImageView
        android:id="@+id/iv_beauty"
        android:layout_width="200dp"
        android:layout_height="100dp"
        android:scaleType="centerCrop"
        android:src="@drawable/beauty"
        app:layout_constraintBottom_toTopOf="@+id/iv_girl"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_chainStyle="spread" />

    <ImageView
        android:id="@+id/iv_girl"
        android:layout_width="200dp"
        android:layout_height="100dp"
        android:scaleType="centerCrop"
        android:src="@drawable/girl"
        app:layout_constraintBottom_toTopOf="@+id/iv_biu"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/iv_beauty" />

    <ImageView
        android:id="@+id/iv_biu"
        android:layout_width="200dp"
        android:layout_height="100dp"
        android:scaleType="centerCrop"
        android:src="@drawable/biu"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/iv_girl" />


</android.support.constraint.ConstraintLayout>
```



#### **Barrier**

可以在多个view之间建立屏障，这样当一边移动时，屏障另一边也会相对移动：

![img](Android--布局.assets/9271486-fe21f14dc464d48f.gif)

```xml
<TextView
          android:id="@+id/TextView1"
          .... />

<TextView
          android:id="@+id/TextView2"
          .... />

<!--在TextView1,TextView2右边-->
<android.support.constraint.Barrier
                                    android:id="@+id/barrier"
                                    android:layout_width="wrap_content"
                                    android:layout_height="wrap_content"
                                    app:barrierDirection="right"
                                    app:constraint_referenced_ids="TextView1,TextView2" />

<TextView
          android:id="@+id/TextView3"
          .....
          app:layout_constraintLeft_toRightOf="@+id/barrier" />
```

> `app:barrierDirection`为屏障所在的位置，可设置的值有：bottom、end、left、right、start、top
> `app:constraint_referenced_ids`为屏障引用的控件，可设置多个(用“,”隔开)



#### **Group**

现在有3个并排的TextView，用Group把TextView1和TextView3归为一组，再设置这组控件的可见性，如下所示：

```xml
<android.support.constraint.Group
        android:id="@+id/group"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="invisible"
        app:constraint_referenced_ids="TextView1,TextView3" />
```

![img](Android--布局.assets/2787721-99d23bdc50679804.png)





> 参考--
>
> https://www.jianshu.com/p/958887ed4f5f
>
> https://www.jianshu.com/p/17ec9bd6ca8a



### 4.MotionLayout

MotionLayout是ConstraintLayout子类，可以帮助管理应用中的运动和view动画，且MotionLayout还能够为任何布局属性添加动画效果。

> *`MotionLayout` 仅适用于其直接子级，不支持嵌套布局结构或 Activity 转换。*

```xml
<!--替换掉标签-->
<!-- before: ConstraintLayout -->
<androidx.constraintlayout.widget.ConstraintLayout .../>
<!-- after: MotionLayout -->
<androidx.constraintlayout.motion.widget.MotionLayout .../>
```



下面是一个完整实例用于实现动图功能：

![img](Android--布局.assets/motion-simple.gif)

```xml
<androidx.constraintlayout.motion.widget.MotionLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/motionLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutDescription="@xml/scene_01"//运动信息
        tools:showPaths="true">

        <View
            android:id="@+id/button"
            android:layout_width="64dp"
            android:layout_height="64dp"
            android:background="@color/colorAccent"
            android:text="Button" />

    </androidx.constraintlayout.motion.widget.MotionLayout>
```

> **创建 MotionScene**：在之前的 `MotionLayout` 示例中，`app:layoutDescription` 属性引用一个 MotionScene。**MotionScene 是一个 XML 资源文件，其中包含相应布局的所有运动描述**。为了将布局信息与运动描述分开，每个 `MotionLayout` 都引用一个单独的 MotionScene。*请注意，MotionScene 中的定义优先于 `MotionLayout` 中的任何类似定义。*

```xml
 <?xml version="1.0" encoding="utf-8"?>
    <MotionScene xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:motion="http://schemas.android.com/apk/res-auto">

        <Transition
            motion:constraintSetStart="@+id/start"
            motion:constraintSetEnd="@+id/end"
            motion:duration="1000">
            <OnSwipe
                motion:touchAnchorId="@+id/button"
                motion:touchAnchorSide="right"
                motion:dragDirection="dragRight" />
        </Transition>

        <ConstraintSet android:id="@+id/start">
            <Constraint
                android:id="@+id/button"
                android:layout_width="64dp"
                android:layout_height="64dp"
                android:layout_marginStart="8dp"
                motion:layout_constraintBottom_toBottomOf="parent"
                motion:layout_constraintStart_toStartOf="parent"
                motion:layout_constraintTop_toTopOf="parent" />
        </ConstraintSet>

        <ConstraintSet android:id="@+id/end">
            <Constraint
                android:id="@+id/button"
                android:layout_width="64dp"
                android:layout_height="64dp"
                android:layout_marginEnd="8dp"
                motion:layout_constraintBottom_toBottomOf="parent"
                motion:layout_constraintEnd_toEndOf="parent"
                motion:layout_constraintTop_toTopOf="parent" />
        </ConstraintSet>

    </MotionScene>
```

> - `<Transition>` 包含运动的基本定义。
>
>   - `motion:constraintSetStart` 和 `motion:constraintSetEnd` 指的是运动的端点。这些端点在 MotionScene 后面的 `<ConstraintSet>` 元素中定义。
>   - `motion:duration` 指定完成运动所需的毫秒数。
>
> - `<OnSwipe>` 可让您通过轻触控制运动。
>
>   - `motion:touchAnchorId` 指的是您可以滑动并拖动的视图。
>   - `motion:touchAnchorSide` 表示我们从右侧拖动视图。
>   - `motion:dragDirection` 表示拖动的进度方向。例如，`motion:dragDirection="dragRight"` 表示当您向右拖动时，进度会增加。
>
> - `<ConstraintSet>` 是定义描述您的运动的各种限制条件的位置。在此示例中，我们为运动的每个端点定义一个 `ConstraintSet`。这些端点垂直居中（通过 `app:layout_constraintTop_toTopOf="parent"` 和 `app:layout_constraintBottom_toBottomOf="parent"`）。在水平方向上，端点位于屏幕最左侧和最右侧。
>
> - 在 `<Constraint>` 中，您可以使用 `<CustomAttribute>` 元素为不仅仅与位置相关的属性或 `View` 属性指定转换。
>
>   ```xml
>    <Constraint
>           android:id="@+id/button" ...>
>           <CustomAttribute
>               motion:attributeName="backgroundColor"
>               motion:customColorValue="#D81B60"/>
>       </Constraint>
>   ```

> 更多示例参考文档--https://developer.android.google.cn/training/constraint-layout/motionlayout/examples



### 5.GridLayout

> 参考——https://blog.csdn.net/qq_62972842/article/details/127747795

**(1)特点**

> - GridLayout布局使用虚细线将布局划分为行、列和单元格，也支持一个控件在行、列上都有交错排列。
> - 可以自己设置布局中组件的排列方式
> - 可以自定义网格布局有多少行、多少列
> - 可以直接设置组件位于某行某列
> - 可以设置组件横跨几行或者几列
>
> **布局属性**
>
> | 属性          | 含义     |
> | ------------- | -------- |
> | rowCount      | 行数     |
> | columnCount   | 列数     |
> | layout_width  | 布局宽度 |
> | layout_height | 布局高度 |
>
> **针对子布局属性**
>
> | 属性              | 含义                       |
> | ----------------- | -------------------------- |
> | layout_row        | 子控件在表格中的行数       |
> | layout_column     | 子控件在表格中的列数       |
> | layout_rowSpan    | 跨行数，需要子元素指定高度 |
> | layout_columnSpan | 跨列数，需要子元素指定宽度 |

```xml
<GridLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp"
            android:columnCount="5"
            android:rowCount="6">
    <Button></Button>
    <Button></Button>
</GridLayout>
```

> **实例二：**
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
>     xmlns:tools="http://schemas.android.com/tools"
>     android:layout_width="match_parent"
>     android:layout_height="match_parent"
>     android:background="@drawable/background"
>     android:gravity="center_horizontal"
>     android:orientation="vertical"
>     android:padding="15dp"
>     tools:context=".MainActivity">
> 
>     <TextView
>         android:layout_width="match_parent"
>         android:layout_height="wrap_content"
>         android:layout_marginTop="30dp"
>         android:background="@drawable/custom_border"
>         android:gravity="right"
>         android:text="0123456789"
>         android:textColor="#0000ff"
>         android:textSize="20sp"/>
> 
>     <GridLayout
>         android:layout_width="wrap_content"
>         android:layout_height="wrap_content"
>         android:layout_marginTop="10dp"
>         android:columnCount="5"
>         android:rowCount="6">
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="0"
>             android:layout_column="0"
>             android:layout_marginRight="5dp"
>             android:text="MC"
>             android:textSize="15dp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="0"
>             android:layout_column="1"
>             android:layout_marginRight="5dp"
>             android:text="MR"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="0"
>             android:layout_column="2"
>             android:layout_marginRight="5dp"
>             android:text="MS"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="0"
>             android:layout_column="3"
>             android:layout_marginRight="5dp"
>             android:text="M+"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="0"
>             android:layout_column="4"
>             android:layout_marginRight="5dp"
>             android:text="M-"
>             android:textSize="15sp"/>
> 
>         <!--   第二排    -->
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="1"
>             android:layout_column="0"
>             android:layout_marginRight="5dp"
>             android:text="←"
>             android:textSize="15dp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="1"
>             android:layout_column="1"
>             android:layout_marginRight="5dp"
>             android:text="CE"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="1"
>             android:layout_column="2"
>             android:layout_marginRight="5dp"
>             android:text="C"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="1"
>             android:layout_column="3"
>             android:layout_marginRight="5dp"
>             android:text="±"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="1"
>             android:layout_column="4"
>             android:layout_marginRight="5dp"
>             android:text="√"
>             android:textSize="15sp"/>
> 
>         <!--        第三行        -->
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="2"
>             android:layout_column="0"
>             android:layout_marginRight="5dp"
>             android:text="7"
>             android:textSize="15dp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="2"
>             android:layout_column="1"
>             android:layout_marginRight="5dp"
>             android:text="8"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="2"
>             android:layout_column="2"
>             android:layout_marginRight="5dp"
>             android:text="9"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="2"
>             android:layout_column="3"
>             android:layout_marginRight="5dp"
>             android:text="/"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="2"
>             android:layout_column="4"
>             android:layout_marginRight="5dp"
>             android:text="%"
>             android:textSize="15sp"/>
> 
>         <!--    第四行    -->
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="3"
>             android:layout_column="0"
>             android:layout_marginRight="5dp"
>             android:text="4"
>             android:textSize="15dp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="3"
>             android:layout_column="1"
>             android:layout_marginRight="5dp"
>             android:text="5"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="3"
>             android:layout_column="2"
>             android:layout_marginRight="5dp"
>             android:text="6"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="3"
>             android:layout_column="3"
>             android:layout_marginRight="5dp"
>             android:text="*"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="3"
>             android:layout_column="4"
>             android:layout_marginRight="5dp"
>             android:text="1/X"
>             android:textSize="15sp"/>
> 
>         <!--        第五行         -->
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="4"
>             android:layout_column="0"
>             android:layout_marginRight="5dp"
>             android:text="1"
>             android:textSize="15dp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="4"
>             android:layout_column="1"
>             android:layout_marginRight="5dp"
>             android:text="2"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="4"
>             android:layout_column="2"
>             android:layout_marginRight="5dp"
>             android:text="3"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="4"
>             android:layout_column="3"
>             android:layout_marginRight="5dp"
>             android:text="-"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="97dp"
>             android:layout_row="4"
>             android:layout_rowSpan="2"
>             android:layout_column="4"
>             android:layout_marginRight="5dp"
>             android:text="="
>             android:textSize="15sp"/>
> 
> <!--         第六行           -->
>         <Button
>             android:layout_width="140dp"
>             android:layout_height="wrap_content"
>             android:layout_row="5"
>             android:layout_column="0"
>             android:layout_columnSpan="2"
>             android:layout_marginRight="5dp"
>             android:text="0"
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="5"
>             android:layout_column="2"
>             android:layout_marginRight="5dp"
>             android:text="."
>             android:textSize="15sp"/>
> 
>         <Button
>             android:layout_width="68dp"
>             android:layout_height="wrap_content"
>             android:layout_row="5"
>             android:layout_column="3"
>             android:layout_marginRight="5dp"
>             android:text="+"
>             android:textSize="15sp"/>
>     </GridLayout>
> </LinearLayout>
> 
> ```
>
>  ![在这里插入图片描述](Android--布局.assets/5b92b814ef0d440192ef60083da59b30.png) 



### 6.FrameLayout

> https://blog.csdn.net/weixin_63975980/article/details/127785152

帧布局是一种层叠式布局，后添加的元素会覆盖在先添加的元素上面（默认添加元素都会在左上角）。在例如地图上构建POI等有帮助

**常用属性：**

>  ![在这里插入图片描述](Android--布局.assets/b2116c17a9474b3a93e5399a77131643.png)

 

### 7.TableLayout

> https://blog.csdn.net/qq_62277763/article/details/128560369

 TableLayout**继承**自`LinearLayout`, 因此它**完全支持**LinearLayout所支持的属性，此外，它还有其他的常用属性。 

在TableLayout中，每一行通过`TableRow`来包裹

> **属性**
>
> 列号都是从0开始算的, 可以设置多个,用逗号隔开比如"0,2", 所有列都生效,则用"*"号即可
>
> | ***\*属性名称\****                | ***\*对应方法\****功能描述               | 描述                                                         |
> | --------------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
> | ***\*android:collapseColumns\**** | ***\*setColumnCollapsed(int,bolean)\**** | ***\*设置指定列号的列为是否隐藏\****                         |
> | ***\*android:shrinkColumns\****   | ***\*setShrinkAllColumns(boolean)\****   | ***\*设置指定的列号列为宽度是否可进行收缩\****               |
> | ***\*android:stretchColumns\****  | ***\*setStretchAllColumns(boolean)\****  | ***\*设置指定列号的列为宽度是否可进行拉伸，尽量把指定的列填充空白部分\**** |
>
> > - layout_column指定
> > - layout_span，合并列

> **案例**
>
> ```xml
> <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
>               xmlns:app="http://schemas.android.com/apk/res-auto"
>               xmlns:tools="http://schemas.android.com/tools"
>               android:layout_width="match_parent"
>               android:layout_height="match_parent"
>               tools:context=".MainActivity"
>               android:orientation="vertical"
>               >
> 
>     <TableLayout
>                  android:layout_width="wrap_content"
>                  android:layout_height="wrap_content"
>                  >
>         <TableRow
>                   >
>             <Button
>                     android:text="sss"
>                     android:layout_column="1"
>                     >
>             </Button>
> 
>             <Button
>                     android:text="2"
>                     >
>             </Button>
>         </TableRow>
> 
>         <TableRow>
>             <Button>
>             </Button>
> 
>             <Button>
>             </Button>
>         </TableRow>
>     </TableLayout>
> 
> </LinearLayout>
> ```
>
> ![1684752398949](Android--布局.assets/1684752398949.png)

### 8.AbsoluteLayout

> 参考——https://blog.csdn.net/qq_62277763/article/details/128671793

绝对布局缺乏灵活性，在没有绝对定位的情况下相比其他类型的布局更难维护，**不建议使用**。（AS已经弃用）

> | 属性                  | 作用            |
> | :-------------------- | :-------------- |
> | android:layout_width  | 组件宽度        |
> | android:layout_height | 组件高度        |
> | android:layout_x      | 设置组件的X坐标 |
> | android:layout_y      | 设置组件的Y坐标 |





### 9.CardView

> [cardview](https://juejin.cn/post/7002526755835609102)

**卡片式布局**，比较常用

```groovy
dependencies {
    implementation "androidx.cardview:cardview:1.0.0"
}
```

常用属性：

`app:cardCornerRadius` 设置CardView圆角 
 `app:cardElevation` 设置CardView阴影 
 `app:cardMaxElevation`设置CardView最大阴影 
 `app:cardBackgroundColor` 设置CardView背景色 `app:cardUseCompatPadding` 设置内边距，V21+的版本和之前的版本仍旧具有一样的计算方式 
 `app:cardPreventCornerOverlap` 在V20和之前的版本中添加内边距，这个属性为了防止内容和边角的重叠 
 `app:contentPadding` 设置卡片内容到四周的边距 
 `app:contentPaddingLeft` 设置卡片内容到左边的边距 
 `app:contentPaddingRight` 设置卡片内容到右边的边距 
 `app:contentPaddingTop` 设置卡片内容到顶部的边距 
 `app:contentPaddingBottom` 设置卡片内容到底部的边距 
 `android:minWidth` 设置卡片的最小宽度 
 `android:minHeight` 设置卡片的最小高度



### 10.DrawerLayout

实现了侧滑菜单的效果布局，

> **使用**
>
> DrawerLayout 是一个布局文件管理器，在这个布局当中允许放入 2 个组件或者是布局:
>
> - 第 1 个用于显示在主屏幕当中
>
> - 第 2 个用于显示滑动菜单中的内容
>
>   在设置滑动菜单实现的内容时在该组件或布局中需要添加 `android:layout_gravity="start"`属性将需要显示的内容放在布局的左侧。
>
> ```xml
> <androidx.drawerlayout.widget.DrawerLayout
>     xmlns:android="http://schemas.android.com/apk/res/android"
>     android:id="@+id/drawerLayout"
>     android:layout_width="match_parent"
>     android:layout_height="match_parent">
>     
>     <!-- 界面主要内容布局区 -->
>     <FrameLayout
>         android:layout_width="match_parent"
>         android:layout_height="match_parent">
>     </FrameLayout>
>     
>     <!-- 左侧弹出的内容 -->
>     <LinearLayout
>         android:layout_width="match_parent"
>         android:layout_height="match_parent"
>         android:layout_gravity="start"
>         android:orientation="vertical">
>     </LinearLayout>
>     
> </androidx.drawerlayout.widget.DrawerLayout>
> ```
>
> 
>
> **设置监听器：**
>
>  可以使用 `setDrawerListener() `方法为该布局设置监听器，根据监听器中的 4 个方法实现滑动菜单在不同的状态下来完成各种任务。 
>
> ```java
> //获取DrawerLayout布局
> DrawerLayout drawerLayout = (DrawerLayout) findViewById(R.id.drawerLayout);
> drawerLayout.setDrawerListener(new DrawerLayout.DrawerListener() {
>     //当抽屉的位置发生变化时调用
>     @Override
>     public void onDrawerSlide(View drawerView, float slideOffset) {
>     }
> 
>     //当抽屉已经处于完全打开的状态时调用
>     @Override
>     public void onDrawerOpened(View drawerView) {
>     }
> 
>     //当抽屉已经完全关闭状态时调用
>     @Override
>     public void onDrawerClosed(View drawerView) {
>     }
> 
>     //抽屉滑动状态改变时调用
>     //状态值STATE_IDLE：闲置、STATE_DRAGGING：拖拽、STATE_SETTLING：固定的
>     @Override
>     public void onDrawerStateChanged(int newState) {
>     }
> 
> });
> ```
>
> 

### 11.CoordinatorLayout 

### 12.AppBarLayout 



### 13.TabLayout

> **参考：**
>
>  [Android原生TabLayout使用全解析](https://blog.csdn.net/yechaoa/article/details/122270969) 

### 14.viewStub



## 改善布局性能

布局的是Android应用中直接影响用户体验的关键部分，如果布局不当会导致占用大量内存，应用界面异常缓慢。

### 优化布局层次结构

在一些进行布局的例子中，会出现一些布局麻烦问题：

> **避免嵌套Layout**
>
> 使用**嵌套的 `LinearLayout`** 实例会导致视图层次结构过深。此外，嵌套多个使用 `layout_weight` 参数的 `LinearLayout` 实例成本非常高，因为每个子级都需要测量两次。如果要反复膨胀布局（例如，在 `ListView` 或 `GridView` 中使用时），要特别注意这方面。



**可以使用布局检查器和布局验证工具调试布局**

![image-20210927160414219](Android--布局.assets/image-20210927160414219.png)

![布局检查器的屏幕截图，其中包含指示“Component Tree”、“布局检查器工具栏”、“Layout Display”和“Attributes”的标签](Android--布局.assets/layout-inspector-callouts.png)

> 用法同Flutter类型，还可以保存快照

![image-20210927161153093](Android--布局.assets/image-20210927161153093.png)

![image-20210927161215774](Android--布局.assets/image-20210927161215774.png)

> 详情参考--
>
> https://developer.android.google.cn/studio/debug/layout-inspector

### 使用Lint进行代码检查

> 参考--
>
> https://blog.csdn.net/zhangwenshuan/article/details/97525140



### 避免重复布局

可以使用 **`<include/>`** 和 **`<merge/>`** 标记在当前布局中嵌入其他布局。

>  例如，“是/否”按钮面板，或包含说明文本的自定义进度条。这也意味着您可以单独提取、管理多个布局中的任何常见应用元素，然后将其添加到各个布局中。

**demo：定义一个可重复使用的标题栏：**

创建一个新的 XML 文件并定义该布局

```xml
 <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/titlebar_bg"
        tools:showIn="@layout/activity_main" >

        <ImageView android:layout_width="wrap_content"
                   android:layout_height="wrap_content"
                   android:src="@drawable/gafricalogo" />
    </FrameLayout>
```

> 上述 XML 中的 [`tools:showIn`](https://developer.android.google.cn/studio/write/tool-attributes#toolsshowin) 属性，在编译过程中会被移除，**它指定了包含此文件的布局**，以便您可以按它嵌入到父级布局中时的**显示效果来预览**（和修改）此文件。



在要添加可重复使用的组件的布局中，添加`<include/>`标签，

例如下面的布局包含上述标题栏

```xml
 <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/app_bg"
        android:gravity="center_horizontal">

        <include layout="@layout/titlebar"/><!--引入布局，避免嵌套-->

        <TextView android:layout_width="match_parent"
                  android:layout_height="wrap_content"
                  android:text="@string/hello"
                  android:padding="10dp" />

        ...

    </LinearLayout>
```

**在include标签里还可以添加布局参数(任何 `android:layout_` 属性)来替代原view中的参数；如果要使用 `<include>` 标记来替换原始布局文件中的属性,必须同时替换 `android:layout_height` 和 `android:layout_width` 才能让其他布局属性生效。**

```xml
 <include android:id="@+id/news_title"
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             layout="@layout/title"/>
```



**使用`<merge>`标记**

在一个布局中添加另一个布局时，`<merge />` 标记有助于消除视图层次结构中的冗余视图组。**也就是说，一个布局要嵌套另一个布局时候，嵌套进去的那个布局可以使用`merge`标签消除冗余ViewGroup**

> 例如，当我要在一个LinearLayout布局里面重复使用组件A，但是组件A的根布局也是LinearLayout，当我重复使用的时候会造成两个视图冗余，也就是说**嵌套的 `LinearLayout` 除了会降低界面的性能之外，没有任何实际用处**。
>
> 为了避免这类情况，可以使用`<merge>` 元素作为可重复使用的布局的根视图
>
> ```xml
> <merge xmlns:android="http://schemas.android.com/apk/res/android">
> 
>      <Button
>          android:layout_width="fill_parent"
>          android:layout_height="wrap_content"
>          android:text="@string/add"/>
> 
>      <Button
>          android:layout_width="fill_parent"
>          android:layout_height="wrap_content"
>          android:text="@string/delete"/>
> 
>  </merge>
> ```
>
> 现在，当您将此布局添加到其他布局中（使用 `<include/>` 标记）时，系统会忽略 `<merge>` 元素并直接在布局中放置两个按钮，以代替 `<include/>` 标记。



### 视图加载延迟

当应用**将来**可能需要加载复杂试图，则可以使用**延迟加载**资源这项重要的方法。可以通过为复杂且很少使用的视图定义*ViewStub*来实现此方法。

> `ViewStub` 是一种没有任何维度的轻量型视图，它不会绘制任何内容或参与布局。因此，扩充和离开视图层次结构的成本比较低。每个 `ViewStub` 只需包含 `android:layout` 属性即可指定要扩充的布局。



*定义ViewStub*

以下 `ViewStub` 适用于半透明进度条叠加层，只有当新的项导入应用中时，它才会显示。

```xml
 <ViewStub
        android:id="@+id/stub_import"
        android:inflatedId="@+id/panel_import"
        android:layout="@layout/progress_overlay"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom" />
```

*加载ViewStub*

如果要加载 `ViewStub` 指定的布局，可通过调用 `setVisibility(View.VISIBLE)` 将其设置为可见，或调用 `inflate()`。

```java
findViewById(R.id.stub_import).setVisibility(View.VISIBLE);
// or
View importPanel = ((ViewStub) findViewById(R.id.stub_import)).inflate();
```

> `inflate()` 方法会在完成后返回扩充的 `View`。因此，如果您需要与布局互动，则无需调用 `findViewById()`。

该布局可见/扩充之后，`ViewStub` 元素将不再是视图层次结构的一部分。它会被替换为经过扩充的布局，该**布局的根视图的 ID 便是由 ViewStub 的 `android:inflatedId` 属性指定的那个 ID**。（为 `ViewStub` 指定的 ID `android:id` 仅在 `ViewStub` 布局可见/扩充之前有效。）



## 二、其他

### 1.LayoutInflater

 LayoutInflater 是 Android 中用于将布局文件转换为 View 对象的工具。它是 Android 框架中的一个重要组件，用于构建用户界面。 

LayoutInflater 提供了以下方法来将布局文件转换为 View 对象：

```java
inflate(int resource, ViewGroup root, boolean attachToRoot)//将布局文件填充到指定的 ViewGroup 中，并指定是否将 View 对象附加到 ViewGroup。第一个参数是布局文件的资源 ID。第二个参数是将布局文件填充到的 ViewGroup。第三个参数指定是否将 View 对象附加到 ViewGroup。
```

> **示例**
>
> ```java
> public class MainActivity extends AppCompatActivity {
> 
>     @Override
>     protected void onCreate(Bundle savedInstanceState) {
>         super.onCreate(savedInstanceState);
> 
>         // 创建 LinearLayout
>         LinearLayout layout = new LinearLayout(this);
>         layout.setOrientation(LinearLayout.VERTICAL);
> 
>         // 将布局文件填充到 LinearLayout
>         layout.addView(LayoutInflater.from(this).inflate(R.layout.activity_main, layout, false));
> 
>         setContentView(layout);
>     }
> }
> ```



**如何获取LayoutInflater**

①从Activity、Fragment、View中获取

```java
LayoutInflater inflater = getLayoutInflater();
```

②从Context中获取

```java
LayoutInflater inflater = (LayoutInflater)getContext().getSystemService(Context.LAYOUT_INFLATER_SERVICE);
```

