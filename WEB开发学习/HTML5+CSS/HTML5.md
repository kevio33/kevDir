# HTML

**Web标准三要素**

| 标准 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| 结构 | 结构用于对网页元素进行整理分类，现阶段主要学的是HTML         |
| 表现 | 表现用于设置网页元素版式、颜色、大小等外观样式，主要指的是CSS |
| 行为 | 行为是指网页模型的定义及交互的编写，现阶段主要学的是JS       |



HTML基本结构标签

| 标签名            | 定义       | 说明                                        |
| ----------------- | ---------- | ------------------------------------------- |
| <html></html>     | HTML标签   | 页面中最大的标签，根标签                    |
| <head></head>     | 文档的头部 | 注意在head标签中我们必须要设置的标签是title |
| <titile></titile> | 文档标题   | 让页面拥有一个属于自己的网页标题            |
| <body></body>     | 文档主体   | 元素包含文档的所有内容                      |



## VSCode以及html文件格式

VScode插件

![image-20220101174915656](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220101174915656.png)

> 据说JS-CSS-HTML Formatter不好用，不推荐
>
> **保存自动格式化**
>
> 在VSCode -》 设置 -》 首选项-》搜索格式化
>
> ![image-20220124181627962](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220124181627962.png)





**文档类型声明标签**

```html
<!DOCTYPE html>

该代码意思是：当前页面采取HTML5版本来显示网页
```



**语言种类**

用来定义当前文档显示的语言

```html
<html lang="en">
```

浏览器检测进行翻译



**字符集**

```html
<meta charset="UTF-8">
```

![image-20220101191032774](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220101191032774.png)





## Emmet语法

![image-20220105191759668](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220105191759668.png)

```html
对于第六个
div.num$*5<!--增序产生5个类名交num的div-->
```



## 一、标签

### 基本标签

#### 1.标题标签

`<h1> - <h6>`大小依次递减，每一个标题独占一行

```html
<h1></h1>
```



#### 2.段落标签

进行文字的分段

```html
<p></p>
```



#### 3.换行标签(单标签)

- 进行强制换行
- 和段落标签不同的是，行与行之间不会有空隙

```html
<br  />
```



#### 4.文本格式化标签

| 语义   | 标签                             | 说明           |
| ------ | -------------------------------- | -------------- |
| 加粗   | `<strong></strong>`或者`<b></b>` | 推荐使用strong |
| 倾斜   | `<em></em>`或者`<i></i>`         | 推荐em         |
| 删除线 | `<del></del>`或者`<s></s>`       | 推荐del        |
| 下划线 | `<ins></ins>`或者`<u></u>`       | 推荐ins        |



#### 5.容器标签

两者没有语义，就是容器，用来装载内容，进行布局

##### div：

​	一个div单独占一行

##### span：

​	一行可以放多个span

![image-20220103152618349](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220103152618349.png)



#### 6.图像标签(单标签)

```html
<img src="图像url"/>
```



基本属性

| 属性   | 属性值   | 说明                                 |
| ------ | -------- | ------------------------------------ |
| src    | 图片路径 | 必需属性                             |
| alt    | 文本     | 替换文本，图像不能显示的文字         |
| title  | 文本     | 提示文本，鼠标放到图像上，显示的文字 |
| width  | 像素     | 设置图像宽度                         |
| height | 像素     | 设置图像高度                         |
| border | 像素     | 设置图像边框粗细                     |



```html
<img src="" alt="图片无法显示时候显示的内容" title="鼠标放上去显示文字" width="500" height="500" />
```



##### 图片路径

| 相对路径分类 | 符号 | 说明                                               |
| ------------ | ---- | -------------------------------------------------- |
| 同一级路径   |      | 图像文件与HTML文件位于同一级 < img src="yht.jpg"/> |
| 下一级路径   | /    | < img src=“images/yht.jpg”/>                       |
| 上一级路径   | ../  | < img src="../yht.jpg"/>                           |



#### 7.超链接标签

| 属性   | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| href   | 用于指定连接目标的url地址                                    |
| target | 用于指定链接页面打开方式，其中_self为默认值，_blank为在新窗口中打开 |



##### 外部连接

​	例如 www.baidu.com

##### 内部连接

​	网站内部页面之间相互链接，直接 链接内部页面名称即可

```html
<a href="yhthtml.html">去往主界面</a>
```



##### 空链接

​	当没有确切的链接内容，点击会返回顶部

```html
<a href="#"></a>
```

##### 下载链接

​	当`链接内容是 .exe 或者 压缩包`时候，点击链接会直接下载



##### 锚点链接

​	跳转到网页某一个模块

- 在链接文本的href属性中，设置属性值为`#名字`形式，

  ```html
  <a href="#two">第二季</a>
  注意要有'#'号
  ```

- 找到目标位置标签，里面添加一个id属性，赋值为刚才的名字

  ```html
  <h3 id="two">
      第二季介绍
  </h3>
  ```



#### 8.特殊字符

HTML页面中，一些特殊符号很难直接使用，例如空格，所以我们可以使用下面表格的字符来替代

![image-20220104131058414](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220104131058414.png)



```html
<!--以空格为例-->
加油&nbsp;加油
```



#### 9.表格标签

表格的基本语法

- `<table></table>`用于定义表格标签
- `<tr></tr>`用于定义表格中的行，必须嵌套在table标签中
- `<td></td>`定义表格中的单元格，必须嵌套在tr中（td 代表table data ，即单元格数据）
- `<th></th>`表头单元格标签(table head)，表头单元格里面的**文本内容加粗居中**显示



```html
<table>
    <tr>
        <th>表头1</th>
        <th>表头2</th>
    </tr>

    <tr>
        <td>hhh</td>
        <td>hhh</td>
    </tr>
</table>
```



**表格相关属性**

| 属性名      | 属性值              | 描述                                             |
| ----------- | ------------------- | ------------------------------------------------ |
| align       | left、center、right | 规定整个表格相对周围元素的对齐方式               |
| border      | 1或“ ”              | 规定表格单元是否拥有边框，默认为“”，表示没有边框 |
| cellpadding | 像素值              | 规定边框与其内容之间的空白，默认1像素            |
| cellspacing | 像素值              | 规定单元格之间空白，默认2像素                    |
| width       | 像素值或百分比      | 规定表格宽度                                     |



##### 表格结构标签

因为表格可能很长，为了更好表示表格语义，将表格分割成表格头部和表格主体两大部分

- `<thead></thead>`表示表格头部区域
- `<tbody></tbody>`表示表格主体区域



##### 合并单元格

- 跨行合并：rowspan="合并行单元格个数"
- 跨列合并：colspan=“合并列单元格个数”

 ***合并代码***

- 跨行：将要合并的最上面一格单元格作为目标单元格，写合并代码
- 跨列：将要合并的最左侧单元格作为目标单元格，写合并代码
- 删除多余的单元格，除目标单元格以外被合并的单元格代码都删除

```html
<table border="1" width="500" height="200">
        <tr>
            <td></td>
            <td colspan="2"></td>
            <!--删除了合并之后多余的td-->
        </tr>

        <tr>
            <td></td>
            <td rowspan="2"></td>
            <td></td>
          
        </tr>
        <tr>
            <td></td>
        <!--删除了合并之后多余的td-->
            <td></td>
        </tr>
    </table>
```

![image-20220104135735970](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220104135735970.png)

#### 10.列表标签

- 无序列表（unordered list）

  ```html
  <ul>
      <li>列表项1</li>
      <li>列表项2</li>
      。。。。。
  </ul>
  ```

  ```
  ul只能放li标签，但是li标签可以容纳其他元素
  ```

  

- 有序列表

  ```html
  <ol>
      <li></li>
  </ol>
  ```

  

- 自定义列表

```html
<dl><!--definition list-->
    <dt>名词1</dt> <!--definition term-->
    <dd>名词解释1</dd><!--definition description-->
    <dd>名词解释2</dd>
</dl>
```



#### 11.表单标签

 **表单域**

是一个包含表单元素的区域

```html
<form>
    <!--会把范围内的表单元素信息提交给服务器-->
</form>
```

常用属性：

| 属性   | 属性值   | 作业                                            |
| ------ | -------- | ----------------------------------------------- |
| action | url地址  | 用于指定接收并处理表单数据的服务器程序的url地址 |
| method | get/post | 用于设置表单数据的提交方式，其取值为get或post   |
| name   | 名称     | 用于指定表单名称，以区分一个页面中多个表单域    |

**表单元素**

- **input** 输入表单(单标签)

  `type`属性用来确立输入的数据类型

  | 属性值   | 描述                                                 |
  | -------- | ---------------------------------------------------- |
  | button   | 点击按钮                                             |
  | checkbox | 定义复选框                                           |
  | file     | 输入字段和“浏览按钮”，供文件上传                     |
  | hidden   | 隐藏输入字段                                         |
  | image    | 图像形式提交按钮                                     |
  | password | 密码字段                                             |
  | radio    | 单选按钮(*多个单选按钮必须拥有相同name才能归为一组*) |
  | reset    | 重置按钮，会清楚表单中所有数据                       |
  | submit   | 提交按钮                                             |
  | text     | 单行输入字段，用户可以输入文本，默认20个字符         |

  `其他属性`

  | 属性        | 属性值     | 描述                                       |
  | ----------- | ---------- | ------------------------------------------ |
  | name        | 用户自定义 | 定义input元素名称                          |
  | value       | 用户定义   | 文本输入框默认值，同时也是按钮显示的文字值 |
  | checked     | checked    | 规定此input元素首次加载应被选中            |
  | maxlength   | 正整数     | 输入字段中字符最大长度                     |
  | placeholder | 自定义     | hint 提示符                                |

  

  **label标签**

  用于绑定一个表单元素，当点击label标签内文本时，浏览器自动将焦点转到对应表单元素上面，增加用户体验

  ```html
  <label for="sex">男</label>
  <input id="sex"  type="radio"/>
  ```

  

- **select** 下拉表单

  ```html
  <select>
      <option>选项1</option>
      <option>选项2</option>
      <option selected="selected">选项3</option>
  </select>
  ```

  

- **textarea**文本域表单

当需要输入的文本内容很多的时候，就需要文本域

| 属性      | 值        | 描述                           |
| --------- | --------- | ------------------------------ |
| autofocus | autofocus | 页面加载后文本区域自动获取焦点 |
| cols      | number    | 文本区内宽度（列数）           |
| disabled  | disabled  | 禁用该文本域                   |
| maxlength | number    | 文本域最大数字数               |
| name      | text      | 文本域名称                     |
| rows      | number    | 文本域的高度（行数）           |

#### 12.button标签

```html
<button type="button">Click Me!</button>
```

> **重要事项：**如果在 HTML 表单中使用 button 元素，不同的浏览器会提交不同的值。Internet Explorer 将提交 <button> 与 < /button >之间的文本，而其他浏览器将提交 value 属性的内容。请在 HTML 表单中使用 [**input 元素**]来创建按钮。





## 二、H5新特性

增加了一些新的标签、新的表单和新的表单属性等。

但是会有兼容性问题，IE9+以上版本浏览器才支持

### 1.H5新增语义化标签

> - < header>：头部标签   
> - < nav >:导航标签
> - < article > 内容标签
> - < section >:定义文档某个区域
> - < aside >:侧边栏标签
> - < footer >:尾部标签

![image-20220113171205544](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220113171205544.png)



### 2.多媒体标签

- 音频：< audio >

  ```html
  <audio src="文件地址" controls="controls"></audio>
  ```

  ![image-20220113181403928](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220113181403928.png)

- 视频：< video >

  ```html
  <!--为了防止有些浏览器不支持mp4格式，那么就要添加多个格式备用-->
  <video src="文件地址" controls="controls">
  	<source src="movie.mp4" type="video/mp4">
      <source src="movie.ogg" type="video/ogg">
  </video>
  ```

  ![image-20220113180645866](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220113180645866.png)

### 3.新增表单

![image-20220113181830208](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220113181830208.png)

#### 新增属性：

![image-20220113182051907](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220113182051907.png)

### 4.< template >标签

html中的`template`标签中的内容在页面中不会显示。但是在后台查看页面DOM结构存在`template`标签。这是因为template标签天生不可见，它设置了`display:none;`属性。

```html
<!--当前页面只显示"我是自定义表现abc"这个内容，不显示"我是template",这是因为template标签天生不可见-->
<template><div>我是template</div></template>
<abc>我是自定义表现abc</abc>
```



**如何操作该标签：**

> - content属性:在js中template标签对应的dom对象存在content属性，对应的属性值是一个dom节点，节点的nodeName是#document-fragment。通过该属性可以获取template标签中的内容，template对象.content可以调用getElementById、querySelector、querySelectorAll方法来获取里面的子节点。
> - innerHTML:可以获取template标签中的html。
>
> ```html
> <template id="tem">
>     <div id="div1">我是template</div>
>     <div>我是template</div>
> </template>
> <script>
>     let o = document.getElementById("tem");
>     console.log(o.content.nodeName);//#document-fragment
>     console.log(o.content.querySelectorAll("div"));//NodeList(2) [div#div1, div]。得到一个类数组
>     console.log(o.content.getElementById("div1"));//<div id="div1">我是template</div>
>     console.log(o.innerHTML);//'<div id="div1">我是template</div><div>我是template</div>'
> </script>
> ```



**在Vue中的template：**

> - 如果template标签在vue实例绑定的元素内部
>
>   > 它是可以显示template标签中的内容，但是查看后台的dom结构不存在template标签。
>   >
>   > ```html
>   > <div id="app">
>   >     <!--此处的template标签中的内容显示并且在dom中不存在template标签-->
>   >     <template>
>   >         <div>我是template</div>
>   >         <div>我是template</div>
>   >     </template>
>   > </div>
>   > <!--此处的template标签中的内容在页面中不显示，但是在dom结构存在该标签及内部结构-->
>   > <template id="tem">
>   >     <div id="div1">我是template</div>
>   >     <div>我是template</div>
>   > </template>
>   > <script src="node_modules/vue/dist/vue.js"></script>
>   > <script>
>   >     let vm = new Vue({
>   >         el: "#app",
>   >     });
>   > </script>
>   > ```
>   >
>   > ```
>   > 注意： vue实例绑定的元素内部的template标签不支持v-show指令，即v-show="false"对template标签来说不起作用。但是此时的template标签支持v-if、v-else-if、v-else、v-for这些指令。
>   > ```
>   >
>   > 



**在Vue中template属性：**

> 将实例中template属性值进行编译，并将编译后的dom替换掉vue实例绑定的元素，如果该vue实例绑定的元素中存在内容，这些内容会直接被覆盖。
> 特点：
>
> 1）如果vue实例中有template属性，会将该属性值进行编译，将编译后的虚拟dom直接替换掉vue实例绑定的元素（即el绑定的那个元素）；
> 2）template属性中的dom结构只能有一个根元素，如果有多个根元素需要使用v-if、v-else、v-else-if设置成只显示其中一个根元素；
> 3）在该属性对应的属性值中可以使用vue实例data、methods中定义的数据。
>
> ```html
> <!--此处页面显示hello-->
> <div id="app"></div>
> <!--此处template标签必须在vue绑定的元素外面定义，并且在页面中不显示下面的template标签中的内容-->
> <template id="first">
>     <div v-if="flag">{{msg}}<div>
>     <div v-else>111<div>
> </template>
> <script src="./node_modules/vue/dist/vue.js"></script>
> <script>
>     let vm = new Vue({
>         el:"#app",
>         data:{
>             msg:"hello",
>             flag:true
>         },
>         template:"#first"//通过该属性可以将自定义的template属性中的内容全部替换app的内容，并且会覆盖里面原有的内容，并且在查看dom结构时没有template标签
>     });
> </script>
> ```
>
> 





> 参考：https://blog.csdn.net/u010510187/article/details/100356624

## 三、元素显示模式

### 1.块元素

常见的有`<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>等`

特点：

- 独占一行
- 高度、宽度、外边距、内边距都可以控制
- **宽度**默认是容器(父级宽度)的100%
- 是一个容器及盒子，里面可以放行级或块级元素

### 2.行内元素

常见有`<a> <strong> <b> <em> <i> <del> <s> <ins> <u> <span>等`

**特点：**

- 相邻行内元素在一行可以显示多个
- 直接设置宽、高是无效的(wrapcontent)
- 默认宽度就是它本身内容宽度
- 行内元素只能容纳文本或其他行内元素

### 3.行内块元素

常见`<img/> <input/> <td/>`

**特点：**

- 和相邻行内元素在一行上，但是他们之间会有空白缝隙。一行可以显示多个
- 默认宽度是本身内容宽度
- 但是可以调整宽高、内外边距



### 4.元素显示模式转换

**(1)转换为块元素：**

由于行内元素不能设置宽度和高度，那么可以通过设置block属性将行内元素转换为块元素

```css
display: block;
```



**(2)转换为行内元素：**

可以将块级元素转换为行内元素，例如div

```css
display: inline;
```



**(3)转换为行内块元素：**

```css
display: inline-block;
```

