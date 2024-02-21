# 存储方式

## 一、SharedPreferences

想要使用SharedPreferences 来存储数据，首先主要获取到SharedPreferences 对象。

<img src="https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/1608182446441.png" alt="1608182446441" style="zoom:67%;" />

新建sharedpreference.Activity

添加布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".DataStorage.SharedPreferencesActivity"
    android:orientation="vertical"
    android:padding="15dp">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/pre_ed"
        android:hint="输入"/>
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="保存"
        android:id="@+id/pre_bt"/>
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/pre_show"
        android:text="显示"/>
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/pre_tv"
        />
</LinearLayout>
```



Activity逻辑书写

```java
public class SharedPreferencesActivity extends AppCompatActivity {

    private EditText editText;
    private Button button,button2;
    private TextView textView;
    private SharedPreferences sharedPreferences;//读操作
    private SharedPreferences.Editor editor;//写操作
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        .....

        //实例化
        sharedPreferences = getSharedPreferences("data",MODE_PRIVATE);//两个参数，文件名和模式，priva模式：其他应用无法读取这个应用数据
        editor = sharedPreferences.edit();

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                editor.putString("name",editText.getText().toString());//键值对
                editor.apply();//要提交,异步存入，commit()同步存入
                ToastUtil.showMsg(getApplicationContext(),"保存成功");
            }
        });

        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                textView.setText(sharedPreferences.getString("name",""));//查询数据并且显示在textview上面
            }
        });
    }
}
```



数据文件存放目录

![image-20210706153955063](Android--存储.assets/image-20210706153955063.png)

![image-20210706154016118](Android--存储.assets/image-20210706154016118.png)

![image-20210706154039787](Android--存储.assets/image-20210706154039787.png)

找到项目名:

![image-20210706154053492](Android--存储.assets/image-20210706154053492.png)



参考:https://blog.csdn.net/qew2017/article/details/99982109





### 迁移至DataStore

>  **为什么要迁移**
>
> SharedPreference可能阻塞UI线程，导致ANR异常 
>
> **DataStore优势**
>
> 以事务方式处理更新数据，事务有四大特性（原子性、一致性、 隔离性、持久性）
>
> **两者比较**
>
> 

DataStore 提供两种不同的实现：`Preferences DataStore` 和 `Proto DataStore`。 

> - **Preferences DataStore** 使用键存储和访问数据。此实现不需要预定义的架构，也不确保类型安全。
>
>   > ```groovy
>   >     // Preferences DataStore (SharedPreferences like APIs)
>   >     dependencies {
>   >         implementation "androidx.datastore:datastore-preferences:1.0.0"
>   > 
>   >         // optional - RxJava2 support
>   >         implementation "androidx.datastore:datastore-preferences-rxjava2:1.0.0"
>   > 
>   >         // optional - RxJava3 support
>   >         implementation "androidx.datastore:datastore-preferences-rxjava3:1.0.0"
>   >     }
>   > 
>   >     // Alternatively - use the following artifact without an Android dependency.
>   >     dependencies {
>   >         implementation "androidx.datastore:datastore-preferences-core:1.0.0"
>   >     }
>   >     
>   > ```
>   >
>   > 
>
> - **Proto DataStore** 将数据作为自定义数据类型的实例进行存储。此实现要求您使用[**协议缓冲区**]来定义架构，但可以确保类型安全。
>
>   > ```groovy
>   >     // Typed DataStore (Typed API surface, such as Proto)
>   >     dependencies {
>   >         implementation "androidx.datastore:datastore:1.0.0"
>   > 
>   >         // optional - RxJava2 support
>   >         implementation "androidx.datastore:datastore-rxjava2:1.0.0"
>   > 
>   >         // optional - RxJava3 support
>   >         implementation "androidx.datastore:datastore-rxjava3:1.0.0"
>   >     }
>   > 
>   >     // Alternatively - use the following artifact without an Android dependency.
>   >     dependencies {
>   >         implementation "androidx.datastore:datastore-core:1.0.0"
>   >     }
>   >     
>   > ```
>   >
>   > 

#### Preferences DataStore存储键值对

> **创建Pre DataStore**
>
> ```java
> RxDataStore<Preferences> dataStore =
>   new RxPreferenceDataStoreBuilder(context, /*name=*/ "settings").build();//name参数制定dataStore名称
> ```
>
> **读取内容**
>
> ```java
> Preferences.Key<Integer> EXAMPLE_COUNTER = PreferencesKeys.int("example_counter");//定义键值
> 
> Flowable<Integer> exampleCounterFlow =
>   dataStore.data().map(prefs -> prefs.get(EXAMPLE_COUNTER));//通过键值取值
> ```
>
> **写入内容**
>
> `edit()`函数，以事务方式更新DataStore数据，其`transform`参数可以放入代码块，进行值更新
>
> ```java
> Single<Preferences> updateResult =  dataStore.updateDataAsync(prefsIn -> {
>   MutablePreferences mutablePreferences = prefsIn.toMutablePreferences();
>   Integer currentInt = prefsIn.get(INTEGER_KEY);
>   mutablePreferences.set(INTEGER_KEY, currentInt != null ? currentInt + 1 : 1);
>   return Single.just(mutablePreferences);
> });
> // The update is completed once updateResult is completed.
> ```

#### Proto DataStore存储类型化对象

> **定义架构**
>
> Proto DataStore 要求在 `app/src/main/proto/` 目录的 proto 文件中保存预定义的架构。此架构用于定义您在 Proto DataStore 中保存的对象的类型。 
>
> ```
> syntax = "proto3";
> 
> option java_package = "com.example.application";
> option java_multiple_files = true;
> 
> message Settings {
>   int32 example_counter = 1;
> }
> ```
>
> > 存储对象的类在编译时由 proto 文件中定义的 `message` 生成。请务必重新构建您的项目。 

> **创建Proto DataStore**
>
> 涉及两个步骤：
>
> 1. 定义一个实现 `Serializer` 的类，其中 `T` 是 proto 文件中定义的类型。此序列化器类会告知 DataStore 如何读取和写入您的数据类型。请务必为该序列化器添加默认值，以便在尚未创建任何文件时使用。
> 2. 使用由 `dataStore` 创建的属性委托来创建 `DataStore` 的实例，其中 `T` 是在 proto 文件中定义的类型。filename` 参数会告知 DataStore 使用哪个文件存储数据，而 `serializer 参数会告知 DataStore 第 1 步中定义的序列化器类的名称。
>
> ```java
> private static class SettingsSerializer implements Serializer<Settings> {
>     @Override
>     public Settings getDefaultValue() {
>         Settings.getDefaultInstance();
>     }
> 
>     @Override
>     public Settings readFrom(@NotNull InputStream input) {
>         try {
>             return Settings.parseFrom(input);
>         } catch (exception: InvalidProtocolBufferException) {
>             throw CorruptionException(“Cannot read proto.”, exception);
>         }
>     }
> 
>     @Override
>     public void writeTo(Settings t, @NotNull OutputStream output) {
>         t.writeTo(output);
>     }
> }
> 
> RxDataStore<Byte> dataStore =
>     new RxDataStoreBuilder<Byte>(context, /* fileName= */ "settings.pb", new SettingsSerializer()).build();
> ```
>
> **读取内容**
>
> ```java
> Flowable<Integer> exampleCounterFlow =
>        dataStore.data().map(settings -> settings.getExampleCounter());
> ```
>
> **写入内容**
>
> ```java
> Single<Settings> updateResult =
>     dataStore.updateDataAsync(currentSettings ->
>                               Single.just(
>                                   currentSettings.toBuilder()
>                                   .setExampleCounter(currentSettings.getExampleCounter() + 1)
>                                   .build()));
> ```
>
> 



## 二、SQLite

> [sqlLite使用](https://blog.csdn.net/huweiliyi/article/details/105461725)

### 	(1)概述：

Android提供了一个SQLiteOpenHelper 帮助类，借助这个类可以非常简单的将数据库进行创建好升级。

### 	(2)实例方法:

SQLiteOpenHelper 中有两个非常重要的实例方法：

- getReadableDatabase() 	

- getWritableDatabase() 。

  

  这两个方法可以创建或者打开一个现有的数据库（如果数据库存在则直接打开，否则创建一个新的数据库），并返回一个可对数据库进行读写操作的对象。

例子:

```java
public class MainActivity extends Activity {
	public static final String PATH_ONE = "KogBill";
	public static final String PATH_NAME = "KogBill.db";
	private SQLiteDatabase db;    //声明SQLiteDatabase ，该对象可以操作数据库

	String path = Environment.getExternalStorageDirectory().getAbsolutePath();
	String path1 = path + File.separator + PATH_ONE;   //需要创建的路径
	String path2 = path + File.separator + PATH_ONE + File.separator + PATH_NAME;//需要创建的文件

	@Override
	protected void onCreate(Bundle savedInstanceState){
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		File f = new File(path1);
		if( !f.exists()){  //创建数据库文件路径
			f.mkdirs();
		}	
		//实例化MySQLiteHelper ，创建指定目录下数据库文件，并创建表
		MySQLiteHelper mSQL = new MySQLiteHelper(MainActivity.this, path2);
		db = mSQL.getWritableDatabase();
	}

	class MySQLiteHelper extends SQLiteOpenHelper{
			private static final int DATABASE_VERSION = 1;//数据库版本号
			private static final String CREATE_TABLE = "create table kog_bill ("
		            + "_id integer primary key autoincrement,"
		            + "date text, "
		            + "breakfast text, "
		            + "lunch text,"
		            + "dinner text,"
		            + "happy text,"
		            + "other text,"
		            + "spare text)";
			
			//方便创建实例，简化构造方法，方法内调用4参数构造方法
			//参数 name 可以是 数据库名称，也可以数据库文件路径（即可以指定数据库文件路径）
			public MySQLiteHelper(Context context, String name) {
				this(context, name, null, DATABASE_VERSION);
			}
			//必须要实现的方法
			public MySQLiteHelper(Context context, String name, CursorFactory factory, int version) {
				super(context, name, factory, version);
			}
	
			@Override
			public void onCreate(SQLiteDatabase db) {
				// 第一次创建数据库时 才会调用
				Log.e("mylog", "创建数据库表");
				db.execSQL(CREATE_TABLE);
			}
	
			@Override
			public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
			}
			
		}
}
```

如果想重新建表，则需要在onUpgrade方法中进行语句书写:

```java
class MySQLiteHelper extends SQLiteOpenHelper{
	.....
	@Override
	public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion){
		db.execSQL("drop table if exists book");  //如果已经存在就删除，防止重复创建
		onCreate(db);  // 再次执行onCreate 方法
	}
}
```

但是onUpgrade方法默认是不执行的，如何让onUpgrade方法执行，需要用到MySQLiteHelper 构造参数中的版本号：

```
private static final int DATABASE_VERSION = 1;//  将版本号 由 1 改为2 
```



### 迁移至Room

## 三、ContentProvider

> 见四大组件

## 四、Room

> 参考——https://developer.android.google.cn/training/data-storage/room?hl=zh-cn

### (1)概览

Room 持久性库在 SQLite 上提供了一个抽象层，具有以下优势：

- 针对 SQL 查询的编译时验证。
- 可最大限度减少重复和容易出错的样板代码的方便注解。
- 简化了数据库迁移路径。

> **使用Room框架的其中一个好处是，如果创建过程中有问题，在编译期间编辑器就会提示你，而不用等到程序运行时。**

### (2)主要组件

- [数据库类]，用于保存数据库并作为应用持久性数据底层连接的主要访问点。
- [数据实体]，用于表示应用的数据库中的表。
- [数据访问对象 (DAO)]，提供您的应用可用于查询、更新、插入和删除数据库中的数据的方法。

![image-20230417150155745](Android--存储.assets/image-20230417150155745.png)

### (3)使用案例

```groovy
dependencies {
    def room_version = "2.5.0"

    implementation "androidx.room:room-runtime:$room_version"
    annotationProcessor "androidx.room:room-compiler:$room_version"

    // To use Kotlin annotation processing tool (kapt)
    kapt "androidx.room:room-compiler:$room_version"
    // To use Kotlin Symbol Processing (KSP)
    ksp "androidx.room:room-compiler:$room_version"

    // optional - RxJava2 support for Room
    implementation "androidx.room:room-rxjava2:$room_version"

    // optional - RxJava3 support for Room
    implementation "androidx.room:room-rxjava3:$room_version"

    // optional - Guava support for Room, including Optional and ListenableFuture
    implementation "androidx.room:room-guava:$room_version"

    // optional - Test helpers
    testImplementation "androidx.room:room-testing:$room_version"

    // optional - Paging 3 Integration
    implementation "androidx.room:room-paging:$room_version"
}
```

**数据实体**

以下代码定义了一个 `User` 数据实体。`User` 的每个实例都代表应用数据库中 `user` 表中的一行。

```java
@Entity
public class User {
    @PrimaryKey
    public int uid;

    @ColumnInfo(name = "first_name")
    public String firstName;

    @ColumnInfo(name = "last_name")
    public String lastName;
}
```

**数据访问对象**

`UserDao` 提供了应用的其余部分用于与 `user` 表中的数据交互的方法。

```java
@Dao
public interface UserDao {
    @Query("SELECT * FROM user")
    List<User> getAll();

    @Query("SELECT * FROM user WHERE uid IN (:userIds)")
    List<User> loadAllByIds(int[] userIds);

    @Query("SELECT * FROM user WHERE first_name LIKE :first AND " +
           "last_name LIKE :last LIMIT 1")
    User findByName(String first, String last);

    @Insert
    void insertAll(User... users);

    @Delete
    void delete(User user);
}
```

**数据库**

以下代码定义了用于保存数据库的 `AppDatabase` 类。 `AppDatabase` 定义数据库配置，并作为应用对持久性数据的主要访问点。数据库类必须满足以下条件：

- 该类必须带有 [`@Database`]注解，该注解包含列出所有与数据库关联的数据实体的 [`entities`] 数组。
- 该类必须是一个抽象类，用于扩展 [`RoomDatabase`]。
- 对于与数据库关联的每个 DAO 类，数据库类必须定义一个具有零参数的抽象方法，并返回 DAO 类的实例。

```java
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    private static final String DATABASE_NAME = "my_db";

    private static MyDataBase myDataBase;

    //单例模式
    public static synchronized MyDataBase getInstance(Context context){
        if(myDataBase==null){
            myDataBase = Room.databaseBuilder(context.getApplicationContext(),MyDataBase.class, DATABASE_NAME).build();
        }

        return myDataBase;
    }
    public abstract UserDao getUserdao();
}
```

> **注意**：
>
> ***①如果应用在单个进程中运行，在实例化 `AppDatabase` 对象时应遵循单例设计模式。***因为每个 `RoomDatabase` 实例的成本相当高。
>
> ②如果您的应用在多个进程中运行，请在数据库构建器调用中包含 `enableMultiInstanceInvalidation()`。这样，如果您在每个进程中都有一个 `AppDatabase` 实例，可以在一个进程中使共享数据库文件失效，并且这种失效会自动传播到其他进程中 `AppDatabase` 的实例。

**创建数据库实例**

```java
AppDatabase db = Room.databaseBuilder(getApplicationContext(),AppDatabase.class, "数据库名称").build();
```

**然后在 `AppDatabase` 中的抽象方法获取 DAO 的实例，转而可以使用 DAO 实例中的方法与数据库进行交互：**

```java
//对于数据库操作，不应该在UI线程进行
new Thread(){
    @Override
    public void run() {
        super.run();
        UserDao userDao = myDataBase.getUserdao();//获取UserDao实例
        userDao.insertUser(new UserEntity(1,"杨凯文"),new UserEntity(2,"文凯杨"));//插入数据
        List<UserEntity> userEntitys = userDao.getUsers();//获取数据
        //传递信息
        Message message = new Message();
        StringBuffer sb = new StringBuffer();
        for(UserEntity userEntity : userEntitys){
            sb.append(userEntity.getUseriId()+","+userEntity.getName()+"\n");
        }
        Bundle bundle = new Bundle();
        bundle.putString("数据库数据",sb.toString());
        message.setData(bundle);
        handler.sendMessage(message);
    }
}.start();

//接收处理信息
Handler handler = new Handler(new Handler.Callback() {
    @Override
    public boolean handleMessage(@NonNull Message msg) {

        textView.append(msg.getData().getString("数据库数据"));

        return true;
    }
});

```

```java
//也可以创建AsyncTask来后台完成数据库操作
private class QueryStudentTask extends AsyncTask<Void, Void, Void>
{
    public QueryStudentTask()
    {
 
    }
 
    @Override
    protected Void doInBackground(Void... arg0)
    {
        studentList.clear();
        studentList.addAll(myDatabase.studentDao().getStudentList());
        return null;
    }
 
    @Override
    protected void onPostExecute(Void result)
    {
        super.onPostExecute(result);
        studentAdapter.notifyDataSetChanged();
    }
}
```





### (4)具体组件解析

#### 1) 使用Room实体定义数据

想上面实例那样，定义一个实体

```java
@Entity
public class User {
    @PrimaryKey 
    public int id;

    public String firstName;
    public String lastName; //字段要公开，或者提供getter/setter方法，否则room无法访问
}
```



**定义表名称**

默认情况下，Room 将类名称用作数据库表名称。同样，字段名称也默认当做数据库中列的名称，如果想自定义，加上注解和属性

```java
@Entity(tableName = "users")
public class User {
    @PrimaryKey
    public int id;

    @ColumnInfo(name = "first_name")
    public String firstName;

    @ColumnInfo(name = "last_name")
    public String lastName;
}
```

> SqlLite中表和列名称不区分大小写

**复合主键**

```java
//定义复合主键
@Entity(primaryKeys = {"firstName", "lastName"})
public class User {
    public String firstName;
    public String lastName;
}
```

**忽略字段**

默认情况下，Room 会为实体中定义的每个字段创建一个列。 如果某个实体中有您不想保留的字段，则可以使用 [`@Ignore`](https://developer.android.google.cn/reference/androidx/room/Ignore?hl=zh-cn) 为这些字段添加注解，如以下代码段所示：

```java
@Entity
public class User {
    @PrimaryKey
    public int id;

    public String firstName;
    public String lastName;

    @Ignore
    Bitmap picture;
}
```

如果实体继承了父实体的字段，则使用 `@Entity` 属性的 [`ignoredColumns`]属性通常会更容易：

```java
@Entity(ignoredColumns = "picture")
public class RemoteUser extends User {
    @PrimaryKey
    public int id;

    public boolean hasVpn;
}
```



**全文搜索**

如果应用需要通过全文搜索 (FTS) 快速访问数据库信息，将 [`@Fts3`] 或 [`@Fts4`] 注解添加到给定实体：

```java
// Use `@Fts3` only if your app has strict disk space requirements or if you
// require compatibility with an older SQLite version.
@Fts4
@Entity(tableName = "users")
public class User {
    // 启用 FTS 的表始终使用 INTEGER 类型的主键且列名称为“rowid”。如果是由 FTS 表支持的实体定义主键，则必须使用相应的类型和列名称。
    @PrimaryKey
    @ColumnInfo(name = "rowid")
    public int id;

    @ColumnInfo(name = "first_name")
    public String firstName;
}
```



**将特定列编入索引**

如需为实体添加索引，请在 [`@Entity`]注解中添加 [`indices`]属性，列出要在索引或复合索引中包含的列的名称。

```java
@Entity(indices = {@Index("name"),@Index(value = {"last_name", "address"})})
public class User {
    @PrimaryKey
    public int id;

    public String firstName;
    public String address;

    @ColumnInfo(name = "last_name")
    public String lastName;

    @Ignore
    Bitmap picture;
}
```

为了指定列的唯一性，可以标记unique为true

```java
@Entity(indices = {@Index(value = {"first_name", "last_name"},unique = true)})
public class User {
    @PrimaryKey
    public int id;

    @ColumnInfo(name = "first_name")
    public String firstName;

    @ColumnInfo(name = "last_name")
    public String lastName;

    @Ignore
    Bitmap picture;
}
```



**添加基于AutoValue的对象**

> 什么是AutoValue：
>
> AutoValue是一个可以自动为值类（value type）生成诸如equals，hashCode，toString等模板方法的工具。这样就使得程序更加短小，简洁，以及更少的bug。
>
> > 参考——https://www.jianshu.com/p/d346188b4177

将带有 `@AutoValue` 注解的类用作实体时，必须在每次使用这些注解时添加 `@CopyAnnotations` 注解，以便 Room 可以正确解释这些方法的自动生成实现。

```java
@AutoValue
@Entity
public abstract class User {
    // Supported annotations must include `@CopyAnnotations`.
    @CopyAnnotations
    @PrimaryKey
    public abstract long getId();

    public abstract String getFirstName();
    public abstract String getLastName();

    // Room uses this factory method to create User objects.
    public static User create(long id, String firstName, String lastName) {
        return new AutoValue_User(id, firstName, lastName);
    }
}
```



#### 2)使用 Room DAO 访问数据

和springboot中的DAO类似，用来映射数据库操作，主要可以通过两种方式来设计接口：

- 可让您在不编写任何 SQL 代码的情况下插入、更新和删除数据库中行的便捷方法。

  > Room 提供了方便的注解，用于定义无需编写 SQL 语句即可执行简单插入、更新和删除的方法。
  >
  > **插入**：
  >
  > ```java
  > @Dao
  > public interface UserDao {
  >  @Insert(onConflict = OnConflictStrategy.REPLACE) // 如果插入主键重复，则替换。还有其他的冲突解决机制
  >  public void insertUsers(User... users);
  > 
  >  @Insert
  >  public void insertBothUsers(User user1, User user2);
  > 
  >  @Insert
  >  public void insertUsersAndFriends(User user, List<User> friends);
  > }
  > //@Insert 方法的每个参数必须是带有 @Entity 注解的 Room 数据实体类的实例或数据实体类实例的集合。调用 @Insert 方法时，Room 会将每个传递的实体实例插入到相应的数据库表中。
  > //如果 @Insert 方法接收单个参数，则会返回 long 值，这是插入项的新 rowId。如果参数是数组或集合，则该方法应改为返回由 long 值组成的数组或集合，并且每个值都作为其中一个插入项的 rowId。
  > ```
  >
  > **删除**:
  >
  > ```java
  > @Dao
  > public interface UserDao {
  >  @Delete
  >  public void deleteUsers(User... users);
  > }
  > ```
  >
  > **更新**：
  >
  > ```java
  > @Dao
  > public interface UserDao {
  >  @Update
  >  public void updateUsers(User... users);
  > }
  > ```

- 可让您编写自己的 SQL 查询以与数据库进行交互的查询方法。

  > 使用 [`@Query`]注解，您可以编写 SQL 语句并将其作为 DAO 方法公开。使用这些查询方法从应用的数据库查询数据，或者需要执行更复杂的插入、更新和删除操作。
  >
  > **简单查询：**
  >
  > ```java
  > @Query("SELECT * FROM user")
  > public User[] loadAllUsers();
  > ```
  >
  > **将函数参数绑定给查询：**
  >
  > ```java
  > @Query("SELECT * FROM user WHERE age > :minAge")
  > public User[] loadAllUsersOlderThan(int minAge);
  > ```
  >
  > ```java
  > @Query("SELECT * FROM user WHERE age BETWEEN :minAge AND :maxAge")
  > public User[] loadAllUsersBetweenAges(int minAge, int maxAge);
  > 
  > @Query("SELECT * FROM user WHERE first_name LIKE :search " +
  >     "OR last_name LIKE :search")
  > public List<User> findUserWithName(String search);
  > ```
  >
  > **传递一组长度未知参数：**
  >
  > ```java
  > @Query("SELECT * FROM user WHERE region IN (:regions)")
  > public List<User> loadUsersFromRegions(List<String> regions);
  > ```
  >
  > **多表查询：**
  >
  > ```java
  > @Query("SELECT * FROM book " +
  >     "INNER JOIN loan ON loan.book_id = book.id " +
  >     "INNER JOIN user ON user.id = loan.user_id " +
  >     "WHERE user.name LIKE :userName")
  > public List<Book> findBooksBorrowedByNameSync(String userName);
  > ```
  >
  > **简单对象查询特定的列：**
  >
  > ```java
  > @Dao
  > public interface UserBookDao {
  > @Query("SELECT user.name AS userName, book.name AS bookName " +
  >        "FROM user, book " +
  >        "WHERE user.id = book.user_id")
  > public LiveData<List<UserBook>> loadUserAndBookNames();
  > 
  > // You can also define this class in a separate file, as long as you add the
  > // "public" access modifier.
  > static class UserBook {
  >     public String userName;
  >     public String bookName;
  > }
  > }
  > ```

  **结合Paging分页查询**

  DAO 可以返回 [`PagingSource`]对象以便与 [Paging 3]搭配使用。

  ```java
  @Dao
  interface UserDao {
    @Query("SELECT * FROM users WHERE label LIKE :query")
    PagingSource<Integer, User> pagingSource(String query);
  }
  ```

  

#### 3)定义对象之间关系

- **中间数据类**

  > ```java
  > @Dao
  > public interface UserBookDao {
  > @Query("SELECT user.name AS userName, book.name AS bookName " +
  >        "FROM user, book " +
  >        "WHERE user.id = book.user_id")
  > public LiveData<List<UserBook>> loadUserAndBookNames();
  > }
  > 
  > public class UserBook {
  >  public String userName;
  >  public String bookName;
  > }
  > ```

- **多重映射**

  > ```java
  > @Query(
  >  "SELECT * FROM user" +
  >  "JOIN book ON user.id = book.user_id"
  > )
  > public Map<User, List<Book>> loadUserAndBookNames();
  > 
  > ```
  >
  > **更推荐该方法来替代中间类的方法**

- **嵌套对象**

  > ```java
  > public class Address {
  >  public String street;
  >  public String state;
  >  public String city;
  > 
  >  @ColumnInfo(name = "post_code") public int postCode;
  > }
  > 
  > @Entity
  > public class User {
  >  @PrimaryKey public int id;
  > 
  >  public String firstName;
  > 
  >  @Embedded public Address address;
  > }
  > ```
  >
  > 表示 `User` 对象的表将包含具有以下名称的列：`id`、`firstName`、`street`、`state`、`city` 和 `post_code`。
  >
  > 如果某个实体具有相同类型的多个嵌套字段，可以通过设置 [`prefix`] 属性确保每个列的唯一性

- **定义不同的关系**

  > **一对一**
  >
  > 父实体的每个实例都恰好对应于子实体的一个实例，反之亦然。
  >
  > 假设有一个音乐在线播放应用，用户在该应用中具有一个属于自己的歌曲库。每个用户只有一个库，而且每个库恰好对应于一个用户。因此，`User` 实体和 `Library` 实体之间就应存在一种一对一的关系。
  >
  > ```java
  > @Entity
  > public class User {//父实例
  >  @PrimaryKey public long userId;
  >  public String name;
  >  public int age;
  > }
  > 
  > @Entity
  > public class Library {//子实例
  >  @PrimaryKey 
  >  public long libraryId;
  >  public long userOwnerId;
  > }
  > ```
  >
  > 然后创建一个对应关联两个实体的类
  >
  > ```java
  > public class UserAndLibrary {
  >  @Embedded 
  >  public User user;
  > 
  >  @Relation(
  >       parentColumn = "userId",
  >       entityColumn = "userOwnerId"
  >  )
  >  public Library library;
  > }
  > 
  > //将 @Relation 注释添加到子实体的实例，
  > //同时将 parentColumn 设置为父实体主键列的名称，并将 entityColumn 设置为引用父实体主键的子实体列的名称。
  > ```
  >
  > 最后，向 DAO 类添加一个方法，用于返回将父实体与子实体配对的数据类的所有实例。
  >
  > ```java
  > @Transaction //应向该方法添加 @Transaction 注释，以确保整个操作以原子方式执行。
  > @Query("SELECT * FROM User")
  > public List<UserAndLibrary> getUsersAndLibraries();
  > ```
  >
  > 
  >
  > **一对多**
  >
  > 父实体的每个实例对应于子实体的零个或多个实例，但子实体的每个实例只能恰好对应于父实体的一个实例。
  >
  > ```java
  > @Entity
  > public class User {
  >  @PrimaryKey public long userId;
  >  public String name;
  >  public int age;
  > }
  > 
  > @Entity
  > public class Playlist {
  >  @PrimaryKey public long playlistId;
  >  public long userCreatorId;//子实体必须包含一个变量，且该变量是对父实体的主键的引用。
  >  public String playlistName;
  > }
  > ```
  >
  > 创建一对多的关系
  >
  > ```java
  > public class UserWithPlaylists {
  >  @Embedded public User user;
  >  @Relation(
  >       parentColumn = "userId",
  >       entityColumn = "userCreatorId"
  >  )
  >  public List<Playlist> playlists;
  > }
  > ```
  >
  > 执行
  >
  > ```java
  > @Transaction
  > @Query("SELECT * FROM User")
  > public List<UserWithPlaylists> getUsersWithPlaylists();
  > ```
  >
  > **多对多**
  >
  > 例如：在音乐在线播放应用示例中，再次考虑用户定义的播放列表。 每个播放列表都可以包含多首歌曲，每首歌曲都可以包含在多个不同的播放列表中。因此，`Playlist` 实体和 `Song` 实体之间应存在多对多的关系。
  >
  > ```java
  > @Entity
  > public class Playlist {
  >  @PrimaryKey public long playlistId;
  >  public String playlistName;
  > }
  > 
  > @Entity
  > public class Song {//子实体不存在对父实体的引用	
  >  @PrimaryKey public long songId;
  >  public String songName;
  >  public String artist;  
  > }
  > 
  > @Entity(primaryKeys = {"playlistId", "songId"})
  > public class PlaylistSongCrossRef {
  >  public long playlistId;
  >  public long songId;
  > }
  > ```
  >
  > 建立实体之间的关系
  >
  > ```java
  > public class PlaylistWithSongs {
  >  @Embedded public Playlist playlist;
  >  @Relation(
  >       parentColumn = "playlistId",
  >       entityColumn = "songId",
  >       associateBy = @Junction(PlaylistSongCrossref.class)//使用 associateBy 属性来确定提供 Playlist 实体与 Song 实体之间关系的交叉引用实体。
  >  )
  >  public List<Song> songs;
  > }
  > 
  > public class SongWithPlaylists {
  >  @Embedded public Song song;
  >  @Relation(
  >       parentColumn = "songId",
  >       entityColumn = "playlistId",
  >       associateBy = @Junction(PlaylistSongCrossref.class)
  >  )
  >  public List<Playlist> playlists;
  > }
  > ```
  >
  > 添加查询方法
  >
  > ```java
  > @Transaction
  > @Query("SELECT * FROM Playlist")
  > public List<PlaylistWithSongs> getPlaylistsWithSongs();
  > 
  > @Transaction
  > @Query("SELECT * FROM Song")
  > public List<SongWithPlaylists> getSongsWithPlaylists();
  > ```
  >
  > > 如果Relation注解无法满足查询需求，需要手动使用Join查询来插入



### (5)异步Dao查询

https://developer.android.google.cn/training/data-storage/room/async-queries?hl=zh-cn



### (6)创建视图

2.1.0 及以上版本支持创建视图

```java
@DatabaseView("SELECT user.id, user.name, user.departmentId," +
              "department.name AS departmentName FROM user " +
              "INNER JOIN department ON user.departmentId = department.id")
public class UserDetail {
    public long id;
    public String name;
    public long departmentId;
    public String departmentName;
}
```

将视图和数据库关联

```java
@Database(entities = {User.class}, views = {UserDetail.class},
          version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```



### (7)数据库版本迁移

> 参考——https://developer.android.google.cn/training/data-storage/room/migrating-db-versions?hl=zh-cn

> SqLite迁移到Room——https://developer.android.google.cn/training/data-storage/room/sqlite-room-migration?hl=zh-cn
>
> 首先还是创建Room需要的实体类、Dao等
>
> 然后创建迁移类
>
> ```java
> static final Migration MIGRATION_1_2 = new Migration(1, 2) {
> @Override
> public void migrate(SupportSQLiteDatabase database) {
>  // Empty implementation, because the schema isn't changing.
> }
> };
> ```
>
> 数据库实例化
>
> ```java
> db = Room.databaseBuilder(
>        context.getApplicationContext(),
>        UsersDatabase.class, "database-name"
>      )
>        .addMigrations(MIGRATION_1_2).build();
> ```
>
> 

### (8)Room对象引用

假设您需要在 Room 数据库中保留 [`Date`] 的实例。Room 不知道如何保留 `Date` 对象，因此您需要定义类型转换器：

```java
public class Converters {
  @TypeConverter
  public static Date fromTimestamp(Long value) {
    return value == null ? null : new Date(value);
  }

  @TypeConverter
  public static Long dateToTimestamp(Date date) {
    return date == null ? null : date.getTime();
  }
}

//接下来，将 @TypeConverters 注解添加到 AppDatabase 类，以便 Room 知道您已定义的转换器类：
@Database(entities = {User.class}, version = 1)
@TypeConverters({Converters.class})
public abstract class AppDatabase extends RoomDatabase {
  public abstract UserDao userDao();
}



@Entity
public class User {
  private Date birthday;
}

@Dao
public interface UserDao {
    //可以使用自定义类型
  @Query("SELECT * FROM user WHERE birthday = :targetDate")
  List<User> findUsersBornOnDate(Date targetDate);
}
```

```java
//还可以在创建database实例的时候添加转换器
AppDatabase db = Room.databaseBuilder(...)
  .addTypeConverter(exampleConverterInstance)
  .build();
```



## 五、LitePal存储





## LevelDB



## 六、应用专属存储空间

### 1.两个存储位置

- 内部存储空间目录：

  > 这些目录既包括用于存储持久性文件的专属位置，也包括用于存储缓存数据的其他位置。因为系统会对这些位置进行加密，所以这些位置非常适合存储只有应用本身才能访问的敏感数据。

- 外部存储空间目录：

  > 这些目录既包括用于存储持久性文件的专属位置，也包括用于存储缓存数据的其他位置。虽然其他应用可以在具有适当权限的情况下访问这些目录，但存储在这些目录中的文件仅供应用使用。

> 如果用户卸载应用，系统会移除保存在应用专属存储空间中的文件。



### 2.从内部存储访问

> 这些目录的空间通常比较小。在将应用专属文件写入内部存储空间之前，应[查询设备上的可用空间]。

#### (1)访问持久性文件

可以使用上下文对象的 [`filesDir`]属性访问拥有应用的普通持久性文件目录。

##### **①访问和存储文件**

利用File访问存储文件

```java
File file = new File(context.getFilesDir(), filename);
```



##### **②流方式读和写文件**

```java
//读文件
FileInputStream fis = context.openFileInput(filename);
InputStreamReader inputStreamReader = new InputStreamReader(fis, StandardCharsets.UTF_8);
StringBuilder stringBuilder = new StringBuilder();
try (BufferedReader reader = new BufferedReader(inputStreamReader)) {
    String line = reader.readLine();
    while (line != null) {
        stringBuilder.append(line).append('\n');
        line = reader.readLine();
    }
} catch (IOException e) {
    // Error occurred when opening raw file for reading.
} finally {
    String contents = stringBuilder.toString();
}
```

> 如果在安装时需要以信息流的形式访问文件，请将文件保存在项目的 `/res/raw` 目录中。您可以使用 [`openRawResource()`]打开这些文件，传入带有 `R.raw` 前缀的文件名作为资源 ID。此方法将返回一个 [`InputStream`]，您可以使用它读取文件。您无法写入原始文件。
>
> > 存储在 res/raw 位置的文件不会被平台编译，而是作为可用的原始资源。
> > 读取原始资源非常简单。
> > 首先调用 Context.getResource 获得当前应用程序上下文的 Resources引用 .
> > 然后调用 openRawResource(int id) 得到 InputStream .
> > 最后，操作 InputStream 得到数据。

```java
//写文件
String filename = "myfile";
String fileContents = "Hello world!";
try (FileOutputStream fos = context.openFileOutput(filename, Context.MODE_PRIVATE)) {
    fos.write(fileContents.toByteArray());
}
```



##### **③查看文件列表**

```java
//调用 fileList() 获取包含 filesDir 目录中所有文件名称的数组
Array<String> files = context.fileList();
```



#### (2)缓存文件

**创建缓存文件**

如果需要暂时存储敏感数据，可以指定缓存目录保存数据

```java
File.createTempFile(filename, null, context.getCacheDir());
```

> 需确定应用当前可用的缓存空间大小，请调用 [`getCacheQuotaBytes()`]。
>
> **注意**：当设备的内部存储空间不足时，Android 可能会删除这些缓存文件以回收空间。因此，请在读取前检查缓存文件是否存在。

**访问缓存目录数据**

```java
File cacheFile = new File(context.getCacheDir(), filename);
```

**删除缓存文件**

- 对代表该文件的 `File` 对象使用 [`delete()`] 方法：

  ```java
  cacheFile.delete();
  ```

- 应用上下文的 [`deleteFile()`]方法，并传入文件名：

  ```java
  context.deleteFile(cacheFileName);
  ```



### 3.从外部存储访问

#### (1)验证存储空间可用性

由于外部存储空间位于用户可能能够移除的物理卷上，因此在尝试从外部存储空间读取应用专属数据或将应用专属数据写入外部存储空间之前，请验证该卷是否可访问。

> 可以通过调用 [`Environment.getExternalStorageState()`]查询该卷的状态。
>
> - 如果返回的状态为 [**`MEDIA_MOUNTED`**]，那么就可以在外部存储空间中***读取和写入***应用专属文件。
> - 如果返回的状态为 [**`MEDIA_MOUNTED_READ_ONLY`**]，**只能读**这些文件。

```java
// Checks if a volume containing external storage is available
// for read and write.
private boolean isExternalStorageWritable() {
    return Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED);
}

// Checks if a volume containing external storage is available to at least read.
private boolean isExternalStorageReadable() {
     return Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED) ||
            Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED_READ_ONLY);
}
```

> 在没有可移除外部存储空间的设备上，请使用以下命令启用虚拟卷，以测试外部存储空间可用性逻辑：
>
> ```shell
> adb shell sm set-virtual-disk true
> ```



#### (2)选择物理存储位置

有时，分配内部存储分区作为外部存储空间的设备也会提供 SD 卡插槽。这意味着设备具有多个可能包含外部存储空间的物理卷，因此需要选择用于应用专属存储空间的物理卷。

```java
//返回数组中的第一个元素被视为主外部存储卷。除非该卷已满或不可用，否则请使用该卷。
File[] externalStorageVolumes =
        ContextCompat.getExternalFilesDirs(getApplicationContext(), null);
File primaryExternalStorage = externalStorageVolumes[0];
```



#### (3)访问持久性文件

如需从外部存储设备访问应用专用文件，调用 [`getExternalFilesDir()`]。

```java
File appSpecificExternalDir = new File(context.getExternalFilesDir(null), filename);
```

> **注意**：在 Android 11（API 级别 30）及更高版本中，应用无法在外部存储设备上创建自己的应用专用目录。

#### (4)缓存文件

**创建缓存文件**

```java
File externalCacheFile = new File(context.getExternalCacheDir(), filename);
```

**移除缓存文件**

```java
externalCacheFile.delete();
```



#### (5)媒体内容

如果应用支持使用仅在您的应用内对用户有价值的媒体文件，最好将这些文件存储在外部存储空间中的应用专属目录中，如以下代码段所示：

```java
@Nullable
File getAppSpecificAlbumStorageDir(Context context, String albumName) {
    // Get the pictures directory that's inside the app-specific directory on
    // external storage.
    File file = new File(context.getExternalFilesDir(Environment.DIRECTORY_PICTURES), albumName);
    if (file == null || !file.mkdirs()) {
        Log.e(LOG_TAG, "Directory not created");
    }
    return file;
}
```

>  [`DIRECTORY_PICTURES`] 是 API 常量提供的目录名称。这些目录名称可确保系统正确处理文件。如果没有适合您文件的[预定义子目录名称](https://developer.android.google.cn/reference/android/os/Environment?hl=zh-cn#fields)，可以改为将 `null` 传递到 `getExternalFilesDir()`。这将返回外部存储空间中的应用专属根目录。



### 4.查询可用空间

如果您事先知道要存储的数据量，您可以通过调用 [`getAllocatableBytes()`]查出设备可以为应用提供多少空间。`getAllocatableBytes()` 的返回值可能大于设备上的当前可用空间量。这是因为系统已识别出可以从其他应用的缓存目录中移除的文件。

如果有足够的空间保存您的应用数据，请调用 [`allocateBytes()`]。否则，您的应用可以请求用户从设备[移除一些文件]或从设备[移除所有缓存文件]。

```java
// App needs 10 MB within internal storage.
private static final long NUM_BYTES_NEEDED_FOR_MY_APP = 1024 * 1024 * 10L;

StorageManager storageManager = getApplicationContext().getSystemService(StorageManager.class);
UUID appSpecificInternalDirUuid = storageManager.getUuidForPath(getFilesDir());//获取存储的路径，并且转换为UUID
long availableBytes = storageManager.getAllocatableBytes(appSpecificInternalDirUuid);//获取可用空间大小
if (availableBytes >= NUM_BYTES_NEEDED_FOR_MY_APP) {
    storageManager.allocateBytes(appSpecificInternalDirUuid, NUM_BYTES_NEEDED_FOR_MY_APP);//
} else {
    // To request that the user remove all app cache files instead, set
    // "action" to ACTION_CLEAR_APP_CACHE.
    Intent storageIntent = new Intent();
    storageIntent.setAction(ACTION_MANAGE_STORAGE);
}
```

> 如需请求用户在设备上选择文件进行移除，请调用包含 [`ACTION_MANAGE_STORAGE`] 操作的 intent。此 intent 会向用户显示提示。如果需要，此提示可以显示设备上的可用空间量。如需显示此人性化信息，请使用以下计算结果：

# 存储策略

## 一、移动设备存储空间

|                | 内容类型                                 | 访问方法                                                     | 所需权限                                                     | 其他应用是否可以访问？                          | 卸载应用时是否移除文件？ |
| :------------- | :--------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :---------------------------------------------- | ------------------------ |
| 应用专属文件   | 仅供您的应用使用的文件                   | ①从内部存储空间访问：可以使用 `getFilesDir()` 或 `getCacheDir()` 方法  ②从外部存储空间访问，可以使用 `getExternalFilesDir()` 或 `getExternalCacheDir()` 方法 | 从内部存储空间访问不需要任何权限 ，如果应用在搭载 Android 4.4（API 级别 19）或更高版本的设备上运行，从外部存储空间访问不需要任何权限 | 否                                              | 是                       |
| 媒体           | 可共享的媒体文件（图片、音频文件、视频） | `MediaStore` API                                             | 在 Android 11（API 级别 30）或更高版本中，访问其他应用的文件需要 `READ_EXTERNAL_STORAGE`  在 Android 10（API 级别 29）中，访问其他应用的文件需要 `READ_EXTERNAL_STORAGE` 或 `WRITE_EXTERNAL_STORAGE`  在 Android 9（API 级别 28）或更低版本中，访问**所有**文件均需要相关权限 | 是，但其他应用需要 `READ_EXTERNAL_STORAGE` 权限 | 否                       |
| 文档和其他文件 | 其他类型的可共享内容，包括已下载的文件   | 存储访问框架                                                 | 无                                                           | 是，可以通过系统文件选择器访问                  | 否                       |
| 应用偏好设置   | 键值对                                   | Jetpack Preferences库                                        | 无                                                           | 否                                              | 是                       |
| 数据库         | 结构化数据                               | Room 持久性库                                                | 无                                                           | 否                                              | 是                       |





### 1.内部存储

随应用卸载被删除

**内部存储：不需要任何权限**

> `/data/data/< applicationId,就是包名 >/shared_prefs`
> `/data/data/< applicationId >/databases`
> `/data/data/< applicationId >/files —— 通过context.getFilesDir() 获取该目录`
> `/data/data/< applicationId >/cache —— 通过context.getCacheDir() 获取该目录`
> Android SDK提供了几个常见的内部存储文件的权限

Context.MODE_PRIVATE ：私有方式存储，其他应用无法访问，覆盖旧的同名文件
Context.MODE_APPEND：私有方式存储，若有旧的同名文件，则在该文件上追加数据



### 2.外部存储

1. 外部存储：

   > - 公有目录：存放一些下载的视频文件等，比如还有movies，fictures，music等公有的一些文件目录
   > - 私有目录：随应用卸载被删除

**公有目录：**

`Environment.getExternalStoragePublicDirectory(int type)`：

>  这里的type类型不能为空：可以是DIRECTORY_MUSIC，DIRECTORY_MOVIES等 通过这个方法获取公有目录下相对应的文件
> Environment.getExternalStorageDirectory()也可以获取。


注：Environment是操作SD卡的工具类

```java
//publicDirectory路径为：/storage/emulated/0/Movies
String publicDirectory = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_MOVIES).getAbsolutePath();
//sdPath路径为： /storage/emulated/0
String sdPath = Environment.getExternalStorageDirectory().getAbsolutePath();
```

**私有目录**

> /mnt/sdcard/Android/data/data/< applicationId >/cache 通过getExternalCacheDir().getAbsolutePath()得到的路径为：
> /storage/emulated/0/Android/data/包名/cache等价于上面的路径。
> /mnt/sdcard/Android/data/data/< applicationId >/files 通过getExternalFilesDir().getAbsolutePath()方法可以获取到路径为：
> /storage/emulated/0/Android/data/包名/files等价于上面的路径。

> 参考——https://blog.csdn.net/lu202032/article/details/119580314



## 二、存储用例的最佳做法

为了让用户更好地控制自己的文件并减少混乱，Android 10 针对应用推出了一种新的存储范例，称为[分区存储]

> 参考——https://developer.android.google.cn/training/data-storage/use-cases?hl=zh-cn

### 1.处理媒体文件



### 2.处理非媒体文件



