# Java反射机制

> [java-反射](https://pdai.tech/md/java/basic/java-basic-x-reflection.html)
>
> [java反射和正射](https://www.cnblogs.com/chanshuyi/p/head_first_of_reflection.html)

## 一、正射

一般知道一个类，直接创建类的对象称为正射

```java
Apple apple = new Apple(); //直接初始化，「正射」
apple.setPrice(4);
```



## 二、反射

 **反射就是在运行时才知道要操作的类是什么，并且可以在运行时获取类的完整构造，并调用对应的方法。** 

```java
//获取类的 Class 对象实例
Class clz = Class.forName("com.chenshuyi.reflect.Apple");

//根据 Class 对象实例获取 Constructor 对象
Constructor constructor = clz.getConstructor();

//使用 Constructor 对象的 newInstance 方法获取反射类对象
Object object = constructor.newInstance();

//获取对象的方法
Method method = clz.getMethod("setPrice", int.class);
//调用method，要传入对象
method.invoke(object, 4);
```



**完整的例子**

```java
public class Apple {

    private int price;

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public static void main(String[] args) throws Exception{
        //正常的调用
        Apple apple = new Apple();
        apple.setPrice(5);
        System.out.println("Apple Price:" + apple.getPrice());
        
        //使用反射调用
        Class clz = Class.forName("com.chenshuyi.api.Apple");
        Method setPriceMethod = clz.getMethod("setPrice", int.class);
        Constructor appleConstructor = clz.getConstructor();
        Object appleObj = appleConstructor.newInstance();
        setPriceMethod.invoke(appleObj, 14);
        Method getPriceMethod = clz.getMethod("getPrice");
        System.out.println("Apple Price:" + getPriceMethod.invoke(appleObj));
    }
}
```



## 三、反射常用API

### 1.获取反射中的Class对象

 在反射中，要获取一个类对象或调用一个类的方法，我们首先需要获取到该类的 Class 对象。 

**第一种，使用 Class.forName 静态方法。**当你知道该类的全路径名时，你可以使用该方法获取 Class 类对象。

```java
Class clz = Class.forName("java.lang.String");
```

**第二种，使用 .class 方法。**

这种方法只适合在**编译前**就知道操作的 Class。

```java
Class clz = String.class;
```

**第三种，使用类对象的 getClass() 方法。**

```java
String str = new String("Hello");
Class clz = str.getClass();
```





### 2.通过反射创建类对象

通过反射创建类对象主要有两种方式：通过 Class 对象的 `newInstance() `方法、通过 Constructor 对象的 `newInstance() `方法。 

> 通过 Constructor 对象创建类对象可以选择特定构造方法;
>
> 通过 Class 对象则**只能使用默认的无参数构造方法**。下面的代码就调用了一个有参数的构造方法进行了类对象的初始化。 



 **第一种：通过 Class 对象的 newInstance() 方法。** 

```java
Class clz = Apple.class;
Apple apple = (Apple)clz.newInstance();
```

 **第二种：通过 Constructor 对象的 newInstance() 方法** 

```java
Class clz = Apple.class;
Constructor constructor = clz.getConstructor();
Apple apple = (Apple)constructor.newInstance();

//调用有参构造函数
Class clz = Apple.class;
Constructor constructor = clz.getConstructor(String.class, int.class);
Apple apple = (Apple)constructor.newInstance("红富士", 15);
```



### 3.获取属性、方法、构造器

 通过 Class 对象的 `getFields() `方法可以获取 Class 类的属性，**但无法获取私有属性**。 

```java
Class clz = Apple.class;
Field[] fields = clz.getFields();
for (Field field : fields) {
    System.out.println(field.getName());
}
```

 而如果使用 Class 对象的 `getDeclaredFields() `方法则可以**获取包括私有属性在内的所有属性**： 

```java
Class clz = Apple.class;
Field[] fields = clz.getDeclaredFields();
for (Field field : fields) {
    System.out.println(field.getName());
}
```

