# TextView

1.通过button是实现简单的跳转，然后显示带有TextView的界面

新建一个EmptyActivity

![1606998253253](D:\typora\图片\1606998253253.png)

然后修改主界面

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:id="@+id/btn_1"
        android:text="登陆"
        android:textSize="20sp"/>
</LinearLayout>
```

![1606998299432](D:\typora\图片\1606998299432.png)

添加点击事件

```java
public class MainActivity extends AppCompatActivity {

    private Button mbtn;//声明button对象
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mbtn = findViewById(R.id.btn_1);//通过id找到要绑定点击事件的按钮
        mbtn.setOnClickListener(new View.OnClickListener() {//点击事件函数
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this,TextViewActivity.class);//声明跳转界面
                startActivity(intent);//执行intent
            }
        });
    }
}
```



TextView相关属性

```xml
 <!--省略文字多余部分，以点代替-->
<TextView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:id="@+id/tv_2"
        android:text="@string/tv_1"
        android:textSize="30sp"
        android:paddingLeft="20dp"
        android:paddingTop="10dp"
        android:maxLines="1"
        android:ellipsize="end" #作用是当文字长度超过textview宽度时的显示方式:
        />
```

![1607000424472](D:\typora\图片\1607000424472.png)



```xml
<TextView
        android:id="@+id/tv_3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:drawableRight="@drawable/weather" #在该文本框右边添加图片，来自drawable目录
        android:paddingLeft="20dp"
        android:paddingTop="10dp"
        android:text="杨海涛"
        android:textSize="30sp" />
```

![1607000501938](D:\typora\图片\1607000501938.png)

```java
        /*设置中划线效果*/
        mtv = findViewById(R.id.tv_4);
        mtv.getPaint().setFlags(Paint.STRIKE_THRU_TEXT_FLAG);//中划线
        //mtv.getPaint().setAntiAlias(true);//去除锯齿
        /*设置下划线效果*/
        mtv2 = findViewById(R.id.tv_5);
        mtv2.getPaint().setFlags(Paint.UNDERLINE_TEXT_FLAG);//下划线
```

![1607000287566](D:\typora\图片\1607000287566.png)

```xml
 <!--设置跑马灯效果-->
<TextView
        android:id="@+id/tv_6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:drawablePadding="2dp"
        android:paddingLeft="20dp"
        android:paddingTop="10dp"
        android:text="万可爱万可爱万可爱万可爱万可爱"
        android:textSize="30sp"
        android:singleLine="true"  #等效于（android:lines="1"  android:ellipsize="end"）
        android:ellipsize="marquee"  #以横向滚动方式显示(需获得当前焦点时)
        android:marqueeRepeatLimit="marquee_forever" #循环次数
        android:focusable="true"  #获取焦点
        android:focusableInTouchMode="true" #获取焦点
  />
```



## 由于在实际的开发中，页面的布局往往比较复杂，如果显示两个跑马灯的文字，这种方法只能实现第一个，第二个不会出现效果。采用如下方法：

创建一个类继承TextView

```java
package com.yht.musicaudio;

import android.annotation.SuppressLint;
import android.content.Context;
import android.util.AttributeSet;
import android.widget.TextView;

@SuppressLint("AppCompatCustomView")
public class Marquee_Text extends TextView {
    public Marquee_Text(Context context) {
        super(context);
    }

    public Marquee_Text(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public Marquee_Text(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    public boolean isFocused() {
        return true;
    }
}

```



然后就可以自己调用自己写的textview

```xml
 <com.yht.musicaudio.Marquee_Text
            android:id="@+id/song_name"
            android:layout_width="100dp"
            android:layout_height="wrap_content"
            android:layout_alignParentRight="true"
            android:layout_alignParentBottom="true"
            android:layout_marginTop="12dp"
            android:layout_marginRight="30dp"
            android:layout_marginBottom="31dp"
            android:text=""
            android:textSize="20sp"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:marqueeRepeatLimit="marquee_forever"
            android:focusable="true"
            android:focusableInTouchMode="true" />

        <com.yht.musicaudio.Marquee_Text
            android:id="@+id/singer_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentRight="true"

            android:layout_marginRight="50dp"
            android:layout_marginTop="40dp"
            android:layout_alignLeft="@+id/song_name"
            android:layout_marginLeft="1dp"
            android:text=""
            android:singleLine="true"
            android:ellipsize="marquee"
            android:marqueeRepeatLimit="marquee_forever"
            android:focusable="true"
            android:focusableInTouchMode="true"/>
```



