<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607844381023.png" alt="1607844381023" style="zoom: 50%;" />



安卓自定义的progressbar样式很多：

![1607844872045](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607844872045.png)

自定义样式

![1607848199283](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607848199283.png)

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607848247344.png" alt="1607848247344" style="zoom: 50%;" />

![1607848094882](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607848094882.png)



## 进度条

```xml
<ProgressBar
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/pb4"
        style="@android:style/Widget.Material.ProgressBar.Horizontal"
        android:max="100"  //进度条长度
        android:progress="10"  //当前进度
        android:secondaryProgress="30"  //二级进度
        tools:ignore="NewApi" />
```

![1607846509760](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607846509760.png)

实现进度条加载功能

```java
public class ProgressBarActivity extends AppCompatActivity {

    private Button button;
    private ProgressBar progressBar;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_progress_bar);

        progressBar = findViewById(R.id.pb3);
        button = findViewById(R.id.pb_btn);//按钮点击开始加载
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                handler.sendEmptyMessage(0);//像handler发送消息
            }
        });
    }

    Handler handler = new Handler(){
        @Override
        public void handleMessage(@NonNull Message msg) {
            super.handleMessage(msg);
            if(progressBar.getProgress()<100){
                handler.postDelayed(runnable,500);//进行处理，如果进度小于100，则0.5s延迟后发送给runnable

            }else {
                ToastUtil.showMsg(ProgressBarActivity.this,"加载成功");
            }
        }
    };

    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            progressBar.setProgress(progressBar.getProgress()+5);//加载进度条
            handler.sendEmptyMessage(0);//继续发送消息给handler
        }
    };
}
```



### progressdialog

```java
button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                ProgressDialog progressDialog = new ProgressDialog(ProgressBarActivity.this);
                progressDialog.setTitle("Tips");
                progressDialog.setMessage("加载中");
                
                //点击其他地方不得取消，若实际中加载完成可调用progressDialog.dismiss();
                progressDialog.setCancelable(false);
                progressDialog.show();
            }
        });
```

![1607849100946](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607849100946.png)

进度条

```java
    button3.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                final ProgressDialog progressDialog = new ProgressDialog(ProgressBarActivity.this);
                progressDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
                progressDialog.setTitle("tips");
                progressDialog.setMessage("加载中");
                progressDialog.setButton(DialogInterface.BUTTON_POSITIVE, "取消", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        progressDialog.dismiss();
                    }
                });
                progressDialog.show();
                progressDialog.setProgress(10);
            }
        });
```

![1607850043302](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607850043302.png)

