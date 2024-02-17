 通过调用下面几个静态函数来获取一个Builder 对象： 

## 四大组件

### 一、Activity

#### 1.简介

Activity是安卓里面最常用也是最重要的组件之一，在编程范式中，应用是通过 `main()` 方法启动的，而 Android 系统与此不同，它会`调用与其生命周期特定阶段相对应的特定回调方法来启动` `Activity` 实例中的代码。

#### 2.创建Activity

1.创建类并且继承Activity或其子类

2.AndroidManifest中声明

3.创建layout布局文件并在onCreat方法中声明添加

**修改页面名称：**

![1608009985987](https://gitee.com/kevinyong/kevin-gallery/raw/master/1608009985987.png)

**更改/取消导航栏**

```xml
android:theme="@style/Theme.AppCompat.Light.NoActionBar"
```



![1608010832482](https://gitee.com/kevinyong/kevin-gallery/raw/master/1608010832482.png)

**设置为默认启动Activity，在声明的activity中加上**

```xml
 <intent-filter>
      <action android:name="android.intent.action.MAIN" />
      <category android:name="android.intent.category.LAUNCHER" />
 </intent-filter>
```



#### 3.Activity生命周期

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1608011901125.png" alt="1608011901125" style="zoom: 50%;" />

Activity提供了六个核心回调

- onCreate()
- onStart()
- onResume()
- onPause()
- onStop()
- onDestroy()

![image-20220217154202689](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217154202689.png)

```java
//几个常用的生命周期方法，可以通过log打印这些方法调用过程和时间，在实际操作中也可以在对应方法下进行操作，比如玩游戏途中接电话，则可以在pause方法中暂停游戏
public class TextViewActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_text_view);
    }

    @Override
    protected void onStart() {
        super.onStart();
    }

    @Override
    protected void onStop() {
        super.onStop();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
    }

    @Override
    protected void onPause() {
        super.onPause();
    }

    //恢复数据
    @Override
    protected void onResume() {
        super.onResume();
    }
}
```

##### onCreate()

onCreate()方法是必须实现的方法，会在首次创建Activity时调用，且整个Activity生命周期只发生一次，可以通过该方法实现诸如：

- 数据绑定到列表：Activity和ViewModel相关联。
- 实例化某些变量。
- 该方法会接收`savedInstanceState` 参数，用于获取此前保存的信息的Bundle对象，如果Activity是第一次创建，则该参数为null

**示例：**

```java
TextView textView;

// 一些短暂的实例
String gameState;

@Override
public void onCreate(Bundle savedInstanceState) {
    // call the super class onCreate to complete the creation of activity like
    // the view hierarchy
    super.onCreate(savedInstanceState);

    // recovering the instance state
    if (savedInstanceState != null) {
        gameState = savedInstanceState.getString(GAME_STATE_KEY);
    }

    // 将界面文件资源ID传递给setContentView来指定XML布局文件
    setContentView(R.layout.main_activity);

   
    textView = (TextView) findViewById(R.id.text_view);
}

// 通过该回调获取之前通过onSaveInstanceState()保存的状态，onCreate里面也可以通过参数获取，该回调只会在onStart()调用完之后调用
@Override
public void onRestoreInstanceState(Bundle savedInstanceState) {
    textView.setText(savedInstanceState.getString(TEXT_VIEW_KEY));
}

// 当Activity要被销毁时调用，保存一些状态
@Override
public void onSaveInstanceState(Bundle outState) {
    outState.putString(GAME_STATE_KEY, gameState);
    outState.putString(TEXT_VIEW_KEY, textView.getText());

    // call superclass to save any view hierarchy
    super.onSaveInstanceState(outState);
}
```

您还可以在 Activity 代码中新建 `View` 对象，并将新建的 `View` 插入到 `ViewGroup` 中，以构建视图层次结构。然后，将根 `ViewGroup` 传递给 `setContentView()` 以使用该布局。具体参考以下

> [界面](https://developer.android.google.cn/guide/topics/ui)



##### onStart()

当Activity创建完之后，会调用该回调变为已开始状态，使得Activity对用户可见，应用会为 Activity 进入前台和支持互动做准备。例如，**应用通过此方法来初始化维护界面的代码。**



##### onResume()

进入“已恢复”状态时**Activity来到前台**，然后调用该回调。这是应用与用户互动的状态。**`应用会一直保持这种状态`**，直到某些事件发生，让焦点远离应用——`例如接到来电、用户导航到另一个 Activity，或设备屏幕关闭。`



##### onPause()

系统将此方法视为用户将要离开您的 Activity 的第一个标志（这并不总是意味着 Activity 会被销毁）；此方法`表示 Activity 不再位于前台`（尽管在用户处于多窗口模式时 Activity 仍然可见）。使用 `onPause()` 方法暂停或调整当 `Activity`处于“已暂停”状态时不应继续（或应有节制地继续）的操作，以及您希望很快恢复的操作。Activity 进入此状态的原因有很多。例如：

- 某个事件会中断应用执行。这是最常见的情况。
- 在 Android 7.0（API 级别 24）或更高版本中，有多个应用在多窗口模式下运行。无论何时，都只有一个应用（窗口）可以拥有焦点，因此系统会暂停所有其他应用。
- 有新的半透明 Activity（例如dialog）处于开启状态。只要 Activity 仍然部分可见但并未处于焦点之中，它便会一直暂停。

还可以使用 `onPause()`方法释放系统资源、传感器（例如 GPS）手柄，或当您的 Activity 暂停且用户不需要它们时仍然可能影响电池续航时间的任何资源。

> onPause()执行非常简单，速度很快，所以不应该使用onPause()来保存应用或用户数据等耗时操作，因为在该方法完成之前，此类工作可能无法完成。



##### onStop()

如果您的 Activity 不再对用户可见或者 Activity 已结束运行并即将终止，说明其已进入“已停止”状态，因此系统将调用 `onStop()` 回调。例如，当新启动的 Activity 覆盖整个屏幕时，可能会发生这种情况。

> 在 `onStop()` 方法中，应用应释放或调整在应用对用户不可见时的无用资源。例如，应用可以暂停动画效果，或从精确位置更新切换到粗略位置更新。

> 您还应使用 **`onStop()` 执行 CPU 相对密集的关闭操作**。例如，如果您无法找到更合适的时机来将信息保存到数据库，可以在 `onStop()` 期间执行此操作。例如将草稿笔记内容保存到持久性存储空间中：



##### onDestroy()

销毁 Ativity 之前，系统会先调用 `onDestroy()`。系统调用此回调的原因如下：

1. Activity 即将结束（由于用户彻底关闭 Activity 或由于系统为 Activity 调用 `finish()`，或者
2. 由于配置变更（例如设备旋转或多窗口模式），系统暂时销毁 Activity

> 如果 Activity 即将结束，onDestroy() 是 Activity 收到的最后一个生命周期回调。如果由于配置变更而调用 onDestroy()，系统会立即新建 Activity 实例，然后在新配置中为新实例调用 `onCreate()`。



##### onRestart()

这个方法在活动由停止状态变为运行状态之前调用，也就是活动被重新启动了。



##### Activity进行跳转的生命周期

> 当一个 Activity 启动另一个 Activity 时，它们都会经历生命周期转换,以下是 Activity A 启动 Activity B 时的操作发生顺序：
>
> 1. Activity A 的 `onPause()` 方法执行。
> 2. Activity B 的 `onCreate()`、`onStart()` 和 `onResume()` 方法依次执行（Activity B 现在具有用户焦点）。
> 3. 然后，如果 Activity A 在屏幕上不再显示，其 `onStop()` 方法执行。



#### 4.跳转界面

##### (1)显式跳转

显式，即直接指定需要打开的activity对应的类。 以下多种方式都是一样的，实际上都是设置Component直接指定Activity类的显式Intent，由MainActivity跳转到SecondActivity：

**1.构造方法传入Component，最常用的方式**

```java
Intent intent = new Intent(this, SecondActivity.class);  
startActivity(intent); 
```

**2.setComponent方法**

```java
ComponentName componentName = new ComponentName(this, SecondActivity.class);  
// 或者ComponentName componentName = new ComponentName(this, "com.example.app016.SecondActivity");  
// 或者ComponentName componentName = new ComponentName(this.getPackageName(), "com.example.app016.SecondActivity");  
  
Intent intent = new Intent();  
intent.setComponent(componentName);  
startActivity(intent); 
```

**3.setClass/setClassName方法**

```java
Intent intent = new Intent();  
  
intent.setClass(this, SecondActivity.class);  
// 或者intent.setClassName(this, "com.example.app016.SecondActivity");  
// 或者intent.setClassName(this.getPackageName(), "com.example.app016.SecondActivity");  
          
startActivity(intent);  
```

显式Intent通过Component可以直接设置需要调用的Activity类，可以唯一确定一个Activity，意图特别明确，所以是显式的。设置这个类的方式可以是Class对象（如SecondActivity.class），

也可以是包名加类名的字符串（如"com.example.app016.SecondActivity"）。这个很好理解，在应用程序内部跳转界面常用这种方式。



##### (2)隐式跳转

隐式，即不是像显式的那样直接指定需要调用的Activity，隐式不明确指定启动哪个Activity，而是`设置Action、Data、Category`，让系统来筛选出合适的Activity。

筛选是根据所有的`<intent-filter>`来筛选。

> AndroidManifest.xml文件中，被调用的Activity要有一个带有`< intent-filter>`并且包含`< action>`的Activity，设定它能处理的Intent，并且category设为"android.intent.category.DEFAULT"。

```xml
<activity  
    android:name="com.example.app016.SecondActivity">  
    <intent-filter>  
        <action android:name="abcdefg"/>  
        <category android:name="android.intent.category.DEFAULT"/>  
    </intent-filter>  
</activity>  
```

然后，在MainActivity，才可以通过这个action name找到上面的Activity。

下面两种方式分别通过setAction和构造方法设置Action，两种方式效果相同。

**1、setAction方法**

```java
Intent intent = new Intent();  
intent.setAction("abcdefg");  
startActivity(intent);  
```


**2、构造方法直接设置Action**

```java
Intent intent = new Intent("abcdefg");  
startActivity(intent); 
```



一个Activity可以处理多种Action 只要你的应用程序够牛逼，一个Activity可以看网页，打电话，发短信，发邮件。。。都可以。 Intent的Action只要是其中之一，就可以打开这个Activity。

```xml
activity  
    android:name="com.example.app016.SecondActivity">  
    <intent-filter>  
        <!-- 可以处理下面三种Intent -->  
        <action android:name="com.example.app016.SEND_EMAIL"/>  
        <action android:name="com.example.app016.SEND_MESSAGE"/>  
        <action android:name="com.example.app016.DAIL"/>  
        <category android:name="android.intent.category.DEFAULT" />  
    </intent-filter>  
</activity>  
```

对于一个Action字符串，系统有可能会找到一个Activity能处理这个Action，也有可能找到多个Activity，也可能一个都找不到。
1、找到一个Activity
很简单，直接打开这个Activity。这个不需要解释。
2、找到多个Acyivity
系统会提示从多个activity中选择一个打开。
例如我们自己开发一个拨号面板应用程序，可以设置activity的< intent-filter>中Action name为"android.intent.action.DIAL"，这样别的程序调用拨号器时，用户可以从Android自带的拨号器和我们自己开发的拨号器中选择。

```xml
<activity  
    android:name="com.example.app016.SecondActivity">  
    <intent-filter>  
        <action android:name="android.intent.action.DIAL"/>  
        <category android:name="android.intent.category.DEFAULT" />  
    </intent-filter>  
</activity>  
```

**这也就是当Android手机装上UC浏览器后，打开网页时会弹出选择Android自带浏览器还是UC浏览器，可能都会遇到过。**

但是如果用户可能会出现勾选默认使用哪一个应用的选项，我们如果不想让用户选择默认的话，可以自定义**`显示应用选择器`**

```java
Intent intent = new Intent(Intent.ACTION_SEND);

// Always use string resources for UI text.
// This says something like "Share this photo with"
String title = getResources().getString(R.string.chooser_title);
// Create intent to show chooser
Intent chooser = Intent.createChooser(intent, title);

// Try to invoke the intent.
try {
    startActivity(chooser);
} catch (ActivityNotFoundException e) {
    // Define what your app should do if no activity can handle the intent.
}
```







**3、一个Activity都没找到**
一个都没找到的话，程序就会出错，会抛出ActivityNotFoundException。比如随便写一个action字符串：

```
Intent intent = new Intent("asasasas");  
startActivity(intent);  
```


![image-20210815180521362](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210815180521362.png)

**所以应该注意try catch异常。**

```java
Intent intent = new Intent("asasasas");  
try  
{  
    startActivity(intent);  
}  
catch(ActivityNotFoundException e)  
{  
    Toast.makeText(this, "找不到对应的Activity", Toast.LENGTH_SHORT).show();  
}  
```


或者也可以**使用Intent的resolveActivity方法判断这个Intent是否能找到合适的Activity**，如果没有，则不再startActivity，或者可以直接禁用用户操作的控件。

```java
Intent intent = new Intent(Intent.ACTION_DIAL);  
if(intent.resolveActivity(getPackageManager()) == null)  
{  
    // 设置控件不可用  
}  
```


注意resolveActivity方法返回值就是显式Intent上面讲到的ComponentName对象，一般情况下也就是系统找到的那个Activity。

但是如果有多个Activity可供选择的话，则返回的Component是com.android.internal.app.ResolverActivity，也就是用户选择Activity的那个界面对应的Activity，这里不再深究。



```java
Intent intent = new Intent(Intent.ACTION_DIAL);  
ComponentName componentName = intent.resolveActivity(getPackageManager());  
if(componentName != null)  
{  
    String className = componentName.getClassName();  
    Toast.makeText(this, className, Toast.LENGTH_SHORT).show();  
}  
```

>  **Tips:在不同应用中只能使用隐式启动**



##### (3)不同应用跳转

###### ①通过自定义action启动

这种方式只需要在代码中设置一个action即可， 系统会自动过滤去找到这个action所对应的Activity

**当前APP的代码**

```java
Intent intent = new Intent();
//这里是采用的自定义action
intent.setAction("transBundle.app");
startActivity(intent);
```

**待启动APP 的activity在AndroidManifest.xml中的配置**

```java
<!- 需要配置对应的自定义action->
<activity
        android:name=".MyActivity"
        android:label="@string/app_name" 
        android:exported="true">
        <intent-filter>
            <action android:name="transBundle.app"/>
            <category android:name="android.intent.category.DEFAULT"/>
        </intent-filter>
    </activity>
```

**例如:**

**跳转到拨打电话界面**

1)Android直接拨打电话

```java
 Intent dialIntent =  new Intent(Intent.ACTION_CALL,Uri.parse("tel:" + phoneNumber));//直接拨打电话
 startActivity(dialIntent);
```

2)Android跳转到拨号界面

 ```java
Intent dialIntent =  new Intent(Intent.ACTION_CALL_BUTTON);//跳转到拨号界面
 startActivity(dialIntent);
 ```

3)Android跳转到拨号界面，同时传递电话号码

 ```java
Intent dialIntent =  new Intent(Intent.ACTION_DIAL,Uri.parse("tel:" + phoneNumber));//跳转到拨号界面，同时传递电话号码
startActivity(dialIntent);
 ```

**查看网页**

```java
Uri webpage = Uri.parse("https://www.android.com");
Intent webIntent = new Intent(Intent.ACTION_VIEW, webpage);
```



###### ②通过指定包名和类名来查找

**直接在当前APP中写以下代码，即可打开指定APP的activity**

```java
ComponentName componetName = new ComponentName(  
            "com.poynt.weibo",  //这个是另外一个应用程序的包名  
            "com.poynt.weibo.ui.IndexActivity");   //这个参数是要启动的Activity的全路径名

        try {  
            Intent intent = new Intent();  
            intent.setComponent(componetName);  
            startActivity(intent);  
        } catch (Exception e) {  
            Toast.makeText(getApplicationContext(), "可以在这里提示用户没有找到应用程序，或者是做其他的操作！", 0).show();  
        }
```



###### ③通过scheme启动

其实这个方法和方法1类似， 只是说增加了scheme参数， scheme更多的用于 在网页或者H5上来启动我们的APP， 比如在手机官网上通过scheme可以直接打开我们的app， 这里我们只是从APP用scheme启动另一个APP。

**当前应用的代码**：

```java
Uri uri = Uri.parse("app://my.test");
Intent intent = new Intent("transBundle.app", uri);
startActivity(intent);
```

**待打开APP的AndroidManifest配置**

```java
 <activity
        android:name=".MyActivity"
        android:label="service" 
        android:exported="true">

        <intent-filter>
            <action android:name="transBundle.app"/>
            <action android:name="android.intent.action.VIEW"/>
            <category android:name="android.intent.category.DEFAULT"/>
            <data android:scheme="app" android:host="my.test"/>//配置scheme
        </intent-filter>
    </activity>
```

其中app类似于[http://www.baidu.com](http://www.baidu.com/)中的 http， 表示传输协议; my.test类似于www.baidu.com， 表示主机名



#### 5.Activity进行数据传递

**Activity之间数据传输**

```java
       /*Activity-A*/
        jumptob = findViewById(R.id.jumtob);
        jumptob.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                 //显示跳转1
                 Intent intent = new Intent(AActivity.this,BActivity.class);
                 Bundle bundle = new Bundle();//通过bundle存放传输数据
                 bundle.putString("name","杨海涛");//放入数据
                 bundle.putInt("id",2018131401);
                 intent.putExtras(bundle);//也可以用intent.putExtra直接传递数据
                 startActivity(intent);
            }
        });
```

```java
        /*Activity-B接收数据*/
        textView = findViewById(R.id.showbundle);
        Bundle bundle = getIntent().getExtras();//通过bundle获取数据
        String name = bundle.getString("name");
        int number = bundle.getInt("id",110);//接收key为id的值，如果没接收到，则获取默认值110
        /*通过intent直接发送的数据获取方式
        Intent intent = getIntent();
        int num = intent.getIntExtra("id")//
        */ 
        textView.setText(name+","+number);
```



进行数据返回

Activity-A里面是有 startActivityForResult

```java
   startActivityForResult(intent,0);//跳转回来该界面时,通过requestCode：0找到该界面然后重写获取结果集方法

     //获取返回结果
    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        //toast显示
        Toast.makeText(AActivity.this,data.getExtras().getString("A"),Toast.LENGTH_SHORT).show();
    }
```



Activity-B里面返回结果集

```java
        button = findViewById(R.id.jumpbacktoa);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent();
                Bundle bundle1 = new Bundle();
                bundle1.putString("A","我回来了");//放入数据
                intent.putExtras(bundle1);//intent装载
                setResult(Activity.RESULT_OK,intent);//传回数据
                finish();//关闭当前界面
            }
        });
```



Activity-A完整代码

```java
public class AActivity extends AppCompatActivity {

    private Button jumptob;
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_a);

        jumptob = findViewById(R.id.jumtob);
        jumptob.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                 //显示跳转1
                 Intent intent = new Intent(AActivity.this,BActivity.class);
                 Bundle bundle = new Bundle();
                 bundle.putString("name","杨海涛");
                 bundle.putInt("id",2018131401);
                 intent.putExtras(bundle);//也可以用putExtra直接传递数据
                 //startActivity(intent);
                 startActivityForResult(intent,0);//跳转回来该节目传数据返回，通过code找到该界面
            }
        });

    }

    //获取返回结果
    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        Toast.makeText(AActivity.this,data.getExtras().getString("A"),Toast.LENGTH_SHORT).show();
    }
}

```



Activity-B完整代码

```java
public class BActivity extends AppCompatActivity {

    private TextView textView;
    private Button button;
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_b);
        textView = findViewById(R.id.showbundle);
        Bundle bundle = getIntent().getExtras();
        String name = bundle.getString("name");
        int number = bundle.getInt("id",110);
        textView.setText(name+","+number);

        button = findViewById(R.id.jumpbacktoa);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent();
                Bundle bundle1 = new Bundle();
                bundle1.putString("A","我回来了");
                intent.putExtras(bundle1);
                setResult(Activity.RESULT_OK,intent);//传回数据
                finish();//关闭当前界面
            }
        });

    }
}

```



#### 6.任务和返回栈

**任务：**

> 在 Android 系统中，**任务是指一组相关的 Activity**。这些 Activity 通常是用户为了完成某个特定任务而启动的，例如查看照片、编辑文档或玩游戏。
>
> 任务可以分为两种类型：
>
> - **标准任务**：标准任务是默认的任务类型。它包含一个或多个 Activity。
> - **单例任务**：单例任务只能包含一个 Activity。

**返回栈**

> **任务由系统管理，并保存在返回栈中**。返回栈是一个 LIFO（后进先出）数据结构，它存储了用户启动的所有 Activity。当用户按下返回键时，系统会从返回栈中弹出 Activity 并将其销毁。

**因此， 一个应用程序可以有多个返回栈，取决于应用程序的任务数量 **

![image-20220217155501000](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217155501000.png)

> 使用命令*adb shell dumpsys activity*获取手机的的activity栈的详细信息



##### 6.1管理任务

在开发中很可能会出现以下几种情况：

- 如果多个Activity可以启动同一个`Activity A`，且`Activity A`不在栈顶，那么就会在返回堆栈创建多个`Activity A`实例

  ![image-20220217155624244](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217155624244.png)

- 在用户离开任务时清除返回堆栈中除根Activity以外的所有Activity

要想解决实现以上方法，可以通过借助`<activity>`清单元素属性和传递给`startActivity`的`intent`来标记实现上述目的

- `<activity>`属性包括：

![image-20220217155721879](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217155721879.png)

- `intent`标记包括:

![image-20220217155740300](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217155740300.png)



###### 6.1.1启动模式

在`manifest`的`<activity>`中给`launchMode`属性指定该activity应该如何与任务关联。launchMode提供了四种可指定属性。

![image-20210815171134342](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210815171134342.png)

```groovy
android:launchMode=""
```

①`Standard`

> 默认值。系统在启动该 Activity 的任务中创建 Activity 的新实例，并将 intent 传送给该实例。Activity 可以多次实例化，每个实例可以属于不同的任务，一个任务可以拥有多个实例。

②`singleTop`

> 这种启动模式下，**如果要启动的Activity已经处于栈的顶部**，那么此时系统不会创建新的实例，而是直接打开此页面，同时它的**`onNewIntent()`**方法会被执行，我们可以通过Intent进行传值，而且它的**`onCreate()，onStart()方法不会被调用`**，因为它并没有发生任何变化。

③`singleTask`

> 在这个模式下，**如果栈中存在这个Activity的实例就会复用这个Activity**，不管它是否位于栈顶，<font color='red'>复用时，会将它上面的Activity全部出栈</font>，因为singleTask本身自带clearTop这种功能。并且会回调该实例的**`onNewIntent()`**方法。(其实这个过程还存在一个任务栈的匹配，因为这个模式启动时，会在自己需要的任务栈中寻找实例，这个任务栈就是通过taskAffinity属性指定。如果这个任务栈不存在，则会创建这个任务栈。不设置taskAffinity属性的话，默认为应用的包名)
>
> - 不设置taskAffinity属性，也就是默在同一个任务栈中。
>
> - 设置taskAffinity属性，singleTask所在的Activity与启动它的Activity处于不同的任务栈中。
>
>   ```xml
>   <activity
>        android:name=".SingleTaskActivity"
>        android:launchMode="singleTask"
>     android:taskAffinity="${applicationId}.singleTask">
>   </activity>
>   ```
>
> 指定了taskAffinity后，我们发现除了taskId有区别外，其他调用基本没有什么区别。**因为MainActivity没有指定taskAffinity属性，默认为包名**，与SingleTaskActivity不同，所以在启动SingleTaskActivity时，发现这个栈不存在，系统首先会创建这个栈然后将SingleTaskActivity压入栈中。之后我们发现只要栈中存在SingleTaskActivity这个实例，就会直接引用。(<font color='red'>**注意，如果设置了taskAffinity，则跳转到新的Activity之后会创建新的任务(Task)，所以需要指定返回到之前Activity方法，否则返回键会直接退出**</font>)
>
> 
>
> **如果您启动了指定 `singleTask` 启动模式的 Activity，而后台任务中已存在该 Activity 的实例，则系统会将该后台任务整个转到前台运行。此时，返回堆栈包含了转到前台的任务中的所有 Activity，这些 Activity 都位于堆栈的顶部。**
>
> ![image-20220217160820690](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217160820690.png)



④`singleInstance`

每个Activity占用一个栈:

> 该模式具备singleTask模式的所有特性外，与它的区别就是，这种模式下的Activity会单独占用一个Task栈，具有全局唯一性，即整个系统中就这么一个实例，由于栈内复用的特性，后续的请求均不会创建新的Activity实例，除非这个特殊的任务栈被销毁了。以singleInstance模式启动的Activity在整个系统中是单例的，如果在启动这样的Activiyt时，已经存在了一个实例，那么会把它所在的任务调度到前台，重用这个实例。



###### 6.1.2Intent标记启动模式

```java
Intent intent = new Intent(mContext, MyActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_NEW_TASK);

//这段代码将启动 MyActivity 并清除与 MyActivity 匹配的任务中的所有活动。这意味着 MyActivity 将成为该任务中的第一个活动。
```

**`FLAG_ACTIVITY_NEW_TASK`**

在新任务(new Task)中启动 Activity。如果您现在启动的 Activity 已经有任务在运行，则系统会将该任务转到前台并恢复其最后的状态。

**`FLAG_ACTIVITY_CLEAR_TASK`**（慎用）

该标记指示系统在启动 Activity 之前清除与 Intent 匹配的任务中的其他所有活动。这意味着 Activity 将成为该任务中的第一个活动 

**`FLAG_ACTIVITY_SINGLE_TOP`**

如果要启动的 Activity 是当前 Activity（即位于返回堆栈顶部的 Activity），则不会创建 Activity 的新实例。

**`FLAG_ACTIVITY_CLEAR_TOP`**

如果要启动的 Activity 已经在当前任务中运行，则不会启动该 Activity 的新实例，而是会销毁位于它之上的所有其他 Activity，并将此 intent 传送给它的已恢复实例（现在位于堆栈顶部）。





##### 6.2清除返回堆栈

如果用户离开任务较长时间，系统会清除任务中除根 Activity 以外的所有 Activity。当用户再次返回到该任务时，只有根 Activity 会恢复。所以可以使用一些属性来修改此行为

**`alwaysRetainTaskState`**

如果在任务的**根 Activity** 中将该属性设为 `"true"`，则不会发生上述默认行为。即使经过很长一段时间后，任务仍会在其堆栈中保留所有 Activity。

**`clearTaskOnLaunch`**

如果在任务的**根 Activity** 中将该属性设为 **`"true"`**，那么只要用户离开任务再返回，堆栈就会被清除到只剩根 Activity。也就是说，它与 **`alwaysRetainTaskState`**正好相反。用户始终会返回到任务的初始状态，即便只是短暂离开任务也是如此。

**`finishOnTaskLaunch`**

该属性与 **`clearTaskOnLaunch`**类似，但它只会作用于单个 Activity 而非整个任务。它还可导致任何 Activity 消失，包括根 Activity。如果将该属性设为 **`"true"`**，则 Activity 仅在当前会话中归属于任务。如果用户离开任务再返回，则该任务将不再存在。



#### 7.“最近使用的应用”屏幕

我们可以在后台看到最近使用的应用屏幕，有时候我们甚至一个应用可以创建多个不同的任务，也就是说，如果同一 Activity 的多个实例包含不同的文档，它们可能会在**最近使用的应用**屏幕中显示为任务。就比如如下Google文档每一个文档都会生成一个任务：

![image-20220217161256971](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217161256971.png)

在较低版本的Android中，所以Activity都显示为单个任务，所以返回按钮是唯一的导航方式，但在Android 5.0及更高版本之后就可以一个Activity创建多个任务：

![image-20220217161317984](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220217161317984.png)



##### 使用Intent标记添加任务：

1.为 Activity 创建新文档时，您可以调用 `startActivity()` 方法并向其传递到启动该 Activity 的 intent。要插入逻辑断点，以便系统将您的 Activity 视为**最近使用的应用**屏幕中的新任务，请将 **`FLAG_ACTIVITY_NEW_DOCUMENT`** 标记传入启动该 Activity 的 `Intent` 的 `addFlags()` 方法。(**使用 `FLAG_ACTIVITY_NEW_DOCUMENT` 标记启动的 Activity 必须在清单中设置 `android:launchMode="standard"` 属性值(默认值)**)



2.当使用了**`FLAG_ACTIVITY_NEW_DOCUMENT`** ，如果您在创建新文档时设置了 **`FLAG_ACTIVITY_MULTIPLE_TASK`** 标记，**则不会搜索是否存在目标Activity直接创建一个栈放入Activity**；如果没有设置此标记，会使系统搜索存在的task栈，去寻找匹配intent的一个activity，如果没有找到就会去新建一个task栈。则此设置支持同一文档在多个任务中打开

```java
 public void createNewDocument(View view) {
          final Intent newDocumentIntent = newDocumentIntent();
          if (useMultipleTasks) {
              newDocumentIntent.addFlags(Intent.FLAG_ACTIVITY_MULTIPLE_TASK);
          }
          startActivity(newDocumentIntent);
      }

      private Intent newDocumentIntent() {
          boolean useMultipleTasks = checkbox.isChecked();
          final Intent newDocumentIntent = new Intent(this, NewDocumentActivity.class);
          newDocumentIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_DOCUMENT);
          newDocumentIntent.putExtra(KEY_EXTRA_NEW_DOCUMENT_COUNTER, documentCounter++);
          return newDocumentIntent;
      }

    }
```

如果找到对应任务则将对应任务带到前台，并通过onNewIntent()获取数据

```java
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_new_document);
        documentCount = getIntent()
                .getIntExtra(DocumentCentricActivity.KEY_EXTRA_NEW_DOCUMENT_COUNTER, 0);
        documentCounterTextView = (TextView) findViewById(
                R.id.hello_new_document_text_view);
        setDocumentCounterText(R.string.hello_new_document_counter);
    }

    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
     setDocumentCounterText(R.string.reusing_document_counter);
    }
```

##### 使用Activity属性添加任务：

还可以在其清单中使用 `<activity>` 属性 **`android:documentLaunchMode`**指定始终启动到新任务。该属性具有四个值，在用户使用应用打开文档时分别会产生以下效果：

**"`intoExisting`"**

Activity 重复使用文档的现有任务。这与设置了 `FLAG_ACTIVITY_NEW_DOCUMENT` 标记但没有设置 `FLAG_ACTIVITY_MULTIPLE_TASK` 标记的效果相同。

**"`always`"**

Activity 会为文档创建新任务，即使文档已打开也一样。使用该值与同时设置了 `FLAG_ACTIVITY_NEW_DOCUMENT` 和 `FLAG_ACTIVITY_MULTIPLE_TASK` 标记的效果相同。

**"`none`"**

Activity 不会为文档创建新任务。**最近使用的应用**屏幕会以默认方式处理 Activity：它会显示应用的单个任务，该任务是从用户上次调用的任何 Activity 恢复的。

**"`never`"**

Activity 不会为文档创建新任务。设置该值会替换 `FLAG_ACTIVITY_NEW_DOCUMENT` 和 `FLAG_ACTIVITY_MULTIPLE_TASK` 标记的行为（如果在 intent 中设置了任意一个），而且**最近使用的应用**屏幕会显示应用的单个任务，该任务是从用户上次调用的任何 Activity 恢复的。

##### 移除任务

当相关的 Activity 完成时，文档任务会自动从**最近使用的应用**屏幕中移除。

- 将 `<activity>` 属性 **[`android:excludeFromRecents`]** 设置为 `true`，即可始终将任务从**最近使用的应用**屏幕中完全排除。
- 将 `<activity>` 属性 **[`android:maxRecents`]**设置为一个整数，即可设置您的应用可在**最近使用的应用**屏幕中包含的最大任务数。默认值为 16。一旦达到最大任务数，最早使用的任务将从**最近使用的应用**屏幕中移除。`android:maxRecents` 最大值为 50（内存较低的设备上为 25）；小于 1 的值无效。
- 使用AppTask类移除

对于在**最近使用的应用**屏幕中创建新任务的 Activity，您可以通过调用 `finishAndRemoveTask()` 方法来指定何时应移除任务并完成与该任务相关联的所有 Activity。

```java
public void onRemoveFromRecents(View view) {
        // The document is no longer needed; remove its task.
        finishAndRemoveTask();
    }
```

##### 保留已完成任务：

如果您即使在任务的 Activity 已完成后也希望在**最近使用的应用**屏幕中保留它，请将 `FLAG_ACTIVITY_RETAIN_IN_RECENTS` 标记传入启动该 Activity 的 Intent 的 `addFlags()` 方法。

```java
 private Intent newDocumentIntent() {
        final Intent newDocumentIntent = new Intent(this, NewDocumentActivity.class);
        newDocumentIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_DOCUMENT |
          android.content.Intent.FLAG_ACTIVITY_RETAIN_IN_RECENTS);
        newDocumentIntent.putExtra(KEY_EXTRA_NEW_DOCUMENT_COUNTER, getAndIncrement());
        return newDocumentIntent;
    }
```

也可以设置**`<activity>`**属性**[`android:autoRemoveFromRecents`]**设置为 `false`



#### 8.加载器

> Android API28中已弃用加载器，推荐使用ViewModel和LiveData组合来处理加载数据。

通过Loader，我们可以从内容提供程序或其他数据源中加载数据，以便展示在FragmentActivity或Activity上显示。为什么需要Loader来加载数据，可以分析下面几个问题：

- 如果直接在 Activity 或片段中获取数据，由于通过界面线程执行查询的速度可能较慢，响应能力的不足将影响您的用户。
- 如果从另一个线程获取数据（方法可能是使用 **`AsyncTask`**），则您需负责通过各种 Activity或Fragment生命周期事件（例如 **`onDestroy()`** 和配置变更）来管理线程和界面线程。



**加载器不仅能够解决上述问题，还具有以下优势：**

- 加载器在单独的线程上运行，以免界面出现卡顿或无响应问题。
- 加载器可在事件发生时提供回调方法，从而简化线程管理。
- 加载器会保留和缓存配置变更后的结果，以免出现重复查询问题。
- 加载器可实现观察器，从而监控基础数据源的变化。例如，`CursorLoader` 会自动注册 `ContentObserver`，以在数据变更时触发重新加载。

##### Loader API摘要

| 类/接口                       | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| LoaderManager                 | · 一种与 `FragmentActivity` 或 `Fragment` 相关联的抽象类，用于管理一个或多个 `Loader` 实例。每个 Activity 或片段只有一个 `LoaderManager`，但 `LoaderManager` 可管理多个加载器。                                                                            · 如要获取 LoaderManager，请从 Activity 或片段调用 `getSupportLoaderManager()`。                                    · · 如要从加载器开始加载数据，请调用 `initLoader()` 或 `restartLoader()`。系统会自动确定是否已存在拥有相同整型 ID 的加载器，并将创建新加载器或重复使用现有的加载器。 |
| LoaderManager.LoaderCallbacks | 此接口包含加载器事件发生时所调用的回调方法。接口定义三种回调方法：       ①**`onCreateLoader(int,Bundle)`**-系统需要创建新加载器时调用。您的代码应创建Loader对象并将其返回系统              ②**`onLoadFinished(Loader<D>, D)`**-加载器在完成数据加载时调用。一般来说，您的代码应向用户显示数据。                ③**`onLoaderReset(Loader<D>)`**-重置之前创建的加载器时调用(当您调用destroyLoader(int)时)，或由于系统销毁Activity或片段而使数据不可用时调用。并且代码应该删除其对加载器数据的任何引用。                                                      **此接口一般在调用initLoader()或restartLoader()时注册** |
| Loader                        | Loader 类执行数据的加载。此类属于抽象类，并且是所有加载器的基类。您可以直接创建 `Loader` 的子类，或使用以下某个内置子类来简化实现：           ①AsyncTaskLoader-抽象加载器，可通过AsyncTask在单独线程上执行加载操作。                                                                        ②CursorLoader - AsyncTaskLoader的具体子类，用于异步加载ContentProvider的数据。该类会查询ContentProvider并返回Cursor |

##### 使用加载器Example

**①创建加载器**

- 通常，您需在 Activity 的 `onCreate()` 方法或片段的 `onActivityCreated()` 方法内初始化 `Loader`。您可以执行如下操作：

```java
// Prepare the loader.  Either re-connect with an existing one,
// or start a new one.
getSupportLoaderManager().initLoader(0, null, this);

/**
*param1:用于标识加载器的唯一 ID。在此示例中，ID 为 0。
*param2:在构建时提供给加载器的可选参数（在此示例中为 null）。
*param3:LoaderManager.LoaderCallbacks 实现，LoaderManager 将调用此实现来报告加载器事件。在此示例中，本地类实现 LoaderManager.LoaderCallbacks         接口，因此其传递对自身的引用 this。
*/
```

调用initLoader()确保加载器已初始化且处于活动状态，但可能出现以下情况:

- 如果 ID 指定的加载器已存在，则重复使用上次创建的加载器。
- 如果 ID 指定的加载器*不*存在，则 `initLoader()` 会触发 `LoaderManager.LoaderCallbacks` 方法 `onCreateLoader()`。在此方法中，您可以实现代码以实例化并返回新加载器。
- 不需要捕获initLoader()方法返回的Loader,LoaderManager会自动管理加载器生命周期。

**②重启加载器**

如果我们想要舍弃旧数据重新开始，那么就可以使用restartLoader()。

```java
public boolean onQueryTextChanged(String newText) {
   //当action bar搜索的文本变化时，重启加载器以执行新的查询，然后加载数据。
    curFilter = !TextUtils.isEmpty(newText) ? newText : null;
    getSupportLoaderManager().restartLoader(0, null, this);
    return true;
}
```

**③LoaderManager回调**

`LoaderManager.LoaderCallbacks` 是一个回调接口，可让客户端与 `LoaderManager` 进行交互。

加载器（特别是 **`CursorLoader`**）在停止运行后，仍需保留其数据。如此一来，应用即可保留 Activity 或片段的 `onStop()` 和 `onStart()` 方法中的数据，以便用户在返回应用时无需等待其重新加载这些数据。您可以使用 `LoaderManager.LoaderCallbacks` 方法了解何时创建新加载器，并告知应用何时停止使用加载器的数据。

`LoaderManager.LoaderCallbacks` 包含以下方法：

- `onCreateLoader()`：针对指定的 ID 进行实例化并返回新的 `Loader`。

  > 当您尝试（例如，通过 **`initLoader()`**）访问加载器时，该方法将检查是否已存在由该 ID 指定的加载器。如果没有，它将触发 **`LoaderManager.LoaderCallbacks`** 方法 **`onCreateLoader()`**。在此方法中，您可以创建新加载器。新加载器通常是 **`CursorLoader`**，但您也可实现自己的 **`Loader`** 子类。
  >
  > 
  >
  > 在此示例中，`onCreateLoader()` 回调方法创建了 `CursorLoader`。您必须使用其构造函数方法来构建 `CursorLoader`，而该方法需要对 `ContentProvider` 执行查询时所需的一系列完整信息。具体而言，该方法需要：
  >
  > - ***uri***：用于检索内容的 URI
  > - ***projection***：要返回的列的列表。传递 `null` 时，将返回所有列，这样会导致效率低下。
  > - ***selection***：一种用于声明要返回哪些行的过滤器，采用 SQL WHERE 子句格式（WHERE 自身除外）。传递 `null` 时，将为指定的 URI 返回所有行。
  > - ***selectionArgs***：您可以在 selection 中包含 ?，它将按照在 selection 中显示的顺序替换为 *selectionArgs* 中的值。该值将会绑定为字串符。
  > - ***sortOrder***：行的排序依据，采用 SQLORDER BY 子句格式（ORDER BY 自身除外）。传递 `null` 时，将使用默认排序顺序（可能并未排序）。

  ```java
  // If non-null, this is the current filter the user has provided.
  String curFilter;
  ...
  public Loader<Cursor> onCreateLoader(int id, Bundle args) {
      // This is called when a new Loader needs to be created.  This
      // sample only has one Loader, so we don't care about the ID.
      // First, pick the base URI to use depending on whether we are
      // currently filtering.
      Uri baseUri;
      if (curFilter != null) {
          baseUri = Uri.withAppendedPath(Contacts.CONTENT_FILTER_URI,
                    Uri.encode(curFilter));
      } else {
          baseUri = Contacts.CONTENT_URI;
      }
  
      // Now create and return a CursorLoader that will take care of
      // creating a Cursor for the data being displayed.
      String select = "((" + Contacts.DISPLAY_NAME + " NOTNULL) AND ("
              + Contacts.HAS_PHONE_NUMBER + "=1) AND ("
              + Contacts.DISPLAY_NAME + " != '' ))";
      return new CursorLoader(getActivity(), baseUri,
              CONTACTS_SUMMARY_PROJECTION, select, null,
              Contacts.DISPLAY_NAME + " COLLATE LOCALIZED ASC");
  }
  ```

  

- `onLoadFinished()`：当先前创建的加载器完成加载时调用。

  当加载器发现应用不再使用这些数据时，即会将其释放。例如，如果数据是来自 `CursorLoader` 的游标，则您不应手动对其调用 `close()`。如果游标放置在 `CursorAdapter` 中，则您应使用 `swapCursor()` 方法，这样旧 `Cursor` 便不会关闭。例如：

  ```java
  // This is the Adapter being used to display the list's data.
  SimpleCursorAdapter adapter;
  ...
  
  public void onLoadFinished(Loader<Cursor> loader, Cursor data) {
      // Swap the new cursor in.  (The framework will take care of closing the
      // old cursor once we return.)
      adapter.swapCursor(data);
  }
  ```

  

- `onLoaderReset()`：当先前创建的加载器重置且因此其数据不可用时调用。

  >  当重置先前创建的加载器重置并因此导致其数据不可用时，将调用此方法。借助此回调，您可以了解何时将释放数据，以便及时移除其引用。  

  此实现会调用值为 `null` 的 `swapCursor()`：

  ```java
  // This is the Adapter being used to display the list's data.
  SimpleCursorAdapter adapter;
  ...
  
  public void onLoaderReset(Loader<Cursor> loader) {
      // This is called when the last Cursor provided to onLoadFinished()
      // above is about to be closed.  We need to make sure we are no
      // longer using it.
      adapter.swapCursor(null);
  }
  ```

  **完整demo**

  ```java
  public static class CursorLoaderListFragment extends ListFragment
          implements OnQueryTextListener, LoaderManager.LoaderCallbacks<Cursor> {
  
      // 用来展示数据适配器
      SimpleCursorAdapter mAdapter;
  
      // If non-null, this is the current filter the user has provided.
      String curFilter;
  
      @Override public void onActivityCreated(Bundle savedInstanceState) {
          super.onActivityCreated(savedInstanceState);
  
          // 占位符，如果没有数据的话显示
          setEmptyText("No phone numbers");
  
          // We have a menu item to show in action bar.
          setHasOptionsMenu(true);
  
          // Create an empty adapter we will use to display the loaded data.
          mAdapter = new SimpleCursorAdapter(getActivity(),
                  android.R.layout.simple_list_item_2, null,
                  new String[] { Contacts.DISPLAY_NAME, Contacts.CONTACT_STATUS },
                  new int[] { android.R.id.text1, android.R.id.text2 }, 0);
          setListAdapter(mAdapter);
  
          //准备创建Loader，有则复用，没有则创建
          getLoaderManager().initLoader(0, null, this);
      }
  
      @Override public void onCreateOptionsMenu(Menu menu, MenuInflater inflater) {
          // Place an action bar item for searching.
          MenuItem item = menu.add("Search");
          item.setIcon(android.R.drawable.ic_menu_search);
          item.setShowAsAction(MenuItem.SHOW_AS_ACTION_IF_ROOM);
          SearchView sv = new SearchView(getActivity());
          sv.setOnQueryTextListener(this);
          item.setActionView(sv);
      }
  
      public boolean onQueryTextChange(String newText) {
          // Called when the action bar search text has changed.  Update
          // the search filter, and restart the loader to do a new query
          // with this filter.
          curFilter = !TextUtils.isEmpty(newText) ? newText : null;
          getLoaderManager().restartLoader(0, null, this);
          return true;
      }
  
      @Override public boolean onQueryTextSubmit(String query) {
          // Don't care about this.
          return true;
      }
  
      @Override public void onListItemClick(ListView l, View v, int position, long id) {
          // Insert desired behavior here.
          Log.i("FragmentComplexList", "Item clicked: " + id);
      }
  
      // These are the Contacts rows that we will retrieve.
      static final String[] CONTACTS_SUMMARY_PROJECTION = new String[] {
          Contacts._ID,
          Contacts.DISPLAY_NAME,
          Contacts.CONTACT_STATUS,
          Contacts.CONTACT_PRESENCE,
          Contacts.PHOTO_ID,
          Contacts.LOOKUP_KEY,
      };
      public Loader<Cursor> onCreateLoader(int id, Bundle args) {
          // This is called when a new Loader needs to be created.  This
          // sample only has one Loader, so we don't care about the ID.
          // First, pick the base URI to use depending on whether we are
          // currently filtering.
          Uri baseUri;
          if (curFilter != null) {
              baseUri = Uri.withAppendedPath(Contacts.CONTENT_FILTER_URI,
                      Uri.encode(curFilter));
          } else {
              baseUri = Contacts.CONTENT_URI;
          }
  
          // Now create and return a CursorLoader that will take care of
          // creating a Cursor for the data being displayed.
          String select = "((" + Contacts.DISPLAY_NAME + " NOTNULL) AND ("
                  + Contacts.HAS_PHONE_NUMBER + "=1) AND ("
                  + Contacts.DISPLAY_NAME + " != '' ))";
          return new CursorLoader(getActivity(), baseUri,
                  CONTACTS_SUMMARY_PROJECTION, select, null,
                  Contacts.DISPLAY_NAME + " COLLATE LOCALIZED ASC");
      }
  
      public void onLoadFinished(Loader<Cursor> loader, Cursor data) {
          // Swap the new cursor in.  (The framework will take care of closing the
          // old cursor once we return.)
          mAdapter.swapCursor(data);
          
          // The list should now be shown.
  		if (isResumed()) {
  			setListShown(true);
  		} else {
  			setListShownNoAnimation(true);
  		}
      }
  
      public void onLoaderReset(Loader<Cursor> loader) {
          // This is called when the last Cursor provided to onLoadFinished()
          // above is about to be closed.  We need to make sure we are no
          // longer using it.
          mAdapter.swapCursor(null);
      }
  }
  ```

  





#### *9.Android应用链接

可以通过设置Android应用链接以在应用中直接转到链接指向的特定内容，从而绕过应用选择对话框。

Android应用中可以创建两种不同类型的链接：

- 深层链接

  > **深层链接**是指将用户直接转到应用中的特定内容的网址。在 Android 中，您可以通过添加 `intent 过滤器`以及从传入的 intent 提取数据来设置深层链接，以便将用户吸引到正确的 Activity。
  >
  > (不过，如果用户设备上安装的**其他应用可以处理相同的 intent**，则用户可能无法直接进入您的应用。例如，点击银行发来的电子邮件中的网址可能会显示一个对话框，询问用户是使用浏览器还是银行自己的应用打开此链接。)

- Android应用链接

>  **Android 应用链接**使应用能将自己指定为给定类型链接的默认处理程序



详情参考——https://developer.android.google.cn/training/app-links





### 二、Service

> 参考： 
>
> https://blog.csdn.net/hdhhd/article/details/80612726

1. 简介：

   service(服务)是一个一种可以在后台执行长时间运行操作而没有用户界面的应用组件。服务可由其他应用组件启动（如Activity），服务一旦被启动将在后台一直运行，即使启动服务的组件（Activity）已销毁也不受影响。 此外，组件可以绑定到服务，以与之进行交互，甚至是执行进程间通信 (IPC)。 例如，服务可以处理网络事务、播放音乐，执行文件 I/O 或与内容提供程序交互，而所有这一切均可在后台进行 

2.两种状态

```
1.启动状态(未绑定状态)
  当应用组件（如 Activity）通过调用 startService() 启动服务时，服务即处于“启动”状态。一旦启动，服务即可在后台无限期运行，即使启动服务的组件已被销毁也不受影响，除非手动调用才能停止服务， 已启动的服务通常是执行单一操作，而且不会将结果返回给调用方。

2.绑定状态
  当应用组件通过调用 bindService() 绑定到服务时，服务即处于“绑定”状态。绑定服务提供了一个客户端-服务器接口，允许组件与服务进行交互、发送请求、获取结果，甚至是利用进程间通信 (IPC) 跨进程执行这些操作。 仅当与另一个应用组件绑定时，绑定服务才会运行。 多个组件可以同时绑定到该服务，但全部取消绑定后，该服务即会被销毁。
```

#### 启动状态：

3.创建服务流程

①创建服务类

②manifest.xml中声明

```xml
<manifest ... >
  ...
  <application ... >
      <service android:name=".service.SimpleService" 
             
               />
      ...
  </application>
</manifest>
```

```
<service android:enabled=["true" | "false"]

    android:exported=["true" | "false"]

    android:icon="drawable resource"

    android:isolatedProcess=["true" | "false"]

    android:label="string resource"

    android:name="string"

    android:permission="string"

    android:process="string" >

</service>
```

- android:exported：代表是否能被其他应用隐式调用，其默认值是由service中有无intent-filter决定的，如果有intent-filter，默认值为true，否则为false。为false的情况下，即使有intent-filter匹配，也无法打开，即无法被其他应用隐式调用。
- android:name：对应Service类名
- android:permission：是权限声明
- android:process：是否需要在单独的进程中运行,当设置为android:process=”:remote”时，代表Service在单独的进程中运行。注意“：”很重要，它的意思是指要在当前进程名称前面附加上当前的包名，所以“remote”和”:remote”不是同一个意思，前者的进程名称为：remote，而后者的进程名称为：App-packageName:remote。
- android:isolatedProcess ：设置 true 意味着，服务会在一个特殊的进程下运行，这个进程与系统其他进程分开且没有自己的权限。与其通信的唯一途径是通过服务的API(bind and start)。
- android:enabled：是否可以被系统实例化，默认为 true因为父标签 也有 enable 属性，所以必须两个都为默认值 true 的情况下服务才会被激活，否则不会激活。 



③service类需要继承重写的方法

```java
public class SimpleService extends Service {
 
    /**
     * 绑定服务时才会调用
     * 必须要实现的方法  
     * @param intent
     * @return
     */
    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
 
    /**
     * 首次创建服务时，系统将调用此方法来执行一次性设置程序（在调用 onStartCommand() 或 onBind() 之前）。
     * 如果服务已在运行，则不会调用此方法。该方法只被调用一次
     */
    @Override
    public void onCreate() {
        System.out.println("onCreate invoke");
        super.onCreate();
    }
 
    /**
     * 每次通过startService()方法启动Service时都会被回调。
     * @param intent
     * @param flags
     * @param startId
     * @return
     */
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        System.out.println("onStartCommand invoke");
        return super.onStartCommand(intent, flags, startId);
    }
 
    /**
     * 服务销毁时的回调
     */
    @Override
    public void onDestroy() {
        System.out.println("onDestroy invoke");
        super.onDestroy();
    }
}
```

④创建Activity进行测试

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
 
    private Button startBtn;
    private Button stopBtn;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        startBtn= (Button) findViewById(R.id.startService);
        stopBtn= (Button) findViewById(R.id.stopService);
        startBtn.setOnClickListener(this);
        assert stopBtn != null;
        stopBtn.setOnClickListener(this);
    }
 
    @Override
    public void onClick(View v) {
        Intent it=new Intent(this, SimpleService.class);//
        switch (v.getId()){
            case R.id.startService:
                startService(it);//启动服务，仅需要传递一个Intent对象
                break;
            case R.id.stopService:
                stopService(it);//暂停服务
                break;
        }
    }
}
```

使用startService方法启动的服务，在服务外部必须使用stopService()方法停止，在内部调用stopSelf()方法停止当前服务

##### 	重写方法：

**(1)onStartCommand(Intent intent, int flags, int startId)**

```java
1.intent ：启动时，启动组件传递过来的Intent，如Activity可利用Intent封装所需要的参数并传递给Service

2.flags：表示启动请求时是否有额外数据，可选值有 0，START_FLAG_REDELIVERY，START_FLAG_RETRY，0代表没有，它们具体含义如下：

START_FLAG_REDELIVERY 
这个值代表了onStartCommand方法的返回值为 
START_REDELIVER_INTENT，而且在上一次服务被杀死前会去调用stopSelf方法停止服务。其中START_REDELIVER_INTENT意味着当Service因内存不足而被系统kill后，则会重建服务，并通过传递给服务的最后一个 Intent 调用 onStartCommand()，此时Intent时有值的。

START_FLAG_RETRY 
该flag代表当onStartCommand调用后一直没有返回值时，会尝试重新去调用onStartCommand()。

3.startId ： 指明当前服务的唯一ID，与stopSelfResult (int startId)配合使用，stopSelfResult 可以更安全地根据ID停止服务。
```

返回值:

```java
实际上onStartCommand的返回值int类型才是最最值得注意的，它有三种可选值， START_STICKY，START_NOT_STICKY，START_REDELIVER_INTENT，它们具体含义如下：

START_STICKY 
  当Service因内存不足而被系统kill后，一段时间后内存再次空闲时，系统将会尝试重新创建此Service，一旦创建成功后将回调onStartCommand方法，但其中的Intent将是null，除非有挂起的Intent，如pendingintent，这个状态下比较适用于不执行命令、但无限期运行并等待作业的媒体播放器或类似服务。

START_NOT_STICKY 
  当Service因内存不足而被系统kill后，即使系统内存再次空闲时，系统也不会尝试重新创建此Service。除非程序中再次调用startService启动此Service，这是最安全的选项，可以避免在不必要时以及应用能够轻松重启所有未完成的作业时运行服务。

START_REDELIVER_INTENT 
  当Service因内存不足而被系统kill后，则会重建服务，并通过传递给服务的最后一个 Intent 调用 onStartCommand()，任何挂起 Intent均依次传递。与START_STICKY不同的是，其中的传递的Intent将是非空，是最后一次调用startService中的intent。这个值适用于主动执行应该立即恢复的作业（例如下载文件）的服务。
```

#### 绑定状态：

绑定服务是Service的另一种变形，当Service处于绑定状态时，其代表着客户端-服务器接口中的服务器。

当**其他组件（如 Activity）绑定到服务时（有时我们可能需要从Activity组件中去调用Service中的方法，此时Activity以绑定的方式挂靠到Service后，我们就可以轻松地获取Service中的指定方法），组件（如Activity）可以向Service（也就是服务端）发送请求，或者调用Service（服务端）的方法**，此时被绑定的Service（服务端）会接收信息并响应，甚至可以通过绑定服务进行执行进程间通信 (即IPC，这个后面再单独分析)。

与启动服务不同的是绑定服务的生命周期通常只在为其他应用组件(如Activity)服务时处于活动状态，不会无限期在后台运行，也就是说宿主(如Activity)解除绑定后，绑定服务就会被销毁。

那么在提供绑定的服务时，该如何实现呢？实际上我们**必须提供一个 IBinder接口的实现类，该类用以提供客户端用来与服务进行交互的编程接口，可以通过三种方法定义接口**：

> - 扩展 Binder 类 
>   如果服务是提供给自有应用专用的，并且Service(服务端)与客户端在相同的进程中运行（常见情况），则应通过扩展 Binder 类并从 onBind() 返回它的一个实例来创建接口。客户端收到 Binder 后，可利用它直接访问 Binder 实现中以及Service 中可用的公共方法。如果我们的服务只是自有应用的后台工作线程，则优先采用这种方法。 不采用该方式创建接口的唯一原因是，服务被其他应用或不同的进程调用。
>
> - 使用 Messenger 
>   Messenger可以翻译为信使，通过它可以在不同的进程中公共传递Message对象(Handler中的Messager，因此 Handler 是 Messenger 的基础)，在Message中可以存放我们需要传递的数据，然后在进程间传递。如果需要让接口跨不同的进程工作，则可使用 Messenger 为服务创建接口，客户端就可利用 Message 对象向服务发送命令。同时客户端也可定义自有 Messenger，以便服务回传消息。**这是执行进程间通信 (IPC) 的最简单方法，因为 Messenger 会在单一线程中创建包含所有请求的队列，也就是说Messenger是以串行的方式处理客户端发来的消息，这样我们就不必对服务进行线程安全设计了。**
>
> - AIDL
>
>   由于**Messenger是以串行的方式处理客户端发来的消息**，如果当前有大量消息同时发送到Service(服务端)，Service仍然只能一个个处理，这也就是Messenger跨进程通信的缺点了，因此如果有大量并发请求，Messenger就会显得力不从心了，这时AIDL（Android 接口定义语言）就派上用场了， 但实际上Messenger 的跨进程方式其底层实现 就是AIDL，只不过android系统帮我们封装成透明的Messenger罢了 。因此，**如果我们想让服务同时处理多个请求，则应该使用 AIDL。** 在此情况下，服务必须具备多线程处理能力，并采用线程安全式设计。使用AIDL必须创建一个定义编程接口的 .aidl 文件。Android SDK 工具利用该文件生成一个实现接口并处理 IPC 的抽象类，随后可在服务内对其进行扩展。





##### 	**拓展Binder类：**

如果我们的**服务仅供本地应用使用，不需要跨进程工作**，则可以实现自有 Binder 类，让客户端通过该类直接访问服务中的公共方法。其使用开发步骤如下

​	-创建BindService服务端，继承自Service并在类中，创建一个实现IBinder 接口的实例对象并提供公共方法给客户端调用

​	-从 onBind() 回调方法返回此 Binder 实例。

​	-在客户端中，从 onServiceConnected() 回调方法接收 Binder，并使用提供的方法调用绑定服务。

**注意：**

**此方式只有在客户端和服务位于同一应用和进程内才有效**，如对于需要将 Activity 绑定到在后台播放音乐的自有服务的音乐应用，此方式非常有效。另一点之所以要求服务和客户端必须在同一应用内，是为了便于客户端转换返回的对象和正确调用其 API。服务和客户端还必须在同一进程内，因为此方式不执行任何跨进程编组。

```java
/**
 * Created by zejian
 * Time 2016/10/2.
 * Description:绑定服务简单实例--服务端
 */
public class LocalService extends Service{
    private final static String TAG = "wzj";
    private int count;
    private boolean quit;
    private Thread thread;
    private LocalBinder binder = new LocalBinder();
 
    /**
     * 创建Binder对象，返回给客户端即Activity使用，提供数据交换的接口
     */
    public class LocalBinder extends Binder {
        // 声明一个方法，getService。（提供给客户端调用）
        LocalService getService() {
            // 返回当前对象LocalService,这样我们就可在客户端调用Service的公共方法了
            return LocalService.this;
        }
    }
 
    /**
     * 把Binder实例返回给客户端
     */
    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }
 
 
    @Override
    public void onCreate() {
        super.onCreate();
        Log.i(TAG, "Service is invoke Created");
        thread = new Thread(new Runnable() {
            @Override
            public void run() {
                // 每间隔一秒count加1 ，直到quit为true。
                while (!quit) {
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    count++;
                }
            }
        });
        thread.start();
    }
 
    /**
     * 公共方法
     * @return
     */
    public int getCount(){
        return count;
    }
    /**
     * 解除绑定时调用
     * @return
     */
     @Override
    public boolean onUnbind(Intent intent) {
        Log.i(TAG, "Service is invoke onUnbind");
        return super.onUnbind(intent);
    }
 
    @Override
    public void onDestroy() {
        Log.i(TAG, "Service is invoke Destroyed");
        this.quit = true;
        super.onDestroy();
    }
}
```

通过onBind方法返回binder对象，而通过binder对象可以调用BindService实例进而调用service公共方法:getCount()

接下来是Activity:

```java
/**
 * Created by zejian
 * Time 2016/10/2.
 * Description:绑定服务实例--客户端
 */
public class BindActivity extends Activity {
    protected static final String TAG = "wzj";
    Button btnBind;
    Button btnUnBind;
    Button btnGetDatas;
    /**
     * ServiceConnection代表与服务的连接，它只有两个方法，
     * onServiceConnected和onServiceDisconnected，
     * 前者是操作者在连接一个服务成功时被调用，而后者是在服务崩溃或被杀死导致的连接中断时被调用
     */
    private ServiceConnection conn;
    private LocalService mService;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_bind);
        btnBind = (Button) findViewById(R.id.BindService);
        btnUnBind = (Button) findViewById(R.id.unBindService);
        btnGetDatas = (Button) findViewById(R.id.getServiceDatas);
        //创建绑定对象
        final Intent intent = new Intent(this, LocalService.class);
 
        // 开启绑定
        btnBind.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d(TAG, "绑定调用：bindService");
                //调用绑定方法
                bindService(intent, conn, Service.BIND_AUTO_CREATE);
            }
        });
        // 解除绑定
        btnUnBind.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d(TAG, "解除绑定调用：unbindService");
                // 解除绑定
                if(mService!=null) {
                    mService = null;
                    unbindService(conn);
                }
            }
        });
 
        // 获取数据
        btnGetDatas.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mService != null) {
                    // 通过绑定服务传递的Binder对象，获取Service暴露出来的数据
 
                    Log.d(TAG, "从服务端获取数据：" + mService.getCount());
                } else {
 
                    Log.d(TAG, "还没绑定呢，先绑定,无法从服务端获取数据");
                }
            }
        });
 
 
        conn = new ServiceConnection() {
            /**
             * 与服务器端交互的接口方法 绑定服务的时候被回调，在这个方法获取绑定Service传递过来的IBinder对象，
             * 通过这个IBinder对象，实现宿主和Service的交互。
             */
            @Override
            public void onServiceConnected(ComponentName name, IBinder service) {
                Log.d(TAG, "绑定成功调用：onServiceConnected");
                // 获取Binder
                LocalService.LocalBinder binder = (LocalService.LocalBinder) service;
                mService = binder.getService();
            }
            /**
             * 当取消绑定的时候被回调。但正常情况下是不被调用的，它的调用时机是当Service服务被意外销毁时，
             * 例如内存的资源不足时这个方法才被自动调用。
             */
            @Override
            public void onServiceDisconnected(ComponentName name) {
                mService=null;
            }
        };
    }
}
```

在客户端中我们创建了一个ServiceConnection对象，该代表与服务的连接，它只有两个方法， onServiceConnected和onServiceDisconnected，其含义如下：

- **onServiceConnected(ComponentName name, IBinder service)**

  > 统会调用该方法以传递服务的　onBind() 方法返回的 IBinder。其中service便是服务端返回的IBinder实现类对象，通过该对象我们便可以调用获取LocalService实例对象，进而调用服务端的公共方法。而ComponentName是一个封装了组件(Activity, Service, BroadcastReceiver, or ContentProvider)信息的类，如包名，组件描述等信息，较少使用该参数。

- **onServiceDisconnected(ComponentName name)** 

  > Android 系统会在与服务的连接意外中断时（例如当服务崩溃或被终止时）调用该方法。注意:当客户端取消绑定时，系统“绝对不会”调用该方法。



##### 使用Messenger:

前面了解了如何使用IBinder应用内同一进程的通信后，接着来了解服务与远程进程（即不同进程间）通信，而不同进程间的通信，最简单的方式就是使用 Messenger 服务提供通信接口，利用此方式，我们无需使用 AIDL 便可执行进程间通信 (IPC)。以下是 Messenger 使用的主要步骤：

- 1.服务实现一个 Handler，由其接收来自客户端的每个调用的回调
- 2.Handler 用于创建 Messenger 对象（对 Handler 的引用）
- 3.Messenger 创建一个 IBinder实例，服务**通过 onBind() 使其返回客户端**
- 4.客户端使用 IBinder实例 将 Messenger（引用服务的 Handler）实例化，然后使用Messenger将 Message 对象发送给服务
- 5.服务在其 Handler 中（在 handleMessage() 方法中）接收每个 Message

示例:

> 首先创建一个集成service类，然后创建一个继承自Handler的类来接收消息.并进行处理。通过IncomingHandler对象创建一个Messenger对象，该对象是与客户端交互的特殊对象，然后在Service的onBind中返回这个Messenger对象的底层Binder即可:

```java
/**
 * Created by zejian
 * Time 2016/10/3.
 * Description:Messenger服务端简单实例,服务端进程
 */
public class MessengerService extends Service {
 
    /** Command to the service to display a message */
    static final int MSG_SAY_HELLO = 1;
    private static final String TAG ="wzj" ;
 
    /**
     * 用于接收从客户端传递过来的数据
     */
    class IncomingHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case MSG_SAY_HELLO:
                    Log.i(TAG, "thanks,Service had receiver message from client!");
                    break;
                default:
                    super.handleMessage(msg);
            }
        }
    }
 
    /**
     * 创建Messenger并传入Handler实例对象
     */
    final Messenger mMessenger = new Messenger(new IncomingHandler());
 
    /**
     * 当绑定Service时,该方法被调用,将通过mMessenger返回一个实现
     * IBinder接口的实例对象
     */
    @Override
    public IBinder onBind(Intent intent) {
        Log.i(TAG, "Service is invoke onBind");
        return mMessenger.getBinder();
    }
}
```

下面看看客户端进程的实现：

```java
/**
 * Created by zejian
 * Time 2016/10/3.
 * Description: 与服务器交互的客户端
 */
public class ActivityMessenger extends Activity {
    /**
     * 与服务端交互的Messenger
     */
    Messenger mService = null;
 
    /** Flag indicating whether we have called bind on the service. */
    boolean mBound;
 
    /**
     * 实现与服务端链接的对象,判断是否绑定成功，并且回调
     */
    private ServiceConnection mConnection = new ServiceConnection() {
        public void onServiceConnected(ComponentName className, IBinder service) {
            /**
             * 通过服务端传递的IBinder对象,创建相应的Messenger
             * 通过该Messenger对象与服务端进行交互
             */
            mService = new Messenger(service);
            mBound = true;
        }
 
        public void onServiceDisconnected(ComponentName className) {
            // This is called when the connection with the service has been
            // unexpectedly disconnected -- that is, its process crashed.
            mService = null;
            mBound = false;
        }
    };
 
    public void sayHello(View v) {
        if (!mBound) return;
        // 创建与服务交互的消息实体Message
        Message msg = Message.obtain(null, MessengerService.MSG_SAY_HELLO, 0, 0);
        try {
            //发送消息
            mService.send(msg);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_messenager);
        Button bindService= (Button) findViewById(R.id.bindService);
        Button unbindService= (Button) findViewById(R.id.unbindService);
        Button sendMsg= (Button) findViewById(R.id.sendMsgToService);
 
        bindService.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d("zj","onClick-->bindService");
                //当前Activity绑定服务端
                bindService(new Intent(ActivityMessenger.this, MessengerService.class), mConnection,
                        Context.BIND_AUTO_CREATE);
            }
        });
 
        //发送消息给服务端
        sendMsg.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sayHello(v);
            }
        });
 
 
        unbindService.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Unbind from the service
                if (mBound) {
                    Log.d("zj","onClick-->unbindService");
                    unbindService(mConnection);
                    mBound = false;
                }
            }
        });
    }
 
}
```

```xml
<!--manifest声明-->
<service android:name=".messenger.MessengerService"
         android:process=":remote"//代表该Service在单独的进程中创建
        />
```



**服务端回复数据，进行双向通信:**

```java
 /**
     * 用于接收从客户端传递过来的数据
     */
    class IncomingHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case MSG_SAY_HELLO:
                    Log.i(TAG, "thanks,Service had receiver message from client!");
                    //回复客户端信息,该对象由客户端传递过来
                    Messenger client=msg.replyTo;
                    //获取回复信息的消息实体
                    Message replyMsg=Message.obtain(null,MessengerService.MSG_SAY_HELLO);
                    Bundle bundle=new Bundle();
                    bundle.putString("reply","ok~,I had receiver message from you! ");
                    replyMsg.setData(bundle);
                    //向客户端发送消息
                    try {
                        client.send(replyMsg);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
 
                    break;
                default:
                    super.handleMessage(msg);
            }
        }
    }
```

客户端接收服务端回复:

```java
 /**
     * 用于接收服务器返回的信息
     */
    private Messenger mRecevierReplyMsg= new Messenger(new ReceiverReplyMsgHandler());
 
 
    private static class ReceiverReplyMsgHandler extends Handler{
        private static final String TAG = "zj";
 
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                //接收服务端回复
                case MessengerService.MSG_SAY_HELLO:
                    Log.i(TAG, "receiver message from service:"+msg.getData().getString("reply"));
                    break;
                default:
                    super.handleMessage(msg);
            }
        }
```

除此以外，还需要在发送信息时候把回复的Messenger对象传给对方:

```java
public void sayHello(View v) {
        if (!mBound) return;
        // 创建与服务交互的消息实体Message
        Message msg = Message.obtain(null, MessengerService.MSG_SAY_HELLO, 0, 0);
        //把接收服务器端的回复的Messenger通过Message的replyTo参数传递给服务端
        msg.replyTo=mRecevierReplyMsg;
        try {
            //发送消息
            mService.send(msg);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
```



#### 前台服务:

**`前台服务被认为是用户主动意识到的一种服务，因此在内存不足时，系统也不会考虑将其终止。`** **前台服务必须为状态栏提供通知，状态栏位于“正在进行”标题下方，这意味着除非服务停止或从前台删除，否则不能清除通知。**例如将从服务播放音乐的音乐播放器设置为在前台运行，这是因为用户明确意识到其操作。 状态栏中的通知可能表示正在播放的歌曲，并允许用户启动 Activity 来与音乐播放器进行交互。

如果需要设置服务运行于前台， 该如何实现？Android官方给我们提供了两个方法:

- **startForeground(int id, Notification notification)** 
  该方法的作用是把当前服务设置为前台服务，其中id参数代表唯一标识通知的整型数，需要注意的是提供给 startForeground() 的整型 ID 不得为 0，而notification是一个状态栏的通知。
- **stopForeground(boolean removeNotification)** 
  该方法是用来从前台删除服务，此方法传入一个布尔值，指示是否也删除状态栏通知，true为删除。 注意该方法并不会停止服务。 但是，如果在服务正在前台运行时将其停止，则通知也会被删除。

简单的案例:

```java
/**
 * Created by zejian
 * Time 2016/10/4.
 * Description:启动前台服务Demo
 */
public class ForegroundService extends Service {
 
    /**
     * id不可设置为0,否则不能设置为前台service
     */
    private static final int NOTIFICATION_DOWNLOAD_PROGRESS_ID = 0x0001;
 
    private boolean isRemove=false;//是否需要移除
 
    /**
     * Notification
     */
    public void createNotification(){
        //使用兼容版本
        NotificationCompat.Builder builder=new NotificationCompat.Builder(this);
        //设置状态栏的通知图标
        builder.setSmallIcon(R.mipmap.ic_launcher);
        //设置通知栏横条的图标
        builder.setLargeIcon(BitmapFactory.decodeResource(getResources(),R.drawable.screenflash_logo));
        //禁止用户点击删除按钮删除
        builder.setAutoCancel(false);
        //禁止滑动删除
        builder.setOngoing(true);
        //右上角的时间显示
        builder.setShowWhen(true);
        //设置通知栏的标题内容
        builder.setContentTitle("I am Foreground Service!!!");
        //创建通知
        Notification notification = builder.build();
        //设置为前台服务
        startForeground(NOTIFICATION_DOWNLOAD_PROGRESS_ID,notification);
    }
 
 
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        int i=intent.getExtras().getInt("cmd");
        if(i==0){
            if(!isRemove) {
                createNotification();
            }
            isRemove=true;
        }else {
            //移除前台服务
            if (isRemove) {
                stopForeground(true);
            }
            isRemove=false;
        }
 
        return super.onStartCommand(intent, flags, startId);
    }
 
    @Override
    public void onDestroy() {
        //移除前台服务
        if (isRemove) {
            stopForeground(true);
        }
        isRemove=false;
        super.onDestroy();
    }
 
    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
}
```

在ForegroundService类中，创建了一个notification的通知，并通过启动Service时传递过来的参数判断是启动前台服务还是关闭前台服务，最后在onDestroy方法被调用时，也应该移除前台服务。以下是ForegroundActivity的实现：

```java
/**
 * Created by zejian
 * Time 2016/10/4.
 * Description:
 */
public class ForegroundActivity extends Activity {
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_foreground);
        Button btnStart= (Button) findViewById(R.id.startForeground);
        Button btnStop= (Button) findViewById(R.id.stopForeground);
        final Intent intent = new Intent(this,ForegroundService.class);
 
 
        btnStart.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                intent.putExtra("cmd",0);//0,开启前台服务,1,关闭前台服务
                startService(intent);
            }
        });
 
 
        btnStop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                intent.putExtra("cmd",1);//0,开启前台服务,1,关闭前台服务
                startService(intent);
            }
        });
    }
}
```

![这里写图片描述](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/20161004113551060)



#### 服务与线程：

- **两者概念的迥异**
  - Thread 是程序执行的最小单元，它是分配CPU的基本单位，android系统中UI线程也是线程的一种，当然Thread还可以用于执行一些耗时异步的操作。
  - Service是Android的一种机制，服务是运行在主线程上的，它是由系统进程托管。它与其他组件之间的通信类似于client和server，是一种轻量级的IPC通信，这种通信的载体是binder，它是在linux层交换信息的一种IPC，而所谓的Service后台任务只不过是指没有UI的组件罢了。
- **两者的执行任务迥异**
  - 在android系统中，线程一般指的是工作线程(即后台线程)，而主线程是一种特殊的工作线程，它负责将事件分派给相应的用户界面小工具，如绘图事件及事件响应，因此为了保证应用 UI 的响应能力主线程上不可执行耗时操作。如果执行的操作不能很快完成，则应确保它们在单独的工作线程执行。
  - Service 则是android系统中的组件，一般情况下它运行于主线程中，因此在Service中是不可以执行耗时操作的，否则系统会报ANR异常，之所以称Service为后台服务，大部分原因是它本身没有UI，用户无法感知(当然也可以利用某些手段让用户知道)，但如果需要让Service执行耗时任务，可在Service中开启单独线程去执行。
- **两者使用场景**
  - 当要执行耗时的网络或者数据库查询以及其他阻塞UI线程或密集使用CPU的任务时，都应该使用工作线程(Thread)，这样才能保证UI线程不被占用而影响用户体验。
  - 在应用程序中，如果需要长时间的在后台运行，而且不需要交互的情况下，使用服务。比如播放音乐，通过Service+Notification方式在后台执行同时在通知栏显示着。
- **两者的最佳使用方式**
  - 在大部分情况下，Thread和Service都会结合着使用，比如下载文件，一般会通过Service在后台执行+Notification在通知栏显示+Thread异步下载，再如应用程序会维持一个Service来从网络中获取推送服务。在Android官方看来也是如此，所以官网提供了一个Thread与Service的结合来方便我们执行后台耗时任务，它就是IntentService，(如果想更深入了解IntentService，可以看另一篇文章：[Android 多线程之IntentService 完全详解](http://blog.csdn.net/javazejian/article/details/52426425))，当然 IntentService并不适用于所有的场景，但它的优点是使用方便、代码简洁，不需要我们创建Service实例并同时也创建线程，某些场景下还是非常赞的！由于IntentService是单个worker thread，所以任务需要排队，因此不适合大多数的多任务情况。
- 总结：
  - 两种没有任何关系

#### 服务生命周期：

![这里写图片描述](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/20161004164521384)

#### [AIDL](#AIDL)

> https://blog.csdn.net/luoyanglizi/article/details/51980630
>
> https://www.jianshu.com/p/29999c1a93cd





### 三、ContentProvider



#### (1)概述：

对于每一个应用程序来说，如果想要访问contentProvider中共享的数据，就一定要借助ContentResolver 类，可以通过Context中的getContentResolver() 方法获取该类的实例。ContentResolver中提供了一系列的方法用于对数据进行CRUD操作，其中insert() 方法用于添加数据，update() 方法用于更新数据，delete() 方法用于删除数据，query() 方法用于查询数据。

##### ①权限

> 使用CP意味着访问存储，因此需要提供权限
>
> ```xml
> <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
> <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
> ```

#### (2)应用：

##### ①访问content provider

> 通过`ContentResolver`访问CP

##### ②内容URI

> ContentResolver 中的增删改查都是接收一个URl参数，这个参数被称为**内容URI**。
>
> > 内容URL给内容提供器中的数据建立了唯一标识符，**它主要由两部分组成：authority 和 path** 。authority 是用于对不同的应用程序做区分的，一般为了避免冲突，都会采用程序包名的方式进行命名。path则是用于对同一应用程序中不同的表做区分，通常都会添加到authority后面：
>
> ```java
> content://com.example.app.provider/table1
> content://com.example.app.provider/table2
> ```
>
> 在使用内容`URL`作为参数的时候，需要将`URL`转换成`URI`对象：
>
> ```java
> Uri uri = Uri.parse("content://com.example.app.provider/table1")
> ```
>
> 当然，有些情况可以将ID值添加到URI末尾，代表访问某一行
>
> ```java
> Uri singleUri = ContentUris.withAppendedId(UserDictionary.Words.CONTENT_URI,4);//访问该conten第四行
> ```
>
> **URI格式**
>
> ```java
> content://com.example.app.provider/table1
> content://com.example.app.provider/table1/1
> 
> * : 表示匹配任意长度的任意字符
> # : 表示匹配任意长度的数字
> 
> //一个能够匹配任意表的内容URI格式就可以写成:
> content://com.example.app.provider/*
> //一个能够匹配表中任意一行数据的内容URI格式就可以写成：
> content://com.example.app.provider/table1/#
> ```
>

##### ③查询

> 使用这个uri对象来查询talbe1表中的数据了：
>
> ```java
> Cursor cursor = getContentResolver().query（
> 	uri,
> 	projection,
> 	selection,
> 	selectionArgs,
> 	sortOrder
> ）;
> ```
>
> | **query()方法参数** | **对应SQL部分**             | **描述**                                 |
> | ------------------- | --------------------------- | ---------------------------------------- |
> | uri                 | `from table_name`           | 指定查询某个应用程序下的某个表           |
> | projection          | `select column1, column2`   | 指定查询的列名                           |
> | selection           | `where [column]=value`      | 指定where约束条件，代表column的值        |
> | selectArgs          | `where column=[value]`      | 为where中的占位符提供具体的值，代表value |
> | orderBy             | `order by column1, column2` | 指定查询结果的排序方式                   |
>
> > **防SQL注入**
> >
> > 推荐使用`selection`、`selectArgs`来一起定义查询的范围条件，例如：
> >
> > ```java
> > String selectionClause =  "id = ?";
> > String[] selectionArgs = {""};
> > selectionArgs[0] = userInput; //最终会拼接成 "id = selectionArgs[0]"
> > ```
>
> 
>
> **查询完进行取值，Cursor是以行为方向移动的**
>
> ```java
> if(cursor != null){
> 	while(cursor.moveToNext()) {
>         //cursor.getColumnIndex("column1")获取ColumnIndex的索引
>         //cursor.getString()获取该列的值
> 		String column1 = cursor.getString(cursor.getColumnIndex("column1"));
> 		int column2 = cursor.getInt(cursor.getColumnIndex("column2"));
> 	}
> 	cursor.close();
> }
> ```
>
> 
>
> **展示数据，可以结合ListView进行数据展示， 通过 `SimpleCursorAdapter` 将其与 `ListView` 关联。 **
>
> ```java
> // 定义要获取的列名
> String[] wordListColumns =
> {
>     UserDictionary.Words.WORD,   // Contract class constant containing the word column name
>     UserDictionary.Words.LOCALE  // Contract class constant containing the locale column name
> };
> 
> // 定义获取的结果列表
> int[] wordListItems = { R.id.dictWord, R.id.locale};
> 
> // Creates a new SimpleCursorAdapter
> cursorAdapter = new SimpleCursorAdapter(
>     getApplicationContext(),               // The application's Context object
>     R.layout.wordlistrow,                  // A layout in XML for one row in the ListView
>     mCursor,                               // The result from the query
>     wordListColumns,                      // A string array of column names in the cursor
>     wordListItems,                        // An integer array of view IDs in the row layout
>     0);                                    // Flags (usually none are needed)
> 
> // Sets the adapter for the ListView
> wordList.setAdapter(cursorAdapter);
> ```



##### ④插入:

> ```java
> ContentValues values = new ContentValues();
> values.put("column1", "text");
> values.put("column2", 1);
> Uri newUri = getContentResolver().insert(uri, values);
> ```
>
> - 如果不想指定值，则可以使用 `ContentValues.putNull(columnName)` 将列设置为 `null`。 
>
> - 系统会自动为插入的行插入`_ID`值作为主键
>
> -  `newUri` 中返回的内容 URI 会按以下格式标识新添加的行： 
>
>   > ```
>   > content://user_dictionary/words/<id_value>
>   > ```
>   >
>   >  如需从返回的 `Uri` 中获取 `_ID` 的值，请调用 `ContentUris.parseId()`。 



##### ⑤更新:

> ```java
> ContentValues valuse = new ContentValues();
> valuse.put("column1", "");
> String selectionClause = "column1 = ? and column2 = ?";
> String[] selectionArgs = {"1","2"};
> getContentResolver().update(uri, values, selectionClause,selectionArgs);
> ```
>
> 



##### ⑥删除:

> ```java
> getContentResolver().delete(uri , "column2 = ?", new String[]{ "1"});
> ```

#### (3)访问CP替代方法

- **批量访问**：可以使用 `ContentProviderOperation` 类中的方法创建一批访问调用，然后通过 `ContentResolver.applyBatch()` 应用这些调用。
- **异步查询**：您应该在单独的线程中进行查询。执行此操作的一种方法是使用 `CursorLoader` 对象。(加载器已经弃用)
- **通过 Intent 访问数据**：尽管您无法直接向提供程序发送 Intent，但您可以向提供程序的应用发送 Intent，后者通常是修改提供程序数据的最佳手段。



##### ①批量访问

> 参考——https://blog.csdn.net/Siobhan/article/details/54983233?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-54983233-blog-72763923.235%5Ev36%5Epc_relevant_default_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-54983233-blog-72763923.235%5Ev36%5Epc_relevant_default_base3&utm_relevant_index=2

>   **bulkInsert**
>
> 如果只是涉及到单表的批量插入，我们可以直接使用 `bulkInsert(Uri uri, ContentValues[] values)` 进行批量插入即可。 
>
> 
>
> **applyBatch**
>
> 批量访问**适用于插入大量行**，或通过同一方法调用在多个表中插入行，或者通常用于以事务（原子操作）的形式跨进程边界执行一组操作。 
>
>  可以创建 `ContentProviderOperation` 对象数组，然后使用 `ContentResolver.applyBatch()` 将其分派给内容提供程序。 
>
> ```java
> //源码
> public @NonNull ContentProviderResult[] applyBatch(@NonNull String authority,
>                                                    @NonNull ArrayList<ContentProviderOperation> operations)
>     throws RemoteException, OperationApplicationException {
>     Objects.requireNonNull(authority, "authority");
>     Objects.requireNonNull(operations, "operations");
> 
>     try {
>         if (mWrapped != null) return mWrapped.applyBatch(authority, operations);
>     } catch (RemoteException e) {
>         return null;
>     }
> 
>     ContentProviderClient provider = acquireContentProviderClient(authority);
>     if (provider == null) {
>         throw new IllegalArgumentException("Unknown authority " + authority);
>     }
>     try {
>         return provider.applyBatch(operations);
>     } finally {
>         provider.release();
>     }
> }
> ```
>
> **创建 ContentProviderOperation对象 **
>
>  通过调用下面几个静态函数来获取一个Builder 对象： 
>
> | 函数           | 用途                                                         |
> | -------------- | ------------------------------------------------------------ |
> | newInsert      | 创建一个用于执行插入操作的Builder（支持多表）                |
> | newUpdate      | 创建一个用于执行更新操作的Builder                            |
> | newDelete      | 创建一个用于执行删除操作的Builder                            |
> | newAssertQuery | 可以理解为断点查询，也就是查询有没有符合条件的数据，如果没有，会抛出一个OperationApplicationException异常 |
>
> ```java
> //创建插入操作对象
> ArrayList<ContentProviderOperation> ops =
>     new ArrayList<ContentProviderOperation>();
> 
> //ContactsContract是联系人相关的协定类
> ContentProviderOperation.Builder op =
>     ContentProviderOperation.newInsert(ContactsContract.RawContacts.CONTENT_URI)
>     .withValue(ContactsContract.RawContacts.ACCOUNT_TYPE, mSelectedAccount.getType())
>     .withValue(ContactsContract.RawContacts.ACCOUNT_NAME, mSelectedAccount.getName());
> 
> ops.add(op.build());
> ```
>
> ```java
> //插入
> try {
>    getContentResolver().applyBatch(ContactsContract.AUTHORITY, ops);
> } catch (RemoteException e) {
>    // do s.th.
> } catch (OperationApplicationException e) {
>    // do s.th.
> }
> ```
>
> > 对于批量插入的优化，参考——https://blog.csdn.net/firedancer0089/article/details/72763923?ydreferer=aHR0cHM6Ly9jbi5iaW5nLmNvbS8%3D



#### (4)自定义CP

构建CP一般分以下几个步骤

1. **为数据设计原始存储**。CP以两种方式提供数据：

   - 文件数据

     通常存储在文件中的数据，如照片、音频或视频。存储在应用的私有空间内。CP可以应其他应用发出的文件请求提供文件句柄。

   - “结构化”数据

     通常存储在数据库、数组或类似结构中的数据。以兼容行列表的形式存储数据。此类数据通常存储在 SQLite 等任何类型的持久存储数据库中。

2. 定义 `ContentProvider` 类及其所需方法的具体实现。

3. 定义提供程序的授权字符串、该字符串的内容 URI 以及列名称。如果您想让提供程序的应用处理 Intent，则还需定义 Intent 操作、Extra 数据和标志。此外，请为访问数据的应用定义必须具备的权限。您应该考虑在单独的协定类中将所有这些值定义为常量，以便之后向其他开发者公开此类。

4. 添加其他可选部分，如示例数据或可在提供程序与云数据之间同步数据的 `AbstractThreadedSyncAdapter` 实现。



##### ①设计数据存储

> 参考——https://developer.android.google.cn/guide/topics/providers/content-provider-creating?hl=zh-cn

##### ②设计URI

- 设计授权

  >  可以将提供程序授权定义为包含该提供程序的软件包名称的扩展名。例如，**如果 Android 软件包名称为 `com.example.<appname>`，则应为提供程序提供 `com.example.<appname>.provider` 授权。** 

- 设计路径结构

  >  如果您有 *table1* 和 *table2* 两个表格，则可以通过合并上一示例中的授权来生成内容 URI `com.example.<appname>.provider/table1` 和 `com.example.<appname>.provider/table2`。路径并不仅限于单个段，并且您无需为各级路径创建表。 

- 处理内容URI ID

  >  按照约定，提供程序会接受末尾拥有行 ID 值的内容 URI，进而提供对表内单个行的访问。同样按照约定，提供程序会将该 ID 值与表的 `_ID` 列进行匹配，并对匹配的行执行请求访问。 
  >
  > **该点的一个用处是，如果使用ListView展示数据，那么我们可以通过id取CP里面找到对于数据修改**

- 内容URI模式

  > sdk提供了  `UriMatcher` 类，它会将内容 URI“模式”映射为整型值。可以在 `switch` 语句中使用这些整型值，为匹配特定模式的一个或多个内容 URI 选择所需操作。 
  >
  > ```java
  > public class ExampleProvider extends ContentProvider {
  >     ...
  >         // Creates a UriMatcher object.
  >         private static final UriMatcher uriMatcher = new UriMatcher(UriMatcher.NO_MATCH);
  > 
  >     static {
  >         //对table3全部操作
  >         uriMatcher.addURI("com.example.app.provider", "table3", 1);
  >         //对table3的某一行操作，因为加入了通配符'#'
  >         uriMatcher.addURI("com.example.app.provider", "table3/#", 2);
  >     }
  >     ...
  >         // Implements ContentProvider.query()
  >         public Cursor query(
  >         Uri uri,
  >         String[] projection,
  >         String selection,
  >         String[] selectionArgs,
  >         String sortOrder) {
  >         ...
  >            //选择要查询的URI
  >             switch (uriMatcher.match(uri)) {
  > 
  > 
  >                     // If the incoming URI was for all of table3
  >                 case 1:
  > 
  >                     if (TextUtils.isEmpty(sortOrder)) sortOrder = "_ID ASC";
  >                     break;
  > 
  >                     // If the incoming URI was for a single row
  >                 case 2:
  > 
  >                     /*
  >                  * Because this URI was for a single row, the _ID value part is
  >                  * present. Get the last path segment from the URI; this is the _ID value.
  >                  * Then, append the value to the WHERE clause for the query
  >                  */
  >                     selection = selection + "_ID = " + uri.getLastPathSegment();
  >                     break;
  > 
  >                 default:
  >                     ...
  >                         // If the URI is not recognized, you should do some error handling here.
  >             }
  >         // call the code to actually do the query
  >     }
  > ```
  >
  > 

##### ③实现Contentprovider类

要是先contentprovider类必须要实现几个基本方法

```
query()
insert()
update()
delete()
getType() 返回内容 URI 对应的 MIME 类型。
onCreate()
```



> **实现onCreate方法**
>
> ```java
> public class ExampleProvider extends ContentProvider{
> 
>     // Defines a handle to the Room database
>     private AppDatabase appDatabase;
> 
>     // Defines a Data Access Object to perform the database operations
>     private UserDao userDao;
> 
>     // Defines the database name
>     private static final String DBNAME = "mydb";
> 
>     public boolean onCreate() {
> 
>         // Creates a new database object.
>         appDatabase = Room.databaseBuilder(getContext(), AppDatabase.class, DBNAME).build();
> 
>         // Gets a Data Access Object to perform the database operations
>         userDao = appDatabase.getUserDao();
> 
>         return true;
>     }
> 
>     ...
> 
>         // Implements the provider's insert method
>         public Cursor insert(Uri uri, ContentValues values) {
>         // Insert code here to determine which DAO to use when inserting data, handle error conditions, etc.
>     }
> }
> ```
>
> 

##### ④实现权限

> 参考——https://juejin.cn/post/7069690786547236871

CP端定义权限

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.anddle.lifetime">


    //声明自定义权限
    <permission
                android:name="com.anddle.provideraccess"
                android:label="provider Permission"
                android:protectionLevel="normal" />

    <application
		....
                //设置权限
                 <provider
    				android:name=".MyContentProvider"
    				android:authorities="com.anddle.mycontentprovider"
    				android:enabled="true"
    				android:exported="true"
    				android:permission="com.anddle.provideraccess" />

    <application/>
</manifest>
```

客户端调用

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.anddle.calculator">

//使用权限
    <uses-permission android:name="com.anddle.provideraccess"/>

    <application/>

</manifest>
```

还可以对某部分URI设置权限。看参考文档



### 四、Broadcast

#### (1)概述:

1）用于不同组件间的通信（含：应用内/不同应用之间）
2）用于多线程通信
3）与android系统的通信

**android 广播分为两个角色：广播发送者、广播接收者**



##### 类型：

> - 标准广播
>
>   > 是一种完全异步执行的广播，广播发出后，所以的广播接收器几乎会在同一时刻接收到这条广播消息。但是意味着他无法被截断
>
> - 有序广播
>
>   > 是一种同步执行的广播，广播发出之后，同一时刻只会有一个广播接收器能够收到这条广播信息，当这个广播接收器逻辑执行完毕之后，广播才会继续传递。优先级高的广播接收器就可以先收到广播消息，并且前面的广播接收器还可以截断广播。

#### (2)接收广播：

Android内置很多系统级别的广播，比如手机开机完成后会发送一条广播，电池电量发生变化会发出一条广播

​	

**自定义广播接收器:**

- 继承BroadcastReceive 基类
- 必须重写onReceive()方法

**注意：**

> 1.广播接收器收到相应广播后，会自动调用onReceive(） 方法
> 2.一般情况下，onReceive方法会涉及与其他组件之间的交互，如 发送Notification，启动server等
> 3.默认情况下，**广播接收器运行在UI线程**，因此，onReceive方法不能执行耗时操作，否则将导致ANR

##### BroadcastReceiver注册:

**注册的方式有两种：静态注册、动态注册**

###### **静态注册:**

>  注册方式：**在AndroidManifest.xml 里通过< receive >标签声明**
>
> **创建方式：右击创建BroadcastReceiver可以快速创建一个广播接收器**
>
> 静态注册的方式可以让程序在未启动情况下就能够接收到广播

```xml
<user-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<user-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>

<receive
    //表示是否启用这个广播接收器
	android:enable="true"/"false"
         
	//exported：是否接受其他应用发出的广播
	//默认值时由receiver 中d有无inter-filter决定，如果有，默认true，否则默认false
	android:exported="true"/"false"
         
	android:icon="drawable resource"
	android:label="string resource"
        
	//创建的接收器的类名
    android:name=".mBroadcastReceiver"
        
	//具有相应权限的广播发送者发送的广播才能被此BroadcastReceiver所接收；
    android:permission="string"
        
	//BroadcastReceiver运行所处的进程
	//默认为app的进程，可以指定独立的进程
	//注：Android四大基本组件都可以通过此属性指定自己的独立进程
    android:process="string" >

<!--用于指定此广播接收器将接收的广播类型-->
 <intent-filter>
     <!--用于接收网络状态改变时发出的广播-->
	<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
     <!--用于接收开机之后系统发出的广播-->
     <action android:name="android.intent.action.BOOT_COMPLETED" />
 </intent-filter>
 </receive>
```

```java
//创建的接收器
public class mBroadcastReceiver extends BroadcastReceiver{
    @Override
    public void onReceive(Context context,Intent intent){
        
    }
}

```



###### **动态注册:**

> 通过代码注册，每当网络状态变化时，onReceive方法就会执行

```java
    //继承BroadcastReceiver类
	class mBroadcastReceiver extends BroadcastReceiver{
        @override
        public void onReceive(Context context,Intent intent){
            
        }
    }

	// 1. 实例化BroadcastReceiver子类 &  IntentFilter
     mBroadcastReceiver mBroadcastReceiver = new mBroadcastReceiver();
     IntentFilter intentFilter = new IntentFilter();

    // 2. 设置接收广播的类型：监听网络变化
    intentFilter.addAction(android.net.conn.CONNECTIVITY_CHANGE);

    // 3. 动态注册：调用Context的registerReceiver（）方法
     registerReceiver(mBroadcastReceiver, intentFilter);


	@override
	protected void onDestroy(){
        //动态注册广播后，需要在相应位置记得销毁广播
		unregisterReceiver(mBroadcastReceiver);
    }

```

> **注意:*动态广播最好在onResume中注册， onPause注销***

> 一个具体的例子来监听网络是否可用
>
> ```java
> public class MainActivity extends AppCompatActivity{
>     ...
>     //内部类自定义广播接收器
>     class NetworkChangeReceiver extends BroadcastReceiver{
>         @override
>         public void onReceive(Context context,Intent intent){
>             
>             //通过getSystemService()得到connectionManager实例，这是一个系统服务类，专门用于管理网络连接
>             ConnectivityManager connectionManager = (ConnectivityManager)getSystemService(Context.CONNECTIVITY_SERVICE);
>             
>             NetworkInfo networkInfo = connectionManager.getActiveNetworkInfo();
>             
>             //判断网络是否有用
>             if(networkInfo != null && networkInfo.isAvailable()){
>                 //网络可用
>             }else{
>                 //网络不可用
>             }
>         }
>     }
> }
> ```
>
> > **不要忘记在Manifest里面进行网络权限声明**

#### (3)发送广播:

**广播的发送是通过intent 的 sendBroasdcast() 方法发送出去**

> - 标准广播
>
>   > ```java
>   > Intent intent = new Intent("com.example.broadcasttest.MY_BROADCAST");
>   > 
>   > sendBroadcast(intent);
>   > ```
>
> - 有序广播
>
>   > ```java
>   > //第二个参数是和权限相关的字符串
>   > sendOrderedBroadcast(intent,null);
>   > ```
>   >
>   > 通过manifest设置优先级
>   >
>   > ```xml
>   > <receive>
>   > 	<intent-filter android:priority="100">
>   >     	<action android:name="..."></action>
>   >     </intent-filter>
>   > </receive>
>   > ```
>   >
>   > 如果优先级高的将广播进行截断:
>   >
>   > ```java
>   > @Override
>   > public void onReceive(){
>   >     abortBroadcast();
>   > }
>   > ```



##### 使用本地广播：

我们上面发送的都是全局广播，可以被其他任何应用程序收到，但是这可能会造成一些问题，比如安全数据泄露，收到广播垃圾等等。

为了解决安全性问题，Android 引入广播机制，使得广播只能在内部传播

> ```java
> 
> //使用manager进行广播管理
> private LocalBroadcastManager localBroadcastManager;
> 
> 
> public void onCreate(Bundle savedInstanceState){
>     ...
>     //获取实例
>     localBroadcastManager = LocalBroadcastManager.getInstance(this);
>     
>     .....
>     //发送广播
>     Intent intent = new Intent("com.example.broadcasttest.MY_BROADCAST");
>     localBroadcastManager.sendBroadcast(intent);
> }
> ```
>
> ```java
> //“自定义”本地接收器
> private LocalReceiver localReceiver;
> //使用manager进行广播管理
> private LocalBroadcastManager localBroadcastManager;
> 
> localReiver = new LoacalReceiver();
> 
> localBroadcastManager.registerReceiver(localReceiver,intentFilter);
> ```
>
> > 实质上就是通过一个LocalBroadcastManager进行管理，其他本质不同；
> >
> > <font color="red">本地广播无法通过静态注册方式进行接收</font>



#### (4)广播应用

用于强制下线









## 跨进程通信方式

#### 1.概述：

 由于应用程序之间不能共享内存。在不同应用程序之间交互数据（跨进程通讯），在android SDK中提供了4种用于跨进程通讯的方式。这4种方式正好对应于android系统中4种应用程序组件：Activity、Content Provider、Broadcast和Service。其中Activity可以跨进程调用其他应用程序的Activity；Content Provider可以跨进程访问其他应用程序中的数据（以Cursor对象形式返回），当然，也可以对其他应用程序的数据进行增、删、改操 作；Broadcast可以向android系统中所有应用程序发送广播，而需要跨进程通讯的应用程序可以监听这些广播；Service和Content Provider类似，也可以访问其他应用程序中的数据，但不同的是，Content Provider返回的是Cursor对象，而Service返回的是Java对象，这种可以跨进程通讯的服务叫AIDL服务。

#### 2.应用：

##### Activity:

Activity的跨进程访问与进程内访问略有不同。虽然它们都需要Intent对象，但跨进程访问并不需要指定Context对象和Activity的 Class对象，而需要指定的是要访问的Activity所对应的Action（一个字符串）。有些Activity还需要指定一个Uri（通过 Intent构造方法的第2个参数指定）。

```java
// 在android系统中有很多应用程序提供了可以跨进程访问的Activity，例如，下面的代码可以直接调用拨打电话的Activity。
Intent callIntent = new  Intent(Intent.ACTION_CALL, Uri.parse("tel:12345678" ));  
startActivity(callIntent);
```



###### (1)将Intent和数据关联：

intent 通常还包括与操作相关的数据，例如您要查看的地址或您要发送的电子邮件。根据您要创建的 intent，数据可能是 `Uri` 或其他几种数据类型之一，也可能该 intent 根本不需要数据。

如果您的数据是 `Uri`，则可以使用一个简单的 `Intent()` 构造函数来定义操作和数据。

```java
Uri webpage = Uri.parse("https://www.android.com");
Intent webIntent = new Intent(Intent.ACTION_VIEW, webpage);
```



**添加额外数据：**

其他类型的隐式 intent 需要“额外”数据，以提供字符串等不同数据类型。您可以使用各种不同的 `putExtra()` 方法添加一条或多条额外数据。

默认情况下，系统根据所包含的 `Uri` 数据确定 intent 需要的相应 [MIME](https://www.cnblogs.com/xuecanmeng/p/5258617.html) 类型。如果您不在 intent 中包含一个 `Uri`，通常应使用 `setType()` 来指定与 intent 相关联的数据类型。设置 MIME 类型可以进一步指定应接收 intent 的 activity 类型。

以下是一些添加了额外数据来指定所需操作的其他 intent：

**发送带有附件的电子邮件**:

```java
Intent emailIntent = new Intent(Intent.ACTION_SEND);
// The intent does not have a URI, so declare the "text/plain" MIME type
emailIntent.setType(HTTP.PLAIN_TEXT_TYPE);
emailIntent.putExtra(Intent.EXTRA_EMAIL, new String[] {"jan@example.com"}); // recipients
emailIntent.putExtra(Intent.EXTRA_SUBJECT, "Email subject");
emailIntent.putExtra(Intent.EXTRA_TEXT, "Email message text");
emailIntent.putExtra(Intent.EXTRA_STREAM, Uri.parse("content://path/to/email/attachment"));
// You can also attach multiple items by passing an ArrayList of Uris
```



**创建日历活动**

```java
// Event is on January 23, 2021 -- from 7:30 AM to 10:30 AM.
Intent calendarIntent = new Intent(Intent.ACTION_INSERT, Events.CONTENT_URI);
Calendar beginTime = Calendar.getInstance();
beginTime.set(2021, 0, 23, 7, 30);
Calendar endTime = Calendar.getInstance();
endTime.set(2021, 0, 23, 10, 30);
calendarIntent.putExtra(CalendarContract.EXTRA_EVENT_BEGIN_TIME, beginTime.getTimeInMillis());
calendarIntent.putExtra(CalendarContract.EXTRA_EVENT_END_TIME, endTime.getTimeInMillis());
calendarIntent.putExtra(Events.TITLE, "Ninja class");
calendarIntent.putExtra(Events.EVENT_LOCATION, "Secret dojo");
```



###### (2)允许其他应用访问你的Activity

如果需要允许其他应用访问我们的Activity，则需要在清单文件中添加`<intent-filter>`进行intent过滤

**过滤的条件有三个：**

- **action**:对要执行的操作命名的字符串。通常是平台定义的值之一，比如 `ACTION_SEND` 或 `ACTION_VIEW`。
- **data**:使用 `<data>`元素在您的 intent 过滤器中指定此内容。使用此元素中的一个或多个属性，您可以只指定 MIME 类型、URI 前缀、URI 架构，也可以指定这些内容的组合以及其他指示所接受数据类型的项。
- **category**:通常与用户手势或 activity 启动的位置有关。系统支持多种不同的类别，但大多数都很少使用。不过，所有隐式 intent 默认使用 `CATEGORY_DEFAULT` 进行定义。

例如

```xml
<activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
        <data android:mimeType="image/*"/>
    </intent-filter>
</activity>
```

> **提示**：如果您希望选择器对话框中的图标与 activity 的默认图标不同，请在<intent-filter> 元素中添加 `android:icon`。



##### 	ContentProvider

 Android应用程序可以使用文件或SqlLite数据库来存储数据。Content Provider提供了一种在多个应用程序之间数据共享的方式（跨进程共享数据）。应用程序可以利用Content Provider完成下面的工作

1. 查询数据
   2. 修改数据
      3. 添加数据
         4. 删除数据

Android系统本身提供了很多Content Provider，例如，音频、视频、联系人信息等等。我们可以通过这些Content Provider获得相关信息的列表。这些列表数据将以Cursor对象返回。因此，从Content Provider返回的数据是二维表的形式。



##### 广播(Broadcast)

 广播是一种被动跨进程通讯的方式。当某个程序向系统发送广播时，其他的应用程序只能被动地接收广播数据。这就象电台进行广播一样，听众只能被动地收听，而不能主动与电台进行沟通。
在应用程序中发送广播比较简单。只需要调用sendBroadcast方法即可。该方法需要一个Intent对象。通过Intent对象可以发送需要广播的数据。



##### <a name=AIDL>Service(AIDL)</a>

> [Android binder机制](https://juejin.cn/post/6892212234496425991)

`AIDL (Android Interface Definition Language，Android 接口定义语言)` 是一种文件格式，用来简化 Binder 的使用。当使用 Binder 的时候，只需要创建一个后缀名为 `.aidl` 的文件，然后像定义接口一样定义方法。定义完毕之后，使用工具 `aidl.exe` 即可生成 Binder 所需要的各种文件。当然，我们的 AS 已经为我们集成了 `aidl.exe`，所以，只需要在定义了 AIDL 文件之后，**编译**即可生成使用 `Binder` 时所需的文件。当然，不使用 AIDL，直接编写 Binder 所需的 java 文件也是可以的。



**Android 中提供了 AIDL 供我们使用Binder进行进程间通信，具体使用参考消息传递机制**