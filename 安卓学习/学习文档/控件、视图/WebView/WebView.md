# WebView

![1607500399012](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607500399012.png)





![1607500610273](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607500610273.png)

![1607500756229](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607500756229.png)





1.访问本地Assets目录下资源

![1607501099935](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607501099935.png)

![1607501550403](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607501550403.png)



创建WebView

```xml
<WebView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/wv1"></WebView>
```

activity

```java
public class WebViewActivity extends AppCompatActivity {

    private WebView webView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_web_view);
        webView = findViewById(R.id.wv1);
        webView.loadUrl("file:///android_asset/kevBaiDu.html");//加载本地assets内容

    }
}
```



加载网络网页

```java
public class WebViewActivity extends AppCompatActivity {

    private WebView webView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_web_view);
        webView = findViewById(R.id.wv1);

        //加载本地网页
        //webView.loadUrl("file:///android_asset/kevBaiDu.html");
        //加载网络资源
        webView.getSettings().setJavaScriptEnabled(true);//添加js支持，否则加载不出来
        webView.setWebViewClient(new MyWebViewClient());//添加webview client,网页跳转在本应用进行
        webView.loadUrl("https://m.baidu.com");//m代表mobile,与web端不同
    }

    class MyWebViewClient extends WebViewClient{

        //当网页跳转时就在本应用上加载,不会让你去其他应用加载
        @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {
           view.loadUrl(request.getUrl().toString());
            return true;
        }

        //网页开始加载显示log
        @Override
        public void onPageStarted(WebView view, String url, Bitmap favicon) {
            super.onPageStarted(view, url, favicon);
            Log.d("webView","started");
        }

        //网页加载结束显示
        @Override
        public void onPageFinished(WebView view, String url) {
            super.onPageFinished(view, url);
            Log.d("weview","ended");
        }
    }

    //是否能够回退，如果可以返回上一级，而不是直接结束activity
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (keyCode == KeyEvent.KEYCODE_BACK && webView.canGoBack()){
            webView.goBack();
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }
}

```



要实现一些其他功能

```java
 //WebChromeClient可以提供一些与网页相关的方法
    class MyWebChromeClient extends WebChromeClient{
        @Override
        //实现网页加载进度条展示
        public void onProgressChanged(WebView view, int newProgress) {
            super.onProgressChanged(view, newProgress);
        }

        @Override
        //加载网页标题
        public void onReceivedTitle(WebView view, String title) {
            super.onReceivedTitle(view, title);
            setTitle(title);
        }
    }

//调用
 webView.setWebChromeClient(new MyWebChromeClient());//添加WebChromeClient
```



activity完整代码

```java
public class WebViewActivity extends AppCompatActivity {

    private WebView webView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_web_view);
        webView = findViewById(R.id.wv1);

        //加载本地网页

        /*webView.getSettings().setDomStorageEnabled(true);
        webView.getSettings().setUseWideViewPort(true);//适应分辨率
        webView.getSettings().setLoadWithOverviewMode(true);
        webView.loadUrl("file:///android_asset/login.html");*/
        //加载网络资源
        webView.getSettings().setJavaScriptEnabled(true);//添加js支持，否则加载不出来
        webView.setWebViewClient(new MyWebViewClient());//添加webview client
        webView.setWebChromeClient(new MyWebChromeClient());//添加WebChromeClient
        webView.loadUrl("https://m.baidu.com");//m代表mobile,与web端不同*/
    }

    class MyWebViewClient extends WebViewClient{

        //当网页跳转时就在本应用上加载,不会让你去其他应用加载
        @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {
           view.loadUrl(request.getUrl().toString());
            return true;
        }

        //网页开始加载显示log
        @Override
        public void onPageStarted(WebView view, String url, Bitmap favicon) {
            super.onPageStarted(view, url, favicon);
            Log.d("webView","started");
        }

        //网页加载结束显示
        @Override
        public void onPageFinished(WebView view, String url) {
            super.onPageFinished(view, url);
            Log.d("weview","ended");
        }
    }
    //WebChromeClient可以提供一些与网页相关的方法
    class MyWebChromeClient extends WebChromeClient{
        @Override
        //实现网页加载进度条展示
        public void onProgressChanged(WebView view, int newProgress) {
            super.onProgressChanged(view, newProgress);
        }

        @Override
        //加载网页标题
        public void onReceivedTitle(WebView view, String title) {
            super.onReceivedTitle(view, title);
            setTitle(title);
        }
    }

    //是否能够回退，如果可以返回上一级
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (keyCode == KeyEvent.KEYCODE_BACK && webView.canGoBack()){
            webView.goBack();
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }
}
```



在java代码中还可以实现js代码

```java
webView.loadUrl("javascript:alert('hello')");
//或者
webView.evaluateJavascript("javascript:alert('hello')");
```

