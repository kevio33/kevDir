# Popup window

创建PopupActivity

创建一个按钮

```xml
 <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="PopUp"
        android:layout_marginTop="30dp"
        android:id="@+id/Pop_up"/>
```

然后创建一个布局文件，设置下拉的内容

![1607861209118](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607861209118.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="数学"
        android:textSize="30sp"
        android:id="@+id/tx_1"/>
    <View
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:background="#000000"/>
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="语文"
        android:textSize="30sp"

        android:layout_marginTop="10dp"/>
    <View
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:background="#000000"/>
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="英语"
        android:textSize="30sp"

        android:layout_marginTop="10dp"/>
    <View
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:background="#000000"/>

</LinearLayout>
```

![1607861222780](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607861222780.png)

PopupActivity里面代码

```java
public class PopupActivity extends AppCompatActivity {

    private Button button;
    private PopupWindow popupWindow;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_popup);
        button = findViewById(R.id.Pop_up);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
               
                View view1 = getLayoutInflater().inflate(R.layout.popup_list,null);//找到视图
                TextView textView = view1.findViewById(R.id.tx_1);//找到textview
                textView.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        popupWindow.dismiss();//popup消失
                        ToastUtil.showMsg(PopupActivity.this,textView.getText().toString());
                    }
                });
                popupWindow = new PopupWindow(view1,button.getWidth(), ViewGroup.LayoutParams.WRAP_CONTENT);//声明对象，添加布局，宽度为button宽度
                popupWindow.setOutsideTouchable(true);//点击外部区域会消失，与Cancleable相反
                popupWindow.setFocusable(true);//每次点击按钮都不会弹出来，而是点一次关掉，下一次打开交替的动作
                popupWindow.showAsDropDown(button);//设置展示的位置

            }
        });
    }
}
```

