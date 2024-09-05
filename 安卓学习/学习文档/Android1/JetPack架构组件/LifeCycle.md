## LifeCycle

> [LifeCycle](https://github.com/jhbxyz/ArticleRecord/blob/master/articles/Jetpack/1Lifecycle%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%E5%92%8C%E5%8E%9F%E7%90%86%E5%88%86%E6%9E%90.md)

如果在生命周期函数调用大量的代码，可能会导致管理混乱，甚至出现onStop()调用结束后，onStart()还在等待回调，例如下面例子：

```java
class MyActivity extends AppCompatActivity {
    private MyLocationListener myLocationListener;

    public void onCreate(...) {
        myLocationListener = new MyLocationListener(this, location -> {
            // update UI
        });
    }

    @Override
    public void onStart() {
        super.onStart();
        Util.checkUserStatus(result -> {
            // what if this callback is invoked AFTER activity is stopped?
            //还在等待回调
            if (result) {
                myLocationListener.start();
            }
        });
    }

    @Override
    public void onStop() {
        super.onStop();
        myLocationListener.stop();
    }
}
```



### androidx.lifecycle

**Lifecycle**是一个类，用于存储有关组件（如 activity 或 fragment）的生命周期状态的信息，并允许其他对象观察此状态。

每个可能的`Lifecycle`状态都在[`Lifecycle.State`](https://developer.android.google.cn/reference/androidx/lifecycle/Lifecycle.State)枚举中表示 。

- [`INITIALIZED`]
- [`CREATED`]
- [`STARTED`]
- [`RESUMED`]
- [`DESTROYED`]

#### ①DefaultLifecycleObserver

可以实现该接口，并且实现方法来监听组件的生命周期

```java
public class MyObserver implements DefaultLifecycleObserver {
    @Override
    public void onResume(LifecycleOwner owner) {
        connect()
    }

    @Override
    public void onPause(LifecycleOwner owner) {
        disconnect()
    }
}
//实现lifeCycleOwner注册observer监听生命周期
myLifecycleOwner.getLifecycle().addObserver(new MyObserver());
```



#### ②LifecycleOwner

**继承该接口必须实现`getLifcycle()`**

实现 `DefaultLifecycleObserver`的组件可与实现 `LifecycleOwner` 的组件完美配合，因为`owner`可以提供生命周期，而观察者可以注册以观察生命周期。

**例子：**

```java
public class Section implements LifecycleOwner {

    private LifecycleRegistry lifecycle;
    private Fragment fragment;

    public Section(){
        lifecycle = new LifecycleRegistry(this);
    }

    public void onCreate(){
        lifecycle.handleLifecycleEvent(Lifecycle.Event.ON_CREATE);
    }

    public void onStart(){
        lifecycle.handleLifecycleEvent(Lifecycle.Event.ON_START);
    }

    public void onResume(){
        lifecycle.handleLifecycleEvent(Lifecycle.Event.ON_RESUME);
    }


    public void init(){
        lifecycle.addObserver(new MyLifecycleObserver());
    }

    @NonNull
    @Override
    public Lifecycle getLifecycle() {
        return lifecycle;
    }
}
```

```java
public class MyLifecycleObserver implements DefaultLifecycleObserver {

    private final String TAG = this.getClass().getSimpleName();

    @Override
    public void onCreate(@NonNull LifecycleOwner owner) {
        DefaultLifecycleObserver.super.onCreate(owner);
        Log.d(TAG, "onCreate: ");
    }

    @Override
    public void onDestroy(@NonNull LifecycleOwner owner) {
        DefaultLifecycleObserver.super.onDestroy(owner);
        Log.d(TAG, "onDestroy: ");
    }

    @Override
    public void onPause(@NonNull LifecycleOwner owner) {
        DefaultLifecycleObserver.super.onPause(owner);
        Log.d(TAG, "onPause: ");
    }

    @Override
    public void onResume(@NonNull LifecycleOwner owner) {
        DefaultLifecycleObserver.super.onResume(owner);
        Log.d(TAG, "onResume: ");
    }

    @Override
    public void onStart(@NonNull LifecycleOwner owner) {
        DefaultLifecycleObserver.super.onStart(owner);
        Log.d(TAG, "onStart: ");
    }

    @Override
    public void onStop(@NonNull LifecycleOwner owner) {
        DefaultLifecycleObserver.super.onStop(owner);
        Log.d(TAG, "onStop: ");
    }
}
```



目前 26.1.0 及更高版本中的 Fragment 和 Activity 已实现 `LifecycleOwner`接口。如果自定义类并使其成为LifecycleOwner，可以使用如下:

```java
public class MyActivity extends Activity implements LifecycleOwner {
    private LifecycleRegistry lifecycleRegistry;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        lifecycleRegistry = new LifecycleRegistry(this);
        lifecycleRegistry.markState(Lifecycle.State.CREATED);
    }

    @Override
    public void onStart() {
        super.onStart();
        lifecycleRegistry.markState(Lifecycle.State.STARTED);
    }

    @NonNull
    @Override
    public Lifecycle getLifecycle() {
        return lifecycleRegistry;
    }
}
```

