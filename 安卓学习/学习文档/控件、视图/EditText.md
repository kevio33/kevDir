# EditText

新建跳转EditText按钮

```xml
 <Button
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:layout_marginTop="10dp"
        android:text="EditText"
        android:textAllCaps="false" #默认组件的text为全部大写，false取消默认
        android:textSize="20sp"/>
```



```xml
 <EditText
        android:id="@+id/ed1"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:hint="Username"  
    />
  <EditText
        android:id="@+id/ed2"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:hint="Password"   #类似于html的placeholder，输入提示
        android:inputType="textPassword"  #出现暗文，可以更改输入类型，例如数字
        android:layout_below="@+id/ed1"
     />
```

![1607241410291](D:\typora\图片\1607241410291.png)



### 设置EditText的监听事件，反馈在log在日志中

```java
        private EditText editText;
        editText = findViewById(R.id.ed1);
        editText.addTextChangedListener(new TextWatcher() {
            @Override
            public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2) {

            }

            @Override
            public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
                Log.d("监听edittext",charSequence.toString());//输入实时监听
            }

            @Override
            public void afterTextChanged(Editable editable) {

            }
        });
```

![1607241846829](D:\typora\图片\1607241846829.png)

