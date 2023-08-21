# ImageView

```xml
 
<ImageView
        android:layout_width="300dp"
        android:layout_height="200dp"
        android:background="#00ff00"
        android:src="@drawable/ultraman"  #图片名称一定要有字母
        android:scaleType="centerCrop"/>  
         <!--scaleType的三个属性：1.fitXY：撑满控件，宽高比会被拉伸；
                                 2.fitCenter/center：保持宽高比缩放，直至能够完全显示
                                 3.centerCrop：保持宽高比缩放，直至完全覆盖控件，裁剪显                                                    示-->
  
```

![1607413953006](D:\typora\图片\1607413953006.png)

## 使用imgView加载网络图片

1.使用第三方库，去git上面找到glide库插件

```groovy
/*在项目gradle里面引入依赖*/
repositories {
  google()
  jcenter()
}

dependencies {
  implementation 'com.github.bumptech.glide:glide:4.11.0'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.11.0'
}
```

用法

```java
// For a simple view:
@Override public void onCreate(Bundle savedInstanceState) {
  ...
  ImageView imageView = (ImageView) findViewById(R.id.my_image_view);

  Glide.with(this).load("http://goo.gl/gEgYUd").into(imageView);
}
```

由于是要进行网络请求，所以要到manifest.xml中开启网络权限

```xml
 <!--声明开启网络权限-->
    <uses-permission android:name="android.permission.INTERNET"/>
```

