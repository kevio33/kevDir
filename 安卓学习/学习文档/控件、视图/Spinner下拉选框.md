# CheckBox

```xml
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="你会什么编程技术"
        android:textSize="25sp" />
    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Java"
        android:textSize="20sp"
        android:id="@+id/cb1"
        />
    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="C++"
        android:textSize="20sp"
        android:id="@+id/cb2"
        />
    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="C#"
        android:textSize="20sp"
        android:id="@+id/cb3"
        />
    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="None"
        android:textSize="20sp"
        android:id="@+id/cb4"
        />
```





![1607395144768](D:\typora\图片\1607395144768.png)

自定义按钮样式

![1607395432666](D:\typora\图片\1607395432666.png)

![1607395527370](D:\typora\图片\1607395527370.png)



编写他的点击事件

```java
    private CheckBox checkBox1;
    private CheckBox checkBox2;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_check_box);

        checkBox1 = findViewById(R.id.cb1);
        checkBox2 = findViewById(R.id.cb2);
        checkBox1.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
                Toast.makeText(CheckBoxActivity.this,b?"选中java":"未选中java",Toast.LENGTH_SHORT).show();
            }
        });

        checkBox2.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean b) {
                Toast.makeText(CheckBoxActivity.this,b?"选中C++":"未选中C++",Toast.LENGTH_SHORT).show();
            }
        });
    }
```

