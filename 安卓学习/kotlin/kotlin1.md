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



**3.1.2**



### 3.2函数修饰符

Kotlin中有4种函数修饰符，分别是`public`、`private`、`protected`和 `internal`

- `public`：默认修饰符，表示对所有类都可见
- `private`：只对类内部可见
- `protected`：对当前类和子类可见
- `internal`：（摒弃了default）对同一模块中的类可见



### 3.3lambda

#### 3.3.1 maxBy

**最简单的lambda形式：**

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





#### 3.3.2map函数

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val newList = list.map { it.toUpperCase() }
for (fruit in newList) {
    println(fruit)
}
```



#### 3.3.3筛选元素

**filter**

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val newList = list.filter { it.length <= 5 }
				.map { it.toUpperCase() }
```



**any和all**

中any函数用于判断集合中是否至少存在一个元素满足指定条件，all函数用于判断集合中是否所有元素都满足指定条 件

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape", "Watermelon")
val anyResult = list.any { it.length <= 5 }
val allResult = list.all { it.length <= 5 }
println("anyResult is " + anyResult + ", allResult is " + allResult)
```



#### 3.3.4调用java函数时api

当kotlin调用一个方法，且该方法接收一个Java`单抽象方法接口（即只有一个方法的接口）`作为参数，就可以使用函数式API

```kotlin
//例如创建runnable接口匿名类实例，kotlin如下
Thread(object : Runnable {
 override fun run() {
 println("Thread is running")
 }
}).start()

//因为满足单抽象方法接口，可以使用函数时api
Thread(Runnable {
 println("Thread is running")
}).start()
```



### 3.4匿名类

创建匿名类实例的时候不再使用new，而是改用了`object`关键字

```kotlin
Thread(object : Runnable {
    override fun run() {
        println("Thread is running")
    }
}).start()
```





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

### 6.1构造函数

#### 主构造体函数

可以在类名后面直接指定主构造函数(默认构造函数)

```kotlin
class Student(val sno: String, val grade: Int) : Person() {
}
```

如果主构造函数有函数体，可以放在init里面

```kotlin
class Student(val sno: String, val grade: Int) : Person() {
    init {
        println("sno is " + sno)
        println("grade is " + grade)
    }
}
```

如果父类主体构造函数有参数

```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int) ://这里参数不能声明为val/var，否则作为子类属性,会和父类重名
Person(name, age) {
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



#### 次构造体函数

次构造函数是通过constructor关键字来定义的。

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





### 6.3 接口

**与java类似，类支持多继承接口**

```kotlin
interface Study {
    fun readBooks()
    fun doHomework()
}
```

和java一样，接口中的方法可以有默认实现

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
//object关键字
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

