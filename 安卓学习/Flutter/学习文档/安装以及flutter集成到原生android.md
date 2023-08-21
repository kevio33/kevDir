# 一、安装

1.官网下载flutter sdk

2.配置环境变量：

将下载的flutter sdk中bin目录放进系统环境变量

![1615038055306](D:\typora\图片\1615038055306.png)

由于下载一些插件需要外网，所以添加镜像环境变量

![1615038000755](D:\typora\图片\1615038000755.png)



3.打开命令提示符界面

![1615038310654](D:\typora\图片\1615038310654.png)

4.打开android,搜索flutter插件

![1615038823579](D:\typora\图片\1615038823579.png)

提示

![img](https://img-blog.csdnimg.cn/20200302232218287.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzE3MDI5Nw==,size_16,color_FFFFFF,t_70) 

解决办法：

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302232226215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzE3MDI5Nw==,size_16,color_FFFFFF,t_70) 

然后：

 ![打钩](https://img-blog.csdnimg.cn/202004201431297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW43OTM5OTE4MzM=,size_16,color_FFFFFF,t_70) 

5.flutter doctor android前面显示感叹号

```
flutter doctor运行失败5（[X] Android Studio (not installed)）
你安装了AS，只是路径是自定义的，所以给Flutter指定你的路径吧
flutter config --android-studio-dir="D:\Android Studio"
```

6.以上配置完成后，打开as，在plugins里面搜索下载flutter插件

7.然后就可以新建一个flutter项目

8.在build gradle的时候一直running ，可能解决办法,添加阿里云镜像

```groovy
buildscript {
    repositories {

        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven { url 'http://maven.aliyun.com/nexus/content/repositories/jcenter' }
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {

        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven { url 'http://maven.aliyun.com/nexus/content/repositories/jcenter' }
        google()
        jcenter()
    }
}

```





在AndroidStudio升级到最新版本之后发现，安装了插件但是重启有没有，原因是:

 **因为我们在修改默认缓存位置之后，Android Studio会将插件下载到`${idea.system.path}/plugins`目录下，而安装位置却是在`${idea.config.path}/plugins`目录下，所以只要修改安装位置那就可以正常的加载部分插件(`.jar`)了。** 

所以在local.properties里面加一句:

```properties
idea.plugins.path=${idea.system.path}/plugins
```

**但是这样的方法会造成如果插件是安装包的话，就需要去该目录下手动解压**

参考:https://blog.csdn.net/include_gril_friend_/article/details/106910688

创建的Flutter项目，可以运行起来；但是打开MainActivity一直报红，提示`Can't resolve symbol FlutterActivity` 错误

 选择android目录右键Flutter->Open Android module in Android Studio,这样就可以在另一个窗口打开android代码，并且不会报错
![Flutter项目打开android代码](https://img-blog.csdnimg.cn/20200808174914652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2tpbmd5YzEyMzQ1Njc4OQ==,size_16,color_FFFFFF,t_70) 



# 二、集成到原生Android

### 	1.在原生app下创建一个flutter module

![image-20210423144527952](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210423144527952.png)

![image-20210423144551620](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210423144551620.png)

此时ide以及自动帮你配置好了一切，只需要调到第三步即可

### 2.进行集成:

在主app项目下的build.gradle下添加

```groovy
 compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
```

### 3.引入flutter模块:

#### 	①通过主modul模块依赖方式进行依赖

将flutter作为module，然后native主工程引入进来。这种方式适合参与人数比较少的项目，如果有多人协作开发的大型项目就不合适了，因为其他人首先要配置Flutter环境，而且团队里面其他人还要配置module的依赖，都要熟悉flutter，成本是很高的。

- 在工程的setting.gradle增加以下配置

  ```groovy
  include ':app'                                    // assumed existing content
  setBinding(new Binding([gradle: this]))                                // new
  evaluate(new File(                                                     // new
    settingsDir.parentFile,                                              // new
    'my_flutter/.android/include_flutter.groovy'//该路径是从app文件夹开始，找到flutter模块
  ))    
  ```

- 在build.gradle中添加依赖

  ```groovy
  dependencies {
    implementation project(':flutter')
  }
  ```

  

#### ②通过AAR方式引入

详情参考:https://flutter.dev/docs/development/add-to-app/android/project-setup



### 4.在android中引用flutter模块

**步骤1：将FlutterActivity添加到AndroidManifest.xml**

```xml
<activity
  android:name="io.flutter.embedding.android.FlutterActivity"
  android:theme="@style/LaunchTheme"
  android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
  android:hardwareAccelerated="true"
  android:windowSoftInputMode="adjustResize"
  />
```

**步骤二:使用缓存加载FlutterEngine,实现预加载**

```java
public class MyApplication extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    // Instantiate a FlutterEngine.
    flutterEngine = new FlutterEngine(this);

    // Start executing Dart code to pre-warm the FlutterEngine.
    flutterEngine.getDartExecutor().executeDartEntrypoint(
      DartEntrypoint.createDefault()
    );

    // Cache the FlutterEngine to be used by FlutterActivity.
    FlutterEngineCache
      .getInstance()
      .put("my_engine_id", flutterEngine);
  }
}
```

当要调用的时候:

```java
myButton.addOnClickListener(new OnClickListener() {
  @Override
  public void onClick(View v) {
    startActivity(
      FlutterActivity
        .withCachedEngine("my_engine_id")
        .build(currentActivity)
      );
  }
});
```

