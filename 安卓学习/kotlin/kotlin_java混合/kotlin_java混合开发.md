



## 一.配置

> 参考—— https://blog.csdn.net/Cyanogen_dom/article/details/107876869
>
>  https://www.jianshu.com/p/ae0c11aad2b2



首先在`java`项目中创建一个`kotlin`文件，项目会提示进行`kotlin`配置，最后两个gradle文件内容如下

```groovy
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
}

android {
    namespace 'com.example.myapplication'
    compileSdk 34

    defaultConfig {
        applicationId "com.example.myapplication"
        minSdk 26
        targetSdk 34
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    //下面这个加了最后才成功
    kotlinOptions {
        jvmTarget = '1.8'
    }

    viewBinding{
        enable = true
    }
}

dependencies {

    implementation 'androidx.appcompat:appcompat:1.4.1'
    implementation 'com.google.android.material:material:1.5.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.3'
    implementation 'androidx.core:core-ktx:+'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.3'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

**项目的gradle**

```groovy
// Top-level build file where you can add configuration options common to all sub-projects/modules.
plugins {
    id 'com.android.application' version '8.0.1' apply false
    id 'com.android.library' version '8.0.1' apply false
    id 'org.jetbrains.kotlin.android' version '1.9.0-Beta' apply false
}
```



## 二.java调用kotlin方法

> [kotlin＜文件中的方法＞和＜类中的方法＞的使用区别](https://blog.csdn.net/Goals1989/article/details/107631096)

现在有一个`MyUtils.kt`文件，文件既有类又有方法

```kotlin
//kotlin文件名：MyUtils.kt
//kotlin类
class MyUtils {
    //类中的方法，使用时需要先创建归属的类
    fun show(info: String) {
        println(info)
    }
}
 
//kotlin文件中的方法，全局静态，只要不重名即可，写在哪里都一样
fun show122(info: String) {
    println(info)
}
```



**Kt调用kt文件方法：**

KtClient.kt 用于演示kt如何调用kt文件中的方法，和类中的方法 

```kotlin
//文件名KtClient，用于演示kt调用kt的方法
fun main() {

    //类中的方法一定要先调用这个类，再从这个类中调用其中的方法
    MyUtils().show("类中的方法")

    //文件中的方法可以直接调用，这个方法是全局静态的，只要全局不重名即可！！！！
    show122("文件中的方法")
}
```



**Java调用kt文件中的方法和类中方法**

```java
public class JavaClient {
    public static void main(String[] args) {
        //类中的方法，相当于Java类中的方法，需要先新建这个类
        new MyUtils().show("fun is in the class");

        //非类中直接的方法，相当于文件类中的静态方法
        MyUtilsKt.show122("fun is out of class");
    }
}
```



**解析：**

kt文件在编译后会生成一个`文件名kt.java`，而里面非类中的方法可以直接通过调用静态方法的方式调用。

```java
MyUtilsKt.show122("fun is out of class");
```

