# Button

Android中所有的控件都具有这两个属性，可选值有3种：**match_parent、fill_parent、wrap_content.**

其中match_parent和fill_parent的意义相同，但官方更推荐match_parent.

==match_parent表示让当前控件的大小和父布局的大小一样，也就是由父布局来决定当前控件的大小==

==wrap_content表示让当前的控件大小能够刚好包含里面的内容，也就是由控件内容决定当前控件的大小==

```xml
<Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        android:text="EditText"
        android:textAllCaps="false" #默认组件的text为全部大写，false取消默认
        android:textSize="20sp"/>
```



### 1.改变button颜色，边框

在drawable里面创建drawable resource file

![1607236391143](D:\typora\图片\1607236391143.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"    android:shape="rectangle">     
     <solid   android:color="#ff0000"></solid> <!--设置颜色-->   
     <corners android:radius="10dp"></corners> <!--设置边框幅度-->
</shape>
```

```xml
android:background="@drawable/btn_1"  //设计文件调用
```

![1607236546705](D:\typora\图片\1607236546705.png)

**2.边框：**

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">

    <stroke android:width="1dp"
            android:color="#00ff00"/> <!--设计边框样式-->
    <solid android:color="#ffffff"/><!--注意！！一定要填充，否则按钮显示不出来-->
</shape>
```

![1607238024215](D:\typora\图片\1607238024215.png)

### 3.按压效果

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="false"><!--按压前样式-->
        <shape>
            <solid android:color="#ff0000"/>
        </shape>
    </item>
    <item android:state_pressed="true"><!--按压时候样式-->
        <shape>
            <solid android:color="#ffffff"/>
        </shape>
    </item>
</selector>
```



### 4.设置按钮点击事件（两种办法）

```java
  //①在Activity里面书写方法，且必须带参数View
    public void showToast(View view){
        /**
         * Toast.LENGTH_SHORT是提示显示的时长
         * 最后必须调用show()方法，否则不会弹出
         */
        Toast.makeText(this,"欢迎您",Toast.LENGTH_SHORT).show();
    }
```

```properties
android:onClick="showToast" #调用
```



或者：

```java
/*或者在OnClick方法里面*/
 mbtn = findViewById(R.id.btn_1);
        mbtn.setOnClickListener(new View.OnClickListener() {//点击事件
            @Override
            public void onClick(View view) {
              Toast.makeText(this,"欢迎您",Toast.LENGTH_SHORT).show();
            }
        });
```

