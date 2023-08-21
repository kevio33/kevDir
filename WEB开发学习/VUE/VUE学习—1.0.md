# 		VUE基础

## 一.Vue介绍

**1.渐进式框架**

> - 意味着可以将Vue作为应用的一部分嵌入其中，带来更丰富的交互体验
> - Vue全家桶：Core+Vue-router+Vuex



**2.Vue特点：**

> - 解耦视图和数据
> - 可复用组件
> - 前端路由技术
> - 状态管理
> - 虚拟DOM



## 二.引入Vue

有很多方式可以引入Vue

**1.CDN**

```html
<!--开发环境版本，包含有帮助的命令行警告-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>

<!--生产环境版本，优化了尺寸和速度-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14"></script>
```



**2.下载和引入**

![image-20220125141035594](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220125141035594.png)

![image-20220125143826749](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220125143826749.png)



**3.NPM**

在用 Vue 构建大型应用时**推荐使用** NPM 安装。NPM 能很好地和诸如 [webpack]或 [Browserify]模块打包器配合使用。同时 Vue 也提供配套工具来开发[单文件组件]。

```
# 最新稳定版
$ npm install vue
```





## 三.使用Vue



### 1.简单应用

**(1)实现hello world**

```html
<div id="app">{{message}}</div><!--应用入口-->

<script src="./vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',  //将app挂载
        data: {  //定义数据
            message: 'hello world'
        }
    });
</script>
```



**(2)实现一个列表**

```html
<div id="app">{{message}}
        <ul>
            <li v-for="item in movies">{{item}}</li>
        </ul>
    </div>

    <script src="./vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: 'hello world',
                movies: ['星际穿越', '旺达幻视', '爱乐之城']
            }
        });
    </script>
```

可以通过chrome测试vue响应式

![image-20220125201028358](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220125201028358.png)

**(3)实现一个计数器**

```html
 <div id="app">
        counter:{{counter}}<br />
     <!--设置响应事件-->
        <button v-on:click="counter++">+</button>
        <button v-on:click="counter--">-</button>
    </div>

    <script src="./vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: 'hello world',
                counter: 0//计数器
            }
        });
```

当然，我们在设置点击事件最好使用函数：

```html
<div id="app">
        counter:{{counter}}<br />
        <button v-on:click="add">+</button>
        <button v-on:click="sub">-</button>
    </div>

    <script src="./vue.js"></script>
    <script>
        //整个都是在引用app这个对象
        const app = new Vue({
            el: '#app',
            data: {
                message: 'hello world',
                counter: 0
            },
            //定义方法
            methods: {
                add: function () {
                    this.counter++
                },
                sub: function () {
                    this.counter--
                }
            }
        });
    </script>
```



**(4)区分vue property和用户property**

除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 `$`，以便与用户定义的 property 区分开来。例如：

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```



> 更多参考--https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B-property



### 2.选项

选项代表者一个Vue示例里面的参数，例如：data，el，methods都是选项，下面介绍一些常用的选项，更多参考官方API

> https://cn.vuejs.org/v2/api



#### DOM

##### (1)el

- **类型**：`string | Element`

- **限制**：只在用 `new` 创建实例时生效。

- **作用**：

  > 提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
  >
  > 
  >
  > 在实例挂载之后，元素可以用 `vm.$el` 访问。
  >
  > ```
  > 如果在实例化时存在这个选项，实例将立即进入编译过程，否则，需要显式调用 `vm.$mount()` 手动开启编译。
  > ```

  

#### 数据

##### (1)data

- **类型**：`Object | Function`

- **限制**：组件的定义只接受 `function`。

- **作用：**

  > Vue 实例的数据对象。Vue 会递归地把 data 的 property 转换为 getter/setter，从而让 data 的 property 能够响应数据变化。
  >
  > 可以通过 `vm.$data` 访问原始数据对象。Vue 实例也代理了 data 对象上所有的 property，因此访问 `vm.a` 等价于访问 `vm.$data.a`。
  >
  > 
  >
  > **注意：**当一个`组件`被定义之后，`data`必须声明为返回一个初始数据对象的函数，因为组件可能被复用，则data就会被共享
  >
  > ```js
  > var data = { a: 1 }
  > 
  > // 直接创建一个实例
  > var vm = new Vue({
  >   data: data
  > })
  > vm.a // => 1
  > vm.$data === data // => true
  > 
  > // Vue.extend() 中 data 必须是函数
  > var Component = Vue.extend({
  >   data: function () {
  >     return { a: 1 }
  >   }
  > })
  > ```



##### (2)methods

- **类型**：`{ [key: string]: Function }`

- **内容**：

  > 可以通过vue实例访问methods，或者在指令表达式中使用。方法中的 `this` 自动绑定为 Vue 实例。
  >
  > **注意：**不要使用箭头函数定义methods，因为this不指向vue实例



##### (3)computed

- **类型**：`{ [key: string]: Function | { get: Function, set: Function } }`

- **内容：**

  > 计算属性用于操作一些复杂的逻辑，它自带setter和getter方法，用于将进行set和get操作，例如：
  >
  > ```js
  > var vm = new Vue({
  >   data: { a: 1 },
  >   computed: {
  >     // 仅读取
  >     aDouble: function () {
  >       return this.a * 2
  >     },
  >     // 读取和设置
  >     aPlus: {
  >       get: function () {
  >         return this.a + 1
  >       },
  >       set: function (v) {
  >         this.a = v - 1
  >       }
  >     }
  >   }
  > })
  > vm.aPlus   // => 2
  > vm.aPlus = 3
  > vm.a       // => 2
  > vm.aDouble // => 4
  > ```
  >
  > 计算属性的结果会被缓存，除非依赖的响应式 property 变化才会重新计算。

- **computed vs  methods：**

  > 使用methods也可以达到计算属性效果，
  >
  > 然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。
  >
  > 
  >
  > 这也同样意味着下面的计算属性将不再更新，因为 `Date.now()` 不是响应式依赖，相比之下，每当触发重新渲染时，调用方法将**总会**再次执行函数。：
  >
  > ```js
  > computed: {
  >   now: function () {
  >     return Date.now()
  >   }
  > }
  > ```

- **computed vs watch:**

  > 不要滥用watch，多使用computed。

- **computed setter：**

  > 计算属性默认只有getter，要人为设置setter
  >
  > ```js
  > computed: {
  >   fullName: {
  >     // getter
  >     get: function () {
  >       return this.firstName + ' ' + this.lastName
  >     },
  >     // setter
  >     set: function (newValue) {
  >       var names = newValue.split(' ')
  >       this.firstName = names[0]
  >       this.lastName = names[names.length - 1]
  >     }
  >   }
  > }
  > ```
  >
  > 



##### (4)watch

- **类型**：`{ [key: string]: string | Function | Object | Array }`

- **作用：**

  > 一个对象，**键是需要观察的表达式，值是对应回调函数**。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个 property。
  >
  > 
  >
  > 虽然有computed选项，但是当需要**在数据变化时执行异步或开销较大的操作时**，watch方式是最有用的。
  >
  > ```js
  > var vm = new Vue({
  >   data: {
  >     a: 1,
  >     b: 2,
  >     c: 3,
  >     d: 4,
  >     e: {
  >       f: {
  >         g: 5
  >       }
  >     }
  >   },
  >   watch: {
  >     a: function (val, oldVal) {
  >       console.log('new: %s, old: %s', val, oldVal)
  >     },
  >       
  >     // 方法名
  >     b: 'someMethod',
  >       
  >     // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
  >     c: {
  >       handler: function (val, oldVal) { /* ... */ },
  >       deep: true
  >     },
  >       
  >     // 该回调将会在侦听开始之后被立即调用
  >     d: {
  >       handler: 'someMethod',
  >       immediate: true
  >     },
  >       
  >     // 你可以传入回调数组，它们会被逐一调用
  >     e: [
  >       'handle1',
  >       function handle2 (val, oldVal) { /* ... */ },
  >       {
  >         handler: function handle3 (val, oldVal) { /* ... */ },
  >         /* ... */
  >       }
  >     ],
  >       
  >     // watch vm.e.f's value: {g: 5}
  >     'e.f': function (val, oldVal) { /* ... */ }
  >   }
  > })
  > vm.a = 2 // => new: 2, old: 1
  > ```
  >
  > 



#### 生命周期

Vue在实例创建过程中提供了和生命周期相关的hook函数

比如 [`created`]钩子可以用来在一个实例被创建之后执行代码：

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```



**生命周期图**

![Vue 实例生命周期](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/lifecycle.png)

##### 生命周期函数

- beforeCreate

  > 在实例初始化之后,进行数据侦听和事件/侦听器的配置之前同步调用。

- created

  > 在实例创建完成后被立即同步调用。在这一步中，实例已完成对选项的处理，意味着以下内容已被配置完毕：数据侦听、计算属性、方法、事件/侦听器的回调函数。然而，挂载阶段还没开始，且 `$el` property 目前尚不可用。

- beforeMount

  > 在挂载开始之前被调用：相关的 `render` 函数首次被调用。
  >
  > **该钩子在服务器端渲染期间不被调用。**

- mounted

  > 实例被挂载后调用，这时 `el` 被新创建的 `vm.$el` 替换了。
  >
  > **注意：**`mounted` **不会**保证所有的子组件也都被挂载完成。如果你希望等到整个视图都渲染完毕再执行某些操作，可以在 `mounted` 内部使用 [vm.$nextTick]：
  >
  > ```js
  > mounted: function () {
  >   this.$nextTick(function () {
  >     // 仅在整个视图都被渲染之后才会运行的代码
  >   })
  > }
  > ```
  >
  > **该钩子在服务器端渲染期间不被调用。**

- beforeUpdate

  > 在数据发生改变后，DOM 被更新之前被调用。这里适合在现有 DOM 将要被更新之前访问它，比如移除手动添加的事件监听器。
  >
  > **该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务器端进行。**

- updated

  > 虚拟 DOM 重新渲染和更新完毕之后被调用。
  >
  > 在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用[**计算属性**]或 [**watcher**]取而代之。
  >
  > **注意：**updated也不会保证所有子组件被渲染完成，所以最好使用vm.$nextTick确保所以组件渲染完成。
  >
  > ```js
  > updated: function () {
  >   this.$nextTick(function () {
  >     //  仅在整个视图都被重新渲染之后才会运行的代码     
  >   })
  > }
  > ```

- activated

  > 被 keep-alive 缓存的组件**激活**时调用。

- deactivated

  > 被 keep-alive 缓存的组件**失活**时调用。

- beforeDestroy

  > 实例销毁之前调用。在这一步，实例仍然完全可用。

- destroyed

  > 实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

- errorCaptured

  > - **类型**：`(err: Error, vm: Component, info: string) => ?boolean`
  >
  > - **内容**：
  >
  >   在**捕获一个来自后代组件的错误时被调用**。此钩子会收到**三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串**。此钩子可以返回 `false` 以阻止该错误继续向上传播。
  >
  >   
  >
  >   **错误传播规则：**
  >
  >   > - 默认情况下，如果全局的 `config.errorHandler` 被定义，所有的错误仍会发送它，因此这些错误仍然会向单一的分析服务的地方进行汇报。
  >   > - 如果一个组件的 inheritance chain (继承链)或 parent chain (父链)中存在多个 `errorCaptured` 钩子，则它们将会被相同的错误逐个唤起。
  >   > - 如果此 `errorCaptured` 钩子自身抛出了一个错误，则这个新错误和原本被捕获的错误都会发送给全局的 `config.errorHandler`。
  >   > - 一个 `errorCaptured` 钩子能够返回 `false` 以阻止错误继续向上传播。本质上是说“这个错误已经被搞定了且应该被忽略”。它会阻止其它任何会被这个错误唤起的 `errorCaptured` 钩子和全局的 `config.errorHandler`。



#### 模板

定义`template`属性，会在挂载之后替换掉`el`的内容

![image-20220207120802708](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207120802708.png)



### 3.指令

#### (1)v-once

通过该指令，可以实现一次性的插值，当数据改变时，插值处内容不会更新

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

#### (2)v-html

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html` 指令：

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

> 这个 `span` 的内容将会被替换成为 property 值 `rawHtml`，直接作为 HTML——会忽略解析 style property 值中的数据绑定。
>
> ![image-20220126154046582](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220126154046582.png)
>
> ![image-20220126154100059](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220126154100059.png)



#### (3)v-bind

该指令用来绑定属性，达到动态更新目的

- **预期**：`any (with argument) | Object (without argument)`
- **参数**：`attrOrProp (optional)`
- **修饰符**：
  - `.prop` - 作为一个 DOM property 绑定而不是作为 attribute 绑定。
  - `.camel` - (2.1.0+) 将 kebab-case attribute 名转换为 camelCase。(从 2.1.0 开始支持)
  - `.sync` (2.3.0+) 语法糖，会扩展成一个更新父组件绑定值的 `v-on` 侦听器。



绑定attribute：

> ```html
> <img v-bind:src="imgUrl" />
> 
> data: {
>     imgUrl: 'https://cn.vuejs.org/images/logo.svg
> }
> ```
>
> **语法糖：简写模式**
>
> ```html
> <img :src="imgUrl" />
> ```



绑定动态attribute：

> ```html
> <button :[key]="value"></button>
> <!--key是属性名，可以动态进行更换-->
> ```



**绑定class**

> ```html
> <!-- class 绑定 ,如果isRed为true，则使用类red的属性
> ，对象语法-->
> <div :class="{ red: isRed }"></div>
> 
> .....
> 
> data:
> 	isRed: flase
> ```
>
> 更多例子
>
> ```html
> <!--数组语法-->
> <div :class="[classA, classB]"></div>
> <div :class="[classA, { classB: isB, classC: isC }]">
> ```



**绑定style（常用在自定义组件）**

> ```html
> <!-- style 绑定 -->
> <div :style="{ fontSize: '50px' }"></div>
> <!--带上单位-->
> <div :style="{ fontSize: size + 'px' }"></div>
> <!--数组语法，每一个元素都是对象-->
> <div :style="[styleObjectA, styleObjectB]"></div>
> styleObjectA: {font-size:50px}
> .....
> ```

#### (4)v-pre

跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法

```html
<p>{{message}}</p>
<p v-pre>{{message}}</p>
```

![image-20220126154822779](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220126154822779.png)

#### (5)v-cloak

这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令**可以隐藏未编译的 Mustache 标签直到实例准备完毕**。

```html
<style>
    [v-cloak] {
      display: none;
    }
</style>

<!--不会显示，直到编译结束。-->
<div v-cloak>
  {{ message }}
</div>
```



#### (6)v-on

- **缩写**：`@`

- **预期**：`Function | Inline Statement | Object`

- **参数**：`event`

- **修饰符**：

  - `.stop` - 调用 `event.stopPropagation()`。

  - `.prevent` - 调用 `event.preventDefault()`。

  - `.capture` - 添加事件侦听器时使用 capture 模式。

  - `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。

  - `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。

  - `.native` - 监听组件根元素的原生事件。

  - `.once` - 只触发一次回调。

  - `.left` - (2.2.0) 只当点击鼠标左键时触发。

  - `.right` - (2.2.0) 只当点击鼠标右键时触发。

  - `.middle` - (2.2.0) 只当点击鼠标中键时触发。

  - `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

    ```
    .passive 会告诉浏览器你不想阻止事件的默认行为。
    ```

    

- **用法**：

  > 绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略。
  >
  > 用在普通元素上时，只能监听[**原生 DOM 事件**]。用在自定义元素组件上时，也可以监听子组件触发的**自定义事件**。
  >
  > ```html
  > <!-- 方法处理器 -->
  > <button v-on:click="doThis"></button>
  > 
  > <!-- 动态事件 (2.6.0+) -->
  > <button v-on:[event]="doThis"></button>
  > 
  > <!-- 内联语句 -->
  > <button v-on:click="doThat('hello', $event)"></button>
  > 
  > <!-- 缩写 -->
  > <button @click="doThis"></button>
  > 
  > <!-- 动态事件缩写 (2.6.0+) -->
  > <button @[event]="doThis"></button>
  > 
  > <!-- 对象语法 (2.4.0+) -->
  > <button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
  > ```
  
  自定义组件监听自定义事件
  
  > ```html
  > <my-component @my-event="handleThis"></my-component>
  > 
  > <!-- 内联语句 -->
  > <my-component @my-event="handleThis(123, $event)"></my-component>
  > 
  > <!-- 组件中的原生事件 -->
  > <my-component @click.native="onClick"></my-component>
  > ```



**关于绑定函数问题：**

> - 如果函数没有参数，则可以省略括号
>
> ```html
> <button @click="add">
>  加号
> </button>
> .....
> .....
> methods:{
> 	add(){
> 
> 	}
> }
> ```
>
> - 如果带参数，则无论是否传参数，都要带括号
>
> ```html
> <button @click="add(1)">加号</button>
> <button @click="add()">加号</button>
> .....
> .....
> methods:{
> 	add(number){
> 		...
> 	}
> }
> ```
>
> - 如果带参数，但是没有写括号，那么就会把这个**event对象传给函数**
>
> ```html
> <button @click="add">加号</button>
> .....
> .....
> methods:{
> 	add(number){
> 		...
> 	}
> }
> ```
>
> - 如果既希望传递参数，又喜欢传递event对象，那么可以
>
> ```html
> <button @click="add(abc,$event)">加号</button>
> .....
> .....
> methods:{
> 	add(number,event){
> 		...
> 	}
> }
> ```



**修饰符**

> ```html
> <!-- 停止冒泡 -->
> <button @click.stop="doThis"></button>
> 
> <!-- 阻止默认行为,例如表单点击提交按钮会直接提交，我们不希望直接提交，所以使用prevent阻止，然后自己定义逻辑dothis -->
> <button @click.prevent="doThis"></button>
> 
> <!-- 阻止默认行为，没有表达式 -->
> <form @submit.prevent></form>
> 
> <!--  串联修饰符 -->
> <button @click.stop.prevent="doThis"></button>
> 
> <!-- 键修饰符，键别名 -->
> <!--监听键盘按下抬起之后-->
> <input @keyup.enter="onEnter">
> 
> <!-- 键修饰符，键代码 -->
> <!--键盘代码-->
> <input @keyup.13="onEnter">
> 
> <!-- 点击回调只会触发一次 -->
> <button v-on:click.once="doThis"></button>
> ```
>
> ![image-20220127164401094](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127164401094.png)
>
> 
>
> 你还可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名]：
>
> ```js
> // 可以使用 `v-on:keyup.f1`
> Vue.config.keyCodes.f1 = 112
> ```

> **更多按键事件参考：**https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E7%A0%81



#### (7)条件判断

##### ①v-if

> 根据表达式的值的 [truthiness] 来有条件地渲染元素。

```
当和 v-if 一起使用时，v-for 的优先级比 v-if 更高。
```

##### ②v-else

> - **不需要表达式**
>
> - **限制**：前一兄弟元素必须有 `v-if` 或 `v-else-if`。
>
> - **用法**：
>
>   为 `v-if` 或者 `v-else-if` 添加“else 块”。
>
>   ```html
>   <div v-if="Math.random() > 0.5">
>     Now you see me
>   </div>
>   <div v-else>
>     Now you don't
>   </div>
>   ```

##### ③v-else-if

> - **类型**：`any`
>
> - **限制**：前一兄弟元素必须有 `v-if` 或 `v-else-if`。
>
> - **用法**：
>
>   表示 `v-if` 的“else if 块”。可以链式调用。
>
>   ```html
>   <div v-if="type === 'A'">
>     A
>   </div>
>   <div v-else-if="type === 'B'">
>     B
>   </div>
>   <div v-else-if="type === 'C'">
>     C
>   </div>
>   <div v-else>
>     Not A/B/C
>   </div>
>   ```



##### [④条件渲染](#tiaojianxuanran)

> 按住ctrl+点击跳转



#### (8)v-show

用于根据条件展示元素的选项是 `v-show` 指令。

```html
<h1 v-show="ok">Hello!</h1>
```

> 不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS property `display`。



##### `v-if` vs `v-show`

>- `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
>- `v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
>
>相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
>
>一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。





#### (9)v-for

可以用 `v-for` 指令基于一个数组来渲染一个列表。`v-for` 指令需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据数组，而 `item` 则是被迭代的数组元素的**别名**。

```html
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>

<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</script>
```



**`v-for` 还支持一个可选的第二个参数，即当前项的索引。**

> ```html
> <ul id="example-2">
>   <li v-for="(item, index) in items">
>     {{ parentMessage }} - {{ index }} - {{ item.message }}
>   </li>
> </ul>
> 
> <script>
> var example2 = new Vue({
>   el: '#example-2',
>   data: {
>     parentMessage: 'Parent',
>     items: [
>       { message: 'Foo' },
>       { message: 'Bar' }
>     ]
>   }
> })
> </script>
> ```
>
> ![image-20220127160951393](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127160951393.png)
>
> 
>
> **也可以用 `of` 替代 `in` 作为分隔符**，因为它更接近 JavaScript 迭代器的语法：
>
> ```
> <div v-for="item of items"></div>
> ```



**遍历对象：**

> 也可以用 `v-for` 来遍历一个对象的 property。
>
> ```html
> <ul id="v-for-object" class="demo">
>   <li v-for="value in object">
>     {{ value }}
>   </li>
> </ul>
> 
> <script>
> new Vue({
>   el: '#v-for-object',
>   data: {
>     object: {
>       title: 'How to do lists in Vue',
>       author: 'Jane Doe',
>       publishedAt: '2016-04-10'
>     }
>   }
> })
> </script>
> ```
>
> 
>
> 你也可以提供第二个的参数为 property 名称 (也就是键名)：
>
> ```html
> <div v-for="(value, name) in object">
>   {{ name }}: {{ value }}
> </div>
> ```
>
> ![image-20220127161235605](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127161235605.png)
>
> 
>
> 还可以用第三个参数作为索引：
>
> ```html
> <div v-for="(value, name, index) in object">
>   {{ index }}. {{ name }}: {{ value }}
> </div>
> ```
>
> ![image-20220127161330525](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127161330525.png)



**使用值范围**

`v-for` 也可以接受整数。在这种情况下，它会把模板重复对应次数。

> ```html
> <div>
>   <span v-for="n in 10">{{ n }} </span>
> </div>
> ```
>
> ![image-20220127163630082](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127163630082.png)



##### v-for和v-if

> **不**推荐在同一元素上使用 `v-if` 和 `v-for`
>
> 当它们处于同一节点，`v-for` 的优先级比 `v-if` 更高，这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。当只想为*部分*项渲染节点时，这种优先级的机制会十分有用，如下：
>
> ```html
> <li v-for="todo in todos" v-if="!todo.isComplete">
>   {{ todo }}
> </li>
> ```
>
> 



##### [列表渲染](#liebiaoxuanran)



#### (10)v-model

可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。



**文本**

> ```html
> <input v-model="message" placeholder="edit me">
> <p>Message is: {{ message }}</p>
> ```



**多行文本**

> ```html
> <span>Multiline message is:</span>
> <p style="white-space: pre-line;">{{ message }}</p>
> <br>
> <textarea v-model="message" placeholder="add multiple lines"></textarea>
> ```



**复选框**

> - 单个复选框
>
>   ```html
>   <input type="checkbox" id="checkbox" v-model="checked">
>   <label for="checkbox">{{ checked }}</label>
>   ```
>
> - 多个复选框
>
>   ```html
>   <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
>   <label for="jack">Jack</label>
>   <input type="checkbox" id="john" value="John" v-model="checkedNames">
>   <label for="john">John</label>
>   <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
>   <label for="mike">Mike</label>
>   <br>
>   <span>Checked names: {{ checkedNames }}</span>
>   
>   <script>
>       new Vue({
>         el: '...',
>         data: {
>           checkedNames: []
>         }
>       })
>   </script>
>   ```
>
>   ![image-20220127173206274](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127173206274.png)



**单选按钮**

> ```html
> <div id="example-4">
>   <input type="radio" id="one" value="One" v-model="picked">
>   <label for="one">One</label>
>   <br>
>   <input type="radio" id="two" value="Two" v-model="picked">
>   <label for="two">Two</label>
>   <br>
>   <span>Picked: {{ picked }}</span>
> </div>
> 
> <script>
> 	new Vue({
>   el: '#example-4',
>   data: {
>     picked: ''
>   }
> })
> </script>
> ```
>
> ![image-20220127173311781](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127173311781.png)



**下拉选项框**

> - 单选
>
>   ```html
>   <div id="example-5">
>     <select v-model="selected">
>       <option disabled value="">请选择</option>
>       <option>A</option>
>       <option>B</option>
>       <option>C</option>
>     </select>
>     <span>Selected: {{ selected }}</span>
>   </div>
>   
>   <script>
>       new Vue({
>         el: '...',
>         data: {
>           selected: ''
>         }
>       })
>   </script>
>   ```
>
>   ![image-20220127173450021](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127173450021.png)
>
> - 动态选项
>
>   用 `v-for` 渲染的动态选项：
>
>   ```html
>   <select v-model="selected">
>     <option v-for="option in options" v-bind:value="option.value">
>       {{ option.text }}
>     </option>
>   </select>
>   <span>Selected: {{ selected }}</span>
>   
>   <script>
>   new Vue({
>     el: '...',
>     data: {
>       selected: 'A',
>       options: [
>         { text: 'One', value: 'A' },
>         { text: 'Two', value: 'B' },
>         { text: 'Three', value: 'C' }
>       ]
>     }
>   })
>   </script>
>   ```
>
>   ![image-20220127173747582](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127173747582.png)



**修饰符**

> - .lazy
>
>   > 在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 。可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步：
>   >
>   > ```html
>   > <!-- 在“change”时而非“input”时更新 -->
>   > <input v-model.lazy="msg">
>   > ```
>
> - .number
>
>   > 如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：
>   >
>   > ```html
>   > <input v-model.number="age" type="number">
>   > ```
>   >
>   > 这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。
>
> - .trim
>
>   > 如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：
>   >
>   > ```html
>   > <input v-model.trim="msg">
>   > ```



### 4.组件

#### **(1)基本示例**

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>

<script>
    
// 创建一个Vue 应用
const app = Vue.createApp({})

// 定义一个名为 button-counter 的新全局组件
app.component('button-counter', {
  data() {
    return {
      count: 0
    }
  },
  template: `
    <button @click="count++">
      You clicked me {{ count }} times.
    </button>`
})
    
   app.mount('#components-demo')
</script>
```

> 因为组件是可复用的实例，所以它们与根实例接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。



> 组件不能访问vue实例的数据，因为组件有自己的data属性，且data属性是一个函数，返回对象，对象包含数据
>
> ```js
> app.component('button-counter', {
>   data() {
>     return {
>       count: 0
>     }
>   })
> ```
>
> 





#### **(2)组件注册**

通常一个应用会以一棵嵌套的组件树的形式来组织：

![Component Tree](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/components.png)

为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。



**命名规范:**

> 1. 全部小写
>
> 2. 包含连字符 (及：即有多个单词与连字符符号连接)
>
>    ```js
>    app.component('my-component-name', {
>      /* ... */
>    })
>    ```
>
>    



**有两种注册方式：**

- **全局注册**

  > 它们在注册之后可以用在任何新创建的组件实例的模板中
  >
  > ```js
  > Vue.component('my-component-name', { /* ... */ })
  > ```

- **局部注册**

  > ```js
  > var ComponentA = { /* ... */ }
  > var ComponentB = { /* ... */ }
  > var ComponentC = { /* ... */ }
  > 
  > new Vue({
  >   el: '#app',
  >   components: {
  >     'component-a': ComponentA,
  >     'component-b': ComponentB
  >   }
  > })
  > ```
  >
  > 注意**局部注册的组件在其子组件中\*不可用\***。例如，如果你希望 `ComponentA` 在 `ComponentB` 中可用，则你需要这样写：
  >
  > ```js
  > var ComponentA = { /* ... */ }
  > 
  > var ComponentB = {
  >   components: {
  >     'component-a': ComponentA
  >   },
  >   // ...
  > }
  > ```
  >
  
  

##### 绑定模板

> 有两种看起来更直观地绑定模板的的方式
>
> - 使用< script >标签
>
>   ```html
>   <html>
>
>       <body>
>           <script type="text/x-template" id="temp">
>           <div>
>           	//模板内容    
>               </div>
>           </script>
>            <script>
>         	Vue.component('cpn',{
>                 template:'#temp'
>             })
>         </script>
>     </body>
>   ```
>
> - 使用< template>标签
>
>   ```html
>   
>   ```
> <template id="temp">
>     	<div>
>             ....
>     </template>
>
>   <script>
>     	vue.component('cpn',{
>             template:'#temp'
>         })
>     </script>
>   ```
> ​```html
>   <script>
>   	vue.component('cpn',{
>           template:'#temp'
>       })
>   </script>
>   ```
>
> 





#### **(3)组件复用**

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>

<script>
app.component('button-counter', {
  data() {
    return {
      count: 0
    }
  },
  template: `
    <button v-on:click="count++">
      You clicked me {{ count }} times.
    </button>`
})
</script>
```



#### **(4)Pro向子组件传递数据**

Prop 是你可以在组件上注册的一些自定义 attribute。为了给博文组件传递一个标题，我们可以用 `props` 选项将其包含在该组件可接受的 prop 列表中：

```js
const app = Vue.createApp({})

app.component('blog-post', {
  props: ['title'],//注册了一个属性，名叫title
  template: `<h4>{{ title }}</h4>`
})

app.mount('#blog-post-demo')
```

使用自定义属性

```html
<div id="blog-post-demo" class="demo">
  <blog-post title="My journey with Vue"></blog-post>
  <blog-post title="Blogging with Vue"></blog-post>
  <blog-post title="Why Vue is so fun"></blog-post>
</div>
```



##### pro的类型

> 可以为 `props` 中的值提供一个带有验证需求的对象,以对象形式列出 prop，这些 property 的名称和值分别是 prop 各自的名称和类型:
>
> ![image-20220128203749465](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220128203749465.png)
>
> ```js
> Vue.component('my-component', {
>   props: {
>     // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
>     propA: Number,
>     // 多个可能的类型
>     propB: [String, Number],
>     // 必填的字符串
>     propC: {
>       type: String,
>       required: true
>     },
>     // 带有默认值的数字
>     propD: {
>       type: Number,
>       default: 100
>     },
>     // 带有默认值的对象
>     propE: {
>       type: Object,
>       // 对象或数组默认值必须从一个工厂函数获取
>       default: function () {
>         return { message: 'hello' }
>       }
>     },
>     // 自定义验证函数
>     propF: {
>       validator: function (value) {
>         // 这个值必须匹配下列字符串中的一个
>         return ['success', 'warning', 'danger'].indexOf(value) !== -1
>       }
>     }
>   }
> })
> ```
>
> 当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

> **注意**那些 prop 会在一个组件实例创建**之前**进行验证，所以实例的 property (如 `data`、`computed` 等) 在 `default` 或 `validator` 函数中是不可用的。
>
> 
>
> ![image-20220128203840254](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220128203840254.png)



##### 传递动态值

> prop 可以通过 `v-bind` 动态赋值，例如：
>
> - **传入字符串**
>
>   > ```html
>   > <!-- 动态赋予一个变量的值 -->
>   > <blog-post v-bind:title="post.title"></blog-post>
>   > 
>   > <!-- 动态赋予一个复杂表达式的值 -->
>   > <blog-post
>   >   v-bind:title="post.title + ' by ' + post.author.name"
>   > ></blog-post>
>   > ```
>
> - **传入数字**
>
>   > ```html
>   > <!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
>   > <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
>   > <blog-post v-bind:likes="42"></blog-post>
>   > 
>   > <!-- 用一个变量进行动态赋值。-->
>   > <blog-post v-bind:likes="post.likes"></blog-post>
>   > ```
>
> - **传入布尔**
>
>   > ```html
>   > <!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
>   > <blog-post is-published></blog-post>
>   > 
>   > <!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
>   > <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
>   > <blog-post v-bind:is-published="false"></blog-post>
>   > 
>   > <!-- 用一个变量进行动态赋值。-->
>   > <blog-post v-bind:is-published="post.isPublished"></blog-post>
>   > ```
>
> - **传入数组**
>
>   > ```html
>   > <!-- 即便数组是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
>   > <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
>   > <blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>
>   > 
>   > <!-- 用一个变量进行动态赋值。-->
>   > <blog-post v-bind:comment-ids="post.commentIds"></blog-post>
>   > ```
>
> - **传入对象**
>
>   > ```html
>   > <!-- 即便对象是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
>   > <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
>   > <blog-post
>   >   v-bind:author="{
>   >     name: 'Veronica',
>   >     company: 'Veridian Dynamics'
>   >   }"
>   > ></blog-post>
>   > 
>   > <!-- 用一个变量进行动态赋值。-->
>   > <blog-post v-bind:author="post.author"></blog-post>
>   > ```
>   >
>   > 
>   >
>   > 如果要传入对象所有的property:
>   >
>   > 可以使用不带参数的 `v-bind` (取代 `v-bind:prop-name`):
>   >
>   > ```html
>   > <blog-post v-bind="post"></blog-post>
>   > 
>   > post: {
>   >   id: 1,
>   >   title: 'My Journey with Vue'
>   > }
>   > ```



##### 非Pro的属性

一个非 prop 的 attribute 是指传向一个组件，但是该组件并没有相应 prop 定义的 attribute。而这些 attribute 会被添加到这个组件的根元素上。

> **禁用Attribute继承**
>
> 如果**不**希望组件的根元素继承 attribute，你可以在组件的选项中设置 `inheritAttrs: false`
>
> ```js
> Vue.component('my-component', {
>   inheritAttrs: false,
>   // ...
> })
> ```
>
> 同时常常搭配***`$attr`***属性使用，该 property 包含了传递给一个组件的 attribute 名和 attribute 值，例如：
>
> ```js
> {
>   required: true,
>   placeholder: 'Enter your username'
> }
> ```
>
> **两者结合能够很好的应用在基础组件**
>
> ```html
> Vue.component('base-input', {
>   inheritAttrs: false,
>   props: ['label', 'value'],
>   template: `
>     <label>
>       {{ label }}
>       <input
>         v-bind="$attrs"
>         v-bind:value="value"
>         v-on:input="$emit('input', $event.target.value)"
>       >
>     </label>
>   `
> })
> ```
>
> 





#### **(5)子组件事件监听**

监听子组件的流程是:

- 父级组件通过 `v-on` 或 `@` 监听子组件实例的任意事件：

  > ```html
  > <blog-post ... @enlarge-text="postFontSize += 0.1"></blog-post>
  > ```

- 同时子组件可以通过调用内建的 [**`$emit`**]方法并传入事件名称来触发一个事件：

  > ```html
  > <button @click="$emit('enlargeText')">
  >   Enlarge text
  > </button>
  > ```

完整代码:

```html
<div id="blog-posts-events-demo" class="demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
       v-for="post in posts"
       :key="post.id"
       :title="post.title"
       @enlarge-text="postFontSize += 0.1"
    ></blog-post>
  </div>
</div>

<script>
	const app = Vue.createApp({
  data() {
    return {
      posts: [
        { id: 1, title: 'My journey with Vue'},
        { id: 2, title: 'Blogging with Vue'},
        { id: 3, title: 'Why Vue is so fun'}
      ],
      postFontSize: 1
    }
  }
})

    app.component('blog-post', {
      props: ['title'],
      template: `
        <div class="blog-post">
          <h4>{{ title }}</h4>
          <button @click="$emit('enlarge-text')">
            Enlarge text
          </button>
        </div>
      `
    })

    app.mount('#blog-posts-events-demo')
</script>
```

> 命名规范：**始终使用 kebab-case 的事件名**。





**可以在组件`emits`选项中列出已抛出的事件**

> ```js
> app.component('blog-post', {
>   props: ['title'],
>   emits: ['enlargeText']
> })
> ```
>
> **通过事件抛出特定值：**
>
> 例如我们要告诉父组件文本要放大的具体倍数,这时可以使用 `$emit` 的第二个参数来提供这个值：
>
> ```html
> <button @click="$emit('enlarge-text', 0.1)">
>   Enlarge text
> </button>
> ```
>
> **可以通过 `$event` 访问到被抛出的这个值：**
>
> ```html
> <blog-post ... @enlarge-text="postFontSize += $event"></blog-post>
> ```
>
> **如果这个事件处理函数是一个方法：**
>
> **那么这个值将会作为第一个参数传入这个方法：**
>
> ```js
> methods: {
>   onEnlargeText(enlargeAmount) {
>     this.postFontSize += enlargeAmount
>   }
> }
> ```



##### **自定义组件的v-model**

> 一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，但是像单选框、复选框等类型的输入控件可能会将 `value` attribute 用于[不同的目的]。`model` 选项可以用来避免这样的冲突：
>
> ```html
> Vue.component('base-checkbox', {
>   model: {
>     prop: 'checked',
>     event: 'change'
>   },
>   props: {
>     checked: Boolean
>   },
>   template: `
>     <input
>       type="checkbox"
>       v-bind:checked="checked"
>       v-on:change="$emit('change', $event.target.checked)"
>     >
>   `
> })
> 
> 
> <!--使用v-model-->
> <base-checkbox v-model="lovingVue"></base-checkbox>
> ```
>
> > lovingVue会传入checked的prop，然后会引发change事件



##### 原生事件绑定到组件

> 如果希望把原生事件直接绑定到组件的根元素的话，那么可以使用native修饰符：
>
> ![image-20220202160632574](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220202160632574.png)
>
> 
>
> 当然，并不总是有效，对于一些特殊的组件例如< input >可能会出现问题，比如自定义组件 `<base-input v-on:focus.native="onFocus"></base-input>`
>
> 的内部模板是:
>
> ```html
> <!--所以根元素实际上是一个 <label> 元素.这时，父级的 .native 监听器将静默失败。它不会产生任何报错，但是 onFocus 处理函数不会如你预期地被调用。-->
> <label>
>   {{ label }}
>   <input
>     v-bind="$attrs"
>     v-bind:value="value"
>     v-on:input="$emit('input', $event.target.value)"
>   >
> </label>
> ```
>
> 
>
> **有一个常用解决方法:**
>
> 有一个`$listeners` property，它是一个对象，里面包含了作用在这个组件上的所有监听器：
>
> ```js
> {
>   focus: function (event) { /* ... */ }
>   input: function (value) { /* ... */ },
> }
> ```
>
> 可以通过该属性配合`v-on="$listeners"` 将所有的事件监听器指向这个组件的某个特定的子元素：
>
> ```js
> Vue.component('base-input', {
>   inheritAttrs: false,
>   props: ['label', 'value'],
>   computed: {
>     inputListeners: function () {
>       var vm = this
>       // `Object.assign` 将所有的对象合并为一个新对象
>       return Object.assign({},
>         // 我们从父级添加所有的监听器
>         this.$listeners,
>         // 然后我们添加自定义监听器，
>         // 或覆写一些监听器的行为
>         {
>           // 这里确保组件配合 `v-model` 的工作
>           input: function (event) {
>             vm.$emit('input', event.target.value)
>           }
>         }
>       )
>     }
>   },
>   template: `
>     <label>
>       {{ label }}
>       <input
>         v-bind="$attrs"
>         v-bind:value="value"
>         v-on="inputListeners"
>       >
>     </label>
>   `
> })
> ```
>
> 此时就可以正常使用了，并且不需要.native监听器了



##### .sync修饰符

> 该修饰符其实是一个语法糖，是关于双向绑定的
>
> 我们的某一个事件作用是：变更某一个属性时推荐以 `update:myPropName` 的模式触发事件
>
> ```js
> this.$emit('update:title', newTitle)//prop中有一个叫title的属性
> ```
>
> 
>
> 父组件通过进行事件的监听，并且更新属性
>
> ```html
> <text-document
>   v-bind:title="doc.title"
>   @update:title="doc.title = $event"
> ></text-document>
> ```
>
> 
>
> 这个时候，可以通过采取语法糖来进行简写
>
> ```html
> <text-document v-bind:title.sync="doc.title"></text-document>
> <!--
> 等同于
> :update:title="doc.title = $event"
> -->
> ```
>
> > 注意，如果是带有`.sync`修饰符的`v-bind`不能有表达式(例如 `v-bind:title.sync=”doc.title + ‘!’”` 是无效的)
>
> 











#### **(6)插槽分发内容**

**什么是插槽**

> 一个复用的组件在不同的页面肯定是需要实现不同页面逻辑的，这个时候我们只需要提取共性写进组件，特性的内容就用插槽来预留



我们经常需要向一个组件传递内容，像这样：

```html
<!--通过插槽，可以在相关的自定义元素插入自己想要的内容-->
<alert-box>
  <button></button>
</alert-box>

<alert-box>
  <a></a>
</alert-box>

<alert-box>
    <!--两个内容都会展示-->
  <button></button>
  <a></a>
</alert-box>


<script>
const app = Vue.createApp({})

app.component('alert-box', {
    //模板里面定义了插槽
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})

app.mount('#slots-demo')
</script>
```



##### **具名插槽:**

> 如果同时具有多个插槽，可以给插槽设置name属性，然后就实现对应名称插入
>
> ```html
> <cpn> <span slot="slot1"></span></cpn>
> .....
> .....
> <slot name="slot1"></slot>
> ```
>
> 
>
> 在vue2.6中，使用v-slot实现具名插槽：
>
> **`slot`属性弃用，具名插槽通过指令参数`v-slot:插槽名`的形式传入，可以简化为`#插槽名`**
>
> ```html
> <base-layout>
>   <template #header>
>     <h1>Here might be a page title</h1>
>   </template>
> ```



##### **作用域插槽:**

> 首先了解下vue的编译作用域概念
>
> ```html
> <body>
>     <div id="app">
>         <h2>我是父组件</h2>
>         <cpn v-show="isShow"></cpn>
>     </div>
> 
>     <template id="cpn">
>         <div>
>             <h2>我是子组件</h2>
>             <button v-show="isShow">按钮</button>
>         </div>
>     </template>
> 
> 
>     <script src="./vue.js"></script>
>     <script>
>         const app = new Vue({
>             el: '#app',
>             data: {
>                 isShow: true
>             },
>             components: {
>                 cpn: {
>                     template: '#cpn',
>                     data() {
>                         return {
>                             isShow: false
>                         }
>                     }
>                 }
>             }
>         })
>     </script>
> </body>
> ```
>
> ![image-20220204125847830](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204125847830.png)
>
> **可以看到，子组件里面的button并没有显示出来，证明子组件button数据访问域是component属性里面的内容**
>
> ```vue
> //在子组件里面只能访问子组件定义的元素：
> 
>  <template id="cpn">
>      <div>
>          <h2>我是子组件</h2>
>          <button v-show="isShow">按钮</button>
>      </div>
>  </template>
> 
> 
> components: {
>                 cpn: {
>                     template: '#cpn',
>                     data() {
>                         return {
>                             isShow: false
>                         }
>                     }
>                 }
>             }
> ```
>
> 所以这就出现了作用域插槽



**什么是作用域插槽**

> 作用域插槽其实就是**带数据的插槽**，即带参数的插槽，简单的来说就是**子组件提供给父组件的参数**，该参数仅限于插槽中使用，父组件可根据子组件传过来的插槽数据来进行不同的方式展现和填充插槽内容。

> 使用作用域插槽，我们可以让父组件收到子组件的数据
>
> 可以将 `user` 作为 `<slot>` 元素的一个 attribute 绑定上去：
>
> ```html
> <!--子组件
> 绑定在 <slot> 元素上的 attribute 被称为插槽 prop
> -->
> <span>
>   <slot v-bind:user="user">
>     {{ user.lastName }}
>   </slot>
> </span>
> ```
>
> 在父级作用域中，可以使用带值的 `v-slot` 来定义提供的插槽 prop 的名字，也就是定义接收数据名称：
>
> ```html
> <current-user>
>     <!--接收的所有插槽的prop的名称就为slotPros，通过该属性可以访问插槽的属性，数据等等-->
>   <template v-slot:default="slotProps">
>     {{ slotProps.user.firstName }}
>   </template>
> </current-user>
> ```
>
> 
>
> 如果插槽不为默认插槽，则需要指定名称
>
> ```html
> <current-user>
>     <!--接收的所有插槽的prop的名称就为slotPros，通过该属性可以访问插槽的属性，数据等等-->
>   <template v-slot:yht-btn="slotProps">
>     {{ slotProps.user.firstName }}
>   </template>
> </current-user>
> ```





##### 解构插槽：

> **什么是解构：**
>
> ```js
> var o = {p: 42, q: true};
> var {p, q} = o;
> 
> console.log(p); // 42
> console.log(q); // true
> ```
>
> **解构插槽:**
>
> ```html
> <current-user v-slot="{ user }">
>   {{ user.firstName }}
> </current-user>
> 
> <!--
> 等同于
> <current-user v-slot:user="user">
>   {{ user.firstName }}
> </current-user>
> -->
> 
> <!--将user重命名为person-->
> <current-user v-slot="{ user: person }">
>   {{ person.firstName }}
> </current-user>
> 
> 
> 
> <!--定义默认值-->
> <current-user v-slot="{ user = { firstName: 'Guest' } }">
>   {{ user.firstName }}
> </current-user>
> ```
>
> 



#### **(7)动态组件**

通过使用保留的 `<component>` 元素，动态地绑定到它的 `is` 特性，可以实现动态组件

```html
<div id="example">
  <button @click="change">切换页面</button>
  <component :is="currentView"></component>
</div>

<script>

var home = {template:'<div>我是主页</div>'};
var post = {template:'<div>我是提交页</div>'};
var archive = {template:'<div>我是存档页</div>'};
    
new Vue({
  el: '#example',
  components: {
    home,
    post,
    archive,
  },
  data:{
    index:0,
    arr:['home','post','archive'],
  },
  computed:{
    currentView(){
        return this.arr[this.index];
    }
  },
  methods:{
    change(){
      this.index = (++this.index)%3;
    }
  }
})
</script>
```



#### (8)解析DOM模板时注意事项

##### 元素位置受限

有些 HTML 元素，诸如 `<ul>`、`<ol>`、`<table>` 和 `<select>`，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 `<li>`、`<tr>` 和 `<option>`，只能出现在其它某些特定的元素内部。

这会导致我们使用这些有约束条件的元素时遇到一些问题。例如：

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```

这个自定义组件 `<blog-post-row>` 会被作为无效的内容提升到外部，并导致最终渲染结果出错。我们可以使用特殊的 [`is`] attribute作为一个变通的办法：

```html
<table>
  <tr is="vue:blog-post-row"></tr>
</table>
```

> 当它用于原生 HTML 元素时，`is` 的值必须以 `vue:` 开头，才可以被解释为 Vue 组件。这是避免和原生[自定义元素]混淆。



##### 大小写不敏感

HTML attribute 名不区分大小写，因此浏览器将所有大写字符解释为小写。这意味着在 DOM 模板中使用时，**驼峰 prop 名称和 event 处理器参数需要使用它们的 kebab-cased (横线字符分隔) 等效值**：

```html
<!-- 在 HTML 中则是横线字符分割 -->

<blog-post post-title="hello!"></blog-post>
```

```js


app.component('blog-post', {
  //  在 JavaScript 中是驼峰式
  props: ['postTitle'],
    
  //字符串模板中可以不用在意这个规则   
  template: `
    <h3>{{ postTitle }}</h3>
  `
})
```



#### (9)访问父子组件方式

##### 父组件访问子组件

- **$children**

  > this.$children是一个数组类型，包含所有子组件对象
  >
  > ```html
  >  <div id="app">
  >         <component-b></component-b>
  >         <component-b></component-b>
  >         <component-b></component-b>
  >         <button @click="getComponent">获取所以自定义组件</button>
  >     </div>
  > 
  >     <script src="./vue.js"></script>
  >     <script>
  >         const app = new Vue({
  >             methods: {
  >                 getComponent() {
  >                     console.log(this.$children)
  >                 }
  >             },
  >             components: {
  >                 'component-b': {
  >                     template: `<div><h5>这是一个自定义组件</h5></div>`
  >                 }
  >             },
  >         });
  > 
  >         app.$mount('#app')
  > ```
  >
  > ![image-20220202175409214](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220202175409214.png)
  >
  > 
  >
  > 也可以通过下标获取特定的某个组件
  >
  > ```js
  > console.log(this.$children[1]);
  > ```

- **$ref**

  > 该属性需要给子组件对应的`ref`属性，然后通过ref名称获取对应组件
  >
  > ```html
  >  <div id="app">
  >         <component-b ref="c1"></component-b>
  >         <component-b></component-b>
  >         <component-b></component-b>
  >         <button @click="getComponent">获取所以自定义组件</button>
  >     </div>
  > <script>
  >         const app = new Vue({
  >             methods: {
  >                 getComponent() {
  >                     //通过refs拿取
  >                     console.log(this.$refs.c1)
  >                 }
  >             },
  >             components: {
  >                 'component-b': {
  >                     template: `<div><h5>这是一个自定义组件</h5></div>`
  >                 }
  >             },
  >         });
  > 
  >         app.$mount('#app')
  > 
  > 
  >     </script>
  > ```
  >
  > ![image-20220202175935417](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220202175935417.png)

##### 子组件访问父组件

- **$parent**

  > 和ref类似，不再赘述

- **$root**

  > 可以访问根组件实例 







### 5.模块化开发

#### (1)前端代码可能问题

由于前端代码量与日俱增，会产生不止一个js文件，每一个js文件的命名就不可避免的会重复，这时候就有可能会产生不同js文件逻辑的混乱。

> 可以通过js闭包的思想解决，因为函数有自己的作用域，所以这时候就可以避免重名的问题
>
> ```js
> ({
>     var i = 1;
>     var sum = function(num1,num2){
>     	return num1 + num2;
> }
> })();
> ```



但是有可能`a.js`想调用`b.js`的一个函数或者变量，这时候可以将该闭包作为一个对象返回，从而实现封装成模块的思想

```js
var module1 = ({
    var obj = {};
    obj.i = 1;
    obj.sum = function(num1,num2){
    	return num1 + num2;
	}

	return obj;
})();


//其他模块调用
console.log(module1.sum(1,2))
```



#### (2)常用模块化规范

- **CommonJS**

  > **导出:**
  >
  > ```js
  > modele.exports = {
  >     flag: true;
  >     test(a,b){
  >         return a+b
  >     }
  > }
  > ```
  >
  > **导入:**
  >
  > ```js
  > let {flag,test} = require('./a.js');//参数为js文件的名称
  > 
  > //等同于
  > let _ma = require('moduleA');
  > let test = _ma.test;
  > let flag = _ma.flag;
  > ```
  >
  > 

- AMD

- CMD

- **ES6的Modules**

  > **导入**
  >
  > 首先，在HTML引入js文件，并且添加`type`就不会出现命名冲突问题:
  >
  > ![image-20220204141629878](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204141629878.png)
  >
  > 
  >
  > **然后导出模块**
  >
  > ```js
  > export {
  > 	flag,sum
  > }
  > 
  > //也可以单独定义
  > var ai = 1;
  > export ai
  > ```
  >
  > 还可以导出函数和类
  >
  > ![image-20220204144048771](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204144048771.png)
  >
  > 
  >
  > **export default:**
  >
  > > 可以导出时候不命名，而是导入者自行命名
  > >
  > > ```js
  > > const address = '1-1'
  > > export default address
  > > ```
  > >
  > > ```js
  > > //导入者命名
  > > import addr from './aaa.js'
  > > ```
  > >
  > > 

  

  > **导入模块**
  >
  > ```js
  > import {flag,sum} from './aaa.js'
  > 
  > //或者
  > import * as info from './aaa.js'
  > ```











## 四.其他

### 1.Vue渲染

#### <span name="tiaojianxuanran">条件渲染</span>

##### (1)用key管理可复用的元素

Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。例如，如果你允许用户在不同的登录方式之间切换：

> ```html
> <template v-if="loginType === 'username'">
>   <label>Username</label>
>   <input placeholder="Enter your username">
> </template>
> <template v-else>
>   <label>Email</label>
>   <input placeholder="Enter your email address">
> </template>
> ```
>
> 那么在上面的代码中切换 `loginType` 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，`<input>` 不会被替换掉——仅仅是替换了它的 `placeholder`。
>
> ![image-20220127153428019](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127153428019.png)
>
> ![image-20220127153455701](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127153455701.png)
>
> 
>
> Vue提供了一种方式，可以表达不要复用这两个元素,只需添加一个具有唯一值的 `key` attribute 即可：
>
> ```html
> <template v-if="loginType === 'username'">
>   <label>Username</label>
>   <input placeholder="Enter your username" key="username-input">
> </template>
> <template v-else>
>   <label>Email</label>
>   <input placeholder="Enter your email address" key="email-input">
> </template>
> ```





#### <span name="liebiaoxuanran">列表渲染</span>

当 Vue 正在更新使用 `v-for` 渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。

这个默认的模式是高效的，但是**只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。

有相同父元素的子元素必须有**独特的 key**。重复的 key 会造成渲染错误。

```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```



### 2.案例

#### (1)案例一

点击列表选项，对应选项改变颜色

> ```html
> <html lang="en">
> <head>
>     <meta charset="UTF-8">
>     <meta name="viewport" content="width=device-width, initial-scale=1.0">
>     <title>Document</title>
> 
>     <style>
>         li:hover {
>             cursor: pointer;
>         }
> 
>         .active {
>             color: burlywood;
>         }
>     </style>
> </head>
> 
> <body>
>     <div id="app">
>         <ol>
>             <li v-for="(item,index) in items" :class="{active: index === currentIndex}" @click="getIndex(index)">
>                 {{item}}
>             </li>
>         </ol>
>     </div>
> 
>     counter:
> 
> 
>     <script src="./vue.js"></script>
>     <script>
>         const app = new Vue({
>             el: '#app',
>             data: {
>                 message: '你好啊',
>                 currentIndex: 0,
>                 items: ['yht1', 'yht2', 'yht3', 'yht4']
> 
>             },
>             methods: {
>                 getIndex(index) {
>                     this.currentIndex = index;
>                 }
>             }
>         });
> 
>     </script>
> </body>
> 
> </html>
> ```
>
> ![image-20220127203206710](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220127203206710.png)