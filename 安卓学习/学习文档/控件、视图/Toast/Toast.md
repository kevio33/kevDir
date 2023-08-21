# Toast

```java
//默认样式
Toast.makeText(getApplicationContext(),"Toast",Toast.LENGTH_LONG).show();//getApplicationContext()就是指当前Activity

//更改显示位置
 Toast toastcenter = Toast.makeText(getApplicationContext(),"toast",Toast.LENGTH_LONG);
 toastcenter.setGravity(Gravity.CENTER,0,0);//让它居中
 toastcenter.show();
```



```java
//更改显示样式，例如加上图片和文字
Toast toastCustom = new Toast(ToastActivity.this);//创建toast对象
LayoutInflater layoutInflater = LayoutInflater.from(ToastActivity.this);//声明布局对象
View view1 = layoutInflater.inflate(R.layout.layout_toast,null);//添加布局，返回view类型

ImageView imageView = view1.findViewById(R.id.iv_toast);
imageView.setImageResource(R.drawable.weather);//为imgview添加图片
TextView textView = view1.findViewById(R.id.tv_toast);
textView.setText("您好");//为textview添加文本

toastCustom.setView(view1);//将view添加进toast
toastCustom.setDuration(Toast.LENGTH_SHORT);//设置显示长度
toastCustom.show();
```

```xml
<!--自定义布局文件layout_toast.xml-->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#99000000"  
    android:gravity="center">
    <!--由于跟布局linearlayout的长宽会失效，所以在设置一个layout-->
    <LinearLayout
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:gravity="center"
        android:orientation="vertical">
        <ImageView
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:id="@+id/iv_toast"
            android:scaleType="centerCrop"/>

        <TextView
            android:id="@+id/tv_toast"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:textColor="#ffffff"
            android:textSize="18sp" />
    </LinearLayout>

</LinearLayout>
```

效果图

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607586077133.png" alt="1607586077133" style="zoom:67%;" />



由于toast也属于对象，所以如果连续按压会导致每一个toast等待前面一个toast结束之后才会显示，为此，书写一个工具类进行封装优化

```java
public class ToastUtil {
    public static Toast toast;
    public static void showMsg(Context context,String msg){//传过来context和text内容
        if(toast == null){
            toast = Toast.makeText(context,msg,Toast.LENGTH_LONG);//如果没有被调用，则初始化toast
        }else{
            toast.setText(msg);//否则只给他添加text
        }
        toast.show();
    }
}
```

```java
//调用
ToastUtil.showMsg(getApplicationContext(),"toastutil");
```

