## java归纳

### 一、二章

1.命名规则：①类名：单词首字母大写

​					  ②方法、变量名：必须是字母、“_”或$开头（可以包含数字），采用驼峰命名法

​                      ③常量：都为大写，单词间以下划线隔开



2.数据类型：

```java
//自动转换，由低到高
byte,short,char—> int —> long—> float —> double 

//强制转换，高到底
去掉高位，低位转换成原码
```



3.转义字符

\uxxxx能够表示任何的char字符

（中文的字符表示范围：\u4e00--\u9fa5）



4.for(变量：数组/集合),增强循环



5.一个对象内存：①对象头：12B

​							②内存

​							③hash 4B（操作数栈宽度）

​							④补齐字节，最终要为8的倍数

6.引用类型比较：

```java
一、复合数据类型比较时：==，equals( )方法。
1.==
  如果作用于基本数据类型的变量，则直接比较其存储的“值”是否相等；
  如果作用于引用类型的变量，则比较的是所指向的对象的地址。
2.equals( )方法比较时：默认使用Object类的equals( )方法，比较的是两个引用是否指向同一个对象

二、String类对象相等比较
String类重写了Object类的equals方法，equals( )比较字符串的值是否相等
==比较的是两个对象地址是否相等

         String str1 = new String() ; 
		String str2 =""; 
		//boolean b1 = str1 == str2;
		System.out.println (str1 == str2); //输出false 
		System.out.println (str1.equals (str2));//输出true

		String str1=null; 
		String str2=null; 
		System.out.println (str1 == str2);//输出true
		System.out.println (str1.equals (str2));//抛出空指针异常
```



<img src="D:\typora\图片\1608602986925.png" alt="1608602986925" style="zoom:67%;" />

7.常见对象内存结构

![1608604771811](D:\typora\图片\1608604771811.png)

### **没有操作数**

![1608604801735](D:\typora\图片\1608604801735.png)



8.append方法：不用每次重新创建一个对象

StringBuffer线程安全

StringBuilder线程不安全



9.静态变量：

- 它们仅能调用其他的static 方法。

- 它们只能访问static数据。

- 静态类

  ![1608619543509](D:\typora\图片\1608619543509.png)



10.父类与子类的类型转换

```java
class parent {
}
class chiled extends parent{
}

parent p = new child();//创建child类型对象，自动强制转换为parent，
//父类不可调用子类的方法、成员变量

```



11.接口：

类可以多继承接口；接口中的变量都是常量（final）修饰，方法都是抽象方法



12.final关键字：①变量->常量；②方法->不能被覆盖；③类->不能被继承



13.IO

通过创建File对象来获取文件元信息

```java
public String getName() 返回由此抽象路径名表示的文件或目录的名称。

public boolean exists() 测试此抽象路径名表示的文件或目录是否存在。

public boolean isDirectory() 测试此抽象路径名表示的文件是否是一个目录。

public boolean isFile() 测试此抽象路径名表示的文件是否是一个标准文件。

public boolean delete() 删除此抽象路径名表示的文件或目录。

public String[] list() 返回由此抽象路径名所表示的目录中的文件和目录的名称所组成字符串数组。

public File[] listFiles() 返回一个抽象路径名数组，这些路径名表示此抽象路径名所表示目录中的文件。

public boolean mkdir() 创建此抽象路径名指定的目录。

public boolean mkdirs() 创建此抽象路径名指定的目录，包括创建必需但不存在的父目录。
```



14.Graphics类成员成员函数

```java
//画线
drawLine()方法在窗口画一条线段
drawLine(int x1,int y1,int x2,int y2)
例:
g.drawLine(3,3,50,50);//画一条线段
g.drawLine(100,100,100,100);//画一个点。

//画矩形
有两种矩形：普通型和圆角型。
(1) 画普通矩形有两个方法：
drawRect(int x,int y,int width,int height)：
其中参数x和y指定左上角的位置，参数width和height是矩形的宽和高。
fillRect(int x,int y,int width,int height)：
是用预定的颜色填充一个矩形，得到一个着色的矩形块。
例:
g.drawRect(80,100,40,25);//画线框
g.setColor(Color.yellow);g.fillRect(20,70,20,30);//画着色块
(2)画圆角矩形也有两个方法：
drawRoundRect(int x,int y,int width, int height, int arcWidth, int arcHeight)：
是用线围起来的圆角矩形。其中参数x和y指定矩形左上角的位置；参数width和heigth是矩形的宽和高；
arcWidth和arcHeight分别是圆角弧的横向直径和圆角弧的纵向直径。
fillRoundRect(int x,int y,int width,int height,int arcWidth,int archeight)：
是用预定的颜色填充的圆角矩形。各参数的意义同前一个方法。
例:
g.drawRoundRect(10,10,150,70,40,25);//画一个圆角矩形
g.setColor(Color.blue); g.fillRoundRect(80,100,100,100,60,40);//涂一个圆角矩形块
g.drawRoundRect(10,150,40,40,40,40);//画圆
g.setColor(Color.red); g.fillRoundRect(80,100,100,100,100,100);//画圆块

//画椭圆形
椭圆形由椭圆的横轴和纵轴确定。画椭圆形有两个方法：
drawOval(int x,int y,int width,int height)：
是画用线围成的椭圆形。其中参数x和参数y指定椭圆形左上角的位置，参数width和height是横轴和纵轴。
fillOval(int x,int y,int width,int height)：
是用预定的颜色填充的椭圆形，是一个着色块。也可以用画椭圆形方法画圆形，
当横轴和纵轴相等时，所画的椭圆形即为圆形。
例:
g.drawOval(10,10,60,120);//画椭圆
g.setColor(Color.cyan);g.fillOval(100,30,60,60);//涂圆块
g.setColor(Color.magenta);g.fillOval(15,140,100,50);//涂椭圆

//画圆弧
画圆弧有两个方法：
drawArc(int x,int y,int width,int height,int startAngle, int arcAngle)：
画椭圆一部分的圆弧线。椭圆的中心是它的外接矩形的中心，
其中参数是外接矩形的左上角坐标(x,y)，宽是width，高是heigh。
参数startAngle的单位是 “度”，起始角度0度是指3点钟方位.
参数startAngle和arcAngle表示从startAngle角度开始，逆时针方向画arcAngle度的弧，
约定，正值度数是逆时针方向，负值度数是顺时针方向
。
fillArc(int x,int y,int width, int height, int startAngle, int arcAngle)：
用setColor()方法设定的颜色,画着色椭圆的一部分。
例:
g.drawArc(10,40,90,50,0,180);//画圆弧线
g.drawArc(100,40,90,50,180,180);//画圆弧线
g.setColor(Color.yellow); g.fillArc(10,100,40,40,0,-270);//填充缺右上角的四分之三的椭圆
g.setColor(Color.green); g.fillArc(60,110,110,60,-90,-270);//填充缺左下角的四分之三的椭圆

//显示字符串
public abstract void drawString(String str, int x, int y)
使用此图形上下文的当前字体和颜色绘制由指定 string 给定的文本。
最左侧字符的基线位于此图形上下文坐标系的 (x, y) 位置处。
参数：
str - 要绘制的 string。
x - x 坐标。
y - y 坐标。
例:
g.drawString("中华人民共和国",100,200);

//显示图片
ImageIcon ii = new ImageIcon("图片.jpg");
Image img = ii.getImage();//获取图片对象给Image
g.drawImage(img,int x,int y,int width,int height,this);//设定图片位置，宽高
```



15.事件处理三要素

①事件源

②事件对象

③监听器



16.线程

创建线程两个方法：

①继承thread类

```java
class Mythread extends Thread{
	public void run(){
	      ......
	}
}

Mythread m = new Mythread();
m.start();
```



②实现Runnable接口

```java
class Mythread implements Runnable{
	public void run(){
		........
	}
}

Mythread m = new Mythread();//创建对象
Thread t = new Thread(m);//转换成thread类型
t.start;
```



17.集合



![1608975996993](D:\typora\图片\1608975996993.png)

![1608976756631](D:\typora\图片\1608976756631.png)

![1608976803105](D:\typora\图片\1608976803105.png)

![1608980869619](D:\typora\图片\1608980869619.png)



![1608981020050](D:\typora\图片\1608981020050.png)

![1608981075936](D:\typora\图片\1608981075936.png)

![1608981095641](D:\typora\图片\1608981095641.png)

![1608981119801](D:\typora\图片\1608981119801.png)