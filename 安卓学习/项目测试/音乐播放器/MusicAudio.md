## 1.将图片等静态资源放进布局文件中

![1610433497789](D:\typora\图片\1610433497789.png)

## 完成主界面的界面书写

```xml
<!--整体布局采用相对布局-->
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:background="@mipmap/Jupiter">

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="70dp"
        android:layout_alignParentBottom="true"
        android:id="@+id/local_music_bottomlayout"
        android:background="#88eeeeee"
        android:padding="0dp">

        <ImageView
            android:layout_width="match_parent"
            android:layout_height="0.5dp"
            android:background="#9933FA"
            />

        <ImageView
            android:layout_width="45dp"
            android:layout_height="48dp"
            android:layout_marginLeft="30dp"
            android:src="@mipmap/前进"
            android:layout_marginTop="10dp"
            android:id="@+id/play_beyond"
            />
        <ImageView
            android:layout_width="45dp"
            android:layout_height="48dp"
            android:layout_marginLeft="100dp"
            android:src="@mipmap/停止播放"
            android:layout_marginTop="12dp"
            android:id="@+id/play_stop"/>

        <ImageView
            android:layout_width="45dp"
            android:layout_height="48dp"
            android:layout_toRightOf="@+id/play_stop"
            android:src="@mipmap/后退"
            android:layout_marginTop="11dp"
            android:layout_marginLeft="28dp"
            android:id="@+id/play_back"/>

        <TextView
            android:id="@+id/song_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentRight="true"
            android:layout_alignParentBottom="true"
            android:layout_marginTop="12dp"
            android:layout_marginRight="54dp"
            android:layout_marginBottom="31dp"
            android:text="告白气球"
            android:textSize="20sp" />

        <TextView
            android:id="@+id/singer_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentRight="true"

            android:layout_marginRight="80dp"
            android:layout_marginTop="40dp"
            android:layout_alignLeft="@+id/song_name"
            android:layout_marginLeft="1dp"
            android:text="周杰伦"
            />
    </RelativeLayout>

</RelativeLayout>
```

以上代码完成了背景和最下边音乐播放的控制

![1610436816036](D:\typora\图片\1610436816036.png)



## 然后使用recyclerview完成播放列表

```xml
 <!--主界面添加recyclerview-->
 <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/local_music_rv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@+id/local_music_bottomlayout">

    </androidx.recyclerview.widget.RecyclerView>
```



创建一个recyclerview的item的布局文件

![1610441277210](D:\typora\图片\1610441277210.png)



```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.cardview.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="10dp"
    android:layout_marginRight="10dp"
    app:contentPadding="10dp"
    app:cardCornerRadius="10dp"
    app:cardElevation="1dp"
    app:cardBackgroundColor="@color/colorGrey">

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:id="@+id/item_music_num"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="1"
            android:layout_centerVertical="true"
            android:textSize="24sp"
            android:textStyle="bold"/>

        <TextView
            android:id="@+id/local_local_song"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="玫瑰花葬礼"
            android:textSize="18sp"
            android:textStyle="bold"
            android:layout_toRightOf="@+id/item_music_num"
            android:singleLine="true"
            android:layout_marginLeft="20dp"
            />

        <TextView
            android:id="@+id/item_singer_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="许嵩"
            android:layout_below="@+id/local_local_song"
            android:layout_alignLeft="@+id/local_local_song"
            android:layout_marginTop="10dp"
            android:textSize="14sp"
            android:textColor="#888"/>

        <TextView
            android:id="@+id/item_segment"
            android:layout_width="2dp"
            android:layout_height="18dp"
            android:background="#888"
            android:layout_toRightOf="@+id/item_singer_name"
            android:layout_marginLeft="10dp"
            android:layout_marginRight="10dp"
            android:layout_alignTop="@+id/item_singer_name"/>

        <TextView
            android:id="@+id/item_song_albun"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="专辑名称"
            android:layout_toRightOf="@+id/item_segment"
            android:layout_alignTop="@+id/item_singer_name"
            android:textSize="14sp"
            android:textColor="#888"
            android:ellipsize="end"
            android:singleLine="true"
            />

        <TextView
            android:id="@+id/item_music_durtion"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_below="@id/item_singer_name"
            android:layout_alignParentRight="true"
            android:text="04:30"
            android:textSize="14sp"
            android:textColor="#888"/>

    </RelativeLayout>
</androidx.cardview.widget.CardView>
```

<img src="D:\typora\图片\1610441368327.png" alt="1610441368327" style="zoom:67%;" />



## 在manifest中声明读取sd卡(内存)的权限

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```



## 在MainActivity完成控件声明等基本操作

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    ImageView nextSong,lastSong,playButton;
    TextView singerTv,songTv;
    RecyclerView musicRv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initView();
    }

    private void initView() {
        /*初始化控件*/
        nextSong = findViewById(R.id.play_back);//下一首
        lastSong = findViewById(R.id.play_beyond);//上一首
        playButton = findViewById(R.id.play_stop);//播放按钮
        singerTv = findViewById(R.id.singer_name);//歌手名称
        songTv = findViewById(R.id.song_name);//歌曲名称
        musicRv = findViewById(R.id.local_music_rv);//recyclerview


        nextSong.setOnClickListener(this);
        lastSong.setOnClickListener(this);
        playButton.setOnClickListener(this);

    }

    @Override
    public void onClick(View view) {

        switch (view.getId()){
            case R.id.play_back:

                break;

            case R.id.play_beyond:

                break;

            case R.id.play_stop:

                break;
        }
    }
}
```



## 对RecyclerView的item所需的控件进行注入，作为数据源

```java
//创建一个javabean类
public class LocalMusicBean {
    private String id;//歌曲编号
    private String song;//歌曲名称
    private String singer;//歌手名称
    private String album;//专辑名称
    private String duration;//歌曲时长
    private String path;//歌曲路径

    //空构造函数
    public LocalMusicBean() {
    }
	//全参构造函数
    public LocalMusicBean(String id, String song, String singer, String album, String duration, String path) {
        this.id = id;
        this.song = song;
        this.singer = singer;
        this.album = album;
        this.duration = duration;
        this.path = path;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getSong() {
        return song;
    }

    public void setSong(String song) {
        this.song = song;
    }

    public String getSinger() {
        return singer;
    }

    public void setSinger(String singer) {
        this.singer = singer;
    }

    public String getAlbum() {
        return album;
    }

    public void setAlbum(String album) {
        this.album = album;
    }

    public String getDuration() {
        return duration;
    }

    public void setDuration(String duration) {
        this.duration = duration;
    }

    public String getPath() {
        return path;
    }

    public void setPath(String path) {
        this.path = path;
    }
}

```



在Main函数声明List

```java
    //数据源
    List<LocalMusicBean> mDatas;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
       .......
       mDatas = new ArrayList<>();
    }
```



## 书写Adapter

```java
//继承自RecyclerView.Adapter
public class LocalMusicAdapter extends RecyclerView.Adapter<LocalMusicAdapter.LocalMusicViewHolder> {

    //用来获取recyclerview和数据
    Context context;
    List<LocalMusicBean> mdatas;

    public LocalMusicAdapter(Context context, List<LocalMusicBean> mdatas) {
        this.context = context;
        this.mdatas = mdatas;
    }

    @NonNull
    @Override
    public LocalMusicViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(context).inflate(R.layout.item_musiclist,parent,false);//将item布局转换为view对象
        LocalMusicViewHolder holder = new LocalMusicViewHolder(view);
        return holder;//将view对象传给viewholder
    }

    //绑定viewholder，并将每一个控件内容进行赋值展示
    @Override
    public void onBindViewHolder(@NonNull LocalMusicViewHolder holder, int position) {

        LocalMusicBean musicBean = mdatas.get(position);//通过点击的recyclerview的对应item找到位置，并且获取data中对应的那一条数据
        //将item中的内容设置为获取到的集合的数据内容
        holder.idTv.setText(musicBean.getId());
        holder.songTv.setText(musicBean.getSong());
        holder.singerTv.setText(musicBean.getSinger());
        holder.albumTv.setText(musicBean.getAlbum());
        holder.durationTv.setText(musicBean.getDuration());

    }

    @Override
    public int getItemCount() {
        return mdatas.size();//返回集合长度，确定生成item的个数
    }

    //通过内部类继承viewholder来获取控件
    class LocalMusicViewHolder extends RecyclerView.ViewHolder{

        //声明并且获取控件
        TextView songTv,singerTv,idTv,albumTv,durationTv;
        public LocalMusicViewHolder(@NonNull View itemView) {
            super(itemView);

            songTv = itemView.findViewById(R.id.local_local_song);
            singerTv = itemView.findViewById(R.id.item_singer_name);
            idTv = itemView.findViewById(R.id.item_music_num);
            albumTv = itemView.findViewById(R.id.item_song_albun);
            durationTv = itemView.findViewById(R.id.item_music_durtion);
        }
    }
}
```



## 在mainActivity中书写加载本地存储的音乐文件的方法

```java
/**
     * //加载本地存储的音乐文件到集合中
     */
  private void loadLocalMusicDatas() {

      //1.获取contentResolver对象，由contentProvider提供数据
      ContentResolver resolver = getContentResolver();

      //2.获取本地音乐存储的Uri地址
      Uri uri = MediaStore.Audio.Media.EXTERNAL_CONTENT_URI;//audio是音频，radio是视频，image是图片

      //3.查询地址
      Cursor cursor = resolver.query(uri,null,null,null,null);

      //4.遍历cursor
      //判断是否能移动到下一个
      int id = 0;//用来实现id自增
      while (cursor.moveToNext()) {
          String song = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.TITLE));//获取歌曲名
          String singer = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.ARTIST));//歌手
          id++;
          String  sid = String.valueOf(id);
          String album = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.ALBUM));//专辑
          String path = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.DATA));//路径
          long duration = cursor.getLong(cursor.getColumnIndex(MediaStore.Audio.Media.DURATION));//时长
          SimpleDateFormat sdf = new SimpleDateFormat("mm:ss");//由于歌曲时长是长整形的毫秒数，所以进行格式转换
          String time = sdf.format(new Date(duration));

          //将数据存进集合
          LocalMusicBean localMusicBean = new LocalMusicBean(sid,song,singer,album,time,path);
          mDatas.add(localMusicBean);
      }
      //数据源变化，提示适配器更新
      localMusicAdapter.notifyDataSetChanged();
    }
```



## 动态申请读取存储权限

```java
 //模板
 //动态的获取权限
        if(ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.READ_EXTERNAL_STORAGE)!= PackageManager.PERMISSION_GRANTED){
            ActivityCompat.requestPermissions(MainActivity.this,new  String[]{Manifest.permission.READ_EXTERNAL_STORAGE},1);
            
        }else {
            
        }
```



```java
 //动态的获取权限
        if(ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.READ_EXTERNAL_STORAGE)!= PackageManager.PERMISSION_GRANTED){
           ActivityCompat.requestPermissions(MainActivity.this,new  String[]{Manifest.permission.READ_EXTERNAL_STORAGE},1);
            initView();
            mDatas = new ArrayList<>();
            //加载本地数据源
            loadLocalMusicDatas();
            localMusicAdapter = new LocalMusicAdapter(MainActivity.this, mDatas);//定义适配器实例
            musicRv.setAdapter(localMusicAdapter);//将适配器和recyclerview进行绑定

            //设置布局管理器
            musicRv.setLayoutManager(new LinearLayoutManager(MainActivity.this));
        }else {
            initView();
            mDatas = new ArrayList<>();

            localMusicAdapter = new LocalMusicAdapter(this, mDatas);//定义适配器实例
            musicRv.setAdapter(localMusicAdapter);//将适配器和recyclerview进行绑定

            //设置布局管理器
            musicRv.setLayoutManager(new LinearLayoutManager(this));

            //加载本地数据源
            loadLocalMusicDatas();
        }
```





## MainActivity里面播放音乐方法

```java
	/*播放音乐*/
    private void playMusic() {
        Log.i("play1","play");

        if (mediaPlayer!=null) {
            try {
                Log.i("play2","play2");
                mediaPlayer.prepare();//预备
                mediaPlayer.start();//播放
            } catch (IOException e) {
                e.printStackTrace();
            }
            Log.i("play3","play3");
            playButton.setImageResource(R.mipmap.stopplay);
        }
    }
```



## 暂停音乐方法

```java
 	/*停止音乐*/
    private void stopMusic() {

        if (mediaPlayer!=null) {
            mediaPlayer.pause();//暂停
            mediaPlayer.seekTo(0);//返回到最开始
            mediaPlayer.stop();//停止
            playButton.setImageResource(R.mipmap.play);//重置播放按钮图片
        }
    }
```





## 通过在Adapter里面的回调接口实现item点击事件

```java
 	//itemClick的setter方法
    public void setOnItemClickListener(OnItemClickListener onItemClickListener) {
        this.onItemClickListener = onItemClickListener;
    }

	//由于RecyclerView没有封装item的点击事件，自定义接口OnItemClickListener
    public interface OnItemClickListener{
        public void OnItemClick(View view,int position);//在MainActivity重写方法
    }
```



## 在onBindView中声明onclick方法

```java
  	//绑定viewholder，并将每一个控件内容进行赋值展示
    @Override
    public void onBindViewHolder(@NonNull LocalMusicViewHolder holder, final int position) {

      ......

        //通过以下方法为每一个item设置绑定点击事件
        holder.itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                onItemClickListener.OnItemClick(view,position);
            }
        });

    }
```

## 在MainActivity中实现该接口

```java
private void setEventListener() {
        localMusicAdapter.setOnItemClickListener(new LocalMusicAdapter.OnItemClickListener() {
            @Override
            public void OnItemClick(View view, int position) {
                currentPlayPosition = position;
                LocalMusicBean musicBean = mDatas.get(position);
                //设置底部显示的歌手姓名和歌曲名称
                songTv.setText(musicBean.getSong());
                singerTv.setText(musicBean.getSinger());
                stopMusic();

                mediaPlayer.reset();
                try {
                    mediaPlayer.setDataSource(musicBean.getPath());//获取路径
                    playMusic();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        });
    }
```



## 剩下的就是对前进、后退、暂停按钮的点击事件，不再赘述

对以及书写的代码进行封装

![1611312305041](D:\typora\图片\1611312305041.png)



## MainActivity完整代码

```java
package com.yht.musicaudio;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import android.Manifest;
import android.content.ContentResolver;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.provider.MediaStore;
import android.util.Log;
import android.view.View;
import android.widget.ImageView;
import android.widget.TextView;
import android.widget.Toast;


import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;




public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    ImageView nextSong,lastSong,playButton;
    TextView singerTv,songTv;
    RecyclerView musicRv;

    //数据源
    List<LocalMusicBean> mDatas;
    LocalMusicAdapter localMusicAdapter;//声明适配器

    private int position;
    //记录当前播放音乐的位置
    int currentPlayPosition = -1;//默认-1
    //记录暂停音乐时进度条位置
    int currentPausePosition = 0;
    MediaPlayer mediaPlayer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


////        //动态的获取权限
        if(ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.READ_EXTERNAL_STORAGE)!= PackageManager.PERMISSION_GRANTED){
            ActivityCompat.requestPermissions(MainActivity.this,new  String[]{Manifest.permission.READ_EXTERNAL_STORAGE},1);

        }else {
            initView();
            mDatas = new ArrayList<>();
            mediaPlayer = new MediaPlayer();
            localMusicAdapter = new LocalMusicAdapter(this, mDatas);//定义适配器实例
            musicRv.setAdapter(localMusicAdapter);//将适配器和recyclerview进行绑定

            //设置布局管理器
            musicRv.setLayoutManager(new LinearLayoutManager(this));

            //加载本地数据源
            loadLocalMusicDatas();
        }
//        }

//        String permissions = "android.permission.SIGNAL_PERSISTENT_PROCESSES";
//
//        RxPermissions rxPermissions = new RxPermissions(MainActivity.this);
//        rxPermissions.requestEach(permissions).subscribe(new Consumer<Permission>() {
//                    @Override
//                    public void accept(Permission permission) throws Exception {
//                        if (permission.granted) {
//                            // 用户已经同意该权限
//                            //result.agree(permission);
//                            initView();
//                            mDatas = new ArrayList<>();
//
//                            localMusicAdapter = new LocalMusicAdapter(MainActivity.this,mDatas);//定义适配器实例
//                            musicRv.setAdapter(localMusicAdapter);//将适配器和recyclerview进行绑定
//
//                            //设置布局管理器
//                            musicRv.setLayoutManager(new LinearLayoutManager(MainActivity.this));
//
//                            //加载本地数据源
//                            loadLocalMusicDatas();
//                        } else if (permission.shouldShowRequestPermissionRationale) {
//                            // 用户拒绝了该权限，没有选中『不再询问』（Never ask again）,那么下次再次启动时，还会提示请求权限的对话框
//                            //result.refuse(permission);
//                        } else {
//                            // 用户拒绝了该权限，并且选中『不再询问』，提醒用户手动打开权限
//                            //result.noMoreQuestions(permission);
//                        }
//                    }
//                });
//
        //设置每一项点击事件
        setEventListener();
   }

    private void setEventListener() {
        localMusicAdapter.setOnItemClickListener(new LocalMusicAdapter.OnItemClickListener() {
            @Override
            public void OnItemClick(View view, int position) {
                currentPlayPosition = position;
                LocalMusicBean musicBean = mDatas.get(position);
                musicInPosition(musicBean);

            }
        });
    }

    /*根据传入对象播放音乐*/
    private void musicInPosition(LocalMusicBean musicBean) {
        //设置底部显示的歌手姓名和歌曲名称
        songTv.setText(musicBean.getSong());
        singerTv.setText(musicBean.getSinger());
        stopMusic();

        mediaPlayer.reset();
        try {
            mediaPlayer.setDataSource(musicBean.getPath());//获取路径
            playMusic();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 播放音乐两种形式
     * 1.暂停到播放
     * 2.从停止到播放
     */
    private void playMusic() {
        Log.i("play1","play");

        if (mediaPlayer!=null) {
            if(currentPausePosition==0){
                try {
                    Log.i("play2","play2");
                    mediaPlayer.prepare();//预备
                    mediaPlayer.start();//播放
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }else {
                //从暂停到播放
                mediaPlayer.seekTo(currentPausePosition);
                mediaPlayer.start();
            }

            Log.i("play3","play3");
            playButton.setImageResource(R.mipmap.play);
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        stopMusic();
    }

    /*停止音乐*/
    private void stopMusic() {

        if (mediaPlayer!=null) {
            currentPausePosition = 0;
            mediaPlayer.pause();//暂停
            mediaPlayer.seekTo(0);//返回到最开始
            mediaPlayer.stop();//停止
            playButton.setImageResource(R.mipmap.play);//重置播放按钮图片
        }
    }

    /*暂停音乐函数*/
    private void pauseMusic() {
        if (mediaPlayer!=null&&mediaPlayer.isPlaying()) {
            currentPausePosition = mediaPlayer.getCurrentPosition();
            mediaPlayer.pause();
            playButton.setImageResource(R.mipmap.stopplay);
        }
    }

    /**
     * //加载本地存储的音乐文件到集合中
     */
    private void loadLocalMusicDatas() {

        //1.获取contentResolver对象，由contentProvider提供数据
        ContentResolver resolver = getContentResolver();

        //2.获取本地音乐存储的Uri地址
        Uri uri = MediaStore.Audio.Media.EXTERNAL_CONTENT_URI;//audio是音频，radio是视频，image是图片

        //3.查询地址
        Cursor cursor = resolver.query(uri,null,null,null,null);

        //4.遍历cursor
        //判断是否能移动到下一个
        int id = 0;//用来实现id自增
        while (cursor.moveToNext()) {
            String song = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.TITLE));//获取歌曲名
            String singer = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.ARTIST));//歌手
            id++;
            String  sid = String.valueOf(id);
            String album = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.ALBUM));//专辑
            String path = cursor.getString(cursor.getColumnIndex(MediaStore.Audio.Media.DATA));//路径
            long duration = cursor.getLong(cursor.getColumnIndex(MediaStore.Audio.Media.DURATION));//时长
            SimpleDateFormat sdf = new SimpleDateFormat("mm:ss");//由于歌曲时长是长整形的毫秒数，所以进行格式转换
            String time = sdf.format(new Date(duration));

            //将数据存进集合
            LocalMusicBean localMusicBean = new LocalMusicBean(sid,song,singer,album,time,path);
            mDatas.add(localMusicBean);
        }
        //数据源变化，提示适配器更新
        localMusicAdapter.notifyDataSetChanged();
    }

    private void initView(){
        /*初始化控件*/
        nextSong = findViewById(R.id.play_next);//下一首
        lastSong = findViewById(R.id.play_beyond);//上一首
        playButton = findViewById(R.id.play_stop);//播放按钮
        singerTv = findViewById(R.id.singer_name);//歌手名称
        songTv = findViewById(R.id.song_name);//歌曲名称
        musicRv = findViewById(R.id.local_music_rv);//recyclerview


        nextSong.setOnClickListener(this);
        lastSong.setOnClickListener(this);
        playButton.setOnClickListener(this);

    }

    @Override
    public void onClick(View view) {

        switch (view.getId()){
            case R.id.play_next:
                if (currentPlayPosition==mDatas.size()-1){
                    Toast.makeText(this,"this is the last song",Toast.LENGTH_SHORT).show();
                    return;
                }
                currentPlayPosition = currentPlayPosition +1;
                LocalMusicBean nextBean = mDatas.get(currentPlayPosition);
                musicInPosition(nextBean);

                break;

            case R.id.play_beyond:
                if (currentPlayPosition==0){
                    Toast.makeText(this,"已经是第一首了，没有上一曲",Toast.LENGTH_SHORT).show();
                    return;
                }
                currentPlayPosition = currentPlayPosition-1;
                LocalMusicBean lastBean = mDatas.get(currentPausePosition);
                musicInPosition(lastBean);

                break;

            case R.id.play_stop:
                if (currentPlayPosition==-1) {
                    //并没有选中要播放的音乐
                    Toast.makeText(this,"choose a song",Toast.LENGTH_SHORT).show();
                    return;
                }
                if (mediaPlayer.isPlaying()){
                    //此时处于播放状态，需要暂停音乐
                    pauseMusic();
                }else {
                    //此时没有播放音乐，点击开始播放音乐
                    playMusic();
                }

                break;
        }
    }


}

```



## Adapter完整代码

```java
package com.yht.musicaudio;

import android.content.Context;
import android.text.Layout;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import java.util.List;

//继承自RecyclerView.Adapter
public class LocalMusicAdapter extends RecyclerView.Adapter<LocalMusicAdapter.LocalMusicViewHolder> {

    //用来获取recyclerview和数据
    Context context;
    List<LocalMusicBean> mdatas;
    OnItemClickListener onItemClickListener;


    //itemClick的setter方法
    public void setOnItemClickListener(OnItemClickListener onItemClickListener) {
        this.onItemClickListener = onItemClickListener;
    }

    public LocalMusicAdapter(Context context, List<LocalMusicBean> mdatas) {
        this.context = context;
        this.mdatas = mdatas;
    }

    //由于RecyclerView没有封装item的点击事件，自定义接口OnItemClickListener
    public interface OnItemClickListener{
        public void OnItemClick(View view,int position);//在MainActivity重写方法
    }

    @NonNull
    @Override
    public LocalMusicViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(context).inflate(R.layout.item_musiclist,parent,false);//将item布局转换为view对象
        LocalMusicViewHolder holder = new LocalMusicViewHolder(view);
        return holder;//将view对象传给viewholder
    }

    //绑定viewholder，并将每一个控件内容进行赋值展示
    @Override
    public void onBindViewHolder(@NonNull LocalMusicViewHolder holder, final int position) {

        LocalMusicBean musicBean = mdatas.get(position);//通过点击的recyclerview的对应item找到位置，并且获取data中对应的那一条数据
        //将item中的内容设置为获取到的集合的数据内容
        holder.idTv.setText(musicBean.getId());
        holder.songTv.setText(musicBean.getSong());
        holder.singerTv.setText(musicBean.getSinger());
        holder.albumTv.setText(musicBean.getAlbum());
        holder.durationTv.setText(musicBean.getDuration());

        //通过以下方法为每一个item设置绑定点击事件
        holder.itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                onItemClickListener.OnItemClick(view,position);
            }
        });

    }

    @Override
    public int getItemCount() {
        return mdatas.size();//返回集合长度
    }

    //通过内部类继承viewholder来获取控件
    class LocalMusicViewHolder extends RecyclerView.ViewHolder{

        //声明并且获取控件
        TextView songTv,singerTv,idTv,albumTv,durationTv;
        public LocalMusicViewHolder(@NonNull View itemView) {
            super(itemView);

            songTv = itemView.findViewById(R.id.local_local_song);
            singerTv = itemView.findViewById(R.id.item_singer_name);
            idTv = itemView.findViewById(R.id.item_music_num);
            albumTv = itemView.findViewById(R.id.item_song_albun);
            durationTv = itemView.findViewById(R.id.item_music_durtion);
        }
    }
}

```

