# 一、RecyclerView实现列表功能

功能：灵活实现大数据集展示，能够显示列表、网格、瀑布流等形式，且不同的ViewHolder能够实现item多元化的功能。

**实质是类似在一个RecyclerView里面可以放多个textview**

```groovy
//引入依赖
implementation 'com.android.support:design:25.3.1'
//或者
implementation 'com.android.support:recyclerview-v7:29+'
```

xml文件添加视图

```xml
<androidx.recyclerview.widget.RecyclerView
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/rv1">

   </androidx.recyclerview.widget.RecyclerView>
```

在对应的activity找到组件并添加布局管理器

```java
  private RecyclerView recyclerView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recyview);
        recyclerView = findViewById(R.id.rv1);
        recyclerView.setLayoutManager(new LinearLayoutManager(recyviewActivity.this));//声明布局管理器，默认垂直


    }
```







创建Adapter

关于adapter：①https://www.jianshu.com/p/ba6db19b9353

**==（recyclerview是用于展示大量数据的，但数据无法直接传给recyclerview用的，adapter适配器就是为此而存在的==** ）

新建布局文件：

![1607429590486](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607429590486.png)

```java
//书写RecyclerView自定义adapter
//仿照adapter书写adapter类
public class LinearAdapter extends RecyclerView.Adapter<LinearAdapter.LinearViewHolder> {

    /**/
    private Context context;
    public LinearAdapter(Context context){
        this.context = context;
    }
    @NonNull
    @Override
    //用于加载 RecyclerView 子项的布局，然后返回一个 ViewHolder 对象，ViewHolder 是一个静态内部类，继承自 RecyclerView.ViewHolder 类。
    public LinearAdapter.LinearViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        return new LinearViewHolder(LayoutInflater.from(context).inflate(R.layout.linear_item,parent,false));
    }

    @Override
    //为子项绑定数据。调用这两个方法后，子项就既有了布局又有了数据。
    public void onBindViewHolder(@NonNull LinearAdapter.LinearViewHolder holder, int position) {
        holder.textView.setText("Hello World!");//为recyclerView的item添加数据
    }

    @Override
    //用于获取 RecyclerView 一共有多少子项.
    public int getItemCount() {
        return 30;
    }

    class LinearViewHolder extends RecyclerView.ViewHolder {

        private TextView textView;//
        public LinearViewHolder(@NonNull View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.textview);//
        }
    }
}
```



在RecyclerView的activity中调用adapter

```java
 private RecyclerView recyclerView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recyview);
        recyclerView = findViewById(R.id.rv1);
        recyclerView.setLayoutManager(new LinearLayoutManager(recyviewActivity.this));//声明布局管理器
        recyclerView.setAdapter(new LinearAdapter(recyviewActivity.this));//调用adapter

    }
```



运行结果图：

![image-20210815233158064](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210815233158064.png)

还可以加分割线以及点击事件：

# 二、加载多个ViewHolder

1.git提供一个库可以加入插件供RecyclerView使用

![1607497609775](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607497609775.png)

2.在之前linearRcyclerView基础上添加多个viewholder

首先建立多个item

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingLeft="1dp"
    android:paddingRight="1dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textview"
        android:background="#fff"
        android:gravity="center"
        android:text="梦比优斯"
        android:textSize="30dp"/>

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="328dp"
        android:src="@drawable/ultraman"
        android:scaleType="centerCrop"
        android:id="@+id/img_view3"/>
</LinearLayout>
```



```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingLeft="1dp"
    android:paddingRight="1dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:id="@+id/textview"
        android:background="#fff"
        android:gravity="center"/>
</LinearLayout>
```





3.更新adapter

```java
//由于创建多个viewHolder，所以adapter里面都使用RecyclerView自带的ViewHolder
public class LinearAdapter extends RecyclerView.Adapter<RecyclerView.ViewHolder > {

    /**/
    private Context context;
    public LinearAdapter(Context context){
        this.context = context;
    }
    @NonNull
    @Override
    public RecyclerView.ViewHolder  onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        if(viewType == 1){
            return new LinearViewHolder(LayoutInflater.from(context).inflate(R.layout.linear_item,parent,false));//
        }else {
            return new LinearViewHolder2(LayoutInflater.from(context).inflate(R.layout.linear_item_2,parent,false));
        }

    }

    @Override
    public void onBindViewHolder(@NonNull RecyclerView.ViewHolder  holder, int position) {
        if(getItemViewType(position) == 1){
            ((LinearViewHolder)holder).textView.setText("Hello World!");//要强制转换类型
        }//由于viewholder2我已经设置了他的内容，所以不需要添加文字或者是图片路径

    }

    //重写此方法，通过position获得他的viewtype
    @Override
    public int getItemViewType(int position) {
        if (position % 2 != 0) {
            return 1;
        }else {
            return 0;
        }
    }

    @Override
    public int getItemCount() {
        return 30;//
    }

    class LinearViewHolder extends RecyclerView.ViewHolder {

        private TextView textView;//
        public LinearViewHolder(@NonNull View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.textview);//
        }
    }
    class LinearViewHolder2 extends RecyclerView.ViewHolder {

        private TextView textView;//
        private ImageView imageView;
        public LinearViewHolder2(@NonNull View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.textview);//找到文字
            imageView = itemView.findViewById(R.id.img_view3);//找到图片
        }
    }
}

```

结果图：

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607499814516.png" alt="1607499814516" style="zoom:67%;" />

# 三、瀑布流形式展示

1.创建RecyclerView

```xml
 <androidx.recyclerview.widget.RecyclerView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/rv_pu">

    </androidx.recyclerview.widget.RecyclerView>
```

2.创建recyclerview的子控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingLeft="1dp"
    android:paddingRight="1dp">

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/iv"
        android:scaleType="centerCrop"/>
</LinearLayout>
```

3.RecyclerView的activity

```java
public class RVpubuActivity extends AppCompatActivity {

    private RecyclerView recyclerView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_rvpubu);
        recyclerView = findViewById(R.id.rv_pu);
        recyclerView.setLayoutManager(new StaggeredGridLayoutManager(3,StaggeredGridLayoutManager.VERTICAL));//有多少列，排列方式
        recyclerView.addItemDecoration(new MyDecoration());
        recyclerView.setAdapter(new StaggerAdapter(RVpubuActivity.this));

    }
    //改变item之间的间距
    class MyDecoration extends RecyclerView.ItemDecoration{
        @Override
        public void getItemOffsets(@NonNull Rect outRect, @NonNull View view, @NonNull RecyclerView parent, @NonNull RecyclerView.State state) {
            super.getItemOffsets(outRect, view, parent, state);
            int gap = getResources().getDimensionPixelSize(R.dimen.dividerHeight2);//获取间距
            outRect.set(gap,gap,gap,gap);
        }
    }
}

```

4.adapter

```java
package com.example.test.RecyclerView;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import com.example.test.R;

public class StaggerAdapter extends RecyclerView.Adapter<StaggerAdapter.LinearViewHolder>{
    /**/
    private Context context;
    public StaggerAdapter(Context context){
        this.context = context;
    }
    @NonNull
    @Override
    public StaggerAdapter.LinearViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        return new StaggerAdapter.LinearViewHolder(LayoutInflater.from(context).inflate(R.layout.stagger_rv_item,parent,false));//
    }

    @Override
    public void onBindViewHolder(@NonNull StaggerAdapter.LinearViewHolder holder, int position) {
       if(position %2 != 0) {
            holder.imageView.setImageResource(R.drawable.ultraman);//为图片添加路径
        }else {
            holder.imageView.setImageResource(R.drawable.jack);
        }
    }

    @Override
    public int getItemCount() {
        return 30;//子项目个数
    }

    class LinearViewHolder extends RecyclerView.ViewHolder {

        private ImageView imageView;
        public LinearViewHolder(@NonNull View itemView) {
            super(itemView);
            imageView = itemView.findViewById(R.id.iv);//找到对应的控件
        }
    }
}

```

效果:

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607489085276.png" alt="1607489085276" style="zoom:67%;" />

# 四、水平布局和网格布局

## 实现水平视图

1.创建recyclerView

```xml
<androidx.recyclerview.widget.RecyclerView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/rv_hor"
        >
</androidx.recyclerview.widget.RecyclerView>
```

2.声明，找到控件,设置布局管理器

```java
    private RecyclerView recyclerView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_hor_rv);

        recyclerView = findViewById(R.id.rv_hor);
        LinearLayoutManager linearLayoutManager = new LinearLayoutManager(HorRVActivity.this);
        linearLayoutManager.setOrientation(RecyclerView.HORIZONTAL);//设置方向为水平
        recyclerView.setLayoutManager(linearLayoutManager);
        recyclerView.setAdapter(new LinearAdapter(HorRVActivity.this));
    }

```

3.创建布局文件，书写recyclerview的子控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:paddingLeft="1dp"
    android:paddingRight="1dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="100dp"
        android:id="@+id/textview"
        android:background="#fff"/><!--设置颜色是因为后面设置分割线的时候能够清晰显示出来-->
</LinearLayout>
```



4.adapter，与第一节的adapter相同，直接沿用

```java
/仿照adapter书写adapter类
public class LinearAdapter extends RecyclerView.Adapter<LinearAdapter.LinearViewHolder> {

    /**/
    private Context context;
    public LinearAdapter(Context context){
        this.context = context;
    }
    @NonNull
    @Override
    public LinearAdapter.LinearViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        return new LinearViewHolder(LayoutInflater.from(context).inflate(R.layout.linear_item,parent,false));//
    }

    @Override
    public void onBindViewHolder(@NonNull LinearAdapter.LinearViewHolder holder, int position) {
        holder.textView.setText("Hello World!");//
    }

    @Override
    public int getItemCount() {
        return 30;//
    }

    class LinearViewHolder extends RecyclerView.ViewHolder {

        private TextView textView;//
        public LinearViewHolder(@NonNull View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.textview);//
        }
    }
}
```



4,设置分隔线

在values下创建配置文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen name="dividerHeight">1dp</dimen>
</resources>
```



```java
 //调用RecyclerView的ItemDecoration自定义分隔线样式
class MyDecoration extends RecyclerView.ItemDecoration{
        @Override
        public void getItemOffsets(@NonNull Rect outRect, @NonNull View view, @NonNull RecyclerView parent, @NonNull RecyclerView.State state) {
            super.getItemOffsets(outRect, view, parent, state);
            outRect.set(0,0,getResources().getDimensionPixelOffset(R.dimen.dividerHeight),0);//四个三处分别代表左，上，右，下
            //if(parent.getChildAdapterPosition(view)!=0){
               //outRect.right = 20;
            //}
        }
    }
```



activity完整代码:

```java
public class HorRVActivity extends AppCompatActivity {

    private RecyclerView recyclerView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_hor_rv);

        recyclerView = findViewById(R.id.rv_hor);
        LinearLayoutManager linearLayoutManager = new LinearLayoutManager(HorRVActivity.this);
        linearLayoutManager.setOrientation(RecyclerView.HORIZONTAL);//设置方向为水平
        recyclerView.setLayoutManager(linearLayoutManager);
        recyclerView.addItemDecoration(new MyDecoration());
        recyclerView.setAdapter(new LinearAdapter(HorRVActivity.this));
    }

    class MyDecoration extends RecyclerView.ItemDecoration{
        @Override
        public void getItemOffsets(@NonNull Rect outRect, @NonNull View view, @NonNull RecyclerView parent, @NonNull RecyclerView.State state) {
            super.getItemOffsets(outRect, view, parent, state);
            outRect.set(0,0,getResources().getDimensionPixelOffset(R.dimen.dividerHeight),0);
            //if(parent.getChildAdapterPosition(view)!=0){
               //outRect.right = 20;
            //}
        }
    }
}
```

结果图

![1607481954582](https://gitee.com/kevinyong/kevin-gallery/raw/master/1607481954582.png)

## 实现网格视图

1.创建recyclerView布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".RecyclerView.GridRVActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/rv_grid">

    </androidx.recyclerview.widget.RecyclerView>
</LinearLayout>
```

recyclerview子控件的布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingLeft="1dp"
    android:paddingRight="1dp">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:id="@+id/textview"
        android:background="#fff"
        android:gravity="center"/> <!--使内容居中-->
</LinearLayout>
```



2.activity文件，主要是设置layout为grid

```java
public class GridRVActivity extends AppCompatActivity {

    private RecyclerView recyclerView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_grid_rv);
        recyclerView = findViewById(R.id.rv_grid);
        recyclerView.setLayoutManager(new GridLayoutManager(GridRVActivity.this,3));//一行展示三个
        recyclerView.setAdapter(new LinearAdapter(GridRVActivity.this));//沿用之前的adapter
    }
}
```

效果：

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/1607483288733.png" alt="1607483288733" style="zoom:67%;" />

# 五、RecyclerView中ViewHolder的机制以及复用

### 	1.什么是复用

<img src="https://gitee.com/kevinyong/kevin-gallery/raw/master/20160701114250664" alt="img" style="zoom:67%;" />

上图比较清晰的画出了viewholder工作原理

可以看到，图中左上角item1上面有一条蓝色的线，item7下面也有一条蓝色的线，这两条线就是屏幕的上下边缘，我们在屏幕中能看到的内容就是item1~item7。

当我们控制屏幕向下滚动时，屏幕上的变化是，item1离开了屏幕，紧接着item8进入了屏幕，这是我们看到的。在item1离开，item8进入的过程中，还有一个我们看不到的过程。当item1离开屏幕时，它会进入Recycler(反复循环器)构件，然后被放到了item8的位置，成为了我们看到的item8。


### 2.viewholder中view错乱

#### 	①图片错乱：

由于复用的原因，时常会导致item中view错乱，例如以下代码：

```java 
public class MyRecyclerAdapter extends RecyclerView.Adapter<MyRecyclerAdapter.MyViewHolder> {
    private static final String TAG = "MyRecyclerAdapter";

    private List<String> mData;
    private Context mContext;
    private LayoutInflater inflater;

    public MyRecyclerAdapter(Context context, List<String> data) {
        this.mContext = context;
        this.mData = data;
        inflater = LayoutInflater.from(mContext);
    }

    @Override
    public int getItemCount() {
        return mData.size();
    }

    @Override
    public void onViewRecycled(MyViewHolder holder) {
        super.onViewRecycled(holder);
        Log.d(TAG, "onViewRecycled: "+holder.imageView.getTag().toString()+", position: "+holder.getAdapterPosition());
    }

    @Override
    public void onBindViewHolder(final MyViewHolder holder, final int position) {
        Log.d(TAG, "onBindViewHolder: 验证是否重用了");
        Log.d(TAG, "onBindViewHolder: 重用了"+holder.imageView.getTag());
        Log.d(TAG, "onBindViewHolder: 放到了"+mData.get(position));
        holder.imageView.setTag(mData.get(position));
        //异步加载一张网络图片
        new AsyncTask<Void, Void, Bitmap>() {
            @Override
            protected Bitmap doInBackground(Void... params) {
                try {
                    URL url = new URL(mData.get(position));
                    Bitmap bitmap = BitmapFactory.decodeStream(url.openStream());
                    return bitmap;
                } catch (MalformedURLException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return null;
            }

            @Override
            protected void onPostExecute(Bitmap bitmap) {
                super.onPostExecute(bitmap);
                holder.imageView.setImageBitmap(bitmap);
            }
        }.execute();
    }

    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        Log.d(TAG, "onCreateViewHolder");
        View view = inflater.inflate(R.layout.item_layout, parent, false);
        return new MyViewHolder(view);
    }

    static class MyViewHolder extends RecyclerView.ViewHolder {

        ImageView imageView;

        public MyViewHolder(View view) {
            super(view);
            imageView = (ImageView) view.findViewById(R.id.id_img);
        }

    }
}
```



**场景A：**

1.第一次运行，RecyclerView载入，不做任何触摸操作
2.Adapter经过onCreateViewHolder()创建了上面我们能看到的8个ViewHolder对象，并且在onBind时启动了8条线程加载图片
3.8张图片全部加载完毕，并且显示到对应的ImageView上
4.控制屏幕向下滚动，第1、第2个item离开屏幕可视区域，第9、第10个item进入屏幕可视区域
5.第1、第2个item被回收，重用到第9、第10个item。第9、第10个item显示的图片是第1和第2个item的图片！！！
6.开启了两条线程，加载第9、第10张图片。等待几秒，第9、第10个item显示的图片突然变成了正确的图片！

以上过程是场景A，经过拆分细化，非常容易看出问题所在。如果当前网络速度很快，第6个步骤的加载速度在1秒甚至0.5秒内，就会造成人眼看到的图片闪烁问题出现，第9、第10个item的图片闪了一下变成了正确的图片。

-------------------------------------------------------------------------------------------------------

**场景B:**

1.第一次运行，RecyclerView载入
2.Adapter经过onCreateViewHolder()创建了上面我们能看到的8个ViewHolder对象，并且在onBind时启动了8条线程加载图片
3.7张图片加载完毕，还有1张未加载完(已知图片一加载速度异常慢)
4.控制屏幕向下滚动，第1、第2个item离开屏幕可视区域，第9、第10个item进入屏幕可视区域
5.第1、第2个item被回收，重用到第9、第10个item。闪烁问题不再重复说，第9、第10张图片加载完毕(看上去一切正常)
6.等待几秒，第一张图片终于加载完成，第9个item突然从正确的图片九变成不正确的图片一 ！！！

以上过程是场景B，问题出现在加载第一张图片的线程T，持有了item1的ImageView对象引用，而这张图片加载速度非常慢，直到item1已经被重用到item9后，过了一段时间，线程T才把图片一加载出来，并设置到item1的ImageView上，然而线程T并不知道item1已经不存在且变成了item9，于是，图片发生错乱了。

---------------------------------------------------------------------------------------------------------

**场景C：**

1.第一次运行，RecyclerView载入
2.Adapter经过onCreateViewHolder()创建了上面我们能看到的8个ViewHolder对象，并且在onBind时启动了8条线程加载图片
3.忽略图片加载情况，直接向下滚动，再向上滚动，再向下滚动，来回操作
4.由于离开了屏幕的item是随机被回收并重用的，所以向下滚动时我们假设item1、item3被回收重用到item9、item10，item2、item4被回收重用到item11、item12
5.向上滚动时，item9、item12被回收重用到item1、item2，item10、item11被回收重用到item3、item4
6.多次上下滚动后，停下，最后发现某一个item的图片在不停变化，最后还不一定是正确的图片

以上过程是场景C，问题出现在**ViewHolder的回收重用顺序是随机**的，**回收时会从离开屏幕范围的item中随机回收，并分配给新的item**，来回操作数次，就会造成有多条加载不同图片的线程，持有同一个item的ImageView对象，造成最后在同一个item上图片变来变去，错乱更加严重。

---------------------------------------------------------------------------------------------------------



**解决办法：**

解决方法其实有很多种，这里列出两种情况:

- 当item还在加载图片的过程中，被移出屏幕可视范围，不需要继续加载这张图片了，可以在onRecycled中取消图片的加载。这样就不会造成图片加载完成设置到其他item的ImageView中了。
- 每一个经过屏幕可视区域的item，加载的图片都要放进缓存中，即使item离开了可视区域，也要加载完毕并放入缓存中，方便下次浏览时能快速加载。每次onBind时对ImageView设置Tag标记，如果Tag标记已经被更改，旧线程加载好的图片不再设置到ImageView中。
  

**当然以上两种情况都别忘了先设置图片占位符，防止回收item的图片直接显示到新item中。**

```java
//解决代码1
public class MyRecyclerAdapter extends RecyclerView.Adapter<MyRecyclerAdapter.MyViewHolder> {
    private static final String TAG = "MyRecyclerAdapter";

    private List<String> mData;
    private Context mContext;
    private LayoutInflater inflater;

    public MyRecyclerAdapter(Context context, List<String> data) {
        this.mContext = context;
        this.mData = data;
        inflater = LayoutInflater.from(mContext);
    }

    @Override
    public int getItemCount() {
        return mData.size();
    }

    @Override
    public void onViewRecycled(MyViewHolder holder) {
        super.onViewRecycled(holder);
        AsyncTask asyncTask = (AsyncTask) holder.imageView.getTag(1);
        asyncTask.cancel(true);
    }

    @Override
    public void onBindViewHolder(final MyViewHolder holder, final int position) {
        //先设置图片占位符
        holder.imageView.setImageDrawable(mContext.getDrawable(R.mipmap.ic_launcher));
        AsyncTask asyncTask = new AsyncTask<Void, Void, Bitmap>() {
            @Override
            protected Bitmap doInBackground(Void... params) {
                try {
                    URL url = new URL(mData.get(position));
                    Bitmap bitmap = BitmapFactory.decodeStream(url.openStream());
                    return bitmap;
                } catch (MalformedURLException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return null;
            }

            @Override
            protected void onPostExecute(Bitmap bitmap) {
                super.onPostExecute(bitmap);
                holder.imageView.setImageBitmap(bitmap);
            }
        };
        holder.imageView.setTag(1,asyncTask);
        asyncTask.execute();
    }

    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = inflater.inflate(R.layout.item_layout, parent, false);
        return new MyViewHolder(view);
    }

    static class MyViewHolder extends RecyclerView.ViewHolder {

        ImageView imageView;

        public MyViewHolder(View view) {
            super(view);
            imageView = (ImageView) view.findViewById(R.id.id_img);
        }

    }
}

//解决办法2
public class MyRecyclerAdapter extends RecyclerView.Adapter<MyRecyclerAdapter.MyViewHolder> {
    private static final String TAG = "MyRecyclerAdapter";

    private List<String> mData;
    private Context mContext;
    private LayoutInflater inflater;

    public MyRecyclerAdapter(Context context, List<String> data) {
        this.mContext = context;
        this.mData = data;
        inflater = LayoutInflater.from(mContext);
    }

    @Override
    public int getItemCount() {
        return mData.size();
    }

    @Override
    public void onViewRecycled(MyViewHolder holder) {
        super.onViewRecycled(holder);
    }

    @Override
    public void onBindViewHolder(final MyViewHolder holder, final int position) {
        //先设置图片占位符
        holder.imageView.setImageDrawable(mContext.getDrawable(R.mipmap.ic_launcher));
        final String url = mData.get(position);
        //为imageView设置Tag,内容是该imageView等待加载的图片url
        holder.imageView.setTag(url);
        AsyncTask asyncTask = new AsyncTask<Void, Void, Bitmap>() {
            @Override
            protected Bitmap doInBackground(Void... params) {
                try {
                    URL url = new URL(mData.get(position));
                    Bitmap bitmap = BitmapFactory.decodeStream(url.openStream());
                    return bitmap;
                } catch (MalformedURLException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return null;
            }

            @Override
            protected void onPostExecute(Bitmap bitmap) {
                super.onPostExecute(bitmap);
                //加载完毕后判断该imageView等待的图片url是不是加载完毕的这张
                //如果是则为imageView设置图片,否则说明imageView已经被重用到其他item
                if(url.equals(holder.imageView.getTag())) {
                    holder.imageView.setImageBitmap(bitmap);
                }
            }
        }.execute();
    }

    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = inflater.inflate(R.layout.item_layout, parent, false);
        return new MyViewHolder(view);
    }

    static class MyViewHolder extends RecyclerView.ViewHolder {

        ImageView imageView;

        public MyViewHolder(View view) {
            super(view);
            imageView = (ImageView) view.findViewById(R.id.id_img);
        }

    }
}
```

#### 	②使用glide加载图片导致错乱问题：

​	原因及应用场景与上面类似，不再分析

​	**解决办法：**

​	**（1）设置占位图：**

​	有两种方式:

​		1）直接在请求中添加placeholder():

```java
Glide.with(this)
        .load(picUrl)
        .placeholder(R.drawable.ic_loading)
        .into(holder.ivThumb)
```

​		2)添加监听，在回调方法中设置

```java
Glide.with(mContext)
     .load(picUrl)
     .error(R.drawable.ic_loading)
     .into(new SimpleTarget<GlideDrawable>() {
         @Override
         public void onResourceReady(GlideDrawable glideDrawable, GlideAnimation<? super  GlideDrawable> glideAnimation) {
             //设置加载的图片
             holder.ivThumb.setImageDrawable(glideDrawable);
         }

         @Override
         public void onStart() {
             super.onStart();
             //设置占位图
             holder.ivThumb.setImageResource(R.drawable.ic_loading);
         }
     });
```

​	**(2)设置TAG**

使用setTag（）方式。但是，Glide图片加载也是使用这个方法，所以需要使用setTag（key，value）方式进行设置，这种方式是不错的一种解决方式，注意取值的时候应该是getTag（key）这个方法，当异步请求回来的时候对比下tag是否一样，再判断是否显示图片,我使用的是position设置tag.（有缓存机制，下次加载更快）

**时间及事件梳理**

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/20190414152308174.jpg)

```java
@Override
    public void onBindViewHolder(final VideoViewHolder holder, final int position) {
        holder.thumbView.setTag(R.id.tag_dynamic_list_thumb, position);
        Glide.with(mContext)
                .load(picUrl)
                .error(R.drawable.video_thumb_loading)
                .into(new SimpleTarget<GlideDrawable>() {
                    @Override
                    public void onResourceReady(GlideDrawable glideDrawable, GlideAnimation<? super GlideDrawable> glideAnimation) {
                        if (position != (Integer) holder.thumbView.getTag(R.id.tag_dynamic_list_thumb))
                            return;
                        holder.thumbView.setImageDrawable(glideDrawable);
                    }

                    @Override
                    public void onStart() {
                        super.onStart();
                        //开始设置占位图片，防止加载之前item图片加载好展示
                        holder.thumbView.setImageResource(R.drawable.ic_loading);
                    }
            });
    }
```

**(3)在onViewRecycled方法中重置item的ImageView并取消网络请求（推荐使用）**

   **在onBindViewHolder中发起加载请求，然后在view被回收时取消网络请求**

```java
@Override
public void onBindViewHolder(VideoViewHolder holder, int position) {
    String istrurl = mImgList.get(position).getImageUrl();
    if (null == holder || null == istrurl || istrurl.equals("")) {
        return;
    }
    //加载图片
    Glide.with(mContext)
            .load(picUrl)
            .placeholder(R.drawable.ic_loading)
            .into(holder.thumbView);
}

@Override
public void onViewRecycled(VideoViewHolder holder) {
    if (holder != null) {
        //取消网络请求
        Glide.clear(holder.thumbView);
        holder.thumbView.setImageResource(R.drawable.ic_loading);
    }
    super.onViewRecycled(holder);
}
```

#### ③edittext复用导致的混乱:

```java
private SparseArray<String> mTextCache = new SparseArray<>();
@Override
    public void onBindViewHolder(@NonNull QuesAdapter.QuesViewHolder holder, int position) {
        Ques que = quesList.get(position);
        holder.textView1.setText(String.valueOf(que.getNum1()));
        holder.textView2.setText(que.getLabel());
        holder.textView3.setText(String.valueOf(que.getNum2()));
        Log.d("position",String.valueOf(position));

	    //缓存里面有则取出来，没有则返回默认值“”
        holder.editText.setText(mTextCache.get(holder.getAdapterPosition(),""));
        //判断是否有textwatcher，有的话就不绑定
        if(holder.editText.getTag(R.id.tag_edittext) instanceof TextSwitcher){
            return;
        }
        TextSwitcher textSwitcher = new TextSwitcher(holder);
        holder.editText.addTextChangedListener(textSwitcher);
        holder.editText.setTag(position);

        //设置tag为
        holder.editText.setTag(R.id.tag_edittext,textSwitcher);
        
    }
//自定义EditText的监听类
    class TextSwitcher implements TextWatcher {


        private QuesViewHolder quesViewHolder;
        public TextSwitcher(QuesViewHolder quesViewHolder){
            this.quesViewHolder = quesViewHolder;
        }
        @Override
        public void beforeTextChanged(CharSequence s, int start, int count, int after) {

        }

        @Override
        public void onTextChanged(CharSequence s, int start, int before, int count) {

        }

        @Override
        public void afterTextChanged(Editable s) {

            //如果填入数据与缓存数据一致则返回
            if(TextUtils.equals(mTextCache.get(quesViewHolder.getAdapterPosition()),s.toString())){
                return;
            }
            //对数据进行缓存
            mTextCache.put(quesViewHolder.getAdapterPosition(),s.toString());
            //回调接口进行数据保存，MainActivity进行具体操作
            SaveETListener listener = (SaveETListener) context;
            if(s!=null){
                listener.SaveEdit(Integer.parseInt(quesViewHolder.editText.getTag().toString()),s.toString());
            }
        }
    }
```



**重写下面方法并且改变返回值能够解决复用,原理不清楚**

```java
//解决recyclerview 的复用问题
    @Override
    public int getItemViewType(int position) {
        return position;
    }
```



# 六、性能优化及高级使用



### 	1.recyclerview三级缓存：

​	(1)**Attached scrap** & **Changed scrap**

```java
ArrayList<ViewHolder> mAttachedScrap 
```

**主要用于插入或者删除itemView**时。先把屏幕内的ViewHolder保存至AttachedScrap中，作用在LayoutManager中：

- 移除：它仅仅把需要从ViewGroup中移除的子view设置它的父view为null，从而实现了从RecyclerView中移除操作detachView()。
- 插入：需要新插入的view从cacheView/Pool中找，没找到则createViewHolder。而从ViewGroup中移除的子view会放到Pool缓存池中,如下图中的itemView b。

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/20190326092808357.png)

```java
ArrayList<ViewHolder> mChangedScrap ：
```

**主要用到刷新屏幕上的itemView数据**，它不需要重新layout，notifyItemChanged()或者notifyItemRangeChanged()

(2) cache Views ：

**保存最近移出屏幕的ViewHolder**，包含数据和position信息，复用时必须是相同位置的ViewHolder才能复用，应用场景在那些需要来回滑动的列表中，当往回滑动时，能直接复用ViewHolder数据，不需要重新bindView。用一个数组保存ViewHolder，实现是：

```java
ArrayList<ViewHolder> mCachedViews //默认大小是2
```

(3)RecyclerViewPool :

缓存池，当cacheView满了后，将cacheView中移出的ViewHolder放到Pool中，放之前**会把ViewHolder数据清除掉**，所以复用时需要重新bindView。实现是： 

```java
SparseArray<ArrayList<ViewHolder>> mScrap;//按viewType来保存ViewHolder，每种类型最大缓存个数默认为5
```

### 2.RecyclerView缓存过程：

在滑动过程中，会将先滑动的itemView保存到CacheView中，**CacheView大小默认是2**，如果超过了最大容量，则按FIFO,将队列头部的itemView出队，保存至缓存池RecyclerViewPool中，**缓存池是按itemView的类型itemType来保存的，每种itemType默认缓存个数是5**，超过了，则直接由GC回收。具体表现如下图

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/20190326091821406.png)

可以看到CacheView缓存中蓝色的块一直最最近两个，而RecycledViewPool中，保存最大是5,超过5了后ViewHolder都被回收。

### 3.recyclerview找缓存过程：

RecyclerView在找到可用ViewHodler的顺序是：如果在缓存CacheViews中找到，则直接复用；如果在缓存池RecycerViewPool找到，则需要bindView；如果没有找到可用的ViewHolder，则需要create新建一个ViewHolder，并bindView绑定view。



### 4.调用notifyDataSetChanged过程:

如果调用notifyDataSetChanged，**每个itemView没有稳定的id的话**，RecyclerView不知道接下来会发生什么，也不知道哪些改变，它假设所有都改变了，会将每一个ViewHolder设置成无效并且放到缓存池Pool中，如果我们仅是把屏幕上的第四条itemView移到第六条的位置，屏幕上所有itemView都会重新layout一遍，这样只能从缓存池RecycledViewPool池中取缓存的ViewHolder，如果不够时，需要重新create ViewHolder.具体实现如下：
![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/2019032609260765.png)

**如果设置了Stable Ids**，即每一个itemView都有一个唯一的id来标识，通过getItemId()来获取这个唯一标识id，当然我们不能用position来标识，因为itemView会复用，位置会乱序。当调用notifyDataSetChanged()方法时，ViewHolder会进入上面的一级缓存mAttachedScrap中，而不是进入缓存池pool中，这样的好处：1）不会存在缓存池pool满的问题，不需要重新createViewHolder; 2) 不需要重新bindView了。
![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/20190326094544879.png)

**设置Stable ids方法:**

```java
mRecyclerViewAdapter.setHasStableIds(true);  
```

**要注意**，使用上述代码的话，Adapter中的getItemId要重写成如下，如果仍用super.getItemId(position)，数据刷新会出错。

```java
 @Override
    public long getItemId(int position) {
        return position;
    }
```

### 5.recyclerview性能优化:

​	(1)recyclerView.setHasFixedSize(true);

当Item的高度如是固定的，设置这个属性为true可以提高性能，尤其是当RecyclerView有条目插入、删除时性能提升更明显。RecyclerView在条目数量改变，会重新测量、布局各个item，如果设置了setHasFixedSize(true)，由于item的宽高都是固定的，adapter的内容改变时，RecyclerView不会整个布局都重绘。具体可用以下伪代码表示：

```java
void onItemsInsertedOrRemoved() {
   if (hasFixedSize) layoutChildren();
   else requestLayout();
}
```

​	

​	(2)使用getExtraLayoutSpace为LayoutManager设置更多的预留空间

在RecyclerView的元素比较高，**一屏只能显示一个元素的时候，第一次滑动到第二个元素会卡顿**。  

RecyclerView (以及其他基于adapter的view，比如ListView、GridView等)使用了缓存机制重用子 view（即系统只将屏幕可见范围之内的元素保存在内存中，在滚动的时候不断的重用这些内存中已经存在的view，而不是新建view）。

这个机制会导致一个问题，启动应用之后，在屏幕可见范围内，如果只有一张卡片可见，当滚动的时 候，RecyclerView找不到可以重用的view了，它将创建一个新的，因此在滑动到第二个feed的时候就会有一定的延时，但是第二个feed之 后的滚动是流畅的，因为这个时候RecyclerView已经有能重用的view了。

如何解决这个问题呢，其实只需重写getExtraLayoutSpace()方法。根据官方文档的描述 getExtraLayoutSpace将返回LayoutManager应该预留的额外空间（显示范围之外，应该额外缓存的空间）。

```java
LinearLayoutManager linearLayoutManager = new LinearLayoutManager(this) {
    @Override
    protected int getExtraLayoutSpace(RecyclerView.State state) {
        return 300;
    }
};
```

​	**(3)RecyclerView数据预取**

android sdk>=21时，支持渲染（Render）线程，RecyclerView数据显示分两个阶段：

1）在UI线程，处理输入事件、动画、布局、记录绘图操作，每一个条目在进入屏幕显示前都会被创建和绑定view；

2）渲染（Render）线程把指令送往GPU。

数据预取的思想就是：将闲置的UI线程利用起来，提前加载计算下一帧的Frame Buffer

在新的条目进入视野前，会花大量时间来创建和绑定view，而在前一帧却可能很快完成了这些操作，导致前一帧的UI线程有一大片空闲时间。RecyclerView开发工程师将创建和绑定移到前一帧，使UI线程与渲染线程同时工作，在一个条目即将进入视野时预取数据。具体如下图，在前一帧的红色虚线圈中，UI线程有一定的空闲时间，可以把第二帧Create B的工作移到前一帧的空闲时间来完成。
![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/6afd4f4be60f4b26f25fe282ab2b8392.png)

具体实现方式是：在 RecyclerView 开始一个滚动时new Runnable对象，根据 layout manager 和滚动的方向预取即将进入视野的条目，可以同时取出一个或多个条目，例如在使用 GridLayoutManager 时新的一行马上要出现的时候。在 25.1 版本中，预取操作被分为单独的创建/绑定操作，比对整组条目操作更容易被纳入 UI 线程的空隙中。具体实现原理可参考
![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/d66429569ae83957dc323cb47189c3dd.png)

如果使用 RecyclerView 提供的LayoutManager，自动使用了这种优化操作。如果使用嵌套 RecyclerView 或者自己实现Layout Manager，则需要在代码中设置。

1）**对于嵌套 RecyclerView**，要获取最佳的性能，在内部的 LayoutManager 中调用 LinearLayoutManager.setInitialItemPrefetchCount()方法（25.1版本起可用）。

```java
例如：如果竖直方向的list至少展示三个条目，调用 setInitialItemPrefetchCount(4)。
```

2）如果自己实现了LayoutManager，需要重写 LayoutManager.collectAdjacentPrefetchPositions()方法。该方法在数据预取开启时被 RecyclerView 调用（LayoutManager 的默认实现什么都不做）。在嵌套的内层 RecyclerView 中，如果想让LayoutManager 预取数据，同样应当实现 LayoutManager.collectInitialPrefetchPositions()。

​	**(4)避免创建过多对象**

onCreateViewHolder 和 onBindViewHolder 对时间都比较敏感，尽量避免繁琐的操作和循环创建对象。例如创建 OnClickListener，可以全局创建一个。同时onBindViewHolder调用次数会多于onCreateViewHolder的次数，如从RecyclerViewPool缓存池中取到的View都需要重新bindView，所以我们可以把监听放到CreateView中进行。

```java
//优化前
@Override
public void onBindViewHolder(ViewHolder holder, int position) {
    holder.setOnClickListener(new View.OnClickListener() {
       @Override
       public void onClick(View v) {
         //do something
       }
    });
}
```

优化后

```java
//优化后
private class XXXHolder extends RecyclerView.ViewHolder {
        private EditText mEt;
        EditHolder(View itemView) {
            super(itemView);
            mEt = (EditText) itemView;
            mEt.setOnClickListener(mOnClickListener);
        }
    }
    private View.OnClickListener mOnClickListener = new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            //do something
        }
    }
```

# 七、RecyclerView删除和添加

### 	1.添加数据

```java
 // 添加数据
   public void addData( int position) {
//   在list中添加数据，并通知条目加入一条
     list.add(position, "我是商品" + position);
     //添加动画
     notifyItemInserted(position);
   }
```

调用

```java
adapter.addData(list.size());
```

### 	2.删除数据

```java
// 删除数据
public void removeData( int position) {
   list.remove(position);
   //删除动画
   notifyItemRemoved(position);
   notifyDataSetChanged();
}
```

调用

```java
//Item里的删除
    removeData(position);
//外面的控件的删除
adapter. removeData(position);
```



# 八、SparseArray

### 	1.概述：

SparseArray存储的是键值对，以int作为key，Object作为value。Sparse有稀疏、缺少的意思。SparseArray应用场景是相对稀少的数据，一般是几百以内。

### 	2.数据结构：

采用两个一维数组，一个是存储key(int类型),一个是存value object。

```java
private int[] mKeys; // 存储key
private Object[] mValues; // 存储value对象
private int mSize; // 记录存储键值对的数量
```

SparseArray在默认构造函数中指定其默认容量大小。默认为10

初始化后`mSize = 0`，实例化mKeys和mValues。

### 	3.扩容：

SparseArray并不像HashMap一样定义有最大容量是多少，最大可以达到Integer.MAX_VALUE，可能会报oom。每次扩容时如果当前容量小于5则扩容是8，否则扩容为原容量的2倍。

```java
public static int growSize(int currentSize) {
	return currentSize <= 4 ? 8 : currentSize * 2;
}
```

### 	4.与hashmap比较

针对上面与HashMap的比较，采用SparseArray还是HashMap，建议根据如下需求选取：

- 如果对内存要求比较高，而对查询效率没什么大的要求，可以是使用SparseArray
- 数量在百级别的SparseArray比HashMap有更好的优势
- 要求key是int类型的，因为HashMap会对int自定装箱变成Integer类型
- 要求key是有序的且是升序