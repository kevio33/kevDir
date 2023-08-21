# document.querySelectorAllJSWebApis



## 一、HTML DOM

### 1.简介

当网页被加载时，浏览器会创建页面的文档对象模型（*D*ocument *O*bject *M*odel）。*HTML DOM* 模型被结构化为*对象树*：

![DOM HTML 树](https://gitee.com/kevinyong/kevin-gallery/raw/master/ct_htmltree.gif)

通过这个对象模型，JavaScript 获得创建动态 HTML 的所有力量：



**DOM 定义了访问文档的标准**：

> “W3C 文档对象模型（DOM）是中立于平台和语言的接口，它允许程序和脚本动态地访问、更新文档的内容、结构和样式。”

W3C DOM 标准被分为 3 个不同的部分：

- Core DOM - 所有文档类型的标准模型
- XML DOM - XML 文档的标准模型
- HTML DOM - HTML 文档的标准模型



**HTML DOM ‘ Document对象 ’：**

文档对象代表网页，访问HTML任何元素都是从访问文档对象开始

### 2.获取HTML元素方法

#### (1)getElementById 

访问 HTML 元素最常用的方法是使用元素的 id。

```js
document.getElementById("demo");
```

#### (2)getElementsByTagName

```js
getElementsByTagName(name);//通过标签名找元素

var x = document.getElementsByTagName("p");
```

> getElementsByTagName() 方法返回 *HTMLCollection* 对象。
>
> HTMLCollection 对象是类数组的 HTML 元素列表（集合）。
>
> ```js
> //代码选取文档中的所有 <p> 元素：
> var x = document.getElementsByTagName("p");
> //通过下标访问
> y = x[1];
> 
> //通过length属性访问长度
> document.getElementById("demo").innerHTML = myNodelist.length;
> ```

`HTMLCollection 并非数组！无法对 HTMLCollection 使用数组方法，比如 valueOf()、pop()、push() 或 join()`



#### (3)getElementsByClassName

```js
getElementsByClassName(name);//通过类名找元素
```

> - 如使用 getElementsByClassName() 方法，某些（老的）浏览器会返回 NodeList 对象而不是 HTMLCollection。
> - 所有浏览器都会为 childNodes 属性返回 **NodeList** 对象。
> - 大多数浏览器会为 querySelectorAll() 方法返回 **NodeList** 对象。



#### (4)通过CSS选择器找

```js
//返回 class="intro" 的所有 <p> 元素列表：
var x = document.querySelectorAll("p.intro");

//只返回第一个选择的元素
document.querySelector("p");
```

> ```js
> var myNodeList = document.querySelectorAll("p");
> 
> //同样也可以通过下标索引访问
> y = myNodeList[1];
> 
> //通过length访问长度
> document.getElementById("demo").innerHTML = myNodelist.length;
> ```



##### 关于HTMLCollection 与 NodeList 的区别：

> HTMLCollection是 HTML 元素的集合。
>
> NodeList 是文档节点的集合。
>
> - NodeList 和 HTML 集合几乎完全相同。
> - HTMLCollection 和 NodeList 对象都是类数组的对象列表（集合）。
> - 它们都有定义列表（集合）中项目数的 length 属性。
> - 它们都可以通过索引 (0, 1, 2, 3, 4, ...) 像数组那样访问每个项目。
> - 访问 HTMLCollection 项目，可以通过它们的名称、id 或索引号。
> - 访问 NodeList 项目，只能通过它们的索引号。
> - 只有 NodeList 对象能包含属性节点和文本节点。
>
> 节点列表不是数组！
>
> 节点数组看起来像数组，但并不是。



#### (5)通过HTML对象选择器查找

查找 id="frm1" 的 form 元素，在 forms 集合中，然后显示所有元素值：

```js
var x = document.forms["frm1"];
var text = "";
 var i;
for (i = 0; i < x.length; i++) {
    text += x.elements[i].value + "<br>";
}
document.getElementById("demo").innerHTML = text;
```



#### (6)创建和删除元素

| 方法                              | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| document.createElement(*element*) | 创建html元素                                                 |
| document.removeChild(*element*)   | 删除HTML元素                                                 |
| document.appendChild(*element*)   | 添加HTML元素                                                 |
| document.replaceChild(*element*)  | 替换HTML元素                                                 |
| document.write(*text*)            | 写入HTML输出流（**千万不要在文档加载后使用 document.write()。这么做会覆盖文档。**） |
| element.insertBefore(para, child) | 在para元素前插入child                                        |

> 如需向 HTML DOM 添加新元素，必须首先创建这个元素（元素节点），然后将其追加到已有元素。
>
> ```js
> <div id="div1">
> <p id="p1">这是一个段落。</p>
> <p id="p2">这是另一个段落。</p>
> </div>
> 
> <script>
> //创建了一个新的 <p> 元素
> var para = document.createElement("p");
> 
> //向 <p> 元素添加文本，则必须首先创建文本节点。这段代码创建了一个文本节点
> var node = document.createTextNode("这是新文本。");
> 
> //向 <p> 元素追加这个文本节点
> para.appendChild(node);
> 
> //向已有元素追加这个新元素
> var element = document.getElementById("div1");
> element.appendChild(para);
> </script>
> ```



**使用 insertBefore() 方法在指定位置前插入子元素**

```js
<div id="div1">
<p id="p1">这是一个段落。</p>
<p id="p2">这是另一个段落。</p>
</div>

<script>
var para = document.createElement("p");
var node = document.createTextNode("这是新文本。");
para.appendChild(node);

var element = document.getElementById("div1");
var child = document.getElementById("p1");
element.insertBefore(para, child);
</script>
```



**删除HTML元素**

> 如需删除某个 HTML 元素，您需要知晓该元素的父：
>
> ```html
> <div id="div1">
> <p id="p1">这是一个段落。</p>
> <p id="p2">这是另一个段落。</p>
> </div>
> 
> <script>
> var parent = document.getElementById("div1");
> var child = document.getElementById("p1");
> parent.removeChild(child);
> </script>
> ```



**替换HTML元素：**

```html
<div id="div1">
<p id="p1">这是一个段落。</p>
<p id="p2">这是另一个段落。</p>
</div>

<script>
var para = document.createElement("p");
var node = document.createTextNode("这是新文本。");
para.appendChild(node);

var parent = document.getElementById("div1");
var child = document.getElementById("p1");
parent.replaceChild(para, child);
</script>
```







### 3.属性

#### (1)innerHTML

获取元素内容最简单的方法是使用 innerHTML 属性。

- innerHTML 属性可用于获取或替换 HTML 元素的内容。
- innerHTML 属性可用于获取或改变任何 HTML 元素，包括 < html > 和 < body >。

##### innerHTML和innerText区别

- innerText无法识别标签，但是innerHTML可以

  ```js
  div.innerText = '<strong>今天是</strong>2019'
  ```

  ![image-20220119202132750](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220119202132750.png)

  ```js
  div.innerHTML = '<strong>今天是</strong>2019'
  ```

  ![image-20220119202210630](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220119202210630.png)

- 两者都是可以读写文件内容，但是innerText不读取标签，并且会省略空格和换行，二innerHTML会读取标签

  ```js
  console.alert(div.innerText);
  
  console.alert(div.innerHTML);
  ```

  ![image-20220119202332717](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220119202332717.png)



#### (2)attribute

改变 HTML 元素的属性值

```js
element.attribute = new value

document.getElementById("myImage").src = "landscape.jpg";//修改了 <img> 元素的 src 属性的值
```



#### (3)setAttribute(*attribute*, *value*)

改变 HTML 元素的属性值

```js
element.setAttribute(attribute, value)
```



#### (4)style.property

改变元素样式

```js
element.style.property = new style

//例如
document.getElementById("p2").style.color = "blue";
```



#### (5)value获取值

通常用于表单验证，如果表单域为空，则函数会提示一条消息，并返回false，防止表单被提交

```js
function validateForm() {
  let x = document.forms["myForm"]["fname"].value;
  if (x == "") {
    alert("Name must be filled out");
    return false;
  }
}

//表单
<form name="myForm" action="/action_page.php" onsubmit="return validateForm()" method="post">
Name: <input type="text" name="fname">
<input type="submit" value="Submit">
</form>
```



**也可以使用required自动验证表单**

```html
<form action="/action_page.php" method="post">
  <input type="text" name="fname" required>
  <input type="submit" value="Submit">
</form>
```





### 4.事件处理

#### (1) onclick 事件

向 onclick 事件添加事件处理程序

```js
document.getElementById(id).onclick = function(){code}

//或者
<button onclick="displayDate()">试一试</button>

//或者
<h1 onclick="this.innerHTML = 'Hello!'">点击此文本！</h1>
```



#### (2)onload / onunload

当用户进入后及离开页面时，会触发 onload 和 onunload 事件。

onload 和 onunload 事件可用于处理 cookie。

```html
<html>
<body onload="checkCookies()">

<p id="demo"></p>

<script>
function checkCookies() {
  var text = "";
  if (navigator.cookieEnabled == true) {
    text = "Cookie 已启用";
  } else {
    text = "Cookie 未启用";
  }
  document.getElementById("demo").innerHTML = text;
}
</script>

</body>
</html>
```



#### (3)onchange事件

onchange经常和输入字段验证结合，当输入字段里面有内容变化时会调用

```html
<html>
<head>
<script>
function myFunction() {
  var x = document.getElementById("fname");
  x.value = x.value.toUpperCase();
}
</script>
</head>
<body>

请输入您的名字：<input type="text" id="fname" onchange="myFunction()">

<p>离开输入字段时，会触发一个函数，将输入文本转换为大写。</p>

</body>
</html>
```

> 上面这段函数，当输入框内容变化是，鼠标焦点移开输入框就会吧字母变为大写



#### (4)onmouseover / onmouseout

onmouseover 和 onmouseout 事件可用于当用户将鼠标移至 HTML 元素上或移出时触发某个函数：

```html
<html>
<body>

<div onmouseover="mOver(this)" onmouseout="mOut(this)" 
style="background-color:#D94A38;width:120px;height:20px;padding:40px;">
请把鼠标移上来</div>

<script>
function mOver(obj) {
  obj.innerHTML = "谢谢您"
}

function mOut(obj) {
  obj.innerHTML = "请把鼠标移上来"
}
</script>

</body>
</html>
```





#### (5)onmousedown / onmouseup

onmousedown, onmouseup 以及 onclick 事件构成了完整的鼠标点击事件。

首先当鼠标按钮被点击时，onmousedown 事件被触发；然后当鼠标按钮被释放时，onmouseup 事件被触发；最后，当鼠标点击完成后，onclick 事件被触发。





#### 事件监听

> (1)addEventListener() 方法为指定元素指定事件处理程序。
>
> addEventListener() 方法为元素附加事件处理程序而不会覆盖已有的事件处理程序。
>
> 能够向一个元素添加多个事件处理程序。能够向一个元素添加多个相同类型的事件处理程序，例如两个 "click" 事件。
>
> ```js
> element.addEventListener("click", myFunction);
> element.addEventListener("click", mySecondFunction);
> ```
>
> ```js
> //可以添加不同类型事件
> element.addEventListener("mouseover", myFunction);
> element.addEventListener("click", mySecondFunction);
> element.addEventListener("mouseout", myThirdFunction);
> ```
>
> (2）能够向任何 DOM 对象添加事件处理程序而非仅仅 HTML 元素，例如 window 对象。
>
> ```js
> //当用户调整窗口大小时触发的事件监听器
> window.addEventListener("resize", function(){
>   document.getElementById("demo").innerHTML = Math.random();
> });
> ```
>
> 
>
> addEventListener() 方法使我们更容易控制事件如何对冒泡作出反应。
>
> 当使用 addEventListener() 方法时，JavaScript 与 HTML 标记是分隔的，已达到更佳的可读性；即使在不控制 HTML 标记时也允许您添加事件监听器。
>
> 能够通过使用 removeEventListener() 方法轻松地删除事件监听器。



**语法：**

```js
element.addEventListener(event, function, useCapture);

//第一个参数是事件的类型（比如 "click" 或 "mousedown"）。

//第二个参数是当事件发生时我们需要调用的函数。

//第三个参数是布尔值，指定使用事件冒泡还是事件捕获。此参数是可选的。
```

> **注意：**<font color="red">请勿对事件使用 "on" 前缀；请使用 "click" 代替 "onclick"。</font>



**例子**

```js
//当用户点击某个元素时提示 "Hello World!"：
element.addEventListener("click", myFunction);

function myFunction() {
    alert ("Hello World!");
}
```





##### **事件传播(冒泡/捕获)**

> 在 HTML DOM 中有两种事件传播的方法：**冒泡和捕获**。
>
> 事件传播是一种定义当发生事件时元素次序的方法。***假如 <div> 元素内有一个 <p>，然后用户点击了这个 <p> 元素，应该首先处理哪个元素“click”事件？***
>
> - **在冒泡中，最内侧元素的事件会首先被处理**，然后是更外侧的：首先处理 <p> 元素的点击事件，然后是 <div> 元素的点击事件。
> - **在捕获中，最外侧元素的事件会首先被处理**，然后是更内侧的：首先处理 <div> 元素的点击事件，然后是 <p> 元素的点击事件。
>
> 在 addEventListener() 方法中，你能够通过使用“useCapture”参数来规定传播类型：
>
> ```js
> addEventListener(event, function, useCapture);
> 
> //默认值是 false:使用冒泡传播
> //true:使用捕获传播。
> 
> ```



##### 移除事件

removeEventListener() 方法会删除已通过 addEventListener() 方法附加的事件处理程序：

```js
element.removeEventListjsener("mousemove", myFunction);
```

### 5.查找HTML对象

| 属性                         | 描述                                            |
| ---------------------------- | ----------------------------------------------- |
| document.anchors             | 返回拥有 name 属性的所有 < a > 元素。           |
| document.baseURI             | 返回文档的绝对基准 URI                          |
| document.body                | 返回 < body > 元素                              |
| document.cookie              | 返回文档的 cookie                               |
| document.doctype             | 返回文档的 doctype                              |
| document.documentElement     | 返回 < html > 元素                              |
| document.documentMode        | 返回浏览器使用的模式                            |
| document.documentURI         | 返回文档的 URI                                  |
| document.domain              | 返回文档服务器的域名                            |
| document.embeds              | 返回所有 < embed > 元素                         |
| document.forms               | 返回所有 < form > 元素                          |
| document.head                | 返回 < head > 元素                              |
| document.images              | 返回所有 < img > 元素                           |
| document.implementation      | 返回 DOM 实现                                   |
| document.inputEncoding       | 返回文档的编码（字符集）                        |
| document.lastModified        | 返回文档更新的日期和时间                        |
| document.links               | 返回拥有 href 属性的所有 < area > 和 < a > 元素 |
| document.readyState          | 返回文档的（加载）状态                          |
| document.referrer            | 返回引用的 URI（链接文档）                      |
| document.scripts             | 返回所有 < script > 元素                        |
| document.strictErrorChecking | 返回是否强制执行错误检查                        |
| document.title               | 返回 < title > 元素                             |
| document.URL                 | 返回文档的完整 URL                              |
| document.applets             | 返回所有 < applet > 元素（HTML5 不建议使用）    |



### 6.改变CSS样式

用一个简单例子来回答

```html

<style>
    #container {
        width: 400px;
        height: 400px;
        position: relative;
        background: yellow;
    }
    #animate {
        width: 50px;
        height: 50px;
        position: absolute;
        background: red;
    } 
</style>

<!--html-->
<div id ="container">
    <div id ="animate">我的动画在这里。</div>
</div>

<script>
	function myMove() {
    var elem =  document.getElementById("animate"); 
    var pos = 0;
    var id = setInterval(frame, 5);//5ms内执行一次
     function frame() {
        if (pos ==  350) {
             clearInterval(id);
        } else {
             pos++; 
             elem.style.top = pos + 'px'; 
             elem.style.left = pos + 'px'; 
        }
     }
}
</script>
```

![image-20220118203556335](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220118203556335.png)



### 7.导航

**DOM 节点**

HTML 文档中的所有事物都是节点：

- 整个文档是文档节点
- 每个 HTML 元素是元素节点
- HTML 元素内的文本是文本节点
- 每个 HTML 属性是属性节点
- 所有注释是注释节点

![image-20220118211336321](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220118211336321.png)



**举个例子**

```html
<html>

   <head>
       <title>DOM 教程</title>
   </head>

  <body>
       <h1>DOM 第一课</h1>
       <p>Hello world!</p>
   </body>

</html> 
```

![DOM HTML 树](https://gitee.com/kevinyong/kevin-gallery/raw/master/dom_navigate.gif)

> 从以上的 HTML 中您能读到以下信息：
>
> - < html> 是根节点
> - < html> 没有父
> - < html> 是 < head> 和 < body> 的父
> - < head> 是 < html> 的第一个子
> - < body> 是 < html> 的最后一个子
>
> 同时：
>
> - < head> 有一个子：< title>
> - < title> 有一个子（**文本节点**）："DOM 教程"
> - < body> 有两个子：< h1> 和 < p>
> - < h1> 有一个子："DOM 第一课"
> - < p> 有一个子："Hello world!"
> - < h1> 和 < p> 是同胞

```html
<title id="demo">DOM 教程</title> 
```

**“ DOM 教程 ” 是属于元素节点 < title> 的文本节点。**

- **文本节点的值能够通过节点的 innerHTML 属性进行访问：**

```js
var myTitle = document.getElementById("demo").innerHTML;
```

- 访问 innerHTML 属性等同于访问首个子节点的 nodeValue：

```js
var myTitle = document.getElementById("demo").firstChild.nodeValue;
```

- 也可以这样访问第一个子节点：

```js
var myTitle = document.getElementById("demo").childNodes[0].nodeValue;
```





通过这样的节点树，能够使得我们在节点之间导航



**访问父节点**

- parentNode



**访问兄弟节点**

(1)包含元素 、 文本结点

- nextSibling
- previousSibling

(2)返回下一个元素结点(**有兼容性问题**)

- node.nextElementSibling
- node.previousElementSibling

(3)封装兼容性函数

![image-20220120155722495](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220120155722495.png)



**访问子节点：**

- firstChild(返回第一个结点，可能不是元素结点)

- **child[ index ] (返回所有子元素,该做法是实际开发最常用的)**

  ```js
  ol.childer[0];
  ```

- firstChild / lastChild

- firstElementChild(拿到第一个元素结点，但是ie 9以下不兼容)

- childNodes[*nodenumber*]



**访问根结点**

有两个特殊属性允许访问完整文档：

- document.body - 文档的 body
- document.documentElement - 完整文档



**nodeName**

nodeName 属性规定节点的名称。

- nodeName 是只读的
- 元素节点的 nodeName 等同于标签名
- 属性节点的 nodeName 是属性名称
- 文本节点的 nodeName 总是 #text
- 文档节点的 nodeName 总是 #document

> ```html
> <h1 id="id01">我的第一张网页</h1>
> <p id="id02">Hello!</p>
> 
> <script>
> document.getElementById("id02").innerHTML  = document.getElementById("id01").nodeName;
> </script>
> 
> 
> 结果： H1
> ```

> 注意：只有通过id查找的元素才可以使用nodeName属性

**nodeValue** 

nodeValue 属性规定节点的值。

- 元素节点的 nodeValue 是 undefined
- 文本节点的 nodeValue 是文本文本
- 属性节点的 nodeValue 是属性值



**nodeType 属性**

nodeType 属性返回节点的类型。nodeType 是只读的。

```html
<h1 id="id01">我的第一张网页</h1>
<p id="id02">Hello!</p>

<script>
document.getElementById("id02").innerHTML  = document.getElementById("id01").nodeType;
</script>

结果：1
```

> 有关type结果的含义：https://www.w3school.com.cn/js/js_htmldom_navigation.asp





## 二、BOM

Browser Object Model即浏览器对象模型，它提供了**独立于内容而与浏览器窗口进行交互的对象**，其核心对象是window



### 1.Window对象

window对象代表浏览器窗口，所有JS对象，函数和变量**自动变成**为window对象成员，甚至是document对象也是window对象属性

```js
window.document.getElementById("header");

//等同于
document.getElementById("header");
```



#### 窗口尺寸

这两个属性都以像素返回尺寸，浏览器窗口（浏览器视口）不包括工具栏和滚动条：

- window.innerHeight - 浏览器窗口的内高度（以像素计）
- window.innerWidth - 浏览器窗口的内宽度（以像素计）



> 由于IE 兼容性问题，通常的方法是
>
> ```js
> var w = window.innerWidth
> || document.documentElement.clientWidth
> || document.body.clientWidth;
> 
> var h = window.innerHeight
> || document.documentElement.clientHeight
> || document.body.clientHeight; 
> ```



#### 其他方法

- window.open() - 打开新窗口
- window.close() - 关闭当前窗口
- window.moveTo() -移动当前窗口
- window.resizeTo() -重新调整当前窗口



#### Screen对象

*window.screen* 对象不带 window 前缀也可以写：

**(1)screen.width**属性返回以像素计的访问者屏幕宽度。

```js
document.getElementById("demo").innerHTML = "Screen Width: " + screen.width;
```

> 该属性和innerWidth区别
>
> ```js
> console.log(window.innerWidth);//可视内容的区域，如果缩小屏幕会变化
> console.log(window.screen.width);//屏幕宽度
> ```
>
> ![image-20220120162952443](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220120162952443.png)

**(2)screen.height** 属性返回以像素计的访问者屏幕的高度。

```js
document.getElementById("demo").innerHTML = "Screen Height: " + screen.height;
```



**(3)screen.availWidth** 属性返回访问者屏幕的宽度，**以像素计，减去诸如窗口工具条之类的界面特征。**



**(4)Window Screen 色深**

screen.colorDepth 属性返回用于显示一种颜色的比特数。

> 所有现代计算机都使用 24 位或 32 位硬件的色彩分辨率：
>
> - 24 bits =16,777,216 种不同的 "True Colors"
> - 32 bits = 4,294,967,296 中不同的 "Deep Colors"
>
> 更老的计算机使用 14 位：65,536 种不同的 "High Colors" 分辨率。
>
> 异常古老的计算机，以及老式的手机使用 8 位：256 中不同的 "VGA colors"。



**(5)screen.pixelDepth**

screen.pixelDepth 属性返回屏幕的像素深度。



#### Location对象

**(1)Window Location Href**

window.location.href 属性返回当前页面的 URL。

```js
document.getElementById("demo").innerHTML = "页面位置是 " + window.location.href;

页面位置是 http://www.w3school.com.cn/js/js_window_location.asp
```

**(2)Window Location 主机名**

window.location.hostname 属性返回（当前页面的）因特网主机的名称。

```js
document.getElementById("demo").innerHTML = "页面主机名是 " + window.location.hostname;

页面主机名是 www.w3school.com.cn
```



**(3)Window Location 路径名**

window.location.pathname 属性返回当前页面的**路径名**。

```js
document.getElementById("demo").innerHTML = "页面路径是 " + window.location.pathname;

页面路径是 /js/js_window_location.asp
```



**(4)Window Location 协议**

window.location.protocol 属性返回页面的 web 协议。

```js
document.getElementById("demo").innerHTML = "页面协议是 " + window.location.protocol;

页面协议是 http:
```



**(5)Window Location 端口**

window.location.port 属性返回（当前页面的）互联网主机端口的编号。

```js
document.getElementById("demo").innerHTML = "端口号是： " + window.location.port;

//如果端口号是默认值（对于 http 为 80，对于 https 为 443），则大多数浏览器将显示 0 或不显示。
```



**(6)window.location.assign() 方法加载新文档。**

加载新文档：

```js
<html>
<head>
<script>
function newDoc() {
    window.location.assign("https://www.w3school.com.cn")
 }
</script>
</head>
<body>

<input type="button" value="Load new document" onclick="newDoc()">

</body>
</html> 
```



#### History

windows.history对象包含浏览器历史

**(1)history.back() **

方法加载历史列表中前一个 URL，这等同于在浏览器中点击后退按钮。

```html
<html>
<head>
<script>
function goBack() {
    window.history.back()
 }
</script>
</head>
<body>

<input type="button" value="Back" onclick="goBack()">

</body>
</html>
```



**(2)history.forward()**

history forward() 方法加载历史列表中下一个 URL。

这等同于在浏览器中点击前进按钮。



**(3)history.go()**

go() 方法从历史列表中加载一个特定的 URL：

```html
<button onclick="myFunction()">后退两页</button>

<script>
function myFunction() {
  window.history.go(-2);
}
</script>
```



| 属性   | 描述              |
| ------ | ----------------- |
| length | 返回历史列表中URL |



#### Navigator

**window.navigator 对象包含有关访问者的信息。**

**(1)是否开启Cookie**

cookieEnabled 属性返回 true，如果 cookie 已启用，否则返回 false：



**(2)浏览器应用程序名称**

appName 属性返回浏览器的应用程序名称：

```html
<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML = "navigator.appName is " + navigator.appName;
</script>

结果： Netscape
Netscape 是 IE11、Chrome、Firefox 和 Safari 的应用程序名称。
```



**(3)浏览器应用程序代码名称**

appCodeName 属性返回浏览器的应用程序代码名称：

```js
navigator.appCodeName;

//"Mozilla" 是 Chrome、Firefox、IE、Safari 以及 Opera 的应用程序代码名称。
```



**(4)appVersion**

appVersion 属性返回有关浏览器的版本信息：

```js
<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML = navigator.appVersion;
</script>
```



**(5)userAgent**

userAgent 属性返回由浏览器发送到服务器的用户代理报头（user-agent header）：

```js
<p id="demo"></p>

<script>
document.getElementById("demo").innerHTML = navigator.userAgent;
</script>
```



**(6)浏览器平台(操作系统)**

```js
document.getElementById("demo").innerHTML = navigator.platform;

结果： Win32
```



**(7)浏览器语言**

```js
navigator.language
```



**(8)Java是否启用**

javaEnabled() 方法返回 true，如果 Java 已启用：

```js
navigator.javaEnabled()
```



#### 弹出框

**(1)确认框**

用户单击“确定”，该框返回 true。如果用户单击“取消”，该框返回 false。

```js
window.confirm("sometext");

//例子
var r = confirm("请按按钮");
if (r == true) {
    x = "您按了确认！";
} else {
    x = "您按了取消！";
}
```

![image-20220120172907428](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220120172907428.png)



**(2)提示框**

如果希望用户在进入页面前输入值，通常会使用提示框。

```js
window.prompt("提示词","默认词");
```

```html
<script>
function myFunction() {
  var txt;
  var person = prompt("请输入您的名字：", "哈利波特");
  if (person == null || person == "") {
    txt = "用户取消输入";
  } else {
    txt = "你好，" + person + "！今天过得好吗？";
  }
  document.getElementById("demo").innerHTML = txt;
}
</script>
```



![image-20220120173244085](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220120173244085.png)



如果要换行显示使用\n

```js
alert("Hello\nHow are you?");
```



#### **Timing事件**

- **setTimeout(**function**,** *milliseconds***)**

  > 单击按钮。等待 3 秒，然后页面会提示 "Hello"：
  >
  > ```js
  > <button onclick="setTimeout(myFunction, 3000)">试一试</button>
  > 
  > <script>
  > function myFunction() {
  >     alert('Hello');
  >  }
  > </script>
  > ```
  >
  > **停止执行**
  >
  > clearTimeout() 方法停止执行 setTimeout() 中规定的函数。
  >
  > ```js
  > myVar = setTimeout(function, milliseconds);
  > clearTimeout(myVar);
  > ```

- **setInterval(**function**,** *milliseconds***)**

  > setInterval() 方法在每个给定的时间间隔重复给定的函数。
  >
  > ```js
  > var myVar = setInterval(myTimer, 1000);
  >  
  > function myTimer() {
  >     var d = new Date();
  >     document.getElementById("demo").innerHTML = d.toLocaleTimeString();
  > }
  > ```
  >
  > **停止执行**
  >
  > clearInterval() 方法停止 setInterval() 方法中指定的函数的执行。
  >
  > ```js
  > myVar = setInterval(function, milliseconds);
  > clearInterval(myVar);
  > ```



#### Cookie

##### (1)创建cookie

```js
document.cookie = "username=Bill Gates";
```

**还可以添加有效日期（UTC 时间）。默认情况下，在浏览器关闭时会删除 cookie：**

```js
document.cookie = "username=John Doe; expires=Sun, 31 Dec 2017 12:00:00 UTC";
```

**通过 path 参数，您可以告诉浏览器 cookie 属于什么路径。默认情况下，cookie 属于当前页。**

```js
document.cookie = "username=Bill Gates; expires=Sun, 31 Dec 2017 12:00:00 UTC; path=/";
```

##### (2)读取Cookie

```js
var x = document.cookie;
```



##### (3)改变Cookie

```js
document.cookie = "username=Steve Jobs; expires=Sun, 31 Dec 2017 12:00:00 UTC; path=/";
```

如果设置了新的Cookie，旧的Cookie不会被覆盖，而是新的cookie添加在末尾





##### (4)删除Cookie

删除 cookie 非常简单。

删除 cookie 时不必指定 cookie 值：

直接把 expires 参数设置为过去的日期即可：

```js
document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
```



##### 完整Cookie实例

访客第一次到达网页时，会要求他填写姓名。然后将该名称存储在 cookie 中。

下次访客到达同一页时，他将收到一条欢迎消息。

例如，我们将创建 3 个JavaScript函数：

1. 设置 cookie 值的函数

   ```js
   function setCookie(cname, cvalue, exdays) {
       var d = new Date();
       d.setTime(d.getTime() + (exdays*24*60*60*1000));
       var expires = "expires="+ d.toUTCString();
       document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
   } 
   
   //上面这个函数的的参数是：cookie 的名字（cname），cookie 的值（cvalue），以及知道 cookie 过期的天数（exdays）。通过把 cookie 名称、cookie 值和过期字符串相加，该函数就设置了 cookie。
   ```

   

2. 获取 cookie 值的函数

   ```js
   function getCookie(cname) {
       var name = cname + "=";
       var decodedCookie = decodeURIComponent(document.cookie);
       var ca = decodedCookie.split(';');
       for(var i = 0; i <ca.length; i++) {
           var c = ca[i];
           while (c.charAt(0) == ' ') {
               c = c.substring(1);
            }
            if (c.indexOf(name) == 0) {
               return c.substring(name.length, c.length);
            }
        }
       return "";
   } 
   
   /*
   把 cookie 作为参数（cname）。
   
   创建变量（name）与要搜索的文本（CNAME”=”）。
   
   解码 cookie 字符串，处理带有特殊字符的 cookie，例如 “$”。
   
   用分号把 document.cookie 拆分到名为 ca（decodedCookie.split(';')）的数组中。
   
   遍历 ca 数组（i = 0; i < ca.length; i++），然后读出每个值 c = ca[i]。
   
   如果找到 cookie（c.indexOf(name) == 0），则返回该 cookie 的值（c.substring(name.length, c.length）。
   
   如果未找到 cookie，则返回 ""。
   */
   ```

3. 检查 cookie 值的函数

```js
function checkCookie() {
    var username = getCookie("username");
    if (username != "") {
        alert("Welcome again " + username);
    } else {
        username = prompt("Please enter your name:", "");
        if (username != "" && username != null) {
            setCookie("username", username, 365);
        }
    }
} 

/*
最后，我们创建检查 cookie 是否设置的函数。

如果已设置 cookie，将显示一个问候。

如果未设置 cookie，会显示一个提示框，询问用户的名字，并存储用户名 cookie 365 天，通过调用 setCookie 函数：
*/
```



## 三、WEBApi



### 1.Form表单验证API

**(1)约束验证DOM方法**

| 属性                | 描述                                       |
| ------------------- | ------------------------------------------ |
| checkValidity()     | 如果 input 元素包含有效数据，则返回 true。 |
| setCustomValidity() | 设置 input 元素的 validationMessage 属性。 |

> checkValidity()方法
>
> ```html
> <input id="id1" type="number" min="100" max="300" required>
> <button onclick="myFunction()">OK</button>
> 
> <p id="demo"></p>
> 
> <script>
> function myFunction() {
>   const inpObj = document.getElementById("id1");
>   if (!inpObj.checkValidity()) {//如果数字超出规定的min和max那么就要返回false
>     document.getElementById("demo").innerHTML = inpObj.validationMessage;
>   }
> }
> </script>
> ```



**(2)约束验证DOM属性：**

| 属性              | 描述                                        |
| ----------------- | ------------------------------------------- |
| validity          | 包含与输入元素有效性相关的布尔属性。        |
| validationMessage | 包含当有效性为 false 时浏览器将显示的消息。 |
| willValidate      | 指示是否将验证 input 元素。                 |

**①validity**

input 元素的有效性属性包含许多与数据有效性相关的属性：

![image-20220120195748264](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220120195748264.png)

> 例如
>
> #### rangeOverflow 属性
>
> ```
> <input id="id1" type="number" max="100">
> <button onclick="myFunction()">OK</button>
> 
> <p id="demo"></p>
> 
> <script>
> function myFunction() {
>   let text = "Value OK";
>   if (document.getElementById("id1").validity.rangeOverflow) {
>     text = "Value too large";
>   }
> }
> </script>
> ```



### 2.Storage存储API

Web Storage API 是一种用于在浏览器中存储和检索数据的简单语法。他非常容易使用：

**(1)LocalStorage对象**

localStorage 对象提供对特定网站的本地存储的访问。它允许您存储、读取、添加、修改和删除该域的数据项。

存储的数据没有到期日期，并且在浏览器关闭时不会被删除。

这些数据将在几天、几周和几年内均可用。



> #### setItem()方法
>
> localStorage.setItem() 方法将数据项存储在 storage 中。
>
> 它接受一个名称和一个值作为参数：
>
> ```js
> localStorage.setItem("name", "Bill Gates");
> ```
>
> 
>
> #### getItem()
>
> localStorage.getItem() 方法从存储（storage）中检索数据项。
>
> 它接受一个名称作为参数：
>
> ```js
> localStorage.getItem("name");
> ```



**其他对应方法**

![image-20220120200651162](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220120200651162.png)



**(2)sessionStorage 对象**

sessionStorage 对象与 localStorage 对象相同。

不同之处在于 sessionStorage 对象存储会话的数据。

当浏览器关闭时，数据会被删除。

> #### setItem() 方法
>
> ```js
> sessionStorage.setItem("name", "Bill Gates");
> ```
>
> 
>
> #### getItem() 方法
>
> ```
> sessionStorage.getItem("name");
> ```



### 3.WebWorker

**(1)什么是WebWorker**

在 HTML 页面中执行脚本时，页面在脚本完成之前是无响应的。

Web Worker 是在后台运行的 JavaScript，独立于其他脚本，不会影响页面的性能。你可以继续做任何你想做的事情：点击、选取内容等，同时 web worker 在后台运行。**web worker适用于CPU密集型任务**



**(2)应用：**

创建WebWorker之前先检查浏览器是否支持

```js
if (typeof(Worker) !== "undefined") {
  // Yes! Web worker support!
  // Some code.....
} else {
  // Sorry! No Web Worker support..
}
```



**创建WebWorker要执行的文件**

> 现在，让我们在外部 JavaScript 中创建我们的 Web Worker。
>
> 在这里，我们创建了一个重要的脚本。该脚本存储在 "demo_workers.js" 文件中：
>
> ```js
> let i = 0;
> 
> function timedCount() {
>   i ++;
>   postMessage(i);
>   setTimeout("timedCount()",500);
> }
> 
> timedCount();
> ```

**创建Web worker对象**

> 以下代码行检查 worker 是否已存在，如果不存在，它会创建一个新的 web worker 对象并运行 "demo_workers.js" 中的代码：
>
> ```js
> if (typeof(w) == "undefined") {
>   w = new Worker("demo_workers.js");
> }
> ```
>
> 然后我们可以发送和接收来自 web worker 的消息。
>
> 向 web worker 添加一个 "onmessage" 事件侦听器。
>
> ```js
> w.onmessage = function(event){
>   document.getElementById("result").innerHTML = event.data;
> };
> ```
>
> 当 Web Worker 发布消息时，将执行事件侦听器中的代码。来自 Web Worker 的数据存储在 event.data 中。

**终止Web Worker**

> 当 web worker 对象被创建时，它会继续监听消息（即使在外部脚本完成之后）直到它被终止。
>
> 如需终止 web worker，并释放浏览器/计算机资源，请使用 terminate() 方法：



**由于 Web Worker 位于外部文件中，因此他们无法访问以下 JavaScript 对象：**

- window 对象
- document 对象
- parent 对象



### 4.Fetch API

Fetch API 接口允许 Web 浏览器向 Web 服务器发出 HTTP 请求。

不再需要 XMLHttpRequest。



**下面例子通过Fetch获取文件并显示内容**

```js
fetch(file)
.then(x => x.text())
.then(y => myDisplay(y));


//由于 Fetch 基于 async 和 await，因此上面的例子这么写可能更容易理解：
async function getText(file) {
  let x = await fetch(file);
  let y = await x.text();
  myDisplay(y);
}
```



### 5.Geolocation API

Geolocation API 用于获取用户的地理位置。

> 参考：https://www.w3school.com.cn/js/js_api_geolocation.asp



## 四、AJAX

### 1.什么是AJAX

AJAX = *A*synchronous *J*avaScript *A*nd *X*ML.

AJAX 仅仅组合了：

- 浏览器内建的 XMLHttpRequest 对象（从 web 服务器请求数据）
- JavaScript 和 HTML DOM（显示或使用数据）
- Ajax 应用程序可能使用 XML 来传输数据，但将数据作为纯文本或 JSON 文本传输也同样常见。
- Ajax 允许通过与场景后面的 Web 服务器交换数据来异步更新网页。这意味着可以更新网页的部分，而不需要重新加载整个页面。



### 2.工作流程

![AJAX](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/ajax.gif)



### 3.使用方式

#### (1)XMLHttpRequest 对象

**Ajax 的核心是 XMLHttpRequest 对象。**XMLHttpRequest 对象用于同幕后服务器交换数据。这意味着可以更新网页的部分，而不需要重新加载整个页面。

**①创建XMLHttpRequest 对象**

> ```js
> variable = new XMLHttpRequest();
> ```
>
> 老版本的 Internet Explorer（IE5 和 IE6）使用 ActiveX 对象：
>
> ```js
> variable = new ActiveXObject("Microsoft.XMLHTTP");
> ```
>
> 为了应对所有浏览器，包括 IE5 和 IE6，请检查浏览器是否支持 XMLHttpRequest 对象。如果支持，创建 XMLHttpRequest 对象，如果不支持，则创建 ActiveX 对象：
>
> ```js
> var xhttp;
> if (window.XMLHttpRequest) {
>     xhttp = new XMLHttpRequest();
>     } else {
>     // code for IE6, IE5
>      xhttp = new ActiveXObject("Microsoft.XMLHTTP");
> }js
> ```



**②XML方法**

![image-20220125131040982](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220125131040982.png)



**③XML属性**

![image-20220125131237198](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220125131237198.png)



#### (2)向服务器发送请求

如需向服务器发送请求，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：

```js
xhttp.open("GET", "ajax_info.txt", true);
xhttp.send();
```



**①使用GET发送请求**

```js
xhttp.open("GET", "demo_get2.asp?fname=Bill&lname=Gates", true);
xhttp.send();
```



**②使用POST发送请求**

```js
xhttp.open("POST", "demo_post.asp", true);
xhttp.send();
```



如需发送表单数据，通过 setRequestHeader() 添加一个 HTTP 头部。请在 send() 方法中规定您需要发送的数据：

```js
xhttp.open("POST", "ajax_test.asp", true);
xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xhttp.send("fname=Bill&lname=Gates");
```

![image-20220125132339465](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220125132339465.png)



#### (3)响应

可以定义当请求接收到应答时所执行的函数。

这个函数是在 XMLHttpResponse 对象的 onreadystatechange 属性中定义的：

```js
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("demo").innerHTML = this.responseText;
  }
};
xhttp.open("GET", "ajax_info.txt", true);
xhttp.send();
```

> **onreadystatechange** 被触发五次（0-4），每次 **readyState** 都发生变化。



**使用回调**

回调函数是一种作为参数被传递到另一个函数的函数。

如果您的网站中有多个 AJAX 任务，那么您应该创建一个执行 XMLHttpRequest 对象的函数，以及一个供每个 AJAX 任务的回调函数。

该函数应当包含 URL 以及当响应就绪时调用的函数。

```js
loadDoc("url-1", myFunction1);

loadDoc("url-2", myFunction2);

function loadDoc(url, cFunction) {
  var xhttp;
  xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      cFunction(this);
    }
  };
  xhttp.open("GET", url, true);
  xhttp.send();
}

function myFunction1(xhttp) {
  // 行动在这里
 } 
function myFunction2(xhttp) {
  // 行动在这里
 } 
```