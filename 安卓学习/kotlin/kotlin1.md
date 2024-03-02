

> [官方文档](https://kotlinlang.org/docs/getting-started.html)

# 一、基础语法

## 1.赋值

### 1.1自动推导

Kotlin中定义一个变量，只允许在变量前声明两种关键字：val和var。

- `val`（value的简写）：用来声明一个不可变的变量，这种变量在初始赋值之后就再也不能重新赋值，对应Java中的final变量。
- `var`（variable的简写）用来声明一个可变的变量，这种变量在初始赋值之后仍然可以再被重新赋值，对应Java中的非final变量。

### 1.2 显式推导

```kotlin
var a : Int = 10
```



## 2.数据类型

Kotlin中数据类型全部是`对象数据类型`

| Java    | Kotlin  |
| ------- | ------- |
| int     | Int     |
| long    | Long    |
| short   | Short   |
| float   | Float   |
| double  | Double  |
| boolean | Boolean |
| char    | Char    |
| byte    | Byte    |
| void    | Unit    |

```kotlin
fun main() {
    var a: Int = 10
    a = a * 10
    println("a = " + a)
}
```



### 2.1Number

Number类是一个抽象类，Int、Float、Long、Double都是子类



### 2.2类型匹配

通过`is`进行类型匹配



### 2.3序列

#### 升序

**双端闭区间**

```kotlin
val a  = 0..10 //创建一个[0,10]的序列
```

**单端闭区间**

```kotlin
val range = 0 until 10 //创建一个[0,10)的区间
```

#### 降序

```kotlin
 val a = 10 downTo 1//创建一个[10,1]的降序区间
```





### 2.4集合



#### 2.4.1 list和set

初始化集合方法

```kotlin
//传统方法
val list = ArrayList<String>()
list.add("Apple")
list.add("Banana")
list.add("Orange")
list.add("Pear")
list.add("Grape")
```



**使用listOf创建不可变集合**

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
```

**mutableListOf创建可变集合**

```kotlin
fun main() {
    val list = mutableListOf("Apple", "Banana", "Orange", "Pear", "Grape")
    list.add("Watermelon")
    for (fruit in list) {
        println(fruit)
    }
}
```

> **set同理**



#### 2.4.2 map

可以使用类似java的初始化方式

```kotlin
val map = HashMap<String, Int>()
map.put("Apple", 1)
map.put("Banana", 2)
map.put("Orange", 3)
map.put("Pear", 4)
map.put("Grape", 5)
```



**但是kotlin不推荐get和put方法进行读写，应该类似python的方式访问map**

```kotlin
map["Apple"] = 1//存数据
val apple = map["Apple"]//读
```



同list，map也有mapOf和mutableMapOf简化创建的代码

```kotlin
val map = mapOf("Apple" to 1, "Banana" to 2, "Orange" to 3, "Pear" to 4, "Grape" to 5)
//这里的to是一个infix函数，进行键值对关联
```

遍历

```kotlin
for ((fruit, number) in map) {
    println("fruit is " + fruit + ", number is " + number)
}
```







### 2.5 Unit / Nothing/Any

> https://www.jianshu.com/p/223e95ac41ef

#### **Unit**

 由于在Kotlin中，一切方法/函数都是表达式，表达式是总是有值的，所以每一个方法都必有一个返回值。如果没有用 `return` 明确的指定，那么一般来说就会用自动帮我们加上 `Unit`，等同于这样: 

```kotlin
fun returnUnit():Unit{
    return Unit
}
```



#### Nothing





#### Any

Any其实就跟Java里的Object是一样的，也就是说在Kotlin中Any取代了Java中的Object，成为了Kotlin中所有类的父类。**不过这个说法还不是很严谨**，因为`Any`是不可为空的，而Kotlin中还有一个 `Any?`，是指可空的 `Any` 。显而易见， `Any?` 是 `Any` 的父类，那么严格来说， `Any?` 是所有类的父类， `Any` 只是所有不可为空的类（也就是没有?）的父类。





## 3.函数

`fun`是定义函数的关键字，无论什么函数都必须声明`fun`

```kotlin
fun methodName(param1: Int, param2: Int): Int {
    return 0
}
```



**语法糖**

当函数只有一行代码，可以不必编写函数体，直接将代码赋值给函数

```kotlin
fun largerNumber(num1: Int, num2: Int) = max(num1, num2) //这里连函数返回值也省略，让虚拟机自动推导
```





### 3.1内置函数

**3.1.1 max()**









### 3.2标准库函数

**3.2.1 repeat**

 用于执行指定次数的循环操作 

```kotlin
repeat(times: Int, action: (Int) -> Unit)
```

例如：

```kotlin
repeat(5) { index ->
    println(index + 1)
}
```



### 3.3函数修饰符

Kotlin中有4种函数修饰符，分别是`public`、`private`、`protected`和 `internal`

- `public`：默认修饰符，表示对所有类都可见
- `private`：只对类内部可见
- `protected`：对当前类和子类可见
- `internal`：（摒弃了default）对同一模块中的类可见



### 3.4lambda

#### （1）maxBy

**简化lambda形式：**

查找长度最长的单词

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val maxLengthFruit = list.maxBy({ fruit: String -> fruit.length })
```

当Lambda参数是函数的最后一个参数时，可以将Lambda表达式移到函数括号的外面

```kotlin
val maxLengthFruit = list.maxBy() { fruit: String -> fruit.length }
```

如果Lambda参数是函数的唯一一个参数的话，还可以将函数的括号省略

```kotlin
val maxLengthFruit = list.maxBy { fruit: String -> fruit.length }
```

当**Lambda表达式的参数列表中只有一个参数**时，也不必声明参数名，而是可以使用`it`关键字来代替

```kotlin
val maxLengthFruit = list.maxBy { it.length }
```





#### （2）map函数

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val newList = list.map { it.toUpperCase() }
for (fruit in newList) {
    println(fruit)
}
```



#### （3）筛选元素

**filter**

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val newList = list.filter { it.length <= 5 }
				.map { it.toUpperCase() }
```



**any和all**

any函数用于判断集合中是否至少存在一个元素满足指定条件，all函数用于判断集合中是否所有元素都满足指定条 件

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val anyResult = list.any { it.length <= 5 }
val allResult = list.all { it.length <= 5 }
println("anyResult is " + anyResult + ", allResult is " + allResult)
```



#### （4）调用java函数时api

当kotlin调用一个方法，且该方法接收一个Java`单抽象方法接口（即只有一个方法的接口）`作为参数，就可以使用**函数式API**

```kotlin
//例如创建runnable接口匿名类实例，kotlin如下
Thread(object : Runnable {
    override fun run() {
        println("Thread is running")
    }
}).start()

//因为满足单抽象方法接口，可以使用函数式api
Thread(Runnable {
    println("Thread is running")
}).start()
```



### 3.5匿名类

**创建匿名类实例的时候不再使用new，而是改用了`object`关键字**

```kotlin
Thread(object : Runnable {
    override fun run() {
        println("Thread is running")
    }
}).start()
```





### 3.6 inline



## 4.条件判断

### if

kotlin中的if可以有返回值，**返回值就是if语句条件的最后一句话**

```kotlin
fun largerNumber(num1: Int, num2: Int): Int {
    val value = if (num1 > num2) {
        num1
    } else {
        num2
    }
    return value
}
```



### when

替代`switch`，且比switch更强大

```
匹配值 -> { 执行逻辑 }
```

例如要查询分数

```kotlin
//有参写法
fun getScore(name: String) = when (name) {
    "Tom" -> 86
    "Jim" -> 77
    "Jack" -> 95
    "Lily" -> 100
    else -> 0
}

//无参写法
fun getScore(name: String) = when  {
    name =="Tom" -> 86
    name =="Jim" -> 77
    name =="Jack" -> 95
    name =="Lily" -> 100
    else -> 0
}
```

还可以使用when实现类型判断

```kotlin
val a = 10.1 
fun checkNumber(a:Number) = when (a) {
    is Int -> println("number is Int")
    is Double -> println("number is Double")
    else -> println("number not support")
}
checkNumber(a)
```



## 5.循环

### 5.1 while

和java一致

### 5.2 for-in

kotlin舍弃了`for-i`循环，而是主要使用`for-in`

```kotlin
for(i in 0..10){//0..10创建序列[0,10]
    print(i)
}
```

使用step设置步长

```kotlin
fun main() {
    for (i in 0 until 10 step 2) {
        println(i)
    }
}
```



## 6.面向对象

> [kotlin中的构造函数](https://juejin.cn/post/6844903872016678919)

### 6.1构造函数

#### 主构造体函数

可以在类名后面直接指定主构造函数(默认构造函数)

```kotlin
class Student(val sno: String, val grade: Int)  {
}
```

如果主构造函数有函数体，可以放在init里面

```kotlin
class Student(val sno: String, val grade: Int) {
    init {
        println("sno is " + sno)
        println("grade is " + grade)
    }
}
```



#### 次构造体函数

次构造函数是通过`constructor`关键字来定义的。

**当一个类既有主构造函数又有次构造函数时，所有的次构造函数都必须调用主构造函数（包括间接调用）**

```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int) :
Person(name, age) {
    constructor(name: String, age: Int) : this("", 0, name, age) {//次构造体函数调用主构造体函数函数
    }
    constructor() : this("", 0) {//调用上一个此构造函数，间接调用主构造体函数
    }
}
```





### 6.2继承

**与java类似，类只支持单继承类**

kotlin中类默认无法被继承，默认都为非抽象类，因此需要加上关键字`open`才可以使得非抽象类能被继承

```kotlin
open class Person {
    ...
}
```

**继承的时候也要显示说明调用父亲的主题构造函数，也就是`Person()`，实现super功能**

```kotlin
class Student(val sno: String, val grade: Int) : Person() {
}
```

如果父类主体构造函数有参数

```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int) : Person(name, age) {//这里参数不能声明为val/var，否则作为子类属性,会和父类重名
    ...
}
```

如果没有主构造函数，那么次构造函数需要通过`super`调用父类的构造函数

```kotlin
class Student : Person {//因为没有主构造函数，因此继承时候也不用加括号
    constructor(name: String, age: Int) : super(name, age) {
    }
}
```



### 6.3 接口

**与java类似，类支持多继承接口**

```kotlin
interface Study {
    fun readBooks()
    fun doHomework()
}
```

和java(1.8开始)一样，接口中的方法可以有默认实现

```kotlin
interface Study {
    fun readBooks()
    fun doHomework() {//默认实现
        println("do homework default implementation.")
    }
}
```





### 6.4数据类

java中如果实现一个entity一般要重写`toString`、`equals`、`hashCode`等方法。**在kotlin中，直接在类前声明`data`关键字即可**，免去代码量

```kotlin
data class Cellphone(val brand: String, val price: Double)
```



### 6.5单例类

不像java中创建单例模式那么繁琐，kotlin提供了`object`关键字可以创建单例类

```kotlin
//object关键字替换掉class
object Singleton {
    fun singletonTest() {
        println("singletonTest is called.")
    }
}
```

调用单例

```kotlin
Singleton.singletonTest()
```





## 7.空指针检查

### 7.1可空类型系统

kotlin的变量默认不为空，因此在编译时就会判断变量是否为空并报错

```kotlin
fun  main(args: Array<String>) {
   n(null)//报错：error: null can not be a value of a non-null type Int
}
private fun n( a:Int){
    print("我是n")
}
```



为了应对开发中的特殊情况，可以在变量名后面加上问号，表示改变量可空

```kotlin
private fun n( a:Int?){
    print("我是n")
}
```



### 7.2判空的辅助工具

#### **`?.`**

当对象不为空时正常调用相应的方法，当对象为空时则什么都不做。比如以下的判空处理代码：

```kotlin
//例如下面判空之后调用something方法可以简化为
if (a != null) {
    a.doSomething()
}

//使用判空辅助工具
a?.doSomething()
```



#### **`?:`**

若**左边表达式**不为空取左边表达式，否则取右边表达式

```kotlin
val c = if (a ! = null) {
    a
} else {
    b
}
```

```kotlin
//简化为
val c = a ?: b
```



**结合使用**

```kotlin
fun getTextLength(text: String?): Int {
    if (text != null) {
        return text.length
    }
    return 0
}
```

通过`?.`和`?:`简化

```kotlin
fun getTextLength(text: String?): Int {
    return text?.length ?: 0
}
```



#### `!!`

让一个可能为null的对象强行通过编译，如果运行时候null为空，会抛出异常

```kotlin
val name: String? = null
val upperCaseName = name!!.toUpperCase() // 不会编译错误，但可能会抛出 NullPointerException 异常
```





#### let

let是一个标准函数，提供了函数式API的接口，并将原始调用对象作为参数传递到lambda表达式中

```kotlin
obj.let { obj2 ->//这里的obj2和obj是一个对象，只是为了区分
 // 编写具体的业务逻辑
}
```



因此结合`?.`简化判空条件可以写为

```kotlin
fun doStudy(study: Study?) {
    study?.let { stu ->
                stu.readBooks()
                stu.doHomework()
    }
}
```



**并且最重要的一点，let可以处理全局变量的判空，而if语句不可以**

```kotlin
var stu:Study = Study()
private fun n(){
    if(stu != null){//报错
        stu.showName()
    }
}
```

**改用使用let判断则没有问题**

```kotlin
var stu:Study = Study()

private fun n(){
    stu?.let{ob->ob.showName()}
}
```



## 8.协程 Coroutines 

> [协程](https://juejin.cn/post/6908271959381901325)

协程是一种轻量级的线程。协程是运行于线程上的，一个线程可以运行多个（几千上万个）协程。Java的线程需要依靠操作系统的调度才能实现不同线程之间的切换，而协程可以仅在编程语言的层面就能实现不同协程间的切换，从而大大提升并发编程效率。

协程允许在**单线程模式**下模拟多线程编程的效果，**代码执行时的挂起与恢复完全是由编程语言来控制的**，和操作系统无关

 

> 使用协程需要单独引入依赖
>
> ```groovy
> dependencies {
>        implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.5.2'
>        implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.5.2'
> }
> ```
>

### 8.1创建协程

#### CoroutineScope

 CoroutineScope 即 **协程作用域**，用于对协程进行追踪。如果我们启动了多个协程但是没有一个可以对其进行统一管理的途径的话，就会导致我们的代码臃肿杂乱，甚至发生**内存泄露**或者**任务泄露**。 

CoroutineScope 大体上可以分为三种：

- GlobalScope。即全局协程作用域，在这个范围内启动的协程可以一直运行直到应用停止运行。GlobalScope 本身不会阻塞当前线程，且启动的协程相当于守护线程，不会阻止 JVM 结束运行
- runBlocking。一个顶层函数，和 GlobalScope 不一样，它会阻塞当前线程直到其内部所有相同作用域的协程执行结束
- 自定义 CoroutineScope。可用于实现主动控制协程的生命周期范围，对于 Android 开发来说最大意义之一就是可以在 Activity、Fragment、ViewModel 等具有生命周期的对象中按需取消所有协程任务，从而确保生命周期安全，避免内存泄露





**（1）`GlobelScope.launch`开启一个顶级协程（协程也层级关系，协程还可以创建子协程）**，这种协程会随着应用结束而结束

```kotlin
GlobalScope.launch {
    println("codes run in coroutine scope")
}
```



**（2）`runBlocking`开启一个协程作用域，会保证协程作用域内的所有子协程和代码执行完之前一直阻塞当前线程**

> 只建议测试时候使用，正式环境会引起某些问题

```kotlin
runBlocking {
    println("codes run in coroutine scope")
    delay(1500)
    println("codes run in coroutine scope finished")
}
```



**（3）`coroutineScope`开启一个协程作用域，只会阻塞当前协程，既不影响其他协程，也不影响任何线程。因此不会造成性能问题，推荐使用！！**

**（4）创建子协程**

`launch`方法当前协程的作用域下创建子协程。子协程的特点是如果外层作用域的协程结束了，该作用域下的所有子协程也会一同结束。相比而言，`GlobalScope.launch`函数创建的永远是顶层协程

```kotlin
fun main() {
    runBlocking {
        launch {//创建子协程
            println("launch1")
            delay(1000)
            println("launch1 finished")
        }
        launch {
            println("launch2")
            delay(1000)
            println("launch2 finished")
        }
    }
}
```



### 8.2阻塞协程

**delay()**

`delay()`函数是一个**非阻塞式**的挂起函数，它**只会挂起当前`协程`**，并不会影响其他协程的运行，**delay函数只能在协程作用域或其他挂起函数中调用**

> 而Thread.sleep()方法会**阻塞当前的`线程`**，这样运行在该线程下的所有协程都会被阻塞

```kotlin
GlobalScope.launch {
    println("codes run in coroutine scope")
    delay(1500)//阻塞协程1.5s
    println("codes run in coroutine scope finished")
}
Thread.sleep(1000)//阻塞线程1s，会阻塞该线程下所有协程
```









### 3.suspend 

> [suspend原理](https://blog.csdn.net/future234/article/details/122248766)

Kotlin提供了一个`suspend`关键字，使用它可以将**任意函数声明成挂起函数**，而**挂起函数之间都是可以互相调用的（ suspend 函数只能由其它 suspend 函数调用，或者是由协程来调用 ）**，如下所示： 

```kotlin
suspend fun printDot() {//挂起函数
    println(".")
    delay(1000)//函数声明为suspend之后就可以在内部调用delay
}
```



**但是仍然存在问题：函数不具备协程作用域**，因此需要通过`coroutineScope`函数来解决。coroutineScope函数也是一个挂起函数。它的特点是会继承外部的协程的作用域并创建一个子协程，借助这个特性，就可以给任意挂起函数提供协程作用域了

```kotlin
suspend fun printDot() = coroutineScope {
    launch {
        println(".")
        delay(1000)
    }
}
```





### 4.取消协程

`scope.cancel()`



## 9.异常

> https://kotlinlang.org/docs/exceptions.html#checked-exceptions



## 新特性

### 字符串内嵌

避免使用`+`进行字符串的拼接

```kotlin
class Obj(var name:String){
    
}

fun main(){
    var obj = Obj("kevin")
    var str = "hello, ${obj.name}. nice to meet you!"//可以直接得到obj.name
}
```



**在某些情况非常简便**

```kotlin
val brand = "Samsung"
val price = 1299.99
println("Cellphone(brand=$brand, price=$price)")
```





### 默认参数

```kotlin
fun printParams(num: Int, str: String = "hello") {
    println("num is $num , str is $str")
}
```



如果默认参数是第一个的情况，该如何避免第二个参数传递给第一个参数

```kotlin
fun printParams(num: Int = 100, str: String) {
    println("num is $num , str is $str")
}
fun main() {
    printParams(str = "world")//通过键值对的形式传递参数，这样避免传递给第一个参数，造成类型不匹配
}
```







### object关键字

> [object关键字的使用场景](https://blog.csdn.net/xlh1191860939/article/details/79460601)

object主要有以下三种使用场景：

- **对象声明**（Object Declaration）
- **伴生对象**（Companion Object）
- **对象表达式**（Object Expression）

#### (1)对象声明

语法含义：将类的声明和定义该类的单例对象结合在一起（即通过object就实现了单例模式） 

即将class关键字替换为object关键字，来声明一个类，与此同时也声明它的一个对象。只要编写这么多代码，这个类就已经是单例的了。 

```kotlin
object RepositoryManager{
    fun method(){
        println("I'm in object declaration")
    }
}
```



> **注意点：**
>
> - 尽管和普通类的声明一样，可以包含属性、方法、初始化代码块以及可以继承其他类或者实现某个接口，但是它**不能包含构造器（包括主构造器以及次级构造器）**
> - 它也可以定义在一个类的内部：
>
> ```kotlin
> class ObjectOuter {
>     object Inner{
>         fun method(){
>             println("I'm in inner class")
>         }
>     }
> }
> fun main(args: Array<String>) {
>     ObjectOuter.Inner.method()
> }
> ```



#### (2)伴生对象（Companion object）

在Kotlin中是**没有static关键字**的，也就是意味着没有了静态方法和静态成员。那么在kotlin中如果要想表示这种概念，取而代之的是`包级别函数（package-level function）`和这里提到的`伴生对象`。

```kotlin
class A{
    companion object 伴生对象名(可以省略){
        //define method and field here
    }
}
```

> **示例**
>
> ```kotlin
> class ObjectTest {
>     companion object MyObjec{
>         val a = 20
>         fun method() {
>             println("I'm in companion object")
>         }
>     }
> }
> ```
>
> **调用**
>
> ```kotlin
> fun main(args: Array<String>) {
>     //方式一
>     ObjectTest.MyObject.method()
>     println(ObjectTest.MyObject.a)
> 
>     //方式二（推荐方式）
>     ObjectTest.method()
>     println(ObjectTest.a)
> }
> ```
>
> > 在定义（定义时如果省略了伴生对象名，那么编译器会为其提供默认的名字Companion）和调用时伴生对象名是可以省略的。 
> >
> > **所以一个类只允许有一个伴生对象，因为可以省略伴生对象名**

#### (3) **对象表达式（Object Expression）** 

在Java中，有匿名内部类

```java
interface Contents {
    void absMethod();
}
public class Hello {

    public Contents contents() {
        return new Contents() {

            @Override
            public void absMethod() {
                System.out.println("method invoked...");
            }
        };
    }

    public static void main(String[] args) {

        Hello hello = new Hello();
        hello.contents().absMethod();    //打印method invoked...
    }
}
```

但是如果在匿名内部类中新添加了一些属性和方法，那么在外界是无法调用的，并且一个匿名内部类最多只可以实现一个接口或类

```java
return new Contents() {
    private int i = 1;

    public int value() {
        return i;
    }

    @Override
    public void absMethod() {
        System.out.println("method invoked...");
    }
};

public static void main(String[] args) {

    Hello hello = new Hello();

    hello.contents().value();  //Cannot resolve method 'value()'
}
```



**而在Kotlin中，则可以访问匿名内部类的属性**

 a.实现一个接口或类 

```kotlin
interface AA {
    fun a()
}

fun main(args: Array<String>) {

    val aa = object : AA {
        override fun a() {
            println("a invoked")
        }
    }

    aa.a()
}
```

b.不实现任何类，并且在匿名内部类中添加方法

```kotlin
fun main(args: Array<String>) {

    val obj = object  {
        fun a() {
            println("a invoked")
        }
    }
    obj.a()  //打印：a invoked
}
```

c.实现多个接口和类

```kotlin
fun main(args: Array<String>) {
    val cc = object : AA, BB() {
        override fun a() {
        }
        override fun b() {
        }
    }
    cc.a()
    cc.b()
}
```



**注意：匿名对象只有定义成`局部变量`或`private变量`时候才有意义，否则编译不通过**

```kotlin
class MyTest {

    private val foo = object {//private变量
        fun method() {
            println("private")
        }
    }

    val foo2 = object {//全局变量
        fun method() {
            println("public")
        }
    }

    fun m() = object {//public函数
        fun method(){
            println("method")
        }
    }

    fun invoke(){

        val local = object {//局部变量
            fun method(){
                println("local")
            }
        }

        local.method()  //编译通过
        foo.method()    //编译通过
        foo2.method()   //编译通不过
        m().method()    //编译通不过
    }
}

```





#### 三者初始化时机

a. `object declaration`：当第一次访问它时才初始化，是一种懒初始化

b. `Companion object`：当它对应的类被加载后，它才初始化，类似Java中的静态代码块

c. `object expression`：一旦它被执行，立马初始化