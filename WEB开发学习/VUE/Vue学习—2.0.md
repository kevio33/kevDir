#  Vue学习—2.0

> 在项目开发中，可能会出现安装很多工具包资源情况，
>
> ![image-20220207124803391](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207124803391.png)



## 一.WebPack

### 1.什么是webpack

是一个JS应用的**静态模块打包工具**

> **模块**
>
> webpack可以让开发人员进行模块化开发，并且会帮助我们处理模块间依赖关系。并且不仅仅是JS，CSS、图片、json文件都可以在webpack中被当做模块来使用。
>
> **打包**
>
> 就是讲webpack中的各种资源模块进行打包合并成一个或多个包(bundle)
>
> 在打包过程中还可以对资源进行处理，如压缩图片，将scss转换成css，ES6语法转换成ES5语法，TypeScript转换成JS等等

> 其他常用打包工具：grunt、gulp

> 官网：https://webpack.js.org/

### 2.安装

**安装要点：**

- webpack依赖node环境

- 通过nmp工具管理node的依赖

- 全局(-g,global)安装webpack(安装3.6.0，因为脚手架2支持)

  ![image-20220204152255941](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204152255941.png)

- **局部安装(后续需要)**

  > 为什么既要全局安装，又要局部安装
  >
  > - 在package.json中定义了scripts时，其中包含了webpack命令，此时要使用局部安装
  >
  > 其实有可能别人的项目中使用的webpack版本和全局安装的不同，所以就需要局部安装webpack。
  >
  > ![image-20220204163314307](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204163314307.png)
  >
  > 然后在`package.json`文件下面就会多一项配置
  >
  > ![image-20220204163605751](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204163605751.png)
  >
  > > 当拉取别人项目时，也需要使用
  > >
  > > ```shell
  > > npm install #下载依赖包
  > > ```
  > >
  > > 
  >
  > 然后以后需要打包就使用:
  >
  > ```
  > npm run [option]命令，就会优先使用本地的webpack
  > ```
  >
  > 



#### --save和--save-dev

- --save:

  > 表示我想在生产环境中使用[安装插件], 同时, 插件的版本信息会被写入package.json中的**dependencies**属性中
  >
  > 可以简写为：-S

- --save-dev:

  > 想在开发和测试环境中使用, 插件的版本信息会被写入package.json中的**devDependencies**属性中
  >
  > 可以简写为：-D



### 3.使用

一般项目会有两个文件夹：

- src：用来存放源码
- dist(distribution)：存放打包文件



在src中创建三个前端文件

![image-20220204155009656](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204155009656.png)

```js
//aaa.js
/*导出 */
export var a1 = 'yht';
export var a2 = '23';
```

```js
//main.js
/*导入并且打印 */
import { a1, a2 } from './aaa.js'
console.log(a1);
console.log(a2);
```



然后将main.js进行打包

![image-20220204155145680](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204155145680.png)

![image-20220204155159009](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204155159009.png)

此时webpack会把我们模块化的js翻译成浏览器能够读懂的语言，只需要在html文件引用打包后的文件即可：

```html
<script src="../dist/bundle.js"></script>
```



### 4.配置

#### (1)package.json

用来配置项目的一些基本信息

> 在终端进行初始化
>
> ![image-20220204160548553](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204160548553.png)
>
> 之后在进行信息输入之后会创建一个文件
>
> ![image-20220204160653847](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204160653847.png)
>
> 里面就会有项目的一些基本信息
>
> ![image-20220204160726992](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204160726992.png)



#### (2)webpack.config.js

可以通过配置该文件，从而避免每一次都要去终端对js文件打包

> > **要先创建package.json**，**目的是动态获取路径并且能够构建项目**
>
> 
>
> webpack.config.js文件的内容
>
> ```js
> //node获取路径
> const path = require('path')
> 
> module.exports = {
>     entry: './src/main.js',
>     output: {
>         path: path.resolve(__dirname,'dist'),//动态获取当前路径并且拼接成绝对路径
>         filename: 'bundle.js'//打包成的文件名
>     }
> }
> ```
>
> 配置好之后我们以后只需要在终端输入webpack即可
>
> ![image-20220204161624562](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204161624562.png)

浏览器正常打印

![image-20220204161724912](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204161724912.png)



**npm run build**

> 当然，还可以更进一步配置
>
> 当在终端输入`npm run [option]`时，node会去package.json的scripts里面找对于option的执行命令
>
> ![image-20220204162323065](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204162323065.png)



#### (3)loader

**什么是loader**

![image-20220204164531962](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204164531962.png)

> 简而言之，就是要打包除js以外的文件，就需要通过loader

**安装loader：**

> - 步骤一：通过npm安装需要使用的loader
> - 步骤二：在webpack.config.js中的modules关键字下进行配置

##### CSS-loader

**就拿我们要打包css文件为例**

> 首先可以去官网找到对应loader
>
> ![image-20220204165910901](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204165910901.png)
>
> 然后在终端npm安装(这里安装的是3.0.0版本)，并且配置
>
> ```shell
> npm install --save-dev css-loader#--save-dev是指开发时依赖
> ```
>
> 
>
> ![image-20220204170303721](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204170303721.png)
>
> > test内容是做匹配
> >
> > use的内容是应用loader
> >
> > 并且读取顺序从右往左，先加载css-loader，再style-loader
>
> > css-loader：负责加载css，但是不负责渲染
> >
> > style-loader：负责渲染
>
> 
>
> 然后创建css文件
>
> ![image-20220204170748800](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204170748800.png)
>
> 之后在`main.js`里面依赖css
>
> ![image-20220204170915785](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204170915785.png)
>
> 还需要安装style-loader
>
> ```shell
> npm install --save-dev style-loader@0.23.1
> ```
>
> 然后在构建
>
> ```shell
> npm run build 
> ```
>
> 更改样式成功
>
> ![](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204172529487.png)



##### url-loader--打包图片

> **webpack5已经内嵌url-loader了**
>
> ```shell
> npm install url-loader@1.1.2 --save-dev
> ```
>
> 然后在webpack.config.js里面配置
>
> ```js
> {
>     test: /\.(png|jpg|gif)$/i,
>         use: [
>             {
>                 loader: 'url-loader',
>                 options: {
>                     limit: 8192//限制图片大小，如果图片大小在限制范围内，则会编译成base64字符串形式；如果大于限制，则会使用file-loader来打包编译
>                 }
>             }
>         ]
> }
> ```
>
> 
>
> **如果使用file-loader打包成文件，那么就会在dist文件夹下面产生一个打包的图片文件，此时要想调用它，就需要在进行一个配置：**
>
> ![image-20220204193206896](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204193206896.png)
>
> ```js
> publicPath: 'dist/';//这样就会优先去dist文件夹下面找资源文件。但是如果html文件也在dist里面，那么就不必写这句话
> ```
>
> 
>
> **对打包之后图片进行命名：**
>
> ![image-20220204194240945](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204194240945.png)



##### ES6->ES5 loader

> 参考官网--https://v4.webpack.docschina.org/loaders/babel-loader/



#### (4)配置Vue

下载vue模块

```shell
npm install vue --save
```



在我们需要使用vue的js文件里面引入vue

```js
import Vue from 'vue'
```



如果报如下错误：

![image-20220204201247735](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204201247735.png)

可以进行配置

![image-20220204201130434](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220204201130434.png)

> 这是因为vue分为两种类型:
>
> - runtime-only：代码中，不可以有任何template
> - runtime-compiler：代码中，可以有template
>
> 而vue实例就是放在template里面的，所以需要选择第二个类型，而恰好只有vue.esm.js有compiler



## 二.Vue抽离写法

**1.创建`APP.vue文件`**：

书写所有`template`有关的代码：包括样式、模板、js，然后匿名导出

```vue
<template>
	....
</template>

<script>
	export default{
        name:'',//name代表着该vue文件名称
        data(){
            return {
                ...
            }
        }
    }
</script>

<style scoped>
    .title{
        ...
    }
</style scoped>
```



**2.js文件进行引用：**

js文件引用vue文件并且进行挂载

```js
import App from [path]

new App({
    el: '#app',
    template: '<App/>',
    components:{
        App
    }
})
```



**3.打包之后，`html`**文件调用打包后的js文件



## 三.CLI脚手架

### 1.什么是CLI

在进行大型项目开发时，我们会进行vue的各种配置，那么手动配置无疑是很花费时间的，所以我们就需要一个工具进行自动构建配置



**CLI(Command-Line Interface)**

是Vue.js官方发布的vue.js项目脚手架，可以快速搭建Vue开发环境以及对于的webpack配置



**CLI3与2的区别：**

> - cli3是基于webpack4打造的，2是基于webpack3打造
> - 3的设计原则是“0配置”，移除了配置文件根目录下，build和config目录
> - 3提供vue ui命令，提供可视化配置。
> - 移除了static文件夹，新增public文件夹，并且index。html移动到public中



### 2.安装

> 安装CLI前提是要安装：
>
> - Node.js
> - webpack

**(1)命令：全局安装cli**

```shell
npm install -g @vue/cli
```

安装好之后查看版本

![image-20220207132003189](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207132003189.png)



**(2)然后使用CLI初始化项目**

```shell
vue create my-project
```

![image-20220207140320483](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207140320483.png)

然后会让选择如何配置，默认还是手动

![image-20220207140408764](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207140408764.png)

手动配置里面通过空格进行配置选择，确定之后回车

![image-20220207140445998](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207140445998.png)

然后会让你选择配置的时候是否创建新的配置文件

> 例如配置bebal创建一个单的的bebal配置文件还是直接加在package.json后面

![image-20220207141154405](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207141154405.png)



然后会出现一个选项，就是**本次创建项目的所有配置可以保存，然后下次创建新项目可以按照这次配置来直接创键**

![image-20220207141443028](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207141443028.png)

> 如果保存了此次配置，可以在下面文件夹删除
>
> ![image-20220207142248279](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207142248279.png)



然后就可以创建项目了，成功后通过以下指令运行项目

```shell
$ cd yhtvuecli3
$ npm run serve
```





### 3.runtime-compiler和runtime-only

- runtime-compiler：

  > 该选项的应用进行构建的时候会经过以下步骤
  >
  > template -> ast -> render -> virtualDOM -> UI

- runtime-only:

  > 性能更高，代码量更少
  >
  > render -> vDOM -> UI



### 4.CLI3项目结构

![image-20220207143639518](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207143639518.png)

> - babel是es语法转换文件



如果自己想要进行一些配置那么需要创建文件

```
vue.config.js
```

内容就是自己配置的内容

```js
module.exports={
    
}
//修改之后，项目会把自己配置的和默认配置的进行合并
```



### 5.快速引入vue文件模板

> 使用`d+tab`
>
> ```vue
> <template>
>   
> </template>
> 
> <script>
> export default {
> 
> }
> </script>
> 
> <style>
> 
> </style>
> ```
>
> 





## 四.Vue-router

> 一个小知识：如何更改页面的url而不刷新页面
>
> - 通过location对象
>
> ```js
> location.hash = 'aaa';
> ```
>
> - 通过HTML5的history模式
>
> ```js
> history.pushState({},'','/yht');//会把当前地址压入地址栈最上面
> 
> history.replace({},'','/yht');//会取代当前地址栈的地址
> 
> history.go(-1);//弹出当前栈顶地址
> //history.back()等于history.go(-1)
> //history.forward()等于history.go(1)
> ```



### 1.安装

可以通过安装脚手架的时候安装，也可以通过npm安装

> 通过npm安装：
>
> ```shell
> npm intall vue-router --save
> ```



### 2.配置

在router文件夹下面的js文件进行配置

![image-20220207172115031](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207172115031.png)

```js
import VueRouter from 'vue-router'
import Vue from 'vue'

//1.安装插件
Vue.use(VueRouter)

//2.新建VueRouter对象
const routers = []
const router = new VueRouter({
    routers
})

//3.将router 对象传入到vue实例中
export default router
```

然后在main.js里面引入

![image-20220207173012917](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220207173012917.png)

### 3.进行使用

**(1)首先，我们创建两个进行路由跳转的组件**

![image-20220208153420311](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220208153420311.png)

内容分别为

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/home">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>



<!--router2-->
<template>
  <div class="home">
    <h1>这是主页</h1>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: "Home",
};
</script>

```



**(2)创建好了文件，然后就进行路由配置**

在`/router/index.js`文件里面进行配置

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../components/Home'
import About from '../components/About.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/home',
    //name: 'Home',
    component: Home
  },
  {
    path: '/about',
    //name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: About
  }
]

const router = new VueRouter({
  routes
})

export default router
```



**(3)然后在`main.js`里面引用**

```js
import router from './router'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  router,//引入router
}).$mount('#app')
```



**(4)在`App.vue`里面引用**

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/home">Home</router-link> | <!--通过router-link实现对路由的链接-->
      <router-link to="/about">About</router-link>
    </div>
    <router-view /> <!--当点击对应链接，router-view的内容就会变为对应的页面-->
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

```

![image-20220208153816099](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220208153816099.png)



#### 问题：

> 第一次的时候，我创建项目时并没有勾选router，后面使自己下载的router，可能是但是路由并没有成功，错误是引入router失败，我怀疑是router版本太高、或者是单独引入的时候没有配置好。



#### router-link

该标签是一个vue-router内置的组件，会被渲染成< a> 标签。

> 属性：
>
> - to：重定向的名称
>
> - tag：希望将link渲染成什么组件
>
>   ```vue
>   <router-link to="/home" tag="button">
>   ```
>
>   ![image-20220208162103809](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220208162103809.png)
>
> - replace：当改变了路由之后，将不能返回上一个路由
>
> - active-class：每一个router-link都会在渲染之后有一个样式类，名字很长，如果需要修改类名，那么就可以通过该属性
>
>   当然，也可以在VueRouter实例里面修改：
>
>   ```js
>   const router = new VueRouter({
>     mode: 'history',
>     routes,
>     linkActiveClass:'active'//修改样式类，这样选中那个路由就会运用该active
>   })
>   ```
>
>   



#### router-view

该标签会根据当前路径，动态渲染出不同的组件，**路由切换时，切换的是router-view挂载的组件，其他内容不会发生改变**



#### 重定向：

当默认进入页面时，可以进行重定向

```js
const routes = [
  {
    path: '/',
    //重定向
    redirect: '/about'
  },
  {
    path: '/home',
    //name: 'Home',
    component: Home
  },
  {
    path: '/about',
    component: About
  }
]
```



#### 采用history

vue-router默认采用hash来进行地址的迁移，但是会造成url出现#号

![image-20220208161531785](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220208161531785.png)



可以通过修改VueRouter的mode来更改地址定向的方式

```js
const router = new VueRouter({
  mode: 'history',
  routes
})
```



#### 不使用router-link

可以通过调用全局的router对象来进行路由定位

```vue
<template>
  <div id="app">
    <div id="nav">
      <button @click="toHome()">去主页</button>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    toHome() {
      console.log("tohome");
        //通过router对象进行跳转
      this.$router.push("/about", () => {});
    },
  },
};
</script>
```

> 该种方式会出现一个问题：
>
> 如果重复跳转一个页面就会报错，那么目前已知有两种解决办法：
>
> - 如上所写，后面添加一个参数
>
> - 再或者重写push方法，具体可参考
>
>   > https://blog.csdn.net/u013595395/article/details/102912835



#### 动态路由

很多时候会实现一个功能：路由信息夹杂id等信息

```
/user/yht
```

要实现这种效果可以使用：v-bind

```vue
<router-link to="'/user/'+userName"></router-link>

...

data(){
	return{
		userName:'yht'
}
}
```

在router配置文件里面

```js
const routes = [
  {
      path:'/user/:userID',
      component:User
  }
]
```



##### 动态获取名称

> 如果想要拿到url中的username，该怎么做？
>
> ```
> 当我们跳转到某个路由的时候，会获取到$route对象，这个对象就是当前我们跳转到的这个route所生成的对象，所以参数可以通过这个route获得
> ```
>
> ```vue
> <!--在对应的route界面的vue文件里面进行获取-->
> <div>
>     {{username}}<!--或者直接{{$route.params.userID}}-->
> </div>
> 
> ....
> <script>
> computed:{
> 	username(){
> 		return this.$route.params.userID//这个userID就是我们在上面route配置文件的参数名称
> }
> }
> </script>
> 
> ```



#### 路由懒加载

由于我们打包之后的所有逻辑代码都是放到一个js文件里面，所以当构建应用时，js包会变得非常大，这时候就可以通过懒加载，将路由对应的组件打包成一个个js代码块，只有在路由被访问到时候，才加载对应的组件：

![image-20220209131653789](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220209131653789.png)





#### 路由嵌套

比如在一个路径中`/home/news`，我们希望一个路径映射一个组件，访问这两个路径也会分别渲染两个组件

> - 创建子组件，然后在router配置文件配置路由
>
>   ```js
>   {
>       path: '/home/:homeID',
>       component: Home,
>       children:[//嵌套路由
>           {
>               path:'news',
>               component:News
>           }
>       ]
>     },
>   ```
>
> - 因为是配置在`home`的子路由，所以在`Home.vue`里面声明组件
>
>   ```vue
>   <div>
>       <router-link to='/home/news'></router-link>
>       
>       <router-view></router-view>
>   </div>
>   ```
>
>   



#### 路由传参

- params类型

  > - /router/:id
  > - 传递方式，在path后面跟上对应值

- query类型

  > - 配置路由格式：/router，也就是普通配置
  >
  >   ```js
  >   {
  >       path: '/profile',
  >       component: () => import('../components/Profile')
  >     }
  >   ```
  >
  >   
  >
  > - 传递方式：对象中使用query的key作为传递方式
  >
  >   ```vue
  >   <router-link :to="{ path: '/profile', query: { name: 'yht', age: '18' } }"
  >           >Profile</router-link
  >         >
  >   ```
  >
  >   
  >
  > - 传递后形成的路径：/router?id=123，/router?id=abc，并且profile获取参数
  >
  >   ```vue
  >   <p>{{ $route.query }}</p>
  >   ```
  >
  >   ![image-20220209141736211](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220209141736211.png)



#### **router和route**

我们在前面已经使用过`router`和`route`了，那他们有什么区别呢：

- router

  > router对象其实是我们配置的`VueRouter`实例
  >
  > ![image-20220209150343852](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220209150343852.png)
  >
  > 它里面有很多内置方法供我们使用，比如push

- route

  > route则是路由对象
  >
  > ![image-20220209150440396](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220209150440396.png)



##### 全局守卫导航

当我们想要实现跳转到某一个页面时候修改网页title时候可以用到守卫导航

```js
//为路由添加meta，也就是元数据
{
    path: '/profile',
    component: () => import('../components/Profile'),
    meta: {
      title: 'profile'
    }
  }


//然后router对象之后调用守卫导航函数，
const router = new VueRouter({
  mode: 'history',
  routes,
})

//导航函数
router.beforeEach((to, from, next) => {
  //从from跳转到to，这时候就取to的meta数据即可
  document.title = to.meta.title;
  next();//不要忘记调用next
})
```

![image-20220209153810156](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220209153810156.png)

> 但是记住，每一个页面都需要加一个meta，否则出现undefined



**除了`守卫（guard以外）`，还有一个钩子函数：**

![image-20220211141253067](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220211141253067.png)

用于跳转之后调用(forEach可以看出是在每一个路由跳转之后)



##### 路由独享守卫

可以给特定的路由配置单独的守卫

```js
{
    path: '/about/:aboutID',
    component: About,
    meta: {
      title: 'about'
    },
     //独享守卫
    beforeEnter:(to,from,next)=>{
      //code
    }
  },
```



##### 组件内守卫

可以为路由组件添加以下配置：

- `beforeRouteEnter`
- `beforeRouteUpdate`
- `beforeRouteLeave`

> ```js
> const UserDetails = {
>   template: `...`,
>   beforeRouteEnter(to, from,next) {
>     // 在渲染该组件的对应路由被验证前调用
>     // 不能获取组件实例 `this` ！
>     // 因为当守卫执行时，组件实例还没被创建！
>   },
>   beforeRouteUpdate(to, from,next) {
>     // 在当前路由改变，但是该组件被复用时调用
>     // 举例来说，对于一个带有动态参数的路径 `/users/:id`，在 `/users/1` 和 `/users/2` 之间跳转的时候，
>     // 由于会渲染同样的 `UserDetails` 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
>     // 因为在这种情况发生的时候，组件已经挂载好了，导航守卫可以访问组件实例 `this`
>   },
>   beforeRouteLeave(to, from,next) {
>     // 在导航离开渲染该组件的对应路由时调用
>     // 与 `beforeRouteUpdate` 一样，它可以访问组件实例 `this`
>   },
> }
> ```
>
> 
>
> 由于beforeRouteEnter守卫**不能访问 this**，可以通过传一个回调给 `next` 来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数：
>
> ```js
> beforeRouteEnter (to, from, next) {
>   next(vm => {
>     // 通过 `vm` 访问组件实例
>   })
> }
> ```



#### keep-alive

keep-alive是Vue内置的组件，**可以使被包含的组件保留状态，或避免被重新渲染**

```vue
<!--当跳转到页面，该组件不会被销毁，而是保留在缓存，下次回到页面的时候又会复用该组件-->
<keep-alive>
	<router-view/>
</keep-alive>
```

> 与keep-alive相关的生命周期函数有两个：
>
> - activated
> - deactivated
>
> 通过这两个生命周期函数可以实现一些功能例如：保存上次浏览的界面的状态



##### 排除不需要保活的组件

> - include：字符串或正则表达式，只有匹配的组件会被缓存
> - exclude：字符串或正则表达式，任何匹配的组件都不会被缓存

```vue
<keep-alive exclude="Home,User"><!--’Home‘就是我们定义组件时候的name属性-->
	<router-view/>
</keep-alive>


...
<!--Home文件-->
export default {
  name: "Home",
};
```





## 五.VueX

### 1.什么是Vuex

Vuex是一个专为Vue.js应用程序开发的状态管理模式。

- 它采用**集中式存储管理**应用的所以组件状态，并以相应规则保证状态以一种可预测的方式发生变化
- 也集成到Vue的官方调试工具devtools中

> 什么是**状态管理**：
>
> 可以简单的将其看成把需要多个组件共享的变量全部存储在一个对象里面；然后，将这个对象放在顶层的Vue实例中，让其他组件可以使用

Vuex和单例模式非常类似，但是单例模式无法做到**响应式**，而**Vuex能够做到响应式**

> 和flutter的GetX非常相似





### 2.何时用到状态管理

![image-20220213121146163](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213121146163.png)







### 3.安装

```shell
npm install vuex@3.0.1 --save
```

![image-20220213142245040](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213142245040.png)

![image-20220213142323182](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213142323182.png)



### 4.vuex-store属性

- **state**

  > 该属性进行状态管理，对变量属性进行全局管理
  >
  > ![image-20220213142041347](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213142041347.png)
  >
  > 然后组件内进行引用
  >
  > ![image-20220213142100916](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213142100916.png)
  >
  > 效果
  >
  > ![image-20220213142131676](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213142131676.png)

- **mutations**

  > 我们修改state内的属性**唯一方式**是通过mutations进行修改
  >
  > 通过在mutations里面定义方法并且传入state对象，就可以访问到属性变量
  >
  > ![image-20220213152608899](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213152608899.png)
  >
  > ```vue
  > <template>
  >   <div class="home">
  >     <!-- <h1>{{ $route.params.homeID }}</h1> -->
  >     <h1>{{ $store.state.counter }}</h1>
  >     <button @click="add">+</button>
  >     <button @click="sub">-</button>
  >   </div>
  > </template>
  > 
  > <script>
  > export default {
  >   name: "Home",
  >   methods: {
  >     add() {
  >         
  >         //1.通过commit方法调用mutations里面的方法
  >       this.$store.commit("increament");
  >         
  >         //2.通过对象方式提交
  >       this.$store.commit({
  >           type:'increament',
  >       })
  >         
  >     },
  >     sub() {
  >         //通过commit方法调用mutations里面的方法
  >       this.$store.commit("decreament");
  >     },
  >   },
  > };
  > </script>
  > ```
  >
  > 
  >
  > 
  >
  > **当然，mutations也支持传自定义参数**
  >
  > ```js
  > //在调用的vue文件里面传入参数-10
  > sub() {
  >     this.$store.commit("decreament", 10);
  >     
  >     //或者；
  >     //注意：这种方式如果提交多个自定义参数({age:10,name:'yht'})，那么最终mutation会将其整合成对象，然后通过第二个参数统一获取
  >     /*
  >     this.$store.commit({
  >         type:'decreament',
  >         subNum:10
  >     })
  >     */
  >    
  > },
  > ```
  >
  > 在mutation里面加上参数即可
  >
  > ![image-20220213165837243](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213165837243.png)
  >
  > 
  >
  > 
  >
  > 往往将所有mutation里面的方法放进一个js文件，通过常量的形式引入，这样避免了出错的问题(类似于之前智慧女职里面的url，静态常量)：
  >
  > ![image-20220213171205433](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213171205433.png)
  >
  > 然后在可以用到这个type的地方统一导入
  >
  > ![image-20220213171402253](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213171402253.png)

- **actions**

  > 在mutation中，禁止进行异步的操作，取而代之的是actions：
  >
  > ![image-20220213175825145](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213175825145.png)
  >
  > 调用actions：
  >
  > ![image-20220213175859823](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213175859823.png)
  >
  > 
  >
  > 可以配合第二个参数传递自定义参数或者回调
  >
  > ```js
  > //调用actions方法
  > $store.dispatch('updateActions',{
  >     message:'数据',
  >     success:()=>{
  >         //处理成功的回调
  >     }
  > })
  > 
  > //actions
  > updateActions(context,payload){
  >     ...
  >     console.log(payload.message);
  >     payload.success();
  > }
  > ```
  >
  > 

- **getters**

  > 和computed属性非常相似，也是通过计算获取state改变之后的结果
  >
  > ```js
  > getters: {
  >     powerCounter(state) {
  >         return state.counter * state.counter;
  >     }
  > }
  > ```
  >
  > 然后在component里面调用getter
  >
  > ![](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213161106575.png)
  >
  > 
  >
  > getters可以有两个参数，一个是state，另外一个是getters
  >
  > ![image-20220213164031397](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213164031397.png)
  >
  > 
  >
  > 
  >
  > **如果想对getter传自定义参数那也是可以的：**
  >
  > 可以通过返回函数的方式
  >
  > ```js
  > getters:{
  >     compareAge(state){
  >         //此处的age正是我们调用时传入的参数
  >         return function(age){
  >             return .....
  >         }
  >     }
  > }
  > 
  > 
  > //调用
  > $store.getters.compareAge(18)
  > ```

- **modules**

  > moduls允许将store分割成模块，而每个模块都拥有自己的state、mutations、action、getters等等
  >
  > ```js
  > const store = new Vue.Store({
  >     modules:{
  >         a:{
  >             state:{
  >                 name:"yht"
  >             },
  >             mutations:{},
  >             actions:{},
  >             getters:{}
  >         }
  >     }
  > })
  > ```
  >
  > ```js
  > //获取方式:它实际上还是大的state进行管理，只不过要加上模块名
  > $store.state.a.name
  > //mutations方法的调用方式和默认的一致
  > 
  > //getters方法调用方式和默认一致，但是在module里面的getter会多一个参数rootState，用以获取根state里面的属性
  > getters:{
  >     fullname(state,getters,rootState){
  >         
  >     }
  > }
  > 
  > //actions：要注意在module里面的actions的上下文是module内部，所以无法调用根的mutations，所以可以通过context.rootGetters / context.rootState 来获取根的对应属性方法
  > ```
  >
  > **actions的参数写法**
  >
  > ![image-20220213190917725](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213190917725.png)





### 5.**Vuex的状态管理图流向**

实际开发中，要按照这样的流向进行修改，也就是要通过：mutation-->state-->vue component，这样devtools才可以监听你的运行轨迹。

![vuex](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/vuex.png)





## 六、Axios

### 1.安装

```shell
npm install axios@0.22.0 --save
```



### 2.使用

#### (1)全局：

![image-20220216170620842](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220216170620842.png)

简单的axios测试

![image-20220216145839818](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220216145839818.png)

![image-20220216145528364](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220216145528364.png)



##### get请求

```js
axios.get({
    url,
    [params]
})

//或者
axios({
 url:'',
 method:'get',
 params:{//params参数专门用于get请求的参数拼接
     type:'pop',
     page:1
 }
}).then(res=>{
    console.log(res);
})
```





##### 并发请求

> 和promise类似，axios也进行了封装
>
> ```js
> axios.all([
>     axios({}),
>     axios({})
> ]).then(results=>{
>     console.log(results[0]);
>     console.log(results[1]);
> })
> ```



##### 全局配置

**全局配置之后，项目的所有axios请求都遵守该配置**

```js
//配置根地址
axios.default.baseURL = 'http://123.123.123.123:8000'
//配置请求头
axios.default.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'


axios({
 url:'/index',
 method:'get',
}).then(res=>{
    console.log(res);
})
```

> **其他常见的配置：**
>
> - transformRequest:[function(data){}]
>
>   >请求前的数据处理
>
> - transformResponse:[function(data){}]
>
>   > 请求后的数据处理
>
> - headers:{'....'}
>
>   > 自定义请求头
>
> - URL查询对象
>
>   > params:{id:12}
>
> - request body
>
>   > data：{key：'aa'}
>
> - 设置超时
>
>   >timeout:1000
>
> - 跨域是否带Token
>
>   > withCredentials:false
>
> - 身份验证信息
>
>   > auth:{uname:'yht',pwd:'12'}
>
> - 响应的数据格式:
>
>   >json/blob/document/arraybuffer/text/stream
>   >
>   >responseType:'json'



#### (2)实例

通过create方法创建实例

```js
const urlReqeust = axios.creat({
    baseUrl:'...'
})
    
urlRequest({
    url:...
})then(res=>{
    
})
```



##### 封装

![image-20220216170101864](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220216170101864.png)

```js
import axios from 'axios'

export default (config) => {

    const instance = axios.create({
        baseURL: 'http://123.207.32.32:8000',
        method: 'get'
    });

    return instance(config);
}
```

```js
//使用
import getHttp from "./network/network";

 mounted() {
    getHttp({
      url: "/home/multidata",
    }).then((res) => {
      return (this.urlMessage = res);
    });
  },
```



#### (3)拦截器

```js
const instance = axios.create({
    ...
})

//请求拦截
instance.interceptors.request.use(config=>{
  //请求发送之前的拦截  
   return config;//记得返回信息，否则拿不到请求配置信息
},error=>{
    //请求失败拦截
})

//相应拦截
instance.interceptors.response.use(res=>{
    return res.data;//取出data并返回
},err=>{
    
})
```











### JSONP

前端开发中，我们常见的一种网络请求方式就是JSONP，该方案主要**解决跨域访问问题**

> 比如我们项目部署在domain2.com服务器上时，是不能直接访问domain2.com服务器上的资料的，所以利用< script >标签的src帮助我们去服务器请求到数据，将数据当做一个JS函数执行，并且执行过程中传入我们需要的json







## Vue调试工具

![vue devtools](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/20181031100636673.png)

### 1.安装

> 一个离线Chrome插件下载地址——https://crxdl.com/



### 2.使用

![image-20220213150926757](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213150926757.png)



在里面可以选择查看对应插件的监听内容

![image-20220213151216047](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213151216047.png)



之后修改了state可以在timeline那儿进行记录翻看

![image-20220213152526164](https://gitee.com/kevinyong/kevin-pic-gall2/raw/master/image-20220213152526164.png)