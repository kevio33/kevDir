# RadioButton

```java
/*由于每一个按钮点击跳转事件都重复写太过繁琐，以下为更新*/
public class MainActivity extends AppCompatActivity {

    private Button mbtn;//声明对象
    private Button mbtnEditText;//edittext
    private Button radiobtn;//radiobutton
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mbtn = findViewById(R.id.btn_1);//找到控件
        mbtnEditText = findViewById(R.id.edittext);
        radiobtn = findViewById(R.id.radiobt);
        setClicklistener();//调用方法
      
        /*之气方法，如果按钮组件过多，每次重写会很麻烦*/
        /*  radiobtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this,RadioActivity.class);
                startActivity(intent);
            }
        });*/
        
        }
		//通过该方法进行点击事件调用
        public void setClicklistener(){
            OnClick onClick = new OnClick();
            mbtn.setOnClickListener(onClick);
            mbtnEditText.setOnClickListener(onClick);
            radiobtn.setOnClickListener(onClick);
        }
    private class OnClick implements View.OnClickListener{//内部类实现接口View.OnClickListener
        @Override
        public void onClick(View view) {
            Intent intent = null;
            switch (view.getId()){//通过switch语句进行选择跳转
                case R.id.btn_1:
                    intent = new Intent(MainActivity.this,TextViewActivity.class);//实现界面跳转
                    break;
                case R.id.edittext:
                    intent = new Intent(MainActivity.this,EditTextActivity.class);
                    break;
                case R.id.radiobt:
                    intent = new Intent(MainActivity.this,RadioActivity.class);
                    break;
            }
            startActivity(intent);
        }
    }
}
```



### **!! gravity属性 !!**

**补上一个需要记忆的点:**

**android:layout_gravity面向的是整个控件对于父控件的位置**

**android:gravity面向的是控件中内容对于这个控件的位置。** 



RadioButton控件的布局文件

```xml
<!--当多个rb时，要设置只能选一个就要放进radiogroup里面-->
<RadioGroup
        android:id="@+id/rg"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"> #排列方式
        <RadioButton
            android:id="@+id/rb_1"
            android:layout_width="50dp"
            android:layout_height="30dp"
            android:text="男"
            android:textSize="20sp"
            android:checked="true"   #默认选中该按钮
            />
        <RadioButton
            android:id="@+id/rb_2"
            android:layout_width="50dp"
            android:layout_height="30dp"
            android:text="女"
            android:textSize="20sp"
            />
 </RadioGroup>
```

![1607392629808](D:\typora\图片\1607392629808.png)

为该组radiobutton添加选中监听事件

```java
private RadioGroup radioGroup;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_radio);

        radioGroup = findViewById(R.id.rg);
        radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {//设置该组的选中事件
            @Override
            public void onCheckedChanged(RadioGroup radioGroup, int i) {
                RadioButton radioButton = radioGroup.findViewById(i);//通过id查找选中的radiobutton
                /**
                 * makeText()，是Toast的一个方法，用来显示信息，分别有三个参数。
                 * 第一个参数：this，是上下文参数，指当前页面显示
                 * 第二个参数：“string string string ”是你想要显示的内容，
                 * 第三个参数：Toast.LENGTH_LONG，是你指你提示消息，显示的时间，这个是稍微长点儿，对应的另一个是ToastLENGTH_SHORT，这个时间短点儿，大概2秒钟。
                 * show()，表示显示这个Toast消息提醒，当程序运行到这里的时候，就会显示出来，如果不调用show()方法，这个Toast对象存在，但是并不会显示，所以一定不要忘记。
                 */
                Toast.makeText(RadioActivity.this,radioButton.getText(),Toast.LENGTH_SHORT).show();//将选中按钮的text显示出来
            }
        });

    }
```

