# 一、基础

## 1.数据类型

### (1)字符串

两种类型字符串表示形式：

- C风格字符串
- C++引入的string类型

**C风格字符串**

--------

 这种形式字符串实际上是使用 **null** 字符 **\0** 终止的一维字符数组。 

```c
char site[7] = {'R', 'U', 'N', 'O', 'O', 'B', '\0'};
//或者写成（不需要手动把\0放到末尾，编译器初始化数组时候自动加上）
char site[] = "RUNOOB";
```



**可以用指针指向字符数组，形成字符串指针**

```c++
char *p = "hello,world!";

//访问元素
char c = *p;// 访问字符数组中的第一个元素
char c = *(p+1);// 访问字符数组中的第二个元素
char c = *(p + strlen(p) - 1);// 访问字符数组中的最后一个元素
```





**string类**

-----

C++标准库`string`类型

```c++
#include <iostream>
#include <string>
 
using namespace std;
 
int main ()
{
   string str1 = "runoob";
   string str2 = "google";
   string str3;
   int  len ;
 
   // 复制 str1 到 str3
   str3 = str1;
   cout << "str3 : " << str3 << endl;
 
   // 连接 str1 和 str2
   str3 = str1 + str2;
   cout << "str1 + str2 : " << str3 << endl;
 
   // 连接后，str3 的总长度
   len = str3.size();
   cout << "str3.size() :  " << len << endl;
 
   return 0;
}
```





### (2)auto

 auto是一个关键字，可以用于声明变量。auto变量的类型由初始化表达式来推断。 

```c++
#include <iostream>
using namespace std;

int main() {
    // 声明一个int类型的auto变量
    auto x = 10;
    // 声明一个std::string类型的auto变量
    auto y = "Hello, world!";
    
    cout << x << endl;
    cout << y << endl;

    return 0;
}
```



## 2.io

### (1)控制台io

- `cin>>`:输入
- `cout<<`输出

> `<<`和`>>`都是操作符重载



## 3.指针运算符

> [指针运算符](https://www.runoob.com/cplusplus/cpp-pointer-operators.html)

### (1)取地址运算符 `&`

c++中推荐使用引用`&`

 `&` 是一元运算符，返回操作数的内存地址。 

### (2)间接寻址运算符 `*`

`*`是一元运算符，返回操作数所指定地址的变量的值。 

### (3)两种运算符操作总结

| 用法                                  | 说明                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| 声明时:基本类型 **`&`**               | 声明**引用变量**。它是某个已存在变量的**别名**，即该引用变量名称与原始变量名称都代表同一个变量。 |
| 声明时:基本类型 **`*`**               | 声明**指针变量**。它的值是另一个变量的**地址**               |
| 声明时:基本类型 `**`                  | 声明**二级指针变量**。它的值是另一个一级"基本类型 *"指针变量的地址 (**指针的指针**)。 |
| 调用时:变量前加 "**&**"               | 使用**取地址运算符**获取该变量的**地址**                     |
| 调用时:指针变量前加 "*****"           | 使用间接寻址运算符获取该指针变量所指向的变量                 |
| 调用时:**二级**指针变量前加 "***\***" | 获取该二级指针变量所指向的指针所指向的变量                   |

```c++
#include <iostream>
using namespace std; 
int main(){
    int  var;     // 声明int类型变量var
    int * ptr;    // 声明指针变量ptr
    ptr = &var;   // 先使用 & 运算符获取变量var的地址，再把该地址赋值给指针变量ptr
    int ** pptr;  // 声明二级指针变量pptr
    pptr = &ptr;  // 先使用 & 运算符获取变量ptr的地址，再把该地址赋值给二级指针变量pptr
    int & ref1 = var;   // 声明引用变量ref1, ref1是变量var的别名(引用必须在创建时被初始化)
    int & ref2 = *ptr;  // 先使用*运算符获取指针变量ptr所指向的变量(即var)，再用该变量(var)初始化引用变量ref2(声明引用变量ref2的同时对它进行初始化)。也就是说，该行代码执行后，ref2也是变量var的别名 
    var = 20
}

Value of var: 20
Value of &var: 0x7ffce63490bc    (var的地址)

Value of ptr: 0x7ffce63490bc    (等于&var)
Value of *ptr: 20        (等于var)
Value of &ptr: 0x7ffce63490b0    (ptr的地址)

Value of pptr: 0x7ffce63490b0    (等于&ptr)
Value of *pptr: 0x7ffce63490bc    (等于ptr, 等于&var)
Value of **pptr: 20        (等于*ptr, 等于var)
Value of &pptr: 0x7ffce63490a8    (pptr的地址)

Value of ref1: 20        (等于var)
Value of &ref1: 0x7ffce63490bc    (等于&var)

Value of ref2: 20        (等于var)
Value of &ref2: 0x7ffce63490bc    (等于&var)
```



### (4)悬空指针

悬空指针的产生原因主要有以下几种：

- 指针指向的内存空间被释放，但指针本身没有被置为空。
- 指针指向的内存空间被释放，但指针被传递给了另一个函数或方法。
- 指针指向的内存空间被释放，但指针被用作函数或方法的返回值。

```c++
// 指针指向的内存空间被释放，但指针本身没有被置为空
int* p = new int(10);
delete p; // 释放内存空间,指针 p 仍然指向已经释放的内存空间

// 指针指向的内存空间被释放，但指针被传递给了另一个函数或方法
void foo(int* p) {
  delete p; // 释放内存空间
}
foo(p); // 指针 p 仍然指向已经释放的内存空间，报错
```



**避免办法：**

```c++
// 指针指向的内存空间被释放，但指针本身没有被置为空
int* p = new int(10);
delete p; // 释放内存空间,指针 p 仍然指向已经释放的内存空间
p = NULL;

// 指针指向的内存空间被释放，但指针被传递给了另一个函数或方法
void foo(int* p) {
  delete p; // 释放内存空间
}
foo(p); // 指针 p 仍然指向已经释放的内存空间，报错
```



### (5)常量指针

```c++
int num = 9;
int num2 = 10;
const int * numberP = &num;

*numberP = 100;//报错，不允许修改常量指针存放地址对应的值
numberP = &num2;//正确，运行重新指向常量指针存放的地址
```



### (6)指针常量

```c++
int num = 9;
int num2 = 10;
int *const numberP = &num;
*num = 100;//正确，运行修改指针常量存放地址对应的值
numberP = &num2;//错误，不允许重新指向指针常量存放的地址
```

### (7)常量指针常量

都不允许修改

```c++
const int * const numerP = &num;
```





## 4.引用

引用是变量的别名，特点是：

- 引用在创建时必须初始化
- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。(指针可以在任何时候指向到另一个对象)

```c++
int a = 10;
int &b = a;

cout<<"地址 "<<"a:"<<&a<<" b:"<<&b<<endl;
cout<<"值 "<<"a:"<<a<<" b:"<<b<<endl;

b = 99;

cout<<"地址 "<<"a:"<<&a<<" b:"<<&b<<endl;
cout<<"值 "<<"a:"<<a<<" b:"<<b<<endl;

//输出：
地址 a:0x61fe14 b:0x61fe14
值 a:10 b:10
地址 a:0x61fe14 b:0x61fe14
值 a:99 b:99
```

> 因此推荐使用引用进行变量数值交换
>
> ```c++
> void changeNumber(int &num1,int &num2){//参数为对变量的引用
>     int temp = 0;
>     temp = num1;
>     num1 = num2;
>     num2 = temp;
> }
> 
> int main(){
>     int a = 10;
>     int b = 99;
> 
>     cout<<"地址 "<<"a:"<<&a<<" b:"<<&b<<endl;
>     cout<<"值 "<<"a:"<<a<<" b:"<<b<<endl;
> 
>     changeNumber(a,b);
>     
>     cout<<"地址 "<<"a:"<<&a<<" b:"<<&b<<endl;
>     cout<<"值 "<<"a:"<<a<<" b:"<<b<<endl;
> }
> 
> //输出
> 地址 a:0x61fe1c b:0x61fe18
> 值 a:10 b:99
> 地址 a:0x61fe1c b:0x61fe18
> 值 a:99 b:10
> ```
>



### (1)常量引用

 常量引用是指不能通过引用来修改其所指向的对象的值。 

```c++
//格式
const type &ref_name = object_name;
```

最常见的用法是用在函数参数，这样变量不能被修改

```c++
void func(const int &a) {
  // 不能修改对象的值
  a = 20; // 编译错误
}

int main() {
  int a = 10;
  func(a); // 可以传入一个普通变量

  // 也可以传入一个常量对象
  const int b = 20;
  func(b); 
}
```

## 5.函数

### (1)默认形参

c++支持默认形参

```c++
void func(int a = 10,double a){
    
}
```

### (2)无名形参

 C++中，形参可以只指定类型，不指定名称。这种形参被称为“无名形参”或“占位符形参”。在函数定义中，可以使用类型名代替形参名来定义一个无名形参。 

无名形参有以下应用场景

> - 需要指定参数类型，但不需要实际参数情况
>
>   ```c++
>   int sum(int, int);
>   
>   int main() {
>     int a, b;
>     cin >> a >> b;
>   
>     // 调用函数，不传入任何实参
>     int result = sum(a, b);
>     cout << result << endl;
>   }
>   ```
>
>   
>
> - 定义函数模板
>
>   ```c++
>   template <typename T>
>   T sum(T a, T b) {
>     return a + b;
>   }
>   
>   int main() {
>     int a = 10;
>     int b = 20;
>   
>     // 调用函数，传入两个整数
>     int result = sum<int>(a, b);
>     cout << result << endl;
>   
>     // 调用函数，传入两个浮点数
>     float c = 1.5;
>     float d = 2.5;
>     float result2 = sum<float>(c, d);
>     cout << result2 << endl;
>   }
>   ```
>
>   

## 6.面向对象

**声明类：**

-----

![1695263869021](c++.assets/1695263869021.png)

头文件声明类及其成员变量和函数，不实现函数，**因为头文件被多个源文件包含，如果在头文件中实现函数，那么每个源文件都会包含相同的函数实现，这会导致代码膨胀和重复定义的错误。** 



**实现类的函数和成员变量:**

----------

![1695263963473](c++.assets/1695263963473.png)

创建`cpp`文件实现类的成员函数，`this`是类指针，指向这个类，可以用于访问成员函数和变量。



### (1)`.`和`->`

> 参考：
>
> [C++中的.和::和：和->的区别](https://blog.csdn.net/s15868887695/article/details/56834685)

- `A.B`则A为对象或者结构体；
- `A->B`则A为指针，->是成员提取，A->B是提取A中的成员B，A只能是指向类、结构、联合的指针；
- `::`是作用域运算符，A::B表示作用域A中的名称B，A可以是名字空间、类、结构；
- `：`一般用来表示继承；



**Example：**

 c++中当定义类对象是指针对象时候，就需要用到->指向类中的成员；当定义一般对象时候时就需要用到"."指向类中的成员。 

```c++
class A
{　　
    public play();
}

//如果定义如下
A *p //则使用：p->play(); 左边是结构指针。
A p //则使用：p.paly(); 左边是结构变量。

```

### (2)创建类对象

```c++
//方式一，创建类的对象
Tests *data = new Tests;
data->prints();
//方式二，创建类的对象
Tests().prints();
//方式三, 调用静态函数，不用创建类的对象
Tests::printss();
```

#### 开辟堆、栈空间

```c++
Student s1 ;//栈区开辟空间
    s1.setName("John");
    cout<<s1.getName()<<endl;

    /*堆区开辟空间*/
    Student *s2 = new Student();
    s2->setName("Mary");
    cout<<s2->getName()<<endl;
    delete s2;//必须手动释放堆空间
```



#### 聚合初始化器 

使用 聚合初始化器 初始化一个类对象

```c++
class Person {
public:
    int age;
    char* name;
};

int main() {
    Person person = {1, "hello"};

    std::cout << person.age << " " << person.name << std::endl;

    return 0;
}
```



### (3)构造函数

特殊构造函数写法

```c++
Student(char *name):name(name),age(age){
    cout<<"赋值给name"<<endl;
}

//等价于
Student(char *name){
    this->name = name;
    this->age = age;
}
```

又例如当前构造函数要调用多参数构造函数

```c++
//会先调用两个参数构造函数，再调用本构造函数
Student(char *name):Student(name,87){
    cout<<"赋值给name"<<endl;
}

Student(char *name,int age){
    this->name = name;
    this->age = age;
}
```



#### 拷贝构造函数

使用`=`会调用默认的拷贝构造函数

```c++
Student s1 = {"John"}; // 栈区开辟空间

Student s2; // 默认拷贝构造函数
s2 = s1;
cout << &s2 << endl;
cout << &s1 << endl;

输出：
0x61fe00
0x61fe08
```

**自定义拷贝构造函数，覆盖默认拷贝构造函数**

```c++
//自定义拷贝构造函数
Student(const Student &s){
    cout << "copy constructor" << endl;
    this->name = s.name;
}


//调用自定义拷贝构造函数
Student s1 = {"kevin"};
Student s2 = s1;

//调用默认拷贝构造函数
Student s3;
s3 = s1;
```

> **注意：**
>
> ```c++
> Student *st1 = new Student("jack",23);
> 
> Student *st2 = st1;//这个不会调用拷贝构造函数，这是st2指向st1的地址
> ```
>
> 



### (4)析构函数

析构函数用于释放对象的内存空间，调用`delete`时，析构函数必被调用，而调用`free`时，析构函数不会调用。**析构函数不具备任何参数**

```c++
~Student(){
    ....
}
```

> 例如当对象中为某个对象分配的内存空间
>
> ```c++
> Student(char *name){
>     this->name = (char *)(malloc(sizeof(char *) * 10)) ;
>     strycpy(this->name,name);
> }
> ```
>
> 此时应该再析构函数里释放内存
>
> ```c++
> ~Student(){
>     if(this->name){
>         free(this->name);
>         this->name = NULL;//执行NULL的地址，避免悬空指针
>     }
> }
> ```







## 7.命名空间

为了防止不同库下面的函数重名情况

```c++
include <iostream>
using namespace std;
 
// 第一个命名空间
namespace first_space{
   void func(){
      cout << "Inside first_space" << endl;
   }
}
// 第二个命名空间
namespace second_space{
   void func(){
      cout << "Inside second_space" << endl;
   }
}
int main ()
{
 
   // 调用第一个命名空间中的函数
   first_space::func();
   
   // 调用第二个命名空间中的函数
   second_space::func(); 
 
   return 0;
}
```

### 嵌套

```c++
namespace namespace_name1 {
   // 代码声明
   namespace namespace_name2 {
      // 代码声明
   }
}

// 访问 namespace_name2 中的成员
using namespace namespace_name1::namespace_name2;
 
// 访问 namespace_name1 中的成员
using namespace namespace_name1;
```

### 不连续的命名空间

命名空间可以放在不同文件下面。

因此下面的命名空间定义可以是定义一个新的命名空间，也可以是为已有的命名空间增加新的元素： 

```c++
namespace namespace_name {
    // 代码声明
}
```

