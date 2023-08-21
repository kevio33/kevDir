# 仿TikTok

> https://github1s.com/running-libo/Tiktok

# 一、项目

# 二、其他

## 1.BaseActivity

> 参考—— [BaseActivity的作用](https://blog.csdn.net/fuxuanyang/article/details/52423488?ydreferer=aHR0cHM6Ly9jbi5iaW5nLmNvbS8%3D) 
>
>  [BaseActivity基类的封装](https://blog.csdn.net/android157/article/details/92790482?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-92790482-blog-52423488.235^v38^pc_relevant_sort_base1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-92790482-blog-52423488.235^v38^pc_relevant_sort_base1&utm_relevant_index=2) 

通常自定义`BaseActivity`继承`Activity`，作为项目Activity基类，作用如下：

> - 开发中经常都要获取屏幕的宽高，这时候就需要用到`BaseActivity`，减少多个`Activity`重复代码获取
> - 用户退出应用，销毁所有的`Activity`
> - 解决`Activity`过多，`Intent`发送混乱的问题， 可以将`Intent`封装到`BaseActivity`的一个方法中。打印的时候打印`this`，就可以显示是从哪个`Acitivity`传递过来的数据了。 

```java
package com.example.neteworknews.activity;
 
import java.util.ArrayList;
 
import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.util.DisplayMetrics;
import android.view.Display;
import android.view.WindowManager;
 
public class BaseActivity extends Activity {
	
 
	ArrayList<Activity> activities = new ArrayList<Activity>();
	
	/**屏幕宽*/
	public static int SCREEN_WIDTH;
	
	/**屏幕高*/
	public static int SCREEN_HEIGHT;
 
 
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		WindowManager manager = getWindowManager();
		Display display = manager.getDefaultDisplay();
		DisplayMetrics dm = new DisplayMetrics();
		display.getMetrics(dm);
		SCREEN_WIDTH = dm.widthPixels;
		SCREEN_HEIGHT = dm.heightPixels;
		addActivity();
		
	}
 
	@Override
	protected void onDestroy() {
		super.onDestroy();
		removeActivity();
	}
 
	public void addActivity() {
		activities.add(this);
	}
 
	public void removeActivity() {
		activities.remove(this);
	}
 
	/**
	 * 结束所以活动
	 */
	public void finishAllAcitivity() {
		for (Activity activity : activities) {
			if (!activity.isFinishing()) {
				activity.finish();
			}
		}
	}
 
	/**
	 * 跳转
	 */
	public void startAction(@SuppressWarnings("rawtypes") Class cls,
			String name, String value) {
		Intent intent = new Intent(this, cls);
		intent.putExtra(name, value);
		startActivity(intent);
	}
}
```

## 2.immersionbar

>  [沉浸式状态栏ImmersionBar的使用](https://www.jianshu.com/p/98c1aba92468) 
>
>  [ImmersionBar沉浸式状态栏](https://blog.csdn.net/weixin_53463734/article/details/124484778) 

## 3.windowManager



## 4.overridePendingTransition

>  [overridePendingTransition的使用](https://blog.csdn.net/u010356768/article/details/104497573) 

## 5.Xtablayout

>  [XTabLayout的使用](https://www.jianshu.com/p/9ae60dcb8f67) 

## 6.tablayout

>  [(205条消息) Android原生TabLayout使用全解析，看这篇就够了_yechaoa的博客-CSDN博客](https://blog.csdn.net/yechaoa/article/details/122270969) 

> **选中字体改变大小—— [TabLayout设置选中字体变大，加粗，透明度_material tablayout 加粗_](https://blog.csdn.net/qq_34895720/article/details/107999808) **

### viewpager嵌套问题



### fragment+viewpager

> **参考：**
>
>  [ViewPager结合Fragment的使用_viewpager+fragment](https://blog.csdn.net/indeedes/article/details/120194401) 
>
>  [Fragment中嵌入Viewpager - 简书 (jianshu.com)](https://www.jianshu.com/p/53e925b3c80f) 

## 7.drawable

该文件夹配置`shape.xml`相关属性

## 8.butterknife

项目中使用了这个工具，但是现在已经淘汰改用`viewbinding`



## 9.Rxbus

> **参考：**
>
>  [RxBus使用](https://www.jianshu.com/p/5dd3fca22552) 

Rxbus属于Rxjava库下一小部分功能，如果项目已经使用Rxjava，就不需要再额外引入EventBus库来使用了，直接使用Rxbus就可以了撒。需要从Eventbus转到Rxbus使用的童鞋可以放心转，使用方式大同小异。

## 10.Swiperefreshlayout

 实现下拉刷新的界面模式。 

```groovy
dependencies {
    implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.1.0"
}
```

## 11.Field类对象

>  参考：
>
> [Field类的使用_field.get_二木成林的博客](https://blog.csdn.net/cnds123321/article/details/119514311) 

## 12.java内部类

> [Java内部类的定义、如何创建内部类、内部类的分类、内部类与外部类的关系](https://blog.csdn.net/zhao_miao/article/details/83245816) 