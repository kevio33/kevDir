# Dialog

### 1.默认样式的dialog

```java
                    AlertDialog.Builder builder = new AlertDialog.Builder(AlertdialogActivity.this);
                    builder.setTitle("请回答");//设置标题
                    builder.setMessage("你觉得杨海涛帅吗");//设置内容
                    builder.setIcon(R.drawable.weather);//设置图标

                    //alertdialog提供三种类型按键，这是positive类型的
                   builder.setPositiveButton("帅", new DialogInterface.OnClickListener() {
                       @Override
                       public void onClick(DialogInterface dialogInterface, int i) {
                           ToastUtil.showMsg(AlertdialogActivity.this,"真诚实");//点击事件为显示toast
                       }
                   });
                   ///neutral类型
                    builder.setNeutralButton("一般般", new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialogInterface, int i) {
                            ToastUtil.showMsg(getApplicationContext(),"滚");
                        }
                    });
                    //negative类型
                   builder.setNegativeButton("不好看", new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialogInterface, int i) {
                            ToastUtil.showMsg(getApplicationContext(),"闭上臭嘴");
                        }
                    });
                    //最重要的要调用show方法
                    builder.show();
```

效果:

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607591407810.png" alt="1607591407810" style="zoom:67%;" />

### 2.设置item

```javascript
                    final String[] arry = new String[]{"男","女"};
                    builder.setTitle("性别");
                    builder.setItems(arry, new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialogInterface, int i) {
                            ToastUtil.showMsg(getApplicationContext(),arry[i]);
                        }
                    });
                    //最重要的要调用show方法
                    builder.show();
```

效果：

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607592109127.png" alt="1607592109127" style="zoom:67%;" />



### 3.实现类似radiobutton功能

```java
final String[] arry2 = new String[]{"男","女"};
builder.setTitle("性别").setSingleChoiceItems(arry2, 0, new DialogInterface.OnClickListener() {
		@Override
		public void onClick(DialogInterface dialogInterface, int i) {
			ToastUtil.showMsg(getApplicationContext(),arry2[i]);
             dialogInterface.dismiss();//点击item以后对话框消失
		}
});
builer.setCancelable(false);//点击空白处对话框不会消失
builder.show();
```

效果:

![1607592731188](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607592731188.png)

### 4.实现checkbox功能

```java
final String[] arry3 = new String[]{"数学","语文","英语"};
boolean[] isSelect = new boolean[]{false,false,false};

builder.setTitle("选择科目");
builder.setMultiChoiceItems(arry3, isSelect, new DialogInterface.OnMultiChoiceClickListener() {
		@Override
		public void onClick(DialogInterface dialogInterface, int i, boolean b) {
         	ToastUtil.showMsg(getApplicationContext(),arry3[i]);
         	}
});
builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
         @Override
         public void onClick(DialogInterface dialogInterface, int i) {

         }
});
builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
         @Override
         public void onClick(DialogInterface dialogInterface, int i) {
         }
});
  builder.show();
```

![1607599819676](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607599819676.png)

### 5.完成登录界面

新建自定义布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">

    <EditText
        android:id="@+id/ed_dialog1"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:hint="Username" />
    <EditText
        android:id="@+id/ed_dialog2"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:hint="Password"
        android:inputType="textPassword"
        android:layout_below="@+id/ed1"/>
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="login"
        android:textSize="20sp"
        android:id="@+id/dialog_login"/>
</LinearLayout>
```



```java
      //找到布局，返回类型为view
       View view1 = LayoutInflater.from(getApplicationContext()).inflate(R.layout.login_dialog,null);
       builder.setTitle("登录界面").setView(view1);//将布局放进dialog
       Button button = view1.findViewById(R.id.dialog_login);
       button.setOnClickListener(new View.OnClickListener() {
             @Override
             public void onClick(View view) {

             }
       });
       builder.show();
```

![1607601157067](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607601157067.png)

