# JS基础语法

## 一、基本使用

在 HTML 中，JavaScript 代码必须位于 *< script> 与 < /script >* 标签之间。



**(1)位置：**

能够在 HTML 文档中放置任意数量的脚本。

脚本可被放置与 HTML 页面的 < body > 或 < head > 部分中，或兼而有之。

```css
<!DOCTYPE html>
<html>
<body> 

<h1>A Web Page</h1>
<p id="demo">一个段落</p>
<button type="button" onclick="myFunction()">试一试</button>

<script>
function myFunction() {
   document.getElementById("demo").innerHTML = "段落被更改。";
}
</script>

</body>
</html>
```

> <font color="red">把脚本置于 **< body >** 元素的底部，可改善显示速度，因为脚本编译会拖慢显示。</font>



**(2)外部脚本**

如需使用外部脚本，请在 <script> 标签的 src (source) 属性中设置脚本的名称：

```html
<script src="myScript.js"></script>
```

> 在 script 标签中使用 defer="true"。defer 属性规定了脚本应该在页面完成解析后执行，但它只适用于外部脚本。



**(3)输出**

**JavaScript 不提供任何内建的打印或显示函数。**

JavaScript 能够以不同方式“显示”数据：

- 使用 **window.alert()** 写入警告框

  > 使用警告框来显示数据
  >
  > ```html
  > <!DOCTYPE html>
  > <html>
  > <body>
  > 
  > <h1>我的第一张网页</h1>
  > 
  > <p>我的第一个段落</p>
  > 
  > <script>
  > window.alert(5 + 6);
  > </script>
  > 
  > </body>
  > </html> 
  > ```
  >
  > ![image-20220115170312091](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220115170312091.png)

- 使用 **document.write()** 写入 HTML 输出

  > ```html
  > <p>我的第一个段落</p>
  > 
  > <script>
  > document.write(5 + 6);
  > </script>
  > ```
  >
  > **出于测试目的，使用 document.write() 比较方便：**
  >
  > ![image-20220115165634584](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220115165634584.png)
  >
  > <font color="red">**注意：**在 HTML 文档完全加载后使用 **document.write()** 将*删除所有已有的 HTML* ：</font>
  >
  > ```html
  > <!DOCTYPE html>
  > <html>
  > <body>
  > 
  > <h2>我的第一张网页</h2>
  > <p>我的第一个段落。</p>
  > 
  > <button type="button" onclick="document.write(5 + 6)">试一试</button>
  > 
  > </body>
  > </html>
  > ```
  >
  > 

  

- 使用 **innerHTML** 写入 HTML 元素

  > 如需访问 HTML 元素，JavaScript 可使用 document.getElementById(id) 方法。
  >
  > **id 属性定义 HTML 元素。innerHTML 属性定义 HTML 内容**：
  >
  > ```html
  > <p id="demo"></p>
  > 
  > <script>
  >  document.getElementById("demo").innerHTML = 5 + 6;
  > </script>
  > ```
  >
  > 更改 HTML 元素的 innerHTML 属性是在 HTML 中显示数据的常用方法。

  

- 使用 **console.log()** 写入浏览器控制台

  > 在浏览器中，可使用 console.log() 方法来显示数据。
  >
  > 通过 F12 来激活浏览器控制台，并在菜单中选择“控制台”。
  >
  > ```html
  > <!DOCTYPE html>
  > <html>
  > <body>
  > 
  > <h1>我的第一张网页</h1>
  > 
  > <p>我的第一个段落</p>
  > 
  > <script>
  > console.log(5 + 6);
  > </script>
  > 
  > </body>
  > </html>
  > ```
  >
  > ![image-20220115170420780](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220115170420780.png)

## 二、基本语法

### 1.语法

![image-20220115171431838](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220115171431838.png)



**(1)类型：**

JS是一种弱类型语言，所有变量定义都使用var.



**(2)标识符：**

构造变量名称（唯一标识符）的通用规则是：

- 名称可包含字母、数字、下划线和美元符号
- 名称必须以字母开头
- 名称也可以 $ 和 _ 开头
- 名称对大小写敏感（y 和 Y 是不同的变量）
- 保留字（比如 JavaScript 的关键词）无法用作变量名称



**(3)变量：**

**Value = undefined**

在计算机程序中，被声明的变量经常是不带值的。值可以是需被计算的内容，或是之后被提供的数据，比如数据输入。

不带有值的变量，它的值将是 undefined。

变量 carName 在这条语句执行后的值是 undefined：

> ```js
> var carName;
> ```

>  **提升**
>
> 通过 var 声明的变量会*提升*到顶端。如果您不了解什么是提升（Hoisting），请学习我们的提升这一章。
>
> 您可以在声明变量之前就使用它：
>
> ```js
> carName = "Audi";
> document.getElementById("demo").innerHTML = carName;
> var carName;
> ```







##### **(4)Let 和 const**

ES2015 引入了两个重要的 JavaScript 新关键词：let 和 const。

这两个关键字在 JavaScript 中提供了块作用域（*Block Scope*）变量（和常量）。

在 ES2015 之前，JavaScript 只有两种类型的作用域：*全局作用域*和*函数作用域*。

> ### JavaScript 块作用域
>
> 通过 var 关键词声明的变量没有块*作用域*。
>
> 在块 *{}* 内声明的变量可以从块之外进行访问。
>
> ```js
> { 
>   var x = 10; 
> }
> // 此处可以使用 x
> ```
>
> **在 ES2015 之前，JavaScript 是没有块作用域的。**
>
> 可以使用 let 关键词声明拥有块作用域的变量。
>
> 在块 *{}* 内声明的变量无法从块外访问：
>
> ```js
> { 
>   let x = 10;
> }
> // 此处不可以使用 x
> ```

> #### 重新声明变量
>
> 使用 var 关键字重新声明变量会带来问题。
>
> 在块中重新声明变量也将重新声明块外的变量：
>
> ```js
> var x = 10;
> // 此处 x 为 10
> { 
>   var x = 6;
>   // 此处 x 为 6
> }
> // 此处 x 为 6
> ```
>
> 使用 let 关键字重新声明变量可以解决这个问题。
>
> 在块中重新声明变量不会重新声明块外的变量：
>
> ```js
> var x = 10;
> // 此处 x 为 10
> { 
>   let x = 6;
>   // 此处 x 为 6
> }
> // 此处 x 为 10
> ```

**!!!在相同的作用域，或在相同的块中，通过 let 重新声明一个 var 变量是不允许的：**

```js
var x = 10;       // 允许
let x = 6;       // 不允许

{
  var x = 10;   // 允许
  let x = 6;   // 不允许
}
```

在**不同**的作用域或块中，通过 let 重新声明变量是允许的：

```js
let x = 6;       // 允许

{
  let x = 7;   // 允许
}

{
  let x = 8;   // 允许
}
```



**const**

通过 const 定义的变量与 let 变量类似，但不能重新赋值：

它没有定义常量值。它定义了对值的常量引用。

因此，我们不能更改常量原始值，但我们可以更改常量对象的属性。

```js
// 可以创建 const 对象：
const car = {type:"porsche", model:"911", color:"Black"};

// 可以更改属性：
car.color = "White";

// 可以添加属性：
car.owner = "Bill";
```

但无法为常量对象赋值

```js
const car = {type:"porsche", model:"911", color:"Black"};
car = {type:"Volvo", model:"XC60", color:"White"};    // ERROR
```

`const和let都不能在声明前使用;`

**var声明的变量没有块作用域**

```js
var cars = ["Saab", "Volvo", "BMW"];
// 此处 cars[0] 为 "Saab"
{
  var cars = ["Toyota", "Volvo", "BMW"];
  // 此处 cars[0] 为 "Toyota"
}
// 此处 cars[0] 为 "Toyota"
```





**(5)HTML中全局变量**

使用 JavaScript 的情况下，全局作用域是 JavaScript 环境。

在 HTML 中，全局作用域是 window 对象。

> - 通过 var 关键词定义的全局变量属于 window 对象：
>
> ```html
> <script>
> var carName = "Audi";
> 
> // 此处的代码能够使用 window.carName
> document.getElementById("demo").innerHTML = "我可以显示 " + window.carName;
> </script>
> ```
>
> - 通过 let 关键词定义的全局变量不属于 window 对象：
>
>   ```js
>   let carName = "porsche";
>   // 此处的代码不可使用 window.carName
>   ```



### **2.运算符：**

**字符串和数字的相加**

一个数字和一个字符串相加将返回一个字符串：

```js
y = "7" + 8;
z = "Hello" + 7;
```



**类型运算符：**

| 运算符     | 描述                                  |
| ---------- | ------------------------------------- |
| typeof     | 返回变量的类型。                      |
| instanceof | 返回 true，如果对象是对象类型的实例。 |

```js
typeof ""                  // 返回 "string"
typeof "Bill"              // 返回 "string"
typeof "Bill Gates"          // 返回 "string"
```







**比较运算符：**

如果将字符串与数字进行比较，那么在做比较时 JavaScript 会把字符串转换为数值。空字符串将被转换为 0。非数值字符串将被转换为始终为 false 的 NaN。

![image-20220115175140655](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220115175140655.png)





**位运算符：**

![image-20220115175327045](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220115175327045.png)

**JavaScript 使用 32 位按位运算数**

JavaScript 将数字**存储为 64 位浮点数**，但所有按**位运算都以 32 位二进制数执行**。

在执行位运算之前，JavaScript 将数字转换为 32 位有符号整数。

执行按位操作后，结果将转换回 64 位 JavaScript 数。

> 使用 4 位无符号二进制数。所以 ~ 5 返回 10。
>
> 由于 JavaScript 使用 32 位有符号整数，JavaScript 将返回 -6。
>
> 0000000000000000000000000000**0101** (5)
>
> 1111111111111111111111111111**1010** (~5 = -6)
>
> 解释：
>
> *5 二进制 101，补满 32位*
> *00000000000000000000000000000101*
> *按位取反*
> *11111111111111111111111111111010*
> *由于32位开头第一个是1，所以这是一个负数，将二进制转换成负数，需要先反码*
> *00000000000000000000000000000101*
> *之后，再+1*
> *00000000000000000000000000000110*
> *转换成十进制为6，加上符号变成负数 -6*





**算数运算符：**

![image-20220115180231561](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220115180231561.png)



### 3.数据类型：

#### 1)数组

##### 声明方式

JavaScript 数组用方括号书写。

数组的项目由逗号分隔。

下面的代码声明（创建）了名为 cars 的数组，包含三个项目（汽车品牌）：

```js
var cars = ["Porsche", "Volvo", "BMW"];
```

**(1)使用对象声明**

```js
var cars = new Array("Saab", "Volvo", "BMW");
```



##### 数组方法：

**(1)长度**

length 属性返回数组的长度（数组元素的数目）。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.length;                       // fruits 的长度是 4
```



**(2)遍历数组**

 Array.foreach() 函数：

```js
var fruits, text;
fruits = ["Banana", "Orange", "Apple", "Mango"];

text = "<ul>";
fruits.forEach(myFunction);
text += "</ul>";

function myFunction(value) {
  text += "<li>" + value + "</li>";
}
```



**(3)添加、删除元素**

- 向数组添加新元素的最佳方法是使用 push() 方法：

  **①push() 方法返回新数组的长度：**

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Lemon");                // 向 fruits 添加一个新元素 (Lemon)
```

②也可以使用 length 属性向数组添加新元素：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits[fruits.length] = "Lemon";     // 向 fruits 添加一个新元素 (Lemon)
```

如果添加下标远远超过数组长度，则其间的内容都是undefined

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits[6] = "Lemon";                 // 向 fruits 添加一个新元素 (Lemon)
```

![image-20220116163750917](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220116163750917.png)

③unshift() 方法（在开头）向数组添加新元素，并“反向位移”旧元素：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.unshift("Lemon");    // 向 fruits 添加新元素 "Lemon"

unshift() 方法返回新数组的长度。
```



- pop() 方法从数组中**删除最后一个元素**：

  **①pop() 方法返回“被弹出”的值：**

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.pop();              // 从 fruits 删除最后一个元素（"Mango"）
```

​       ②shift() 方法会**删除首个**数组元素，并把所有其他元素“位移”到更低的索引。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.shift();            // 从 fruits 删除第一个元素 "Banana"
```

​	③delete 运算符来*删除*：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
delete fruits[0];           // 把 fruits 中的首个元素改为 undefined
```

​	④splice() 方法可用于向数组添加新项：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 2, "Lemon", "Kiwi");
/*第一个参数（2）定义了应添加新元素的位置（拼接）。

第二个参数（2）定义应删除多少元素。

其余参数（“Lemon”，“Kiwi”）定义要添加的新元素。

splice() 方法返回一个包含已删除项的数组：*/
```

![image-20220116164831455](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220116164831455.png)









**(4)识别数组**

- ECMAScript 5 定义了新方法 Array.isArray()：

```js
Array.isArray(fruits);     // 返回 true
```

- 创建您自己的 isArray() 函数以解决此问题：

  ```js
  function isArray(x) {
      return x.constructor.toString().indexOf("Array") > -1;
  }
  ```

- 假如对象由给定的构造器创建，则 *instanceof* 运算符返回 true：

  ```js
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
   
  fruits instanceof Array     // 返回 true
  ```



**(5)结合数组为字符串**

- join() 方法也可将所有数组元素结合为一个字符串。

它的行为类似 toString()，但是您还可以规定分隔符：

```js
var fruits = ["Banana", "Orange","Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.join(" * "); 
```



**(6)合并数组**

concat() 方法通过合并（连接）现有数组来创建一个新数组：

```
var myGirls = ["Cecilie", "Lone"];
var myBoys = ["Emil", "Tobias", "Linus"];
var myChildren = myGirls.concat(myBoys);   // 连接 myGirls 和 myBoys
```





**(7)slice裁剪数组**

slice() 可接受两个参数，比如 (1, 3)。

该方法会从开始参数选取元素，直到结束参数（不包括）为止。

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1, 3); 
```





**(8)数组排序**

sort() 方法以字母顺序对数组进行排序：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();            // 对 fruits 中的元素进行排序
```

> sort() 函数按照*字符串*顺序对值进行排序。
>
> 该函数很适合字符串（"Apple" 会排在 "Banana" 之前）。
>
> 不过，如果数字按照字符串来排序，则 "25" 大于 "100"，因为 "2" 大于 "1"。
>
> 正因如此，**sort() 方法在对数值排序时会产生不正确的结果。**
>
> 我们通过一个*比值函数*来修正此问题：
>
> ```js
> var points = [40, 100, 1, 5, 25, 10];
> points.sort(function(a, b){return a - b}); 
> ```

**什么是比值函数**

比较函数的目的是定义另一种排序顺序。

比较函数应该返回一个负，零或正值，这取决于参数：

```js
function(a, b){return a-b}

/*当比较 40 和 100 时，sort() 方法会调用比较函数 function(40,100)。

该函数计算 40-100，然后返回 -60（负值）。

排序函数将把 40 排序为比 100 更低的值。
*/
```

当 sort() 函数比较两个值时，会将值发送到比较函数，并根据所返回的值（负、零或正值）对这些值进行排序。

**排序对象数组**

> ```js
> var cars = [
> {type:"Volvo", year:2016},
> {type:"Saab", year:2001},
> {type:"BMW", year:2010}];
> ```
>
> 即使对象拥有不同数据类型的属性，sort() 方法仍可用于对数组进行排序。
>
> 解决方法是通过比较函数来对比属性值：
>
> ```js
> cars.sort(function(a, b){return a.year - b.year});
> ```







**(9)查找Max、Min**

- 对数组使用 Math.max()

您可以使用 Math.max.apply 来查找数组中的最高值：

```js
function myArrayMax(arr) {
    return Math.max.apply(null, arr);
}
```

- 最快的解决方法是使用“自制”方法。

  此函数遍历数组，用找到的最高值与每个值进行比较：

```js
function myArrayMax(arr) {
    var len = arr.length
    var max = -Infinity;
    while (len--) {
        if (arr[len] > max) {
            max = arr[len];
        }
    }
    return max;
}
```





**(10)反转**

reverse() 方法反转数组中的元素。

可以使用它以降序对数组进行排序：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();            // 对 fruits 中的元素进行排序
fruits.reverse();         // 反转元素顺序
```



**(11)迭代**

- **forEach()** 方法为每个数组元素调用一次函数（回调函数）。

```js
var txt = "";
var numbers = [45, 4, 9, 16, 25];
numbers.forEach(myFunction);

function myFunction(value, index, array) {
  txt = txt + value + "<br>"; 
}

/*或者*/
function myFunction(value) {
  txt = txt + value + "<br>"; 
}
```

| 参数  | 意义     |
| ----- | -------- |
| value | 项目值   |
| index | 索引     |
| array | 数组本身 |

- **map()**

  map() 方法通过对每个数组元素执行函数来创建新数组。

  map() 方法不会对没有值的数组元素执行函数。

  map() 方法不会更改原始数组。

  ```js
  var numbers1 = [45, 4, 9, 16, 25];
  var numbers2 = numbers1.map(myFunction);
  
  /*可以省略后面两个参数*/
  function myFunction(value, index, array) {
    return value * 2;
  }
  ```

- **filter()** 方法创建一个包含通过测试的数组元素的新数组。

  这个例子用值大于 18 的元素创建一个新数组：

```js
var numbers = [45, 4, 9, 16, 25];
var over18 = numbers.filter(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```

```
45,25
```

- **reduce()** 方法在每个数组元素上运行函数，以生成（减少它）单个值。（从左至右）

```js
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduce(myFunction);

function myFunction(total, value, index, array) {
  return total + value;
}

结果返回总和：99
```

- **reduceRight()** 方法在每个数组元素上运行函数，以生成（减少它）单个值。**从右到左**

- **every()** 方法检查所有数组值是否通过测试。

  ```js
  var numbers = [45, 4, 9, 16, 25];
  var allOver18 = numbers.every(myFunction);
  
  /*检查是否所有都大于18*/
  function myFunction(value, index, array) {
    return value > 18;
  }
  
  结果：false
  ```

- **some()** 方法检查某些数组值是否通过了测试。

  ```js
  var numbers = [45, 4, 9, 16, 25];
  var someOver18 = numbers.some(myFunction);
  
  function myFunction(value, index, array) {
    return value > 18;
  }
  ```

- **find()** 方法返回通过测试函数的**第一个数组元素**的值。

  ```js
  var numbers = [4, 9, 16, 25, 29];
  var first = numbers.find(myFunction);
  
  /*找到第一个大于18的数组元素*/
  function myFunction(value, index, array) {
    return value > 18;
  }
  
  结果：25
  ```

- **findIndex()** 方法返回通过测试函数的第一个数组元素的**索引。**





##### 数组常量：

- 用const声明**数组不能重新赋值**

  > ```js
  > const cars = ["Saab", "Volvo", "BMW"];
  > cars = ["Toyota", "Volvo", "Audi"];    // 错误
  > ```

- 用const声明数组的**元素可以更改**

  > ```js
  > // 您可以创建常量数组：
  > const cars = ["Saab", "Volvo", "BMW"];
  > 
  > // 您可以更改元素：
  > cars[0] = "Toyota";
  > 
  > // 您可以添加元素：
  > cars.push("Audi");
  > ```

- const变量声明时必须赋值

  ```js
  const cars = ["Saab", "Volvo", "BMW"];
  
  ```

  





#### 2)对象：

JavaScript 对象用花括号来书写。

对象属性是 *name*:*value* 对，由逗号分隔。

```js
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
```

> 对象具有属性和方法：
>
> - 属性：
>
>   ```js
>   var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
>   ```
>
> - 方法：
>
>   方法是作为属性存储的函数
>
>   ```js
>   var person = {
>     firstName: "Bill",
>     lastName : "Gates",
>     id       : 678,
>     fullName : function() {
>       return this.firstName + " " + this.lastName;
>     }
>   };
>   ```

**①对象访问：**

```js
objectName.propertyName
/*或者*/
objectName["propertyName"]
/*或者*/
const myArray = Object.values(person);//通过使用 Object.values()，任何 JavaScript 对象都可以被转换为数组：
```



在 JavaScript 中，几乎“所有事物”都是对象。

- 布尔是对象（如果用 *new* 关键词定义）
- 数字是对象（如果用 *new* 关键词定义）
- 字符串是对象（如果用 *new* 关键词定义）
- 日期永远都是对象
- 算术永远都是对象
- 正则表达式永远都是对象
- 数组永远都是对象
- 函数永远都是对象
- 对象永远都是对象

**②对象遍历**

```js
for (variable in object) {
    要执行的代码
}js
```



**③删除属性**

delete 关键词从对象中**删除属性**：

delete 操作符被设计用于对象属性。它对变量或函数没有影响。

delete 操作符不应被用于预定义的 JavaScript 对象属性。这样做会使应用程序崩溃。

```js
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
delete person.age;   // 或 delete person["age"];
```



**④getter/setter**

使用getter

```js
// 创建对象：
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "en",
  get lang() {
    return this.language;
  },
  set lang(lang) {
    this.language = lang;
  }
};

// 使用 getter 来显示来自对象的数据：
document.getElementById("demo").innerHTML = person.lang;
```



使用setter

```js
class Car {
  constructor(brand) {
    this._carname = brand;
  }
  get carname() {
    return this._carname;
  }
  set carname(x) {
    this._carname = x;
  }
}

let myCar = new Car("Ford");
myCar.carname = "Volvo";
document.getElementById("demo").innerHTML = myCar.carname;
```







**⑤Object.defineProperty() 方法也可用于添加 Getter 和 Setter：**

```js
// 定义对象
var obj = {counter : 0};

// 定义 setters
Object.defineProperty(obj, "reset", {
  get : function () {this.counter = 0;}
});
Object.defineProperty(obj, "increment", {
  get : function () {this.counter++;}
});
Object.defineProperty(obj, "decrement", {
  get : function () {this.counter--;}
});
Object.defineProperty(obj, "add", {
  set : function (value) {this.counter += value;}
});
Object.defineProperty(obj, "subtract", {
  set : function (value) {this.counter -= value;}
});

// 操作计数器：
obj.reset;
obj.add = 5;
obj.subtract = 1;
obj.increment;
obj.decrement;
```





##### Map()

| Method    | Description               |
| --------- | ------------------------- |
| new Map() | 创建新的Map对象           |
| set()     | 为Map对象中键设置值       |
| get()     | 获取Map对象键的值         |
| entries() | 返回Map对象键值对实体数组 |
| keys()    | 返回键数组                |
| values()  | 返回值数组                |
| size      | 获取Map的元素数量         |
| clear()   | 删除Map中所有元素         |
| delete()  | 删除由键指定元素          |
| has(key)  | 如果键存在，返回true      |
| forEach() | 迭代                      |



##### Set()

Set 是唯一值的集合。

每个值在 Set 中只能出现一次。

一个 Set 可以容纳任何数据类型的任何值。

| 方法/属性 | 描述                |
| --------- | ------------------- |
| add()     | 添加新元素          |
| clear()   | 删除所有元素        |
| delete()  | 删除指定元素        |
| entries() | 返回值数组          |
| has()     | 存在值返回true      |
| forEach() | 为每个元素调用回调  |
| keys()    | 返回Set对象中值数组 |
| values()  | 与keys()相同        |
| size      | 返回元素计数        |



##### ES6增强写法

```js
    const name = 'yht';
    const age = 12;

    //ESC6
    const obj = {
        name,
        age
    };

    //ESC5
    // const obj = {
    //     name: name,
    //     age: age
    // };
```

![image-20220127140154372](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127140154372.png)





#### 3)Undefined和Null

- Undefined

在 JavaScript 中，没有值的变量，其值是 undefined。typeof 也返回 undefined。

```js
var person;                  // 值是 undefined，类型是 undefined。
```



- Null

在 JavaScript 中，null 的数据类型是对象。

您可以把 null 在 JavaScript 中是对象理解为一个 bug。它本应是 null。

您可以通过设置值为 null 清空对象：

```js
var person = null;           // 值是 null，但是类型仍然是对象
```

您也可以通过设置值为 undefined 清空对象：

```js
var person = undefined;           // 值是 undefined，类型是 undefined。
```



**Undefined 与 null 的值相等，但类型不相等：**

```js
typeof undefined              // undefined
typeof null                   // object
null === undefined            // false
null == undefined             // true
```



#### 4)函数

```js
function myFunction(p1, p2) {
    return p1 * p2;              // 该函数返回 p1 和 p2 的乘积
}
```



##### call()方法

它可以用来调用所有者对象作为参数的方法。

通过 call()，能够使用属于另一个对象的方法。

> 本例调用 person 的 fullName 方法，并用于 person1：
>
> ```js
> var person = {
>     fullName: function() {
>         return this.firstName + " " + this.lastName;
>     }
> }
> var person1 = {
>     firstName:"Bill",
>     lastName: "Gates",
> }
> 
> person.fullName.call(person1);  // 将返回 "Bill Gates"
> ```



##### apply()

apply() 方法与 call() 方法非常相似：

> 不同之处是：
>
> call() 方法分别接受参数。
>
> apply() 方法接受数组形式的参数。
>
> ```js
> person.fullName.apply(person1, ["Oslo", "Norway"]);
> ```
>
> ```js
> person.fullName.call(person1, "Oslo", "Norway");
> ```



**apply函数详解**

```
functionObject.apply( [ thisObj [, argsArray ]] )
```

> `apply()`函数用于调用当前函数`functionObject`，并可同时使用指定对象`thisObj`作为本次函数执行时函数内部的`this`指针引用。



**应用：**

> 由于js的数组没有max()方法，所以可以使用apply
>
> ```js
> Math.max.apply(null, [1,2,3]); // 也会返回 3
> 
> Math.max.apply(Math, [1,2,3]); // 也会返回 3
> 
> Math.max.apply(" ", [1,2,3]); // 也会返回 3
> ```
>
> 



##### !!闭包!!

通过闭包(自调用函数)来实现计数器，是的外部无法干涉该计数器的counter变量

```js
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();

add();
add();
add();

// 计数器目前是 3 
```

> **解释**
>
> 变量 add 的赋值是自调用函数的返回值。
>
> 这个自调用函数只运行一次。它设置计数器为零（0），并返回函数表达式。
>
> 这样 add 成为了函数。最“精彩的”部分是它能够访问父作用域中的计数器。
>
> 这被称为 JavaScript *闭包*。它使函数拥有“*私有*”变量成为可能。
>
> 计数器被这个匿名函数的作用域保护，并且只能使用 add 函数来修改。
>
> 闭包指的是有权访问父作用域的函数，即使在父函数关闭之后。



##### ES6增强写法

```js
	//ES5
    const obj = {
        a: function(){
            ...
        }
    }
    	
	//ES6写法
    const obj = {
        a(){
            ...
        }
    }
```









#### 5)事件

事件就是HTML对JS的响应，例如：

- HTML 网页完成加载
- HTML 输入字段被修改
- HTML 按钮被点击

```html
<button onclick='document.getElementById("demo").innerHTML=Date()'>现在的时间是？</button>

<!--改变自身-->
<button onclick="this.innerHTML=Date()">现在的时间是？</button>
```

**常见事件**

| 事件        | 描述                       |
| ----------- | -------------------------- |
| onchange    | HTML元素被改变             |
| onclick     | 用户点击HTML元素           |
| onmouseover | 用户把鼠标移动到HTML元素上 |
| onmouseout  | 鼠标移开                   |
| onkeydown   | 用户按下键盘按键           |
| onload      | 浏览器完成页面加载         |



#### 6)字符串

**(1)字符串长度**

```js
var sln = txt.length;
```

**(2)转义字符**

和dart一样，单引号里面不能使用单引号，双引号里面不可以使用双引号，如果需要，使用转义字符

| 代码 | 结果       |
| ---- | ---------- |
| \ '  | '          |
| \ '' | ''         |
| \ \  | \          |
| \ b  | 退格键     |
| \ f  | 换页       |
| \ n  | 新行       |
| \ r  | 回车       |
| \ t  | 水平制表符 |
| \ v  | 垂直制表符 |

**反斜杠可以在代表字符串换行：**

```js
document.getElementById("demo").innerHTML = "Hello \
Kitty!";
```



**(3)字符串创建对象**

字符串可以通过关键字new定义为对象

```js
var firstName = new String("Bill")
```



##### 字符串方法

**(1)查找字符串出现位置**

indexOf() 方法返回字符串中指定文本首次出现的索引（位置）：

- indexOf() 方法返回字符串中指定文本*首次*出现的索引（位置）：

**从头开始检索**

```js
var str = "The full name of China is the People's Republic of China.";
var pos = str.indexOf("China");
```

- lastIndexOf() 方法返回指定文本在字符串中*最后*一次出现的索引：

**从尾开始检索**

```js
var str = "The full name of China is the People's Republic of China.";
var pos = str.lastIndexOf("China", 18);//指定起始位置
```

`如果未找到文本， indexOf() 和 lastIndexOf() 均返回 -1。`



**(2)检索字符串**

- search() 方法搜索特定值的字符串，并返回匹配的位置：

```js
var str = "The full name of China is the People's Republic of China.";
var pos = str.search("locate");
```

- match() 方法根据正则表达式在字符串中搜索匹配项，并将匹配项作为 Array 对象返回。

```js
let text = "The rain in SPAIN stays mainly in the plain";
text.match(/ain/g)    // 返回数组 [ain,ain,ain]
```

```js
string.match(regexp)
```

- includes() ：如果字符串包含指定值，includes() 方法返回 true。

```js
string.includes(searchvalue, start)
```

- 如果字符串以指定值开头，则 startsWith() 方法返回 true，否则返回 false：

  ```js
  string.startsWith(searchvalue, start)
  ```





**(3)提取字符串**

有三种提取部分字符串的方法：

- slice(*start*, *end*)

  > slice() 提取字符串的某个部分并在新字符串中返回被提取的部分。
  >
  > 这个例子裁剪字符串中位置 7 到位置 13 的片段：
  >
  > ```js
  > var str = "Apple, Banana, Mango";
  > var res = str.slice(7,13);
  > ```
  >
  > ```
  > Banana
  > ```
  >
  > 如果某个参数为负，则从字符串的结尾开始计数。
  >
  > 这个例子裁剪字符串中位置 -12 到位置 -6 的片段：

- substring(*start*, *end*)

  > substr() 类似于 slice()。
  >
  > 不同之处在于第二个参数规定被提取部分的*长度*。

- substr(*start*, *length*)

  > replace() 方法用另一个值替换在字符串中指定的值：
  >
  > ```js
  > str = "Please visit Microsoft!";
  > var n = str.replace("Microsoft", "W3School");
  > ```
  >
  > replace() 方法不会改变调用它的字符串。它返回的是新字符串。
  >
  > 默认地，replace() *只替换首个匹配*：
  >
  > 
  >
  > 如需执行大小写不敏感的替换，请使用正则表达式 /i（大小写不敏感）：
  >
  > ```js
  > str = "Please visit Microsoft!";
  > var n = str.replace(/MICROSOFT/i, "W3School");
  > ```



**(4)转换为大小写**

- 通过 toUpperCase() 把字符串转换为大写：
- 通过 toLowerCase() 把字符串转换为小写：



**(5)连接字符串**

concat() 连接两个或多个字符串：

```js
var text1 = "Hello";
var text2 = "World";
text3 = text1.concat(" ",text2);
```



**(6)trim**

trim() 方法删除字符串两端的空白符：

```js
var str = "       Hello World!        ";
alert(str.trim());
```



**(7)提取字符串字符**

- charAt(*position*)

  > charAt() 方法返回字符串中指定下标（位置）的字符串

- charCodeAt(*position*)

  > charCodeAt() 方法返回字符串中指定索引的字符 **unicode 编码**



**(8)属性访问：**

```js
var str = "HELLO WORLD";
str[0];                   // 返回 H
```

- 如果找不到字符，[ ] 返回 undefined，而 charAt() 返回空字符串。
- 它是只读的。str[0] = "A" 不会产生错误（但也不会工作！）



**(9)转换为数组**

可以通过 split() 将字符串转换为数组：

```js
var txt = "a,b,c,d,e";   // 字符串
txt.split(",");          // 用逗号分隔
txt.split(" ");          // 用空格分隔
txt.split("|");          // 用竖线分隔
```



##### 字符串模板

- *模板字面量*使用反引号 (``) 
- 通过使用*模板字面量*，可以在字符串中同时使用单引号和双引号：
- *模板字面量*提供了一种将变量和表达式插入字符串的简单方法。该方法称为字符串插值（string interpolation）。

```js
let firstName = "John";
let lastName = "Doe";

let text = `Welcome ${firstName}, ${lastName}!`;
```

- *模板字面量*允许字符串中的表达式：

```js
let price = 10;
let VAT = 0.25;

let total = `Total: ${(price * (1 + VAT)).toFixed(2)}`;
```



##### 数字字符串

在所有数字运算中，JavaScript 会尝试将字符串转换为数字：

```js
var x = "100";
var y = "10";
var z = x / y;       // z 将是 10
```

**除了+以外，因为+对于字符串是级联运算符**

```js
var x = "100";
var y = "10";
var z = x + y;       // z 不会是 110（而是 10010）
```





#### 7)数值

**①NaN - 非数值**

NaN 属于 JavaScript 保留词，指示某个数不是合法数。

尝试用一个非数字字符串进行除法会得到 NaN（Not a Number）：

可使用全局 JavaScript 函数 isNaN() 来确定某个值是否是数：

```js
var x = 100 / "Apple";
isNaN(x);               // 返回 true，因为 x 不是数
```

`NaN 是数，typeof NaN 返回 number：`



**②Infinity**

Infinity （或 -Infinity）是 JavaScript 在计算数时超出最大可能数范围时返回的值。

```js
var myNumber = 2;

while (myNumber != Infinity) {          // 执行直到 Infinity
    myNumber = myNumber * myNumber;
}
```

`Infinity 是数：typeOf Infinity 返回 number。`



**③进制转换**

默认地，Javascript 把数显示为十进制小数。

但能够使用 toString() 方法把数输出为十六进制、八进制或二进制。

```js
var myNumber = 128;
myNumber.toString(16);     // 返回 80
myNumber.toString(8);      // 返回 200
myNumber.toString(2);      // 返回 10000000
```



**④数值也可以是对象：**

```js
var x = 123;
var y = new Number(123);
```



##### 数值方法：

**(1)toExponential() 方法**

toExponential() 返回字符串值，它包含已被四舍五入并**使用指数计数法**的数字。

参数定义小数点后的字符数：

```js
toExponential([n])//n可选，定义小数点后面位数
```



```js
var x = 9.656;
x.toExponential(2);     // 返回 9.66e+0
x.toExponential(4);     // 返回 9.6560e+0
x.toExponential(6);     // 返回 9.656000e+0
```



**(2)toFixed() 方法**

toFixed() 返回字符串值，它包含了指定位数小数的数字：

```js
var x = 9.656;
x.toFixed(0);           // 返回 10
x.toFixed(2);           // 返回 9.66
x.toFixed(4);           // 返回 9.6560
x.toFixed(6);           // 返回 9.656000
```



**(3)toPrecision() 方法**

toPrecision() 返回字符串值，它包含了指定长度的数字：

```js
var x = 9.656;
x.toPrecision();        // 返回 9.656
x.toPrecision(2);       // 返回 9.7
x.toPrecision(4);       // 返回 9.656
x.toPrecision(6);       // 返回 9.65600
```



**(4)valueOf() 方法**

valueOf() 以数值返回数值：

```js
var x = 123;
x.valueOf();            // 从变量 x 返回 123
(123).valueOf();        // 从文本 123 返回 123
```



**(5)将变量转换为数值**

- **Number()** 方法

  > Number() 可用于把 JavaScript 变量转换为数值：
  >
  > ```js
  > x = true;
  > Number(x);        // 返回 1
  > x = false;     
  > Number(x);        // 返回 0
  > x = new Date();
  > Number(x);        // 返回 1404568027739
  > x = "10"
  > Number(x);        // 返回 10
  > x = "10 20"
  > Number(x);        // 返回 NaN
  > ```
  >
  > Number() 还可以把日期转换为数字：
  >
  > ```js
  > Number(new Date("2019-04-15"));    // 返回 1506729600000
  > ```

- **parseInt()** 方法

  > parseInt() 解析一段字符串并返回数值。允许空格。只返回首个数字：
  >
  > ```js
  > parseInt("10");         // 返回 10
  > parseInt("10.33");      // 返回 10
  > parseInt("10 20 30");   // 返回 10
  > parseInt("10 years");   // 返回 10
  > parseInt("years 10");   // 返回 NaN
  > ```

- **parseFloat()** 方法

  > parseFloat() 解析一段字符串并返回数值。允许空格。只返回首个数字：
  >
  > ```js
  > parseFloat("10");        // 返回 10
  > parseFloat("10.33");     // 返回 10.33
  > parseFloat("10 20 30");  // 返回 10
  > parseFloat("10 years");  // 返回 10
  > parseFloat("years 10");  // 返回 NaN
  > ```



#### 8)日期

默认情况下，JavaScript 将使用浏览器的时区并将日期显示为全文本字符串：

```js
var d = new Date();

结果：Tue Apr 02 2019 09:01:19 GMT+0800 (中国标准时间)

//toDateString() 方法将日期转换为更易读的格式：
//Sun Jan 16 2022
var d = new Date();
document.getElementById("demo").innerHTML = d.toDateString();

//toUTCString() 方法将日期转换为 UTC 字符串（一种日期显示标准）。
//Sun, 16 Jan 2022 12:32:27 GMT

```

有 4 种方法创建新的日期对象：

- **new Date()**

- **new Date(year, month, day, hours, minutes, seconds, milliseconds)**

  > ```js
  > var d = new Date(2018, 11, 24, 10, 33, 30, 0);
  > 
  > JavaScript 从 0 到 11 计算月份。
  > 一月是 0。十二月是11。
  > ```

- **new Date(milliseconds)**

  > 如果只有一个参数，则默认为毫秒
  >
  > 1970年 1 月 1 日加上100 000 000 000毫秒，大约是 1973 年 3 月 3 日：
  >
  > ```js
  > var d = new Date(100000000000);
  > ```
  >
  > 一天（24 小时）是 86 400 000 毫秒。

- **new Date(date string)**

  >  从日期字符串创建一个新的日期对象：
  >
  > ```js
  > var d = new Date("October 13, 2014 11:13:00");
  > ```



##### 日期格式：

有四种 JavaScript 日期输入格式：

![image-20220116203348344](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220116203348344.png)

> 更多参考：https://www.w3school.com.cn/js/js_date_formats.asp



##### 获取时间：

| 方法              | 描述                                     |
| ----------------- | ---------------------------------------- |
| getTime()         | 方法返回自 1970 年 1 月 1 日以来的毫秒数 |
| getFullYear()     | 获取四位年(yyyy)                         |
| getMonth()        | 获取月(0-11)                             |
| getDate()         | 获取天(1-31)                             |
| getHours()        | 获取小时(0-23)                           |
| getMinute()       | 获取分(0-59)                             |
| getSeconds()      | 获取秒(0-59)                             |
| getMilliseconds() | 获取毫秒(0-999)                          |
| getDay()          | 获取星期(0-6)                            |

##### 设置时间

![image-20220116204415685](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220116204415685.png)



#### 9)数学Math：

```js
Math.PI; // 返回 3.141592653589793

Math.round(x) //返回值是 x 四舍五入为最接近的整数

Math.pow(x, y) //返回值是 x 的 y 次幂

Math.sqrt(x) //返回 x 的平方根

Math.abs(x) //返回 x 的绝对（正）值

Math.ceil(x) //的返回值是 x 上舍入最接近的整数：6.4 -> 7

Math.floor(x) //的返回值是 x 下舍入最接近的整数

Math.min() 和 Math.max() //可用于查找参数列表中的最低或最高值

Math.random() //返回介于 0（包括）与 1（不包括）之间的随机数

Math.sin(x) //返回角 x（以弧度计）的正弦

Math.cos(x) //返回角 x（以弧度计）的余弦
```

> 如果您希望使用角度替代弧度，则需要将角度转换为弧度：
>
> ```js
> Math.sin(90 * Math.PI / 180);     // 返回 1（90 度的正弦）
> ```



**随机数的巧用**

Math.random() 与 Math.floor() 一起使用用于返回随机整数。

```js
Math.floor(Math.random() * 10);		// 返回 0 至 9 之间的数
```



#### 10)**循环：**

- **for in** 语句循环遍历对象的属性：

```js
for (key in object) {
  // code block to be executed
}
```

- **for of** 语句循环遍历可迭代对象的值。

```js
const cars = ["BMW", "Volvo", "Mini"];

let text = "";
for (let x of cars) {
  text += x;
}
```



#### 11)构造器属性：

constructor 属性返回所有 JavaScript 变量的构造器函数。

```js
"Bill".constructor                 // 返回 "function String()  { [native code] }"
(3.14).constructor                 // 返回 "function Number()  { [native code] }"
false.constructor                  // 返回 "function Boolean() { [native code] }"
[1,2,3,4].constructor              // 返回 "function Array()   { [native code] }"
{name:'Bill', age:62}.constructor  // 返回" function Object()  { [native code] }"
new Date().constructor             // 返回 "function Date()    { [native code] }"
function () {}.constructor         // 返回 "function Function(){ [native code] }"
```

您可以通过检查 constructor 属性来确定某个对象是否为数组（包含单词 "Array"）：

```js
function isArray(myArray) {
    return myArray.constructor.toString().indexOf("Array") > -1;
}

/*或者更简单，您可以检查对象是否是数组函数：*/
function isArray(myArray) {
    return myArray.constructor === Array;
}
```



#### 12)正则表达式：

**(1)语法：**

```
/pattern/modifiers;
```

/w3school/i 是一个正则表达式。

w3school 是模式（pattern）（在搜索中使用）。

i 是修饰符（把搜索修改为大小写不敏感）。





**(2)应用**

正则表达式常用于两个*字符串方法*：search() 和 replace()。

search() 方法使用表达式来搜索匹配，然后返回匹配的位置。

replace() 方法返回模式被替换处修改后的字符串。

使用正则表达式执行搜索字符串中 "w3school" 的大小写不敏感的**搜索**：

```js
var str = "Visit W3School";
var n = str.search(/w3school/i); 
```

使用大小写不明的正则表达式以 W3school 来**替换**字符串中的 Microsoft：

```js
var str = "Visit Microsoft!";
var res = str.replace(/microsoft/i, "W3School"); 
```



##### RegExp对象

- **test()** 是一个正则表达式方法。

它通过模式来搜索字符串，然后根据结果返回 true 或 false。

下面的例子搜索字符串中的字符 "e"：

```js
var patt = /e/;
patt.test("The best things in life are free!"); 

结果：true

/*或者简写*/
/e/.test("The best things in life are free!");
```



- **exec()** 方法是一个正则表达式方法。

  它通过指定的模式（pattern）搜索字符串，并返回已找到的文本。

  如果未找到匹配，则返回 null。

  ```js
  /e/.exec("The best things in life are free!");
  
  结果:e
  ```

  

**(3)正则表达式修饰符**

| 修饰符 | 描述                   |
| ------ | ---------------------- |
| i      | 执行大小写不敏感的匹配 |
| g      | 执行全局匹配           |
| m      | 执行多行匹配           |



**(4)正则表达式模式**

| 表达式 | 描述                   |
| ------ | ---------------------- |
| [abc]  | 查找方括号之间任何字符 |
| [0-9]  | 查找任何0至9的数字     |
| (x\|y) | 查找由\|分隔的任何选项 |



**(5)元字符**

| 元字符 | 描述                                  |
| ------ | ------------------------------------- |
| \d     | 查找数字                              |
| \s     | 查找空白字符                          |
| \b     | 匹配单词边界                          |
| \uxxxx | 查找以十六进制数xxxx规定的unicode字符 |



**(6)Quantifiers定义量词：**

| 量词 | 描述                            |
| ---- | ------------------------------- |
| n+   | 匹配任何包含至少一个n的字符串   |
| n*   | 匹配任何包含零个或多个n的字符串 |
| n?   | 匹配任何包含零个或一个n的字符串 |



> 更多参考https://www.w3school.com.cn/jsref/jsref_obj_regexp.asp



#### 13)错误控制

可以通过try、catch、throw配合进行自定义错误

```js
var message, x;
message = document.getElementById("message");
message.innerHTML = "";
x = document.getElementById("demo").value;
try { 
    if(x == "") throw "空的";
    if(isNaN(x)) throw "不是数字";
    x = Number(x);
    if(x < 5) throw  "太小";
    if(x > 10) throw "太大";
}
catch(err) {
    message.innerHTML = "输入是 " + err;//捕获异常
}
```



##### 错误对象

error 对象提供两个有用的属性：name 和 message。

| 属性    | 描述               |
| ------- | ------------------ |
| name    | 设置或返回错误名   |
| message | 设置或返回错误消息 |



其中name属性可以返回六个不同值，也就是6种不同异常：

![image-20220117130944441](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220117130944441.png)

> - RangeError
>
>   RangeError 会在您使用了合法值的范围之外的数字时抛出。
>
>   例如：您不能将数字的有效位数设置为 500。
>
>   ```js
>   var num = 1;
>   try {
>       num.toPrecision(500);   // 数无法拥有 500 个有效数
>    }
>   catch(err) {
>       document.getElementById("demo").innerHTML = err.name;
>   } 
>   ```
>
> - ReferenceError
>
>   假如您使用（引用）了尚未声明的变量，则 ReferenceError 会被抛出：
>
>   ```js
>   var x;
>   try {
>       x = y + 1;   // y 无法被引用（使用）
>    }
>   catch(err) {
>       document.getElementById("demo").innerHTML = err.name;
>   } 
>   ```
>
> - SyntaxError
>
>   假如您计算带语法错误的代码，会 SyntaxError 被抛出：
>
>   ```js
>   try {
>       eval("alert('Hello)");   // 缺少 ' 会产生错误
>   }
>   catch(err) {
>        document.getElementById("demo").innerHTML = err.name;
>   } 
>   ```
>
> - TypeError
>
>   假如您使用的值不在期望值的范围之内，则 TypeError 被抛出：
>
>   ```js
>   var num = 1;
>   try {
>       num.toUpperCase();   // 您无法将数字转换为大写
>    }
>   catch(err) {
>       document.getElementById("demo").innerHTML = err.name;
>   } 
>   ```
>
> - URIError
>
>   假如您在 URI 函数中使用非法字符，则 URIError 被抛出：
>
>   ```js
>   try {
>       decodeURI("%%%");   // 您无法对这些百分号进行 URI 编码
>    }
>   catch(err) {
>       document.getElementById("demo").innerHTML = err.name;
>   } 
>   ```



#### 14)严格模式：

通过在脚本或函数的开头添加 "use strict"; 来声明严格模式。

在脚本开头进行声明，拥有全局作用域（脚本中的所有代码均以严格模式来执行）,换句话说就是编译前检查：

```js
"use strict";
x = 3.14;       /js/ 这会引发错误，因为 x 尚未声明
```

##### 为什么使用严格模式？

> 严格模式使我们更容易编写“安全的” JavaScript。
>
> 严格模式把之前可接受的“坏语法”转变为真实的错误。
>
> 举例来说，在普通的 JavaScript 中，错打变量名会创建新的全局变量。在严格模式中，此举将抛出错误，这样就不可能意外创建全局变量。
>
> 在普通 JavaScript 中，如果向不可写属性赋值，开发者不会得到任何错误反馈。
>
> 在严格模式中，向不可写的、只能读取的、不存在的属性赋值，或者向不存在的变量或对象赋值，将抛出错误。



#### 15)this

它拥有不同的值，具体取决于它的使用位置：

- 在方法中，this 指的是所有者对象。

  > ```js
  > var person = {
  >   firstName: "Bill",
  >   lastName : "Gates",
  >   id       : 678,
  >   fullName : function() {
  >     return this.firstName + " " + this.lastName;
  >   }
  > };
  > ```
  >
  > **this 指的是 person 对象。**
  >
  > person 对象是 fullName 方法的拥有者。

- 单独的情况下，this 指的是全局对象。

  > 在单独使用时，拥有者是全局对象，因此 this 指的是全局对象。
  >
  > 在浏览器窗口中，全局对象是 [object Window]：
  >
  > ```js
  > var x = this;
  > ```
  >
  > 

- 在**函数中，this 指的是全局对象。**

  > 在函数中，this 指的是全局对象 [object Window]。
  >
  > ```js
  > function myFunction() {
  >   return this;
  > }
  > ```

- 在函数中，严格模式下，this 是 undefined。

  > 严格模式不允许默认绑定。
  >
  > 因此，在函数中使用时，在严格模式下，this 是未定义的（undefined）。
  >
  > ```js
  > "use strict";
  > function myFunction() {
  >   return this;
  > }
  > ```

- 在事件中，this 指的是接收事件的元素。

  > 在 HTML 事件处理程序中，this 指的是接收此事件的 HTML 元素：
  >
  > ```js
  > <button onclick="this.style.display='none'">
  >   点击来删除我！
  > </button>
  > ```
  
- 在箭头函数中，指向父级的作用域

- 在匿名函数值，永远指向window

  ```js
  setTimeOut(function(){
  	console.log(this)//为window对象
  },5000)
  ```

  

#### 16)箭头函数

```js
hello = function() {
  return "Hello World!";
}

/*变为*/
hello = () => {
  return "Hello World!";
}

/*只有一个语句*/
hello = () => "Hello World!";
```



关于箭头函数使用this，详情参考：

> https://www.w3school.com.cn/js/js_arrow_function.asp



#### 17)类

请使用关键字 class 创建类。

> JavaScript 类*不是*对象。
>
> 它只是 JavaScript 对象的*模板*。

- 请始终添加名为 constructor() 的方法：

```js
class Car {
  constructor(name, year) {
    this.name = name;
    this.year = year;
  }
}
```

- class方法：

  ```js
  class Car {
    constructor(name, year) {
      this.name = name;
      this.year = year;
    }
    age() {
      let date = new Date();
      return date.getFullYear() - this.year;
    }
  }
  ```

- 创建带有属性的类

  ```js
  class car {
      name;
      constructor(name) {
          this.name = name;
  }
  
  build() {
      window.alert(this.name);
  }
  ```



##### 继承：

**extends**关键字

```js
class Car {
  constructor(brand) {
    this.carname = brand;
  }
  present() {
    return 'I have a ' + this.carname;
  }
}

class Model extends Car {
  constructor(brand, mod) {
    super(brand);//通过在 constructor 方法中调用 super() 方法，我们调用了父级的 constructor 方法，获得了父级的属性和方法的访问权限
    this.model = mod;
  }
  show() {
    return this.present() + ', it is a ' + this.model;
  }
}

let myCar = new Model("Ford", "Mustang");
document.getElementById("demo").innerHTML = myCar.show();
```



##### static:

静态方法只可以在类上调用，不可以在对象上调用

```js
class Car {
  constructor(name) {
    this.name = name;
  }
  static hello() {
    return "Hello!!";
  }
}

let myCar = new Car("Ford");

// 您可以在 Car 类上调用 'hello()' ：
document.getElementById("demo").innerHTML = Car.hello();

// 但不能在 Car 对象上调用：
// document.getElementById("demo").innerHTML = myCar.hello();
// 此举将引发错误。
```







#### 18)JSON

- 将JSON文本转换为JS对象

> 首先，创建包含 JSON 语法的 JavaScript 字符串：
>
> ```js
> var text = '{ "employees" : [' +
> '{ "firstName":"Bill" , "lastName":"Gates" },' +
> '{ "firstName":"Steve" , "lastName":"Jobs" },' +
> '{ "firstName":"Alan" , "lastName":"Turing" } ]}';
> ```
>
> 然后，使用 JavaScript 的内建函数 JSON.parse() 来把这个字符串转换为 JavaScript 对象：
>
> ```js
> var obj = JSON.parse(text);
> ```

- 将JS对象转换为json

  > JSON.stringify() 进行字符串化
  >
  > ```js
  > const person = {
  >   name: "Bill",
  >   age: 19,
  >   city: "Seattle"
  > };
  > 
  > let myString = JSON.stringify(person);
  > ```

#### 19)调试

*debugger* 关键词会停止 JavaScript 的执行，并调用（如果有）调试函数。

这与在调试器中设置断点的功能是一样的。

如果调试器不可用，debugger 语句没有效果。

如果调试器已打开，此代码会在执行第三行之前停止运行。

```js
var x = 15 * 5;
debugger;
document.getElementbyId("demo").innerHTML = x; 
```

![image-20220117142508259](https://gitee.com/kevinyong/kevin-gallery/raw/master/image-20220117142508259.png)





#### 20)Async

##### 回调：

回调是作为参数传递给另一个函数的函数。

使用回调，您可以通过回调调用计算器函数（myCalculator），并在计算完成后让计算器函数运行回调：

```js
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

function myCalculator(num1, num2, myCallback) {
  let sum = num1 + num2;
  myCallback(sum);
}

myCalculator(5, 5, myDisplayer);
```

> 在上面的例子中， myDisplayer 是函数的名称。
>
> 它作为参数传递给 myCalculator()。
>
> 当您将函数作为参数传递时，请记住不要使用括号。
>
> 正确：myCalculator(5, 5, myDisplayer);
>
> 错误：myCalculator(5, 5, myDisplayer());



##### 异步

**①等待超时**

使用 JavaScript 函数 **setTimeout()** 时，可以指定超时时执行的回调函数：

> ```js
> setTimeout(myFunction, 3000);
> 
> function myFunction() {
>   document.getElementById("demo").innerHTML = "I love You !!";
> }
> ```
>
> 在上面的示例中，myFunction 被用作回调。
>
> 函数（函数名）作为参数传递给 setTimeout()。
>
> 3000 是超时前的毫秒数，所以 3 秒后会调用 myFunction()。



**②等待间隔**

使用 JavaScript 函数 **setInterval()** 时，可以指定每个间隔执行的回调函数：

> ```js
> setInterval(myFunction, 1000);
> 
> function myFunction() {
>   let d = new Date();
>   document.getElementById("demo").innerHTML=
>   d.getHours() + ":" +
>   d.getMinutes() + ":" +
>   d.getSeconds();
> }
> ```



##### 应用：

如果您创建函数来**加载外部资源**（如脚本或文件），则在内容完全加载之前无法使用这些内容。

这是使用回调的最佳时机。

此例加载一个 HTML 文件 (mycar.html)，并在文件完全加载后在网页中显示该 HTML 文件：

**等待文件：**

> ```js
> function myDisplayer(some) {
>   document.getElementById("demo").innerHTML = some;
> }
> 
> function getFile(myCallback) {
>   let req = new XMLHttpRequest();
>   req.open('GET', "mycar.html");
>   req.onload = function() {
>     if (req.status == 200) {
>       myCallback(this.responseText);
>     } else {
>       myCallback("Error: " + req.status);
>     }
>   }
>   req.send();
> }
> 
> getFile(myDisplayer);
> ```



##### Promise:

和dart的Future非常相似：

Promise.then() 有两个参数，**一个是成功时的回调，另一个是失败时的回调。**

两者都是可选的，因此可以为成功或失败添加回调。

> - 声明一个Promise:
>
>   ```js
>   let myPromise = new Promise(function(myResolve, myReject) {
>     let x = 0;
>   
>   // 生成代码（这可能需要一些时间）
>   
>     if (x == 0) {
>       myResolve("OK");
>     } else {
>       myReject("Error");
>     }
>   });
>   ```
>
> - 调用promise
>
>   ```js
>   myPromise.then(
>     function(value) { /* code if successful */ },
>     function(error) { /* code if some error */ }
>   );
>   ```
>   
> - 通过箭头函数
>
>   ```js
>   //又或者通过箭头函数
>   //resolve:当请求成功
>   //reject:当请求失败
>   new Promise((resolve,reject)=>{
>       if(x==0){
>           resolve();
>       }else{
>           reject()
>       }
>   }).then(
>   	(valueSuccess)=>{
>           
>       }
>   ).catch((error)=>{
>       //处理reject(),也可以通过catch来处理
>   })
>   ```

###### Promise三种状态：

> - pending：等待状态，比如正在进行网络请求，或者定时器没有到时间
> - fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()
> - reject：拒绝状态，当我们主动回调reject时，就处于该状态

###### then的连续调用：

> - 方案一：直接返回值
>
>   ```js
>   new Promise(function(resolve, reject) {
>   
>         setTimeout(() => resolve(1), 1000); // (*)
>   
>       }).then(function(result) { // (**)
>   
>         alert(result); // 1
>         return result * 2;
>   
>       }).then(function(result) { // (***)
>   
>         alert(result); // 2
>         return result * 2;
>   
>       }).then(function(result) {
>   
>         alert(result); // 4
>         return result * 2;
>   
>       });
>   ```
>
>   
>
> - 方案二：返回promise作为结果
>
>   ```js
>   new Promise(function(resolve, reject) {
>   
>     setTimeout(() => resolve(1), 1000);
>   
>   }).then(function(result) {
>   
>     alert(result); // 1
>   
>     return new Promise((resolve, reject) => { // (*)
>       setTimeout(() => resolve(result * 2), 1000);
>     });
>   
>   }).then(function(result) { // (**)
>   
>     alert(result); // 2
>   
>     return new Promise((resolve, reject) => {
>       setTimeout(() => resolve(result * 2), 1000);
>     });
>   
>   }).then(function(result) {
>   
>     alert(result); // 4
>   
>   });
>   ```



###### Promise.all

>当要执行多个异步任务，此时可以调用promise.all命令
>
>```js
>Promise.all([
>    new Promise((resolve,reject)=>{
>        ...
>        resolve(data)
>    }),
>    
>    new Promise((resolve,reject)=>{
>        ...
>        resolve(data)
>    })
>]).then(result=>{//此时的result是一个数组，结果就是对应的promise结果
>    consloe.log(result[0]);
>    consloe.log(result[1])
>})
>```
>
>



##### Async关键字

函数前的关键字 **async 使函数****返回 promise：**

```js
async function myFunction() {
  return "Hello";
}

/*等同于*/
async function myFunction() {
  return Promise.resolve("Hello");
}


/*结合promise*/
async function myFunction() {
  return "Hello";
}
myFunction().then(
  function(value) {myDisplayer(value);},
  function(error) {myDisplayer(error);}
);
```



##### Await关键字

await 关键字只能在 async 函数中使用。

```js
async function myDisplay() {
  let myPromise = new Promise(function(myResolve, myReject) {
    myResolve("I love You !!");
  });
  document.getElementById("demo").innerHTML = await myPromise;//在async中不再使用then，而是await，并且await返回的直接是前面声明myResolve的值
}

myDisplay();
```



#### 21)prototype

https://www.cnblogs.com/yjf512/archive/2011/06/03/2071914.html

https://www.cnblogs.com/yangjinjin/archive/2013/02/01/2889103.html

https://blog.csdn.net/qq_38722097/article/details/88046377