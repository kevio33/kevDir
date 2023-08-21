# ScrollView

## 注意：scrollview里面只能有一个直接子元素，所以可以创建一个linearlayout，然后再linear里面创建控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--整个页面布局设置为ScrollView，垂直的-->
<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:padding="10dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        >
        <Button
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:id="@+id/btn_1"
            android:text="登陆"
            android:textSize="20sp" />

        <Button
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:text="EditText"
            android:textAllCaps="false"
            android:textSize="20sp"
            android:id="@+id/edittext"/>

        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Border"
            android:textSize="20sp"
            android:background="@drawable/btn_press"
            android:onClick="showToast"/>

        <Button
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:text="RadioButton"
            android:textAllCaps="false"
            android:textSize="20sp"
            android:id="@+id/radiobt"/>
        <Button
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:text="CheckBox"
            android:textAllCaps="false"
            android:textSize="20sp"
            android:id="@+id/cb"/>
        <Button
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:text="ImgView"
            android:textAllCaps="false"
            android:textSize="20sp"
            android:id="@+id/im_view"/>
        <Spinner
            android:id="@+id/planets_array"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content" />
        <!--在里面设置一个水平的ScrollView-->
        <HorizontalScrollView
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="horizontal">
                <Button
                    android:layout_width="match_parent"
                    android:layout_height="50dp"
                    android:text="CheckBox"
                    android:textAllCaps="false"
                    android:textSize="20sp"
                    android:id="@+id/btn1"/>
                <Button
                    android:layout_width="match_parent"
                    android:layout_height="50dp"
                    android:text="ImgView"
                    android:textAllCaps="false"
                    android:textSize="20sp"
                    android:id="@+id/btn2"/>
                <Button
                    android:layout_width="match_parent"
                    android:layout_height="50dp"
                    android:text="ImgView"
                    android:textAllCaps="false"
                    android:textSize="20sp"
                    android:id="@+id/btn3"/>
                <Button
                    android:layout_width="match_parent"
                    android:layout_height="50dp"
                    android:text="ImgView"
                    android:textAllCaps="false"
                    android:textSize="20sp"
                    android:id="@+id/btn4"/>
                <Button
                    android:layout_width="match_parent"
                    android:layout_height="50dp"
                    android:text="ImgView"
                    android:textAllCaps="false"
                    android:textSize="20sp"
                    android:id="@+id/btn5"/>
            </LinearLayout>
        </HorizontalScrollView>
    </LinearLayout>

</ScrollView>
```

![1607420111228](D:\typora\图片\1607420111228.png)