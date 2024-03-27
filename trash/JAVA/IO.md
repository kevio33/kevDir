# 1.流的作用和概念

**流**：代表任何有能力产出数据的数据源对象或者是有能力接受数据的接收端对象

**流的本质**:**数据传输**，根据数据传输特性将流抽象为各种类，方便更直观的进行数据操作。 

**作用：为数据源和目的地建立一个输送通道**

# 2.IO采用模型

Java的IO模型设计非常优秀，它使用Decorator(装饰者)模式，按功能划分Stream，您可以动态装配这些Stream，以便获得您需要的功能。

```java
例如，您需要一个具有缓冲的文件输入流，则应当组合使用FileInputStream和BufferedInputStream。
```

# 3.IO流的分类

![image-20210503193819438](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210503193819438.png)

## 	3.1按流方向:

- 输入流：从别的地方(本地文件，网络上的资源等)获取资源 输入到 我们的程序中
- 输出流：从我们的程序中 输出到 别的地方(本地文件)， 将一个字符串保存到本地文件中，就需要使用输出流。

![image-20210503194017177](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210503194017177.png)

## 	3.2按处理数据单位不同:

1字符 = 2字节 、 1字节(byte) = 8位(bit) 、 一个汉字占两个字节长度

- 字节流：每次读取(写出)一个字节，当传输的资源文件有中文时，就会出现乱码
- 字符流：每次读取(写出)两个字节，有中文时，使用该流就可以正确传输显示中文。

> **字符流的由来：** Java中字符是采用Unicode标准，一个字符是16位，即一个字符使用两个字节来表示。为此，JAVA中引入了处理字符的流。因为数据编码的不同，而有了对字符进行高效操作的流对象。本质其实就是基于字节流读取时，去查了指定的码表。

## 3.3按照功能不同:

  - **节点流**：从或向一个特定的地方（节点）读写数据。即节点流是直接操作文件，网络等的流，例如`FileInputStream`和`FileOutputStream`，他们**直接从文件中读取或往文件中写入字节流。**

  ![image-20210503194248160](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210503194248160.png)

  - **处理流**：是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。如`BufferedReader`。处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装

![image-20210503194401168](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20210503194401168.png)



# 4.四个基本抽象流类型

|        | 输入流      | 输出流       |
| ------ | ----------- | ------------ |
| 字节流 | InputStream | OutputStream |
| 字符流 | Reader      | Writer       |

## 如何选择:

```java
1、首先自己要知道是选择输入流还是输出流，这就要根据自己的情况而定，如果你想从程序写东西到别的地方，那么就选择输出流，反之用输入流

2、然后考虑传输数据时，是选择使用字节流传输还是字符流，也就是每次传1个字节还是2个字节，有中文肯定就选择字符流了。（详情见1.8）

3、前面两步就可以选出一个合适的节点流了，比如字节输入流inputStream，如果要在此基础上增强功能，那么就在处理流中选择一个合适的即可。
```



## IO流常用到的五类一接口

在整个Java.io包中最重要的就是5个类和一个接口。5个类指的是`File、OutputStream、InputStream、Writer、Reader`；一个接口指的是`Serializable`

1. `File（文件特征与管理）`：**File类是对文件系统中文件以及文件夹进行封装的对象**，可以通过对象来操作文件和文件夹。 **File类保存文件或目录的各种元数据信息**，包括文件名、文件长度、最后修改时间、是否可读、获取当前文件的路径名，判断指定文件是否存在、获得当前目录中的文件列表，创建、删除文件和目录等方法。 
2. `InputStream（二进制格式操作）`：抽象类，基于字节的输入操作，是所有输入流的父类。定义了所有输入流都具有的共同特征。
3. `OutputStream（二进制格式操作）`：抽象类。基于字节的输出操作。是所有输出流的父类。定义了所有输出流都具有的共同特征。
4. `Reader（文件格式操作）`：抽象类，基于字符的输入操作。
5. `Writer（文件格式操作）`：抽象类，基于字符的输出操作。
6.  `RandomAccessFile（随机文件操作）`：一个独立的类，直接继承至Object，可以从文件的任意位置进行存取（输入输出）操作。

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/1186236-20170628084745102-439177264.png)



## 具体的IO流方法分析

### 	(1)输入字节流InputStream

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/874710-20170530112251336-478728150.png)

>     ByteArrayInputStream：字节数组输入流，该类的功能就是从字节数组(byte[])中进行以字节为单位的读取，也就是将资源文件都以字节的形式存入到该类中的字节数组中去，我们拿也是从这个字节数组中拿
>     
>     PipedInputStream：管道字节输入流，它和PipedOutputStream一起使用，能实现多线程间的管道通信
>     
>     FilterInputStream ：装饰者模式中处于装饰者，具体的装饰者都要继承它，所以在该类的子类下都是用来装饰别的流的，也就是处理类。具体装饰者模式在下面会讲解到，到时就明白了
>     
>     BufferedInputStream：缓冲流，对处理流进行装饰，增强，内部会有一个缓存区，用来存放字节，每次都是将缓存区存满然后发送，而不是一个字节或两个字节这样发送。效率更高
>     
>     DataInputStream：数据输入流，它是用来装饰其它输入流，它“允许应用程序以与机器无关方式从底层输入流中读取基本 Java 数据类型”
>     
>     FileInputSream：文件输入流。它通常用于对文件进行读取操作
>     
>     File：对指定目录的文件进行操作。注意，该类虽然是在IO包下，但是并不继承自四大基础类。
>     
>     ObjectInputStream：对象输入流，用来提供对“基本数据或对象”的持久存储。通俗点讲，也就是能直接传输对象（反序列化中使用）

### 	(2)输出字节流OutputStream

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/874710-20170530121318274-160516931.png)

> - `OutputStream` 是所有的输出字节流的父类，它是一个抽象类。
> - `ByteArrayOutputStream`、`FileOutputStream` 是两种基本的介质流，它们分别向Byte 数组、和本地文件中写入数据。
> - `PipedOutputStream `是向与其它线程共用的管道中写入数据，
> - `ObjectOutputStream` 和所有`FilterOutputStream` 的子类都是装饰流(序列化中使用)。

### (3)字符输入流Reader

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/874710-20170530121521321-799067835.png)

```java
Reader 是所有的输入字符流的父类，它是一个抽象类。

CharReader、StringReader 是两种基本的介质流，它们分别将Char 数组、String中读取数据。PipedReader 是从与其它线程共用的管道中读取数据。

BufferedReader 很明显就是一个装饰器，它和其子类负责装饰其它Reader 对象。

FilterReader 是所有自定义具体装饰流的父类，其子类PushbackReader 对Reader 对象进行装饰，会增加一个行号。

InputStreamReader 是一个连接字节流和字符流的桥梁，它将字节流转变为字符流。FileReader 可以说是一个达到此功能、常用的工具类，在其源代码中明显使用了将FileInputStream 转变为Reader 的方法。我们可以从这个类中得到一定的技巧。Reader 中各个类的用途和使用方法基本和InputStream 中的类使用一致。后面会有Reader 与InputStream 的对应关系。
```



### (4)字符输出流Writer

![img](https://gitee.com/kevinyong/kevin-gallery/raw/master/874710-20170530121746211-57243059.png)

```java
Writer 是所有的输出字符流的父类，它是一个抽象类。

CharArrayWriter、StringWriter 是两种基本的介质流，它们分别向Char 数组、String 中写入数据。PipedWriter 是向与其它线程共用的管道中写入数据，

BufferedWriter 是一个装饰器为Writer 提供缓冲功能。

PrintWriter 和PrintStream 极其类似，功能和使用也非常相似。

OutputStreamWriter 是OutputStream 到Writer 转换的桥梁，它的子类FileWriter 其实就是一个实现此功能的具体类（具体可以研究一SourceCode）。功能和使用和OutputStream 极其类似，后面会有它们的对应图。
```

# 5.字符流和字节流

## (1)转换

**文本文件在硬盘中以字节流的形式存储时，通过`InputStreamReader`读取后转化为字符流给程序处理，程序处理的字符流通过`OutputStreamWriter`转换为字节流保存。**

```java
OutputStreamWriter(OutputStream out):将字节流以字符流输出。

InputStreamReader(InputStream in)：将字节流以字符流输入。
```

## (2)区别

**处理单元不同：**

- 字节流：以字节为单位，可以处理任何类型的数据，如图片、音频、视频等二进制文件。 
- 字符流：以字符（Unicode码元）为单位，根据码表映射字符，一次可能读多个字节。 

**编码方式不同：**

- 字节流：但是字节流不考虑编码，处理二进制方面有优势
- 字符流：考虑数据的编码方式，如UTF-8、GBK等，在读写文本数据时需要指定正确的编码方式，否则可能会导致数据乱码。 

**是否设置缓冲区：**

- 字节流： **默认不使用缓冲区**，是文件本身直接操作的，这意味着每调用一次`read`方法都会请求操作系统来读取一个字节（这往往会伴随着一次磁盘IO，因此效率会比较低。要使用内存缓冲区以提高读取的效率，应该使用`BufferedInputStream`。 ），**字节流是与文件本身操作的，所以字节流操作文件时候，即使不关闭资源，文件也会输出。**
- 字符流： 由于字符流在输出前实际上是要完成Unicode码元序列到相应编码方式的字节序列的转换，所以它会使用内存缓冲区来存放转换后得到的字节序列，等待都转换完毕再一同写入磁盘文件中。 所以如果不调用flush或close方法，不会输出任何内容。



## (3)使用情景

- 字节流可以处理任何对象，但一般用来处理图像，视频，以及PPT，Word等二进制数据。
- 字符流一般用于处理纯文本类型的文件，如TXT文件、HTML文件等

>  **注意：**
>
> **字节流可以用来处理纯文本文件，但是字符流不能用于处理图像视频等非文本类型的文件。**
>
> 可以通过将二进制数据转换为字符数据的方式来间接地使用字符流处理二进制数据。 







# 6.处理流

**BufferedReader,**

**BufferedWriter,**

**BufferedInputStream,**

**BufferedOutputsStream**

处理流都是在结点流基础上进行的，带有Buffered的流又称为缓冲流，缓冲流处理文件的输入输出的速度是最快的。

# 7.Scanner类

Java 5添加了java.util.Scanner类，这是一个用于扫描输入文本的新的实用程序。它是以前的StringTokenizer和Matcher类之间的某种结合。由于任何数据都必须通过同一模式的捕获组检索或通过使用一个索引来检索文本的各个部分。于是可以结合使用正则表达式和从输入流中检索特定类型数据项的方法。这样，除了能使用正则表达式之外，Scanner类还可以任意地对字符串和基本类型(如int和double)的数据进行分析。借助于Scanner，可以针对任何要处理的文本内容编写自定义的语法分析器。

- 字节流的套接：在Scanner的构造方法中Scanner(InputStream source)，InputStream只要经过适当的套接，总能获得你想要的流接口。
- 字符流的套接：Scanner(Readable source)，你需要使用Java SE5中新加入的一个接口Readable，该接口表示“具有read()方法的某种东西”，查看Readable接口的API你可以发现你想要的带有Reader的类基本都在其中。
  

```java
//Scanner套接file对象
File f = new File("E:\\Campus\\大三上\\java\\作业\\第三次作业\\result.txt");
try {
    Scanner sc = new Scanner(f);
    while(sc.hasNext()) {
        System.out.println(sc.next());
    }
} catch (FileNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
}

//套接readable对象
File f = new File("E:\\Campus\\大三上\\java\\作业\\第三次作业\\result.txt");
try {
    FileReader fr = new FileReader(f);
    Scanner sc = new Scanner(fr);
    while(sc.hasNext()) {
        System.out.println(sc.next());
    }
} catch (FileNotFoundException e1) {
    // TODO Auto-generated catch block
    e1.printStackTrace();
}
//套接inputstream
File f = new File("E:\\Campus\\大三上\\java\\作业\\第三次作业\\result.txt");
try {
    InputStream is = new FileInputStream(f);
    Scanner sc = new Scanner(is);
    while(sc.hasNext()) {
        System.out.println(sc.next());
    }
} catch (FileNotFoundException e1) {
    // TODO Auto-generated catch block
    e1.printStackTrace();
}
```

### scanner和bufferedreader区别

- Scanner是一个可以使用正则表达式来分析基本类型和字符串的简单文本扫描器！也就是控制台应用程序最为常用的文本输入方式！Scanner取得输入数据的依据是空格符：如按下空格键，Tab键或者Enter键，Scanner就会返回下一个输入。所以说Scanner不能输入空格，如果你希望取得含有空格的字符串BufferedReader可以做到。
- BufferedReader是字符输入流中读取文本，缓冲各个字符，从而提供字符、数组和行的高效读取！速度要比Scanner快！而且也可以设置缓冲区的大小，或者可使用默认的大小。大多数情况下，默认值就足够大了。

### 综合练习:

```java
File f = new File("E:\\Campus\\大三上\\java\\作业\\第三次作业\\result.txt");
if(f.isFile())
    System.out.println("是文件");

try {
    InputStream is = new FileInputStream(f);//读入文件，也可以调用read方法读入
    Scanner sc = new Scanner(is);//inputStream对象读取
    StringBuffer sb = new StringBuffer();
    FileWriter fw = new FileWriter("E:\\Campus\\大三上\\java\\作业\\第三次作业\\yht.txt",true);//写入文件，如果不存在文件创建
    while(sc.hasNext()) {
        sb.append(sc.next()+"\r\n");
    }
    System.out.println(sb);
    fw.append(sb);
    fw.flush();//关闭缓冲流
    fw.close();

} catch (FileNotFoundException e1) {
    // TODO Auto-generated catch block
    e1.printStackTrace();
} catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
}
```

```java
//读取图片
File f = new File("C:\\Users\\Administrator\\Pictures\\Screenshots\\屏幕截图(86).png");
		if(f.isFile())
			System.out.println("是文件");
		try {
			InputStream is = new FileInputStream(f);
			BufferedInputStream bfi = new BufferedInputStream(is);
			FileOutputStream fw = new FileOutputStream("E:\\Campus\\大三上\\java\\作业\\第三次作业\\yht.png");
			int i;
			while((i = bfi.read())!=-1) {
				fw.write(i);
			}
			fw.flush();
			fw.close();
			
		} catch (FileNotFoundException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
```



# 8.序列化

## (1)概述：

将保存在内存中的对象数据转化为二进制数据流进行传输，任何对象都可以序列化

## (2)作用:

把一个Java对象写入到硬盘或者传输到网路上面的其它计算机，这时我们就需要自己去通过java把相应的对象写成转换成字节流。对于这种通用的操作，我们为什么不使用统一的格式呢？没错，这里就出现了java的序列化的概念。在Java的OutputStream类下面的子类ObjectOutput-Stream类就有对应的WriteObject(Object object) 其中要求对应的object实现了java的序列化的接口。

## (3)实现:

**实现java.io.Serializable接口**

```java
//序列化类：java.ioObjectOutputStream
 
//讲对象变为指定的二进制数据
 
class Book implements Serializable{
 
	private String title;
 
	private double price;
 
	public Book(String tit,double pri){
 
		this.title=tit;
 
		this.price=pri;
 
	}
 
	public String toString() {
 
		return "书名："+this.title+",价格："+this.price;
 
	}
 
}
 
public class Demo10 {
 
	public static void main(String[] args) throws Exception {
 
       //序列化到指定的文本
       ObjectOutputStream oos=new ObjectOutputStream(new FileOutputStream(new File("e:"+File.separator+"demoA.txt")));

       oos.writeObject(new Book("java开发", 45.2));
       oos.close();
 
	}
 
}
```

## (4)反序列化

```java
class Book implements Serializable{
 
	private String title;
 
	private double price;
 
	public Book(String tit,double pri){
 
		this.title=tit;
 
		this.price=pri;
 
	}
 
	public String toString() {
 
		return "书名："+this.title+",价格："+this.price;
 
	}
 
}
 
public class Demo11 {
 
	public static void main(String[] args) throws FileNotFoundException, IOException, ClassNotFoundException {
 
		ObjectInputStream ois=new ObjectInputStream(new FileInputStream(new File("e:"+File.separator+"demoA.txt")));
 
		Object obj=ois.readObject();
 
		Book book=(Book) obj;
 
		System.out.println(book);
 
		ois.close();
 
	}
 
}
```

# 9.transient

序列化和反序列化是将整个对象所有属性进行序列号，那么使用transient可以将某些内容使之无法被序列化

```java
class Book implements Serializable{
 
	private transient String title;//无法被序列化
 
	private double price;
 
	public Book(String tit,double pri){
 
		this.title=tit;
 
		this.price=pri;
 
	}
 
	public String toString() {
 
		return "书名："+this.title+",价格："+this.price;
 
	}
 
}
```

# 10.flush和close：

对于字符流一般写入的时候想要马上看到，一般需要`flush()`

- `close()` 用于关闭流，在关闭流之前会先刷新缓冲区中的数据到目标中。 
- `flush()`  用于将缓冲区中的数据立即刷新到目标中，**但不会关闭流**。 



# 11.StreamTokenizer

`StreamTokenizer`类*接收输入流并将其解析为“令牌”，允许一次读取一个令牌*。解析过程由表和多个可以设置为各种状态的标志来控制。流标记器可以识别标识符，数字，引用的字符串和各种注释样式。

从输入流读取的每个字节被视为`'\u0000'`至`'\u00FF'`范围内的`'\u0000'`  `'\u00FF'` 。 字符值用于查找字符的五个可能属性： *空格* ， *字母* ，  *数字* ， *字符串引号*和*注释字符* 。 每个角色都可以有零个或多个这些属性。 

> 以下是**Java.io.StreamTokenizer**类的**字段** -
>
> - **double nval** - 如果当前标记是数字，则此字段包含该数字的值。
> - **String sval** - 如果当前标记是单词标记，则此字段包含一个字符串，该字符串给出单词标记的字符。
> - **static int TT_EOF** - 一个常量，指示已读取流的末尾。
> - **static int TT_EOL** - 一个常量，指示已读取行的结尾。
> - **static int TT_NUMBER** - 表示已读取数字标记的常量。
> - **static int TT_WORD** - 表示已读取字令牌的常量。
> - **int ttype** - 调用nextToken方法后，该字段包含刚刚读取的令牌类型。

> **构造函数**
>
> ```java
> Reader r = new BufferedReader(new InputStreamReader(is));
> StreamTokenizer st = new StreamTokenizer(r);//这将创建一个解析给定字符流的标记生成器。
> ```
>
> **方法：**
>
> - commenChar(int ch) - 指定某个字符为注释字符，此字符之后直到行结尾都被stream tokenizer忽略。
> - eolIsSignificant(boolean flag) - 决定一个行结束符是否被当作一个基本的符号处理，如果是true，则被当作一个基本符号，不当作普通的分隔符，如果是false，则保持原义，即当作普通的分隔符。
> - lineno() - 返回当前流所在的行号。
> - **lowerCaseMode(boolean flag)** - 决定是否读取一个单词时是否转变成小写。
> - **nextToken()** - 分析下一个。
> - ordinaryChar(int ch) - 指定字符在这个tokenizer中保持原义，即只会把当前字符认为普通的字符，不会有其他的语义。
> - ordinaryChars(int low, int hi) - 指定范围内的字符保持语义，同上
> - parseNumbers() - 当stream tokenizer遭遇到一个单词为双精度的浮点数时，会把它当作一个数字，而不是一个单词。
> - pushBack() - 回退，会引起下一个nextToken方法返回当前值。
> - quoteChar(int ch) - 指定当前字符为当前tokenizer中的分隔符，在两个符号之间被当作一个字符串解析。
> - resetSyntax() - 重置语法表使所有的字符都被认为是“ordinary”。
> - slashSlashComments(boolean flag) - 如果为true，则/*与*/之间的都被认为是注释，反之，不是。
> - slashStartComments(boolean flag) - 如果为true，则//之后到行结尾的所有都被认为是注释，反之，不是。 
> - **whitespaceChars(int low, int hi)** - 字符low与hi之间的所有字符都被当作为空格符，即被认识为tokenzier的分隔符。
> - **wordChars(int low, int hi)** - 字符low与hi之间的所有字符都被当作为单词的要素。 一个单词是由一个单词要素后面跟着0个或者更多个单词要素或者数字要素。
>
>
> ```java
> //例子：用来读取控制台输入
> public static void main(String args[]) throws IOException {
>     StreamTokenizer st = new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in)));
>     st.whitespaceChars('"', '"');//设置"为间隔符
>     st.nextToken();
>     String a = st.sval;
>     st.nextToken();
>     String b = st.sval;
>     System.out.println(a);
>     System.out.println(b);
> }
> ```

> 参考——https://blog.csdn.net/sunboylife/article/details/106570340
>
> https://blog.csdn.net/cy973071263/article/details/105588082