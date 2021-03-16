# Vue学习

## 1 Vue 前端体系、前后端分离

### 1.1 概述

前端化工程 ssm + vue  



Java全栈工程师

- 后台开发：主打

- 前端:html css js jQuery

- 运维:docker  项目发布，服务器如何运行一个项目 linux安装tomcat mysql



Vue 是一套用于构建用户界面的渐进式框架，发布于2014年2月，与其他大型框架不同的是，Vue被设计为可以**自底向上逐层应用**。Vue的核心层只关注视图层，不仅仅易于上搜，还便于与第三方库(如:vue-router 跳转，vue-resource 通信 vuex 管理) 或既有项目整合:

视图  :   HTML +CSS +JS  给用户看，刷新后台给的数据

网络通信 :

页面跳转: 

Anjular.js   Vue.js  React.js

### 1.2 前端知识体系

想要成为真正的互联网全站工程师，



JQuery:  JavaScript框架，优点是简化DOM操作

**`前端三大框架：`** 

Angular js : mvc的方式搬到前端, 对前端程序员不太友好，

- View JSP {{}}   

- DATA   

- Controller  

  MVVM  数据双向绑定

React  :FaceBook出品，一款高性能的js前端框架， 提出 虚拟DOM，利用内存，用于减少DOM的操作，有效提升了前端渲染效率。，缺点是 需要学习额外的一门语言JSX



VUE  一款渐进式JavaScript框架，渐进式框架就是逐步实现新特性的意思，如实现了模块化开发、路由、状态管理等新特性，

​	特点是综合了Angular.js和React.js ，集合了前面的特点和好处，支持MVVM，支持虚拟Dom，并且不需要单独学习一门新语言，

​	计算属性--->Vue的特色，就是利用虚拟化DOM的概念，



Axios，前端的通信框架，也就是AJAX，因为vue的边界很明显，就是为了处理Dom，所以并不具备通信能力，此时就需要使用一个通信框架与服务器交互,不用这个用jquery提供的AJAX通信功能也一样可以做。



`UI框架`

Ant-Design  阿里巴巴的，基于React的UI框架

ElementUI 、ice, 饿了吗出品，是基于Vue的UI框架

AmazeUI  又叫软妹子UI，是一款HTML5的跨屏前端框架

BootStrap  Twitter退出的一个用于前端开发的开源工具包



`JavaScript构建工具`

Babel  JS编译工具，主要用于浏览器不支持ES6新特性，比如用于编译TypeScript

WebPack，模块打包工具，主要作用是打包 压缩 、合并及按序加载， 我们要学习

以上这些已经把webApp开发所需要的的技能都全部梳理完毕了

### 1.3 三端统一

**混合开发Hybird App**

主要目的是实现一套代码三端统一，(PC、Android、IOS.ipa）  并能够调用到设备底层硬件(例如摄像头  GPS 传感器) 打包方式主要有一下两种

云打包  Hbuild-->HbuildX, Dcloud出品，  API Cloud

本地打包  Cordova （前身是PhoneGap）



**微信小程序**   详情见微信官网，这里就是介绍一个方便微信小程序UI开发的框架 WeUI



### 1.4 后端技术

​	前端人员为了方便开发也需要掌握一定的后端技术，但是我们Java后台人员指导后台知识体系极其庞大复杂，所以为了方便前端人员开发后台应用，就出现了NodeJS这样的技术。

​	NodeJS的作者已经声称放弃NodeJS ,说是架构做的不好再加上笨重  node_modules，可能让作者不爽了吧，开始开发全新架构的Deno

既然是后台的技术，那肯定也需要框架和项目管理工具，NodeJS框架及项目管理工具如下:

- Express:  NodeJS框架

- KOA:  Express 简化版

- NPM : 项目综合管理工具，类似于Maven

- YARN  npm的替代方案，类似于maven和gradle的关系



**主流前端框架**



Vue.js



**ElementUI**

​	Element是饿了么前端开源维护的Vue Ui组件库，组件齐全，基本涵盖后台所需的所有组件，文档讲解的详细，例子也非常丰富，主要用于开发pc端的页面，是一个质量非常高的Vue UI组件库

​	vue-element-admin 是一个和vue整合的框架 [Vue-element-Admin 官网](https://panjiachen.gitee.io/vue-element-admin-site/zh/) 



​	备注：属于前端主流框架，选型时可考虑使用，主要特点是桌面端支持较多

**ICE**

​	飞冰是阿里巴巴基于React/Angular/Vue的中后台应用解决方案，在阿里巴巴内部，已经有270个项目几乎所有的bu都在使用，飞冰包含了一条从设计端到开发端的完整链路，帮助用户快速搭建属于自己的中后台应用。

### 1.5 前后端的分离演化史

**A 早期 大后端模式**

回顾一下springmvc的后端请求逻辑

![image-20210125171211960](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210125171211960.png)



缺点：

- 前后端职责纠缠不清， 模板引擎功能强大，依旧可以通过拿下上下文变量来实现各种业务逻辑，这样，只要前端弱一点，往往就会被后端要求在模板层写出不少业务代码，还有一个很大的灰色地带controller，页面路由等功能本来是前端关注的，但是却由后端来实现，controller本身和model往往也纠缠不清，看了让人咬牙的业务代码经常会在controller层，这些问题不能全部归结于程序员素养，否则jsp就够了

- 对前端发挥的局限性，性能优化如果只在前端做空间非常有限，于是我们经常需要后端 和前端合作，但由于后端框架限制，很难使用 comet\bigpipe等技术方案来优化性能

注：在2005年以前，包括早期的jsp,php可以称之为web1.0时代，在这里想说一句，如果你是一名java初学者，请你不要把一些陈旧的技术当回事，比如jsp，因为时代在变，技术在变，什么都在变，当我们去给大学做实训时，有些同学会认为我们没有讲什么干货，其实不然，只能说是你认知里的干货对于市场来说早就过时了而已。

**B 基于AJAX带来的SPA时代**

时间回到2005年AJAX(Asynchronous JavaScript And XML,异步JavaScript和XML，老技术新用法) 被证实提出来并开始使用CDN作为静态资源存储，于是出现了JavaScript王者归来(在这之前JS都是用来在网页进行贴狗皮膏药打广告)的SPA(Single Page Application) 单页面应用时代.

![image-20210125172132142](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210125172132142.png)



优点：

​	前后端分工非常清晰，前后端的关键协作点是AJAX接口。看起来是如此美妙，但是回过头来看看的话，这与JSP时代的区别不大，复杂度从服务端的JSP里转移到了浏览器的JavaScript，浏览器变得很复杂，类似Spring MVC，这个时代开始出现了浏览器的分层架构：

![image-20210125172354055](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210125172354055.png)



缺点：

	- 前后端接口约定  如果后端借口一塌糊涂，如果后端业务模型不稳定，那么前端开发会很痛苦，不少团队也有类似尝试，通过接口规则、接口平台等方式来做，有了和后端一起的沉淀的接口规则，还可以用模拟数据，是的前后端可以在约定接口后实现高效并行开发
	- 前端开发的复杂度控制   SPA应用大多以功能交互型为主，JavaScript代码过10万行很正常，大量的js代码的组织，与View层的绑定，都不是容易的事情



**C 前端为主的MV*时代 大前端时代** 

集大成者  MVVM +DOM 

MVC （同步通信为主）：  Model  view Controller--AngularJS

MVP(异步通信为主)：  Model  View   Presenter  

MVVM (异步通信为主):  Model  View ViewModel

​	为了降低前端开发的复杂度，涌现出来大量的前端框架，比如 AngularJS、React、 Vue.js 、EmberJS等等，这些框架总的原则都是先按照类型分层，比如Template、Controllers、Models，然后再做层内的切分，比如

![image-20210125205458343](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210125205458343.png)



优点

- **前后端职责清晰** 前端工作在浏览器，后端工作在服务器，清晰的分工，可以让开发并行，测试数据的模拟不难，前端可以本地开发，后端则可以专注在业务逻辑的处理，输出RESTFul等接口。

- **前端开发的复杂度可控** 前端代码很重，但合理的分层，让前端代码能各司其职，这一块很有意思的，简单的如模块特性的选择，就有很多很多讲究，并非越强大越好，限制什么，留下那些自由，代码应该如何组织，所有这些设计，得花一本书厚度去说明

- **部署相对独立**  可以快速改进产品体验

缺点：

- **代码不能复用**，比如后端依旧需要对数据进行各种校验，校验逻辑无法复用浏览器端的代码，如果可以复用，那么后端的数据校验可以相对简单化

- **全异步 对SEO不利**，往往还需要服务端做同步渲染的降级方案

- **性能并非最佳**，特别在移动互联网环境下

- **SPA不能满足所有需求**，依旧存在大量多页面应用，URLDesign需要后端配合，前端无法完全掌控

### 1.6 Node.js带来全栈开发时代



前端为主的MV模式解决了很多问题，但如上所述，依旧存在不少不足之处，随着NodeJs的兴起，JavaScript开始有能力运行在服务端，这意味着可以有一种新的研发模式



在这种研发模式下，前后端的职责很清晰，对前端来说，两个UI各司其职

Front-End UI  Layer 处理浏览器层的展现逻辑，通过CSS渲染样式，通过JavaScript添加交互功能，HTML的生成也可以放在这层，具体看应用的场景

Back-ENd  Ui Layer 处理路由 模板，数据获取，Cookie等，通过路由，前端终于可以自主把控URL Design，这样无论是单页面还是多页面的应用，前端都可以自由的调控，后端也终于可以摆脱对展现层的强关注，转而可以专心于业务逻辑层的开发。

通过Node,WebServer层也是JavaScript代码，这意味着部分代码可以前后复用，需要SEO的场景可以在服务端同步渲染，由于异步请求太多导致的性能问题也可以通过服务端来缓解，前一种模式的不足，通过这种模式几乎都能够完美的解决掉

于JSP模式比较，全真模式看起来是一种回归，也的确是一种向原始模式的回归，不过是一种螺旋上升式的回归。

基于NodeJs的全栈模式，依旧面对很多挑战

- 需要前端对服务端编程有更进一步的认识，比如TCP/IP等网络知识的掌握
- NODEJS层与JAVA层的高校通信，NodeJS模式下，都在服务端，RESTFul HTTp通信未必是高效的，通过SOAP等方式通信更高效，一切都需要在验证中进行
- 对部署、运维的熟练了解，需要更多知识点和实操经验
- 大量历史遗留问题如何过渡，这可能是最大的阻力

看到这里，相信很多同学就可以理解，为什么我总在课堂说，前端想学后台很难，而后台程序员学习任何东西都很简单，因为我们后端程序具备相对完善的知识体系

全栈!  So Easy





 综上所述，模式也好，技术也罢，没有好坏优劣之分，只有适合于不适合，前后分离的开发思想主要是基于SOC，上面种种模式，都是让前后端的职责更清晰，分工更高效合理



## 2. 第一个vue程序

Vue 是一套用于构建用户界面的渐进式框架，发布于2014年2月，与其他大型框架不同的是，vue被设计为可以自底向上的逐层应用，Vue的和辛苦只关注视图层，不仅易于上手，还便于与第三方库(Vue-router,vue-resource,vuex) 或既有的项目整合

**Vue是MVVM模式的实现者**

- Model 模型层，在这里表示为JavaScript对象
- View 视图层，在这里表示为DOM（HTML操作的元素）
- ViewModel  连接视图和数据的中间件，Vue.js就是MVVM的ViewModel层的实现者



​	在MVVM框架中，是不允许数据和视图直接通信的，只能通过ViewModel来通信，而ViewModel 就是定义一个Observer观察者

- ViewModel  能够观察到数据的变化，并对视图对应的内容进行更新
- ViewModel 能够监听到视图的变化，并能够通知数据发生改变

至此，我们明白，Vue.js就是一个MVVM的实现者，他的核心就是实现了DOM监听和数据的绑定。

**为什么我们要使用Vue.js**

- 轻量，特别小，体积小，Vue.js压缩后只有20kb，Angular压缩后是56kb， React压缩后是44kb
- 移动优先，更适合移动端，比如移动端的Touch事件
- 易上手，学习曲线平稳，文档期权
- 吸取了Angular和React的长处，并拥有自己独特的功能，比如计算属性
- 开源，社区的活跃度高



**编写Vue程序前IDE准备**

IDEA可以下载安装VUE的插件，注意VUE不支持IE8以及以下版本，因为VUE使用了IE8无法模拟ECMAScript5特性。但他所支持兼容ECMAScript 5的浏览器。

**下载地址**

开发版本：

​	包含完整的警告和调试模式: https://vuejs.org/js/vue.js

​	删除了警告 30.98kb   https://vuejs.org/js/vue.min.js

cdn

 - <script src ="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

   

   

![image-20210121001734181](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210121001734181.png)

### 2.1 第一个vue.js



开发工具:

Vscode  Hbuilder  Sublime Text  ，WebStorm

Step 1、 在IDEA中安装一个Vue的插件  Vue.js



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>-->
</head>
<body>
    <!--view层 和jsp${}取值一样-->
    <div id="app">
        {{message}}
    </div>
<!--第一步 导入vue.js-->
<script src="//cdn.bootcss.com/vue/2.5.2/vue.min.js"></script>
    
<script>
    var vm = new Vue({
        el:"#app",
        // Model ：shuju 
        data:{
            message:"hello,vue!"
        }
    });
</script>
</body>
</html>
```



测试：

​	为了能够更直观的体验Vue带来的数据绑定功能，我们需要在浏览器测试一番，操作流程如下：

1、在浏览器上运行第一个vue应用程序，进入开发者工具

2、在控制台输入vm.message = "Hello World"然后回车，你会发现浏览器显示的内容会直接变成了Hello World

此时，就可以在控制台直接输入vm.message来修改值，中间可以省略data的， 在这个操作中，我们并没有主动的操作DOM，就让页面的内容发生了变化，这就是借助了Vue的数据绑定功能实现的。

MVVM模式要求ViewModel层就是使用观察者模式来实现数据的监听与绑定，以做到数据与视图的快速响应。

> V-bind绑定属性实例

实例代码

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-bind 声明式渲染</title>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<!-- v-bind:title="message" 绑定语句，将message的值和span的title属性绑定-->

<div id="app-2">
    <span v-bind:title="message">
        鼠标悬停几秒钟查看此处动态绑定的提示信息!
    </span>
</div>

<script>
    var app2 = new Vue({
        el: '#app-2',
        data: {
            message: '页面加载与' + new Date().toLocaleDateString()
        }
    })
</script>
</body>
</html>
```

​	你看到的v-bind等被称为指令。指令带有前缀v-， 以表示他们是Vue提供的特殊特性。可能你已经踩到了，它们会在渲染的DOM上应用特殊的响应式行为，在这里该指令的意思是：“将这个元素节点的title特性和vue实例的message属性保持一致”。

​	如果你再次打开浏览器的JavaScript控制台，输入app.message='新消息',就会再一次看到这个绑定了title特性的HTML已经进行了更新。

### 2.2 Vue的判断和循环

判断 v-if ,v-else-if ,v-else

示例代码

```html
<!-- v-if="seen" vue的判断语句，判断是否为真，如果seen 为真，则展示，如果seen为假，则不显示-->

<div id="app-3">
    <p v-if="seen">现在你看到我了</p>
</div>

<div id="app-1">
    <p v-if="type==='A'">This is A</p>
    <p v-else-if="type==='B'">This is B</p>
    <p v-else-if="type==='C'">This is C</p>
    <p v-else>This is D</p>
</div>

<script>
    var app1 = new Vue({
        el: '#app-1',
        data: {
            type: 'A'
        }
    });
    var app3 = new Vue({
        el: '#app-3',
        data: {
            seen: true
        }
    });
</script>
```



循环语句 v-for ,对应的示例代码如下

```html
<!--vue 的for 语法， v-for-->
<div id="app-4">
    <ol>
        <li v-for="todo in todos">
            {{todo.text}}
        </li>
        
        <li v-for="(item, index) in items">
            {{item.message}}--{{index}}
        </li>
    </ol>
</div>

<script>
    var app4 = new Vue({
        el: '#app-4',
        data: {
            todos: [
                {text: '学习JavaScript'},
                {text: '学习Vue'},
                {text: '学习牛项目'},
            ],
            items: [
                {message: '狂神说java'},
                {message: '狂神说vue'},
                {message: '狂神说运维'},
                {message: '狂神说linux'},
            ]

        }
    });
</script>


```



### 2.3 vue的事件监听

可以使用`v-on`指令监听dom事件，并在触发时运行一些javascript代码。

v-on: 绑定的事件='对应的方法函数'

v-on  是代表这个dom 要绑定时间

: click   代表要绑定的时间是 点击 ， 可以有很多事件

='sayHi'  当用户点击这个控件调用的方法是sayHi

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>处理用户输入</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

</head>
<body>
    <div id="app-5">
        <p>{{message}}</p>
        <button v-on:click="reverseMessage">反转消息</button>
    </div>
    <script>
        var app5 = new Vue({
            el:'#app-5',
            data:{
                message:'Hello Vue.js!'
            },
            methods:{//vue的方法必须定义在vue的Method中
              reverseMessage:function (){
                  this.message = this.message.split('').reverse().join('')
              }
            }
        })
    </script>
</body>
</html>
```

### 2.4 vue7个常用属性

学习vue我们必须之到它的7个属性，8个 方法，以及7个指令。787原则

- el属性    用来指示vue编译器从什么地方开始解析 vue的语法，可以说是一个占位符。

- data属性   用来组织从view中抽象出来的属性，可以说将视图的数据抽象出来存放在data中。

- template属性   用来设置模板，会替换页面元素，包括占位符。

- methods属性   放置页面中的业务逻辑，js方法一般都放置在methods中

- render属性   创建真正的Virtual Dom

- computed属性   用来计算

- watch属性

- - watch:function(new,old){}
  - 监听data中数据的变化
  - 两个参数，一个返回新值，一个返回旧值，

### 2.5 vue的双向数据绑定

Vue.js是一个MVVM框架，既数据双向绑定，既当数据发生变化的时候，视图也就发生了变化，当视图发生了变化的时候，数据也会跟随发生同步的变化，这也算是Vue.js的精髓指出了。

值得注意的是，我们所说的数据双向绑定，一定是对于UI控件来说的，非UI控件不会涉及到数据双向绑定，单向数据绑定是使用状态管理工具的前提，如果我们使用vuex,那么数据流也是单向的，这时就会和双向数据绑定有冲突



为什么需要数据双向绑定

在Vue.js中，如果使用vuex，实际上数据还是单向的，之所以说是数据双向绑定，这是用UI空间来说，对于我们处理表单，Vue.js的双向数据绑定用起来就特别舒服了，既两者并不互斥，在全局性数据流使用单向，方便追踪，局部性数据流使用双向，简单易操作



在表单中使用双向数据绑定

我们可以使用v-model指令在表单的input textarea,及select元素上创建双向数据绑定，它会根据控件类型自动选取正确的方法来更新元素，尽管有些神奇，但是v-model的本质不过是语法糖，它负责监听用户的输入事件以更新数据，并对一些极端场景做一些特殊处理

通俗地讲，双向绑定 比如:有一个input输入框和一个p标签，输入框输入内容，p标签内容变更，这个叫双向绑定

注意： v-model会忽略所有表单元素的value,checked,selected特性的初始值而总是将vue实例的数据作为数据来源，你应该通过javascript在组件的data选项中声明初始值!

示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-model双向绑定</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <!--Vue 还提供了 v-model 指令，它能轻松实现表单输入和应用状态之间的双向绑定。-->
    <div id="app-6">
        <p>{{message}}</p>
        <input v-model="message" type="text">
    </div>
    <!--radio 输入框的双向绑定-->
    <div id="app-7">
        性别:
        <input type="radio" name="sex" value="男" v-model="check_value">男
        <input type="radio" name="sex" value="女" v-model="check_value">女
        <p>
            选中的是:{{check_value}}
        </p>
    </div>
    <!--上述例子中，p的数据就和input数据实现了绑定-->
    <script>
        var app6 = new Vue({
            el:'#app-6',
            data:{
                message:'hello vue!'
            }
        })
        var app7 = new Vue({
            el:'#app-7',
            data: {
                check_value:''
            }
        })
    </script>
</body>
</html>
```



下拉框的双向绑定

```html
    <!--v-model绑定select-->
    <div id="app-8">
        <select name="" id="" v-model="selected">
            <option value="" disabled>请选择</option>
            <option >A</option>
            <option >B</option>
            <option >C</option>
            <option >D</option>
        </select>
        <span>value:{{selected}}</span>
    </div>

	 <script>
        var app8 = new Vue({
            el:'#app-8',
            data: {
                selected:''
            }
        })
    </script>

```



### 2.6 vue的第一个组件

在设计开发中，我们并不会以一下方式开发组件，而是用vue-cli创建vue模板文件的方式开发，以下知识只是为了让大家理解什么是组件

使用vue.component()方法注册组件，格式如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue组件</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<!--view层  模板-->
    <div id="app">
        <!--组件，传递给组件中的值，没法直接接收，只能通过props-->
        <qinjiang v-for="item in items" v-bind:qin="item"></qinjiang>
    </div>
<script>
    //component
    Vue.component('qinjiang',{
        props:['qin'],
        template: '<li>{{qin}}</li>'
    });

    var vm = new Vue({
        el:'#app',
        data:{
            items : ["Java","Linux","前端"]
        }
    })
</script>
</body>
</html>
```

说明

- `v-for = "item in items "`: 遍历vue 实例中定义的名为items的数组，并且创建同等数量的组件
- `v-bind:qin="item"`:  将遍历的item项绑定到组件中props定义的名为qin的属性上，=号左边的qin为props中定义的属性名，右边为item in items中遍历的item项目的值
- Vue.component() :注册组件
- my-component-li 自定义组件的名字
- template 组件的模板

**使用props属性传递参数**

像上面那样用组件没有任何得意义，所以我们需要传递参数的组件，因此就需要使用props属性

注意： 默认规则下，props属性里面的值不能为大写!



一个关于组件数据绑定的逻辑关系

![image-20210126164941390](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210126164941390.png)



## 3 Axios异步通信



**什么是Axios？**  Axios是一个开源的可以在浏览器和NodeJS的异步通信框架，她的主要作用就是实现AJAX异步通信，其功能特点是：

- 从浏览器创建XMLHttpRequests
- 从node.js创建http请求
- 支持Promise API [js中的链式编程]
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换json数据
- 客户端支持防御XSRF（跨站请求伪造）

github  https://github.com/axios/axios

中文文档： http://www.axios-js.com



**为什么使用axios?**

​	因为vue.js是一个视图层框架，作者严格遵守Soc（关注度分离原则），所以vue.js并不包含ajax的通信功能，为了解决通信问题，作者单独开发一个名为vue-resource的插件，不过进入2.0版本以后停止了对这个插件的维护并推荐axios框架，少用jquery，因为jquery操作dom太频繁!



第一个axios程序

咱们开发的接口大部分采用json格式，可以现在项目模拟一段json数据，数据内容如下，创建一个名为data.jso的文件并写入如下内容，放在根目录

```json
{
  "name": "狂神说JAVA",
  "url": "http://blog.kuangstudy.com",
  "page": 1,
  "isNonProfit": true,
  "address": {
    "street": "含光门",
    "city": "陕西西安",
    "country": "中国"
  },
  "links":[
    {
      "name": "bilibili",
      "url": "https://space.bilibili.com/95256449"
    },
    {
      "name": "狂神说java",
      "url": "https://blog.kuangshenstudy.com"
    },
    {
      "name": "百度",
      "url": "https://www.baidu.com"
    }
  ]
}
```

测试示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>axios实例</title>
    <style>
        [v-cloak]{
            display: none;
        }
    </style>
</head>
<body>
<div id="vue" v-cloak>
    <div>{{info.name}}</div>

    <div>{{info.address}}</div>
    <div>{{info.address.street}}</div>
    <a v-bind:href="info.url">点我去狂神</a>
</div>

<!--引入js文件-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    var vm = new Vue({
        el:'#vue',
        mounted(){//钩子函数
            axios.get('data.json').then(response=>(this.info=response.data));
        },
        data:{
            message:"hello,vue1!"
        },
        data(){
            return {
                //请求的返回参数格式，必须和json的字符串格式一样
                info:{
                    name:null,
                    url:null,
                    page:null,
                    address:{
                        street:null,
                        city:null,
                        country:null
                    }
                }
            }
        }
    })
</script>
</body>
</html>
```







Vue的声明周期

官方文档:https://cn.vuejs.org/v2/guide/instance.html # 生命周期图示

​	Vue实体有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载DOM、渲染、更新、渲染、卸载等一系列过程，我们称这是VUE的生命周期，通俗说就是Vue实例从创建到销毁的过程，就是声明周期。

​	在vue的整个生命周期中，他提供了一系列的时间，可以让我们在时间出发时注册js方法，可以让我们用自己注册的js方法控制整个大局，在这些事件响应方法中this直接指向的是vue的实例

![Vue å®ä¾çå½å¨æ](https://cn.vuejs.org/images/lifecycle.png)



### 3.1 计算属性

将计算的结果，保存在属性中，内存中运行，虚拟DOM

可以把它想象成缓存

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app" v-cloak>
    当前时间currentTime1:<p>{{currentTime1()}}</p>
    当前时间currentTime2:<p>{{currentTime2}}</p>
</div>


<!--引入js文件-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
           message:'hello,kuangshen'
        },
        methods:{
            currentTime1:function (){
                return Date.now();//返回一个当前时间戳
            }
        },
        computed:{//计算属性  methods和computed里面的方法不能重名
            currentTime2:function (){
                return Date.now();//返回一个当前时间戳
            }
        }

    })
</script>
</body>
</html>
```



用法:

computed和methods一样使用，但是

- computed里面的函数名不能和methods里面的函数名相同
- computed生成的是属性，在模板使用时，使用{{currentTime2}},而methods里面方法是用 {{currentTime1}}
- 当computed里生成属性后，内容就不会发生变更，如下图

![image-20210126212821973](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210126212821973.png)

- 当发生computed数据刷新时，computed就会重新计算



结论： 

调用方法时，每次都需要进行计算，既然有计算过程则必定产生系统开销，那如果这个结果是不经常变化的，就可以考虑将这个结果缓存起来，采用计算属性可以很方便的做到这一点，计算属性主要特性就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销；



## 4. vue slot 内容分发

<span style="pading:10px; background:red;color:#fff">1 、什么是插槽？</span>

在vue.js中我们使用<slot>元素作为承载分发内容的出口，作者称其为插槽，可以应用在组合组件的场景中。插槽就是在子组件中提供给父组件使用的一个占位符，用<slot></slot> 表示，父组件可以在这个占位符中填充任何模板代码，如 HTML、组件等，填充的内容会替换子组件的<slot></slot>标签,如下代码:

1、在子组件中我们放置一个slot占位符

```javascript
Vue.component("child", {
    template: `<div>
<h1>今天天气情况:</h1>
<slot></slot>
</div>`

});
```

2、在父组件中，我们给这个占位符填充内容，这个例子中的父组件是 <div>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>作用域插槽</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
    <!-- <script type="text/javascript" src="vue.js"></script> -->
</head>
<body>
<div id="app">
    <div>
        <div>使用slot分发内容</div>
        <div style="margin-top: 30px">
            <child>
                <div>多云，最高气温34度，最低气温28度，微风!</div>
            </child>
        </div>
    </div>
</div>
</body>
<script type="text/javascript">
    Vue.component("child", {
        template: `<div>
                        <h1>今天天气情况:</h1>
                        <slot></slot>
                    </div>`

    });
    let app = new Vue({
        el: "#app",
        data: {
            msg: '这是动态传入的slot的内容',
        }
    })
</script>
</html>
```

这段代码，展示出来的效果，就是下面的样子

![image-20210219103152198](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210219103152198.png)

4、现在来看看，如果子组件中没有放插槽，同样的父组件中在子组件中填充内容，会是啥样的， 我们先去掉子组件中的插槽代码 slot

```javascript
Vue.component("child", {
    //演示无插槽代码
    template: `<div>
                    <h1>今天天气情况:</h1>
                   <!-- <slot></slot>--> 
                </div>`

});
```

5、父组件和原来代码一样，照常填充内容

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>作用域插槽</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
    <!-- <script type="text/javascript" src="vue.js"></script> -->
</head>
<body>
<div id="app">
    <div>
        <div>使用slot分发内容</div>
        <div style="margin-top: 30px">
            <child>
                <div>多云，最高气温34度，最低气温28度，微风!</div>
            </child>
        </div>
    </div>
</div>
</body>
<script type="text/javascript">
    Vue.component("child", {
        //演示无插槽代码
        template: `<div>
                        <h1>今天天气情况:</h1>
                       <!-- <slot></slot>-->
                    </div>`

    });
    let app = new Vue({
        el: "#app",
        data: {
            msg: '这是动态传入的slot的内容',
        }
    })
</script>
</html>
```

执行后，展示出来的效果如下：

![image-20210219103423900](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210219103423900.png)

所以总结为： 如果子组件没有使用插槽，父组件如果需要往子组件中填充模板或者html，是没有办法办到的。！

<span style="pading:10px; background:red;color:#fff">2、插槽有哪些类别</span>

具名插槽 

给插槽取一个名字的叫做具名插槽，一个子组件可以放多个插槽，而且可以放在不同的地方，而父组件填充内容时，就可以根据这个名字，把内容填充到子组件对应的插槽中。

如下代码

1、在我们的子组件child中，我们设置两个插槽的位置，并分别命名为header和footer, 代码如下：

```javascript
    Vue.component("child", {
        //演示具名插槽
        template: `  <div>
                        <div class="header">
                          <h1>我是页头标题</h1>
                          <div>
                            <slot name="header"></slot>
                          </div>
                        </div>

                        <div class="footer">
                            <h1>我是页尾标题</h1>
                            <div>
                                <slot name="footer"></slot>
                            </div>
                        </div>
                        </div>`

    });
```

2、在我们的父组件div中，我们来填充内容，我们的父组件通过使用v-slot:[name]的方式，来告诉vue指定到对应子组件的哪个插槽中。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>作用域插槽</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
    <!-- <script type="text/javascript" src="vue.js"></script> -->
</head>
<body>
<div id="app">
    <div>
        <div>使用slot分发内容</div>
        <div style="margin-top: 30px">
            <child>
                <template v-slot:header>
                    <h1>我是页尾的具体内容!</h1>
                </template>

                <template v-slot:footer>
                    <h1>我是页尾的具体内容!</h1>
                </template>
            </child>
        </div>
    </div>
</div>
</body>
<script type="text/javascript">
    Vue.component("child", {
        //演示具名插槽
        template: `  <div>
                        <div class="header">
                          <h1>我是页头标题</h1>
                          <div>
                            <slot name="header"></slot>
                          </div>
                        </div>

                        <div class="footer">
                            <h1>我是页尾标题</h1>
                            <div>
                                <slot name="footer"></slot>
                            </div>
                        </div>
                        </div>`

    });
    let app = new Vue({
        el: "#app",
        data: {
            msg: '这是动态传入的slot的内容',
        }
    })
</script>
</html>
```

我们看一下展示出来的效果，如下

![image-20210219104911996](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210219104911996.png)

接下来再来看看，父组件中填充内容的时候，顺序调换下，看下能不能内容能不能对应上：

1. 子组件代码不变，父组件代码中填充顺序调换下(如图，在父组件中，footer 和 header 的填充位置对换)：

```html
<div>
    <div>使用slot分发内容</div>
    <div style="margin-top: 30px">
        <child>
            <template v-slot:footer>
                <h1>我是页尾的具体内容!</h1>
            </template>

            <template v-slot:header>
                <h1>我是页头的具体内容!</h1>
            </template>

        </child>
    </div>
</div>
```

展示的效果和原来一样。由此得出结论：

即使父组件对插槽的填充的顺序打乱，只要名字对应上了，就可以正确渲染到对应的插槽中。即： 父组件填充内容时，是可以根据这个名字把内容填充到对应插槽中的



例子：

比如准备制作一个待办事项组件(todo),该组件由代办标题(todo-title)和待办内容(todo-items)组成，但这三个组件又是相互独立的，该如何进行操作呢？



**step1: 定义一个待办事项的组件**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue-slot插槽学习</title>
</head>
<body>
<!--view层 模板-->
<div id="app">
    <todo></todo>
</div>
<!--1.引入js文件-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    Vue.component('todo',{
        template:'<div>\
                       <div>待办事项</div>\
                       <ul>\
                           <li>学习狂神说java</li>\
                           <li>学习狂神说java</li>\
                        </ul>\
                   </div>'
    });

    var vm = new Vue({
        el:'#app'
    });

</script>
</body>
</html>
```

**step2 我们需要让待办事项的标题和值实现动态绑定，怎么做呢？ 我们可以做一个插槽!**

```javascript
<script>
    //slot: 插槽
    Vue.component('todo', {
        template: '<div>\
                      <slot></slot>\
                       <ul>\
                           <slot></slot>\
                        </ul>\
                   </div>'
    });

    //定义第一个插槽
    Vue.component('todo-title',{
        template: '<div>标题</div>>'
    })
    //定义第二个插槽
    Vue.component('todo-items',{
        template:'<li>Java学习</li>'
    })
    var vm = new Vue({
        el: '#app'
    });

</script>
```

注意改变了几处：

	- 在todo这个组件里面，放入插槽 <slot></slot>
	- 定义插槽组件  todo-title和todo-items

然后到view里面改成如下形式:

```html
<!--view层 模板-->
<div id="app">
    <todo>
        <todo-title></todo-title>
        <todo-items></todo-items>
    </todo>
</div>
```

**step3. 一个个修改对应变量，如下**

 先把todo-title做成可变的组件

```javascript
    //定义第一个插槽
    Vue.component('todo-title',{
        props:['title'],
        template: '<div>{{title}}</div>>'
    })
```

在把todo-items做成可变的组件

```javascript
    //定义第二个插槽
    Vue.component('todo-items',{
        props: ['item'],
        template:'<li>{{item}}</li>'
    })
```

再到我们的vm里面定义数据源

```javascript
    var vm = new Vue({
        el: '#app',
        //定义组件的数据来源
        data:{
            todoItems :['狂神说java','狂神说前端','狂神说Linux']
        }
    });
```

**step 4. 将插槽和组件关联起来**

先给两个slot插槽命名

```javascript
    //slot: 插槽
    Vue.component('todo', {
        template: '<div>\
                      <slot name="todo-title"></slot>\
                       <ul>\
                           <slot name="todo-items"></slot>\
                        </ul>\
                   </div>'
    });
```

再到 我们的view里面的自定义标签绑定到slot对应插槽里面

```html
<!--view层 模板-->
<div id="app">
    <todo>
        <todo-title slot="todo-title"></todo-title>
        <todo-items slot="todo-items"></todo-items>
    </todo>
</div>
```



step 5. 再把插槽对应的组件的变量和vue的数据源绑定

```html
<!--view层 模板-->
<div id="app">
    <todo>
        <todo-title slot="todo-title" v-bind:title="title"></todo-title>
        <todo-items slot="todo-items" v-for="item in todoItems" v-bind:item="item"></todo-items>
    </todo>
</div>
```

v-bind:title="title"  绑定 从vm的data里面取出来titile 绑定到这个组件的props里面的title里面，其中:title是 组件的props属性，"title" 是vm的data里面的值

 v-for="item in todoItems" v-bind:item="item"  :  从vm的data里面取出来todoitems

## 5. 自定义事件

​	通过以上的代码不难发现，数据项在vue的实例中,但删除操作要在组建中完成，那么组件应该如何才能删除Vue实例中的数据呢？此时就涉及到了参数传递和事件分发了，Vue为我们提供了自定义事件的功能很好的帮助我们解决了这个问题，使用this.$emit('自定义事件名', 参数) , 实现的过程如下:

**step1. 先编写三个组件，分别为todo,todo-title,todo-items**

```javascript
    Vue.component('todo', {
        template:
            '<div>\
                <slot name="slot-title"></slot>\
                <ul>\
                <slot name="slot-items"></slot>\
                </ul>\
            </div>'
    });
    Vue.component('todo-title',{
        props:['title'],
        template: '<div>{{title}}</div>'
    })
    Vue.component('todo-items',{
        props: ['item','index'],
        template:'<li>{{index}}---{{item}} <button v-on:click="remove">delete</button></li>',
        methods:{
            remove:function (index){
                alert("11111");
                this.$emit('remove132',index)
            }
        }
    })
```



说明:

1、在todo里面，slot的name是自定义，并不需要和todo-title及todo-items的组件名称相同；

2、组件里面可以添加方法，

3、组件只能使用props里面的变量或者属性

**step 2. 在页面编辑我们的view视图**

```html
<div id="app">
    <todo>
        <!--组件1-->
        <todo-title
                slot="slot-title"
                v-bind:title="title">
        </todo-title>
        <!--组件2-->
        <todo-items
                slot="slot-items"
                v-for="(item,index) in items"
                v-bind:item="item"
                v-bind:index="index"
                v-on:remove132="removeitems"> <!--自定义事件, 这个在组件里面有对应的-->
        </todo-items>
    </todo>
</div>
```

说明:

1、在这里的slot ="slot-title" ，需要和下面的插槽的组件名称相同，既表示，这个组件插到哪个插槽位置；

2、v-bind:title="title" 这里表示将组件的props属性同 var vm = new Vue()里面的data中的title绑定，前面的v-bind:title代表组件属性，"title"为vm的data元素；

3、v-for="(item,index) in items" 里面的item，index表示从 vm里面的items循环读取 item，并获取index

4、v-bind:item ='item' 可以缩写成  :item='item'

5、v-on:remove123="removeitems" 这个是自定义事件，其中 remove为自定义的事件名，通过在组件的methods里面编写 

this.$emit ('remove132', index) 指明自定义事件的名称，后面的removeitems为 vm里面的methods

**step 3. 具体绑定方法的方式**

在vm里面自定义方法：

```javascript
var vm = new Vue({
    el: '#app',
    data:{
        title:'秦老师培训java',
        items:['狂神说JAVA','狂神说Vue','狂神说Linux']
    },
    methods: {
        removeitems:function (index){
            this.items.splice(index,1);
        }
    }
});
```

在组件里面自定义方法

```javascript
Vue.component('todo-items',{
    props: ['item','index'],
    template:'<li>{{index}}---{{item}} <button v-on:click="remove">delete</button></li>',
    methods:{
        remove:function (index){
            alert("11111");
            this.$emit('remove132',index)
        }
    }
})
```

说明:

1、删除按钮在组件定义，但是数组数据在vm里面定义，所以，在组件里面，我们编写<button v-on:click="remove">delete</button>  调用自己的方法，我们在方法里面，再调用vm里面的方法，通过  this.$emit('remove132',index)

2、在vm里面，我们执行删除数据的操作，

  removeitems:function (index){
            this.items.splice(index,1);
        }

**狂神笔记记录的过程：**

1、在vew的实例中,增加了methods对象并定义了一个名为removeitems的方法

```javascript
//vue
var vm = new Vue({
    el: '#app',
    data:{
        title:'秦老师培训java',
        items:['狂神说JAVA','狂神说Vue','狂神说Linux']
    },
    methods: {
        //这个方法可以被模板中自定义事件触发
        removeitems:function (index){
            console.log("删除 "+this.todoItems[index]+" 成功");
            //splice() 方法从/从数组中添加/删除项目，然后返回被删除的项目,其中index
            this.items.splice(index,1);
        }
    }
});
```

2、修改todo-items待办内容组件的代码，增加一个删除按钮，并且绑定事件!

```javascript
Vue.component('todo-items',{
    props: ['item','index'],
    template:'<li>{{index}}---{{item}} <button v-on:click="remove">delete</button></li>',
    methods:{
        remove:function (index){
            alert("11111");
            //这里的remove123是自定义的事件名称，需要在html中使用v-on:remove123的方式
            this.$emit('remove132',index)
        }
    }
})
```



3、修改todo-items 待办内容组件的HTML代码，增加一个自定义事件，比如叫remove，可以和组件的方法绑定，然后绑定到Vue的方法中!

```javascript
<!--增加了v-on:remove="removeitems(index)" 自定义事件，该事件会调用Vue实例中定义的事件-->
    <todo-items
slot="slot-items"
v-for="(item,index) in items"
v-bind:item="item"
v-bind:index="index"
v-on:remove132="removeitems"> <!--自定义事件, 这个在组件里面有对应的-->
    </todo-items>
```



**Vue 入门小结**

核心：数据驱动，组件化

优点：借鉴了AngulaJS的模块化开发和React的虚拟Dom,虚拟Dom就是把Dom操作放到内存中执行：

常用的属性：

- v-if
- v-else-if
- v-else
- v-for
- v-on 绑定事件，简写@
- v-model 数据双向绑定
- v-bind 给组件绑定参数 简写:

组件化

- 组合组件slot插槽
- 组件内部绑定事件需要使用到this.$emit ("事件名", 参数);
- 计算属性的特色，缓存计算数据

遵循SOC关注度分离原则，Vue是纯粹的视图框架，并不包含，比如Ajax之类的通信功能，为了解决通信问题，我们需要使用Axios框架做异步通信；



**说明**

Vue的开发都是要基于NodeJs,实际开发采用vue-cli脚手架开发，vue-router路由，vuex做状态管理，Vue UI，界面我们一般使用ElementUI或者ICE来快速搭建前端项目~

官网

https://element.eleme.cn/#/zh-CN

https://ice.work

## 6 第一个vue-cli项目

**什么是Vue-cli？** Vue-cli官方提供的一个脚手架，用于快速生成一个Vue的项目模板；

预先定义好的目录结构及基础代码，就好比咱们在创建Maven项目时可以选择创建一个骨架夏目，这个骨架项目就是脚手架，我们的开发更加的加速

**vue有哪些功能?**

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

**需要的环境:**

- Node.js  http://nodejs.cn/download/

  安装就无脑下一步就好，安装在自己的环境目录下

- git  http://git-scm.com/downloads

  镜像: https://npm.taobao.org/mirrors/git-for-windows



**确认nodejs安装成功**

- cmd下输入 node -v 查看是否能够正确打印出来版本号即可
- cmd下输入npm -v 查看是否能够正确打印出版本号即可

这个npm。就是一个软件包管理工具，就和linux下面的apt软件安装差不多

安装Node.js 淘宝竞相加速器(cnpm)

这样子的话，下载会快很多

```cmd
# -g 就是全局安装
npm install cnpm -g

# 或者使用如下语句解决npm速度慢的问题
npm install --registry=https://registry.npm.taobao.org

```

安装的过程可能有点慢~，耐心等待! 虽然安装了cnpm,但是尽量少用!

安装的位置是: `c:\users\Administrator\AppData\Roaming\npm`

![image-20210127174341354](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210127174341354.png)

**安装vue-cli**

```shell
cnpm install vue-cli -g
# 测试是否安装成功
# 查看可以基于哪些模板创建vue应用程序，通常我们选择webpack
vue list
```

![cd](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210127181909237.png)



第一个vue-cli应用程序

**1、创建一个Vue项目，我们随便建立一个空的文件夹在电脑上，我这里在D盘下新建一个目录**

`D:\project\vue-study`;

**2、创建一个基于webpack模板的vue应用程序**

```shell
# 这里的myvue是项目名称，可以根据自己的需求起名

vue init webpack myvue
```

一路都选择no即可;

说明: 

- project name： 项目名称，默认 回车即可
- project description  项目描述 默认 回车 即可
- Author 项目作者， 默认 回车 即可
- install vue-router  是否安装vue-router ，选择n不安装(后期我们手动添加)
- use ESLint lint your code 是否使用ESLint做代码检查，选择n不安装(后期需要再手动添加)
- set up unit tests 单元测试相关，选择n 不安装(后期需要再手动添加)
- setup e2e tests with nightwatch 单元测试相关，选择n不安装 (后期需要再手动添加)
- should we run npm install for you after the project has been created  创建完成后直接初始化，选择n，我们手动执行，运行结果!

执行完成后：

![image-20210127183251775](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210127183251775.png)

**3. 接着，初始化并运行**

```shell
 cd myvue
 npm install  / 建议使用cnpm install 因为npm很多国外网址打不开
 npm run dev
```



4、对于在IDEA里面我们没法直接运行 npm run dev，提示权限不够的解决办法：

进入到 F:\IntelliJ IDEA 2020.2.3\bin\idea64.exe, 点击，选择鼠标右键，选择兼容性，选择以管理员运行即可



5、我们对使用npm 生成的项目，可以查看一下项目的结构

![image-20210128115523507](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210128115523507.png)



## 7.webpack的学习和使用

`什么是webpack`， 

本质上，webpack是一个现代的javaScript应用程序的静态模块打包器(module bundler),当webpack处理程序时，他会递归地构建一个依赖关系图(Dependency graph)。其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或者多个bundle

​	webpack是当下最热门的前端资源模块化管理和打包工具，他可以将许多松散耦合的模块按照依赖和规则打包成符合生产环境部署的前端资源，还可以将需要加载的模块进行代码分离，等到时机需要时在进行异步加载，通过loader转换，任何形式的资源都可以当做模块，比如CommonsJs,AMD,ES6，CSS，JSON，CoffeesScript,LESS等；

​	伴随着移动互联网的大潮，当今越来越多的网站已经从网页模式进化到了webapp模式，他们运行在现代的浏览器里面，使用html5,css4,es6等新的技术来开发丰富的功能，网页已经不仅仅是完成浏览器的基本需求，webapp通常是一个SPA，单页面应用，没一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的js代码，这给前端的开发流程和资源组织带来了巨大的挑战。

​	前端开发和其他开发工作的主要区别，首先是前端基于多语言，多层次的编码和组织工作，其次前端产品的交付是基于浏览器的，这些资源是通过增量加载的方式运行到浏览器端，如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器快速、优雅的加载和更新，就需要一个模块化系统，这个理想中的模块化系统是前端工程师多年来一直探索的难题。



`前端的模块化演进路径`

<span style="pading:10px; background:blue;color:#fff"> 1. script标签</span>

```javascript
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="module3.js"></script>
<script src="module4.js"></script>
<script src="module5.js"></script>
```

优点:

- 服务器端模块便于重用
- NPM中已经有超过45万个可用的模块包
- 简单易用

缺点:

	- 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源时异步加载的
	- 不能非阻塞的并行加载多个模块

实现:

- 服务端的NodeJS
- Browserify,浏览器端的CommonsJS实现，可以使用NPM模块，但是编译后打包后文件体积较大
- module-webmake,类似browserify，但是不如browserify灵活
- wreq,browserify的前身



<span style="pading:10px; background:blue;color:#fff"> 2.AMD</span>

Asynchronous Module Defintion规范其实主要是一个接口  define(id?. dependencies?, factory); 他要在声明模块的时候指定所有的依赖dependencies,并且还要当做形参传递到factory中，对于依赖的模块提前执行。

```
define("module", ["dep1",'dep2'], function (d1,d2){
  return someExportedValue;
});
require (["module","../file.js"], function (module,file){});
```

优点：

- 适合在浏览器环境中异步加载模块
- 可以并行加载多个模块

缺点：

- 提高了开发成本，代码的阅读和书写方式比较困难，模块定义方式的语义不畅
- 不符合通用的模块化思维方式，是一种妥协的实现

实现

- requireJS
- curl

<span style="pading:10px; background:blue;color:#fff"> 4.CMD</span>

Commons Module Definition规范和AMD很相似，尽量保持简单，并与CommonsJS和NodeJS的Modules规范保持了很大的兼容性。

```
define(function (require,exports,module){
  var $ = require("jquery");
  var Spinning = require("./spinning");
  exports.doSomething = ....;
  module.exports =....;
  return someExportedValue;
});
```

优点：

	- 依赖就近，延迟执行
	- 可以很容易的在NodeJS中运行

缺点

	- 依赖SPM打包，模块加载逻辑偏重

实现

- Sea.js
- coolie

<span style="pading:10px; background:blue;color:#fff"> 5.ES6模块</span>

EcmaScript6标准增加了JavaScript语言层面的模块体系定义。 ES6模块的设计思想，是尽量静态化，是编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonsJS和AMD模块，都只能运行时确定这些东西。

```
import "jquery"
export function doStuff(){}
module "localModule"{}
```

优点：

- 容易进行静态分析
- 面向未来的EcmaScript标准

缺点

- 原生的浏览器还没有实现该标准
- 全新的命令，新版的NodeJS才支持

实现

- Babel



大家期望的模块系统

​	可以兼容多重模块风格，尽量可以利用已由的代码，不仅仅只是JavaScript模块化，还有CSS，图片，字体资源也需要模块化!



<span style="border-bottom:3px solid; background:blue;color:#fff; ">安装WebPark</span>

webpack是一款模块加载兼打包工具，它能够把各种资源如js,jsx,es6,sass,less,图片等都作为模块来处理和使用

安装

```
npm install webpack -g   //大宝工具
npm install webpack-cli -g   //客户端

```

测试是否安装成功

`webpack -v`

`webpack-cli -v`

![image-20210128125545307](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210128125545307.png)



<span style="border-bottom:3px solid; background:blue;color:#fff; ">配置Webpack</span>

创建webpack.config.js配置文件

- entry  入口文件，指定webpack用哪个文件作为项目的入口
- output 输出，指定webpack把处理完成的文件放置到指定路径
- module 模块，用于处理各种类型的文件
- plugins 插件，如热更新，代码重用等
- resolve  设置路径指向
- watch  监听，用于设置文件改动后直接打包

```javascript
module.exports = {
	entry:"",
	output:{
		path:"",
		filename:""
	},
	module:{
		loaders:[{test:/\.js$/,loader:""}]
	},
	plugins:{},
	resolve:{},
	watch:true
}
```

直接运行webpack 命令打包

<span style="pading:10px; background:blue;color:#fff; ">使用Webpack</span>

1、创建项目 ,既新建文件夹 webpack-study，然后用idea 打开这个文件夹;

2、创建一个名为modules的目录，用于放置js模块等资源文件

3、在modules下创建文件，例如hello.js，用于编写js模块相关的代码

```javascript
//暴露一个方法: sayHi
exports.sayHi = function(){
    ducument.write("<div>Hello Webpack</div>")
};
```

4、在modules下创建一个名为main.js的入口文件，用于打包时设置entry属性

```javascript
//require 导入一个模块，就可以调用这个模块中的方法了
var hello = require('./hello');
hello.sayHi();
```

5、在项目目录下创建webpack.config.js配置文件，使用webpack命令打包

```javascript
module.exports={
    entry:"./modules/main.js",
    output:{
        filename:"./js/bundle.js"
    }
};
```

6、在项目目录下创建html页面，如index.html,导入webpack打包后的js文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>
<!--前端的模块化开发-->
<script src="dist/main.js"></script>
</body>
</html>
```

7、在IDEA控制台中直接执行webpack，如果失败的话，就使用管理员权限运行即可!

8、运行HTML看效果

说明:

```shell
# 参数  --watch 用于监听变化

webpack --watch
```



## 8. vue-router路由

学习的时候，尽量的打开官方的文档

Vue Router 是Vue.js官方的路由管理器。他和Vue.js 的核心深度集成，让构建单页面应用变得易如反掌，包含的功能有

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于Vue.js过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的css class的连接
- html5历史模式或者hash模式，在IE9中自动降级
- 自定义的滚动条行为

<span style="pading:10px; background:blue;color:#fff; ">1、安装vue-router</span>

基于第一个`vue-cli` 进行测试学习，先查看node_modules中是否存在vue-router

vue-router是一个插件包，所以我们还是需要用npm/cnpm 来进行安装的，打开命令行工具，进入你的项目目录，输入下面的命令。

> npm install vue-router --save-dev

如果npm安装不了，就得使用cnpm使用

如果在一个模块化工程中使用它，必须通过Vue.use()明确地安装路由功能，我们可有打开之前生成的myvue，进入到main.js 添加如下代码:

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

<span style="pading:10px; background:blue;color:#fff; ">2、测试vue-router</span>

step 1、先把没用的东西删除掉，就是src下面的内容

​	/assets	删除里面东西

​	/components  删除里面东西

step 2、`components` 目录下存放我们自己编写的组件

step 3、进入到/src/components/目录下，咱们自己定义一个`Content.vue`组件

```html
<template>
  <div>
    <h1>内容页</h1>
  </div>
</template>

<script>
export default {
  name: "Content"
}
</script>

<style scoped> /*scoped代表这个样式只在这个页面生效*/

</style>
```

step 4、安装路由，在src目录下，新建一个文件夹:router，专门存放路由

```javascript
import Vue from 'vue'
//导入路由插件
import VueRouter from "vue-router";
//导入我们上面定义的组件
import Content from "../components/Content";
import Main from "../components/Main"
//安装路由
Vue.use(VueRouter);

//配置导出路由
export default new VueRouter({
  routes:[
    {
      //路由的路径 和spring的 @RequestMapping 类似的
      path:'/content',
      //路由的名称
      name:'content',
      //跳转的组件
      component:Content
    },
    {
      //路由的路径
      path:'/main',
      //路由名称
      name:'main',
      //跳转的组件
      component:Main
    }
  ]
});

```

注意:

bug1 .import VueRouter from "vue-router" 的VueRouter必须和 export default new VueRouter({的 VueRouter名字一致

bug2. 在这个路由里面，我们把components里面的两个组件import进来

```
import Content from "../components/Content";
import Main from "../components/Main"
```

bug3. 在routes里面， component: Comtent，不是component:'Comtent'



step 5、进入到/src/components/目录下，咱们自己定义一个`Main.vue`组件

```vue
<template>
  <div>
    <h1>首页</h1>
  </div>
</template>

<script>
export default {
  name: "Main"
}
</script>

<style scoped>

</style>
```



step 6. 进入到项目主要入口`main.js`中配置我们的路由器

```vue
import Vue from 'vue'
import App from './App'
//导入上面创建的路由配置目录
import router from './router' //自动扫描里面的路由配置
//来关闭生产模式下给出的提示
Vue.config.productionTip = false

new Vue({
  el: '#app',
  //配置路由
  router,
  components: { App },
  template: '<App/>'
});
```



step 7. 在`App.vue` 中使用我们的路由，

```vue
<template>
  <div id="app">
    <h1>Vue-Router</h1>
    <router-link to="/main">首页</router-link>
    <router-link to="/content">内容页</router-link>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```



## 9 vue实战快速上手

### 9.1 vue+elementUI实现登录页

我们采用实战教学模式并结合ElementUI组件库，将所需要的知识点应用到实际中，以最快的速度带领大家掌握Vue的使用。

<span style="pading:10px; background:blue;color:#fff; ">1、创建工程</span>

注意，命令行都需要使用管理员模式运行

step 1. 创建一个名称为hello-vue的工程  `vue init webpack hello-vue`, 中间一路N就行

step 2. 安装依赖，我们需要安装的依赖有 vue-router、element-ui、sall-loader和node-sass四个插件

```shell
# 进入到工程目录
cd hello-vue
#安装 vue-router
npm install vue-router --save-dev
# 安装element-ui
npm i element-ui -S
# 安装依赖
npm install
# 安装SAAS加速器
cnpm install sass-loader node-sass --save-dev
# 启动测试
npm run dev
```

npm命令解释:

- `npm install moduleName`: 安装模块到项目目录下
- `npm install -g moduleName`: -g意思是将模块安装到全局，具体安装到磁盘哪个位置，要看npm config prefix的位置
- `npm install -save moduleName` : --save的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖，-S为该命令的缩写
- `npm install -save-dev moduleName` : --save-dev 的意思是将模块安装到项目目录下, 并在package文件的devDependencies 节点写入依赖，-D为该命令的缩写。



等执行完成后，就可以使用idea打开我们的文件了，进去后，先把src目录下面的 assert内的内容和components里面的内容 及App.vue里面的内容删掉



<span style="pading:10px; background:blue;color:#fff; ">2、创建登录页面</span>

把没有用的初始化东西删掉!

在源码目录中创建如下结构：

- assets 用于存放资源文件
- components 用于存放vuew功能组件
- views 用于存放Vue视图组件
- router 用于存放vue-router配置

![image-20210129002207676](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210129002207676.png)

step 1. 在Views目录下创建一个名为Main.vue的视图组件，创建首页视图

```vue
<template>
  <h1>首页</h1>
</template>

<script>
export default {
  name: "Main"
}
</script>

<style scoped>

</style>

```



step 2. 创建登录页面视图   在views目录下创建一个名为Login.vue的视图组件，其中el-* 的元素为ElementUI组件

```vue
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登陆</h3>
      <el-form-item label="账户" prop="username">
        <el-input type="text" placeholder="请输入账户" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('loginForm')">登陆</el-button>
      </el-form-item>
    </el-form>

    <el-dialog
      title="温馨提示"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <span>请输入账户和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible=false">确定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      form: {
        username: '',
        password: '',
      },
      //表单验证，需要在el-form-item元素增加prop属性
      rules: {
        username: [
          {required: true, message:'账号不可为空',trigger:'blur'}
        ],
        password: [
          {required: true, message:'密码不可为空',trigger:'blur'}
        ]
      },
      //对话框显示和隐藏
      dialogVisible:false
    }
  },
  methods:{
    onSubmit(formName){
      // 为表单绑定验证功能
      this.$refs[formName].validate((valid)=>{
        if (valid){
          //使用vue-router路由到指定页面，该方式称之为编程式导航
          this.$router.push("/main");
        }else{
          this.dialogVisible=true;
          return false
        }
      });
    }

  }
}
</script>

<style scoped>
.login-box {
  border: 1px solid #dcdfe6;
  width: 350px;
  margin:180px auto;
  padding: 35px 35px 15px 35px;
  border-radius:5px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  box-shadow: 0 0 25px #909399;
}
.login-title{
  text-align: center;
  margin: 0 auto 40px auto;
  color: #303133;
}
</style>
```





step 3.创建路由，在router目录下创建一个名为index.js的vue-router路由配置文件

```vue
import Vue from 'vue'
import Router from 'vue-router'

import Login from '../views/Login'
import Main from '../views/Main'

Vue.use(Router)

export default new Router({
  routes:[
    {
      //登录页
      path: '/login',
      name:'Login',
      component:Login
    },
    {
      //登录页
      path: '/main',
      name:'Main',
      component:Main
    },
  ]
})
```

step 4. 注册组件， 进入main.js中，将我们的Login组件和Main组件以及elementui和router等注册到main.js中

```javascript
import Vue from 'vue'
import App from './App'
// 1. 扫描router目录
import router from './router'

Vue.config.productionTip = false

// 2.添加ElementUI
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

//3.注册router和ElementUI
Vue.use(router)
Vue.use(ElementUI)

new Vue({
  el: '#app',
  //4. 添加router
  router,
  //5. 添加elementui
  render:h=>h(App) //ElementUI
})

```

step 5. 进入到App.vue中，将vue-router添加，是的路由可以成功添加

```vue
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App',
}
</script>

```

step 6. 此时执行npm run dev就可以了

总结:

bug1. 在index.js中，routes里面，是component，不是components

bug2. sass-load默认安装为10.0.1，需要降级，进入到项目根目录下，找到package.json，找到"sass-loader": "^7.3.1", 改为 7.3.1，修改完成后，执行cnpm install，安装后即可

### 9.2 vue 嵌套路由

嵌套路由又叫做子路由，在实际应用中，通常由多层嵌套的组件组合而成，同样滴，URL中各段动态路径也按照某种结构对应嵌套的各层组件，例如：

![image-20210129115039476](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210129115039476.png)



step 1、用户信息组件，在views/user目录下创建一个名为Profile.vue的视图组件

```vue
<template>
<h1>我是个人信息页</h1>
</template>

<script>
export default {
  name: "UserProfile"
}
</script>

<style scoped>

</style>
```

注意: 这里export default里面的name，我们可以自己定义一个容易被记忆的名字

step 2. 用户列表组件，在views/user目录下创建一个名为List.vue的视图组件；

```vue
<template>
  <div>
    用户列表
  </div>
</template>

<script>
export default {
  name: "UserList"
}
</script>

<style scoped>

</style>

```

step 3 . 配置嵌套路由修改Router目录下面的index.js 路由配置文件，代码如下：

```vue
import Vue from 'vue'
import Router from 'vue-router'

import Login from '../views/Login'
import Main from '../views/Main'
//用于嵌套的路由组件
import UserProfile from '../views/user/Profile' 
import UserList from '../views/user/List'

Vue.use(Router)

export default new Router({
  routes:[
    {
      //登录页
      path: '/login',
      name:'Login',
      component:Login
    },
    {
      //主页
      path: '/main',
      name:'Main',
      component:Main,
      //配置嵌套路由的样式
      children:[
        {path:'/user/profile', component:UserProfile},
        {path:'/user/list', component:UserList}
      ]
    }
  ]
})
```

说明: 主要在路由配置中增加了children数组配置，用于在该数组下设置嵌套路由



step 4. 修改首页视图，我们修改了Main.vue视图组件，此处使用ElementUI布局容器组件，代码如下：

```vue
<template>
  <div>
    <el-container>
      <!--侧边栏-->
      <el-aside width="200px">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
            <el-menu-item-group>
              <el-menu-item index="1-1">
                <router-link to="/user/profile">个人信息</router-link>
              </el-menu-item>
              <el-menu-item index="1-2">
                <router-link to="/user/list">用户列表</router-link>
              </el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
            <el-menu-item-group>
              <el-menu-item index="2-1">分类管理</el-menu-item>
              <el-menu-item index="2-2">内容列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>

          <el-submenu index="3">
            <template slot="title"><i class="el-icon-caret-right"></i>系统管理</template>
            <el-menu-item-group>
              <el-menu-item index="3-1">用户设置</el-menu-item>
              <el-menu-item index="3-2">用户权限</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>

      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>个人信息</el-dropdown-item>
              <el-dropdown-item>退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-header>
        <!--主页东西-->
        <el-main>
          <router-view/>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "Main"
}
</script>

<style scoped lang="css">
  .el-header{
    background-color: #b3c0d1;
    color:#333;
    line-height: 60px;
  }
  .el-aside{
    color: #333;
  }
</style>
```

说明:

### 9.3 参数传递及重定向

<span style="pading:10px; background:blue;color:#fff; ">方法1，采用绑定方式传参</span>

首先，在Main.Vue组件中，我们如果需要进行传参，仍然使用router-link , 当只有一个url，可以直接使用<router-link to="/user/list">用户列表</router-link>，当需要传参时候，我们可以用:  `:to="{name:'profile', params:{id:2}}"` 来进行从传递参数

```vue
<router-link :to="{name:'profile', params:{id:2}}">个人信息</router-link>
```

然后，我们进入路由，index.js中，将id绑定到路由上，形式为 `/:id`

```vue
{path:'/user/profile/:id', name:'profile', component:UserProfile},
```

最后，在Profile.vue展示出来，方式是：{{$route.params.id}}

```vue
<template>
  <div>
    我是个人信息页
    {{$route.params.id}}
  </div>
</template>

<script>
export default {
  name: "UserProfile"
}
</script>

<style scoped>

</style>
```

<span style="pading:10px; background:blue;color:#fff; ">2. 采用方法而 push方式传参</span>

首先，我们的index.js里面的routes进行一下变更，加入 props:true

```vue
{path:'/user/profile/:id', name:'profile', component:UserProfile, props:true},
```

然后，进入到我们的Main.vue里面，传参入口用法如下

```
<router-link :to="{name:'profile', params:{id:2}}">个人信息</router-link>
```

最后，进入到展示视图 Profile.vue，通过组件接收参数

```
<template>
  <div>
    <h1>我是个人信息页</h1>
    {{id}}
  </div>
</template>

<script>
export default {
  props:['id'],
  name: "UserProfile"
}
</script>

<style scoped>

</style>
```

使用push方法，主要是在展示组件中，直接放到了props属性中，取值方式发生了变化

<span style="pading:10px; background:blue;color:#fff; ">3. 实现vue重定向</span>

url不变是转发，那在vue实现重定向的方式就是增加一个redirect:'url' 即可



首先，进入到index.js里面，编辑路由，如 第三个 gohome

```vue
  routes: [{
      //主页
      path: '/main',
      name: 'Main',
      component: Main,
      //配置嵌套路由的样式
      children: [
        {path: '/user/profile/:id', name: 'profile', component: UserProfile, props: true},
        {path: '/user/list', component: UserList}
      ]
    }, {
      //登录页
      path: '/login',
      name: 'Login',
      component: Login
    }, {
      path: '/goHome',
      redirect: '/main'
    }]
```

然后，到Main.Vue里面，增加一个<router-link:to> 即可

```vue
<el-menu-item index="1-3">
  <router-link to="/goHome">回到首页</router-link>
</el-menu-item>
```

这样我们就实现了重定向

### 9.4  Vue设置路由跳转的两种方法

方法1 <router-link :to="..." >
to里的值可以是一个字符串路径，或者一个描述地址的对象。例如：

```shell
// 字符串
<router-link to="apple"> to apple</router-link>
// 对象
<router-link :to="{path:'apple'}"> to apple</router-link>
// 命名路由
<router-link :to="{name: 'applename'}"> to apple</router-link>
//直接路由带查询参数query，地址栏变成 /apple?color=red
<router-link :to="{path: 'apple', query: {color: 'red' }}"> to apple</router-link>
// 命名路由带查询参数query，地址栏变成/apple?color=red
<router-link :to="{name: 'applename', query: {color: 'red' }}"> to apple</router-link>
//直接路由带路由参数params，params 不生效，如果提供了 path，params 会被忽略
<router-link :to="{path: 'apple', params: { color: 'red' }}"> to apple</router-link>
// 命名路由带路由参数params，地址栏是/apple/red
<router-link :to="{name: 'applename', params: { color: 'red' }}"> to apple</router-link>
```

方法 2  router.push(...)方法

同样的规则也适用于router.push(...)方法。

```shell
// 字符串
router.push('apple')
// 对象
router.push({path:'apple'})
// 命名路由
router.push({name: 'applename'})
//直接路由带查询参数query，地址栏变成 /apple?color=red
router.push({path: 'apple', query: {color: 'red' }})
// 命名路由带查询参数query，地址栏变成/apple?color=red
router.push({name: 'applename', query: {color: 'red' }})
//直接路由带路由参数params，params 不生效，如果提供了 path，params 会被忽略
router.push({path:'applename', params:{ color: 'red' }})
// 命名路由带路由参数params，地址栏是/apple/red
router.push({name:'applename', params:{ color: 'red' }})
```

3、注意点

**1、关于带参数的路由总结如下：**

> 无论是直接路由“path" 还是命名路由“name”，带查询参数query，地址栏会变成“/url?查询参数名：查询参数值“;
> 直接路由“path" 带路由参数params params 不生效;
> 命名路由“name" 带路由参数params 地址栏保持是“/url/路由参数值”;

**2、设置路由map里的path值：**

>  带路由参数params时，路由map里的path应该写成: path:'/apple/:color' ;
>  带查询参数query时，路由map里的path应该写成: path:'/apple' ；

**3、获取参数方法：**

> 在组件中： {{$route.params.color}}
> 在js里： this.$route.params.color



### 9.5 404及路由钩子

<span style="pading:10px; background:blue;color:#fff; ">1. 用户登录实现传参的一个过程</span>

step 1. 在Login.vue里面，设置传递的参数

![image-20210129173338152](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210129173338152.png)

step 2. 进入到我们路由index.js中，完善Main的routes

![image-20210129173607104](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210129173607104.png)

step 3. 接下来我们进入到Main.vue里面，我们就可以吧用户给显示出来了

![image-20210129173810091](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210129173810091.png)

路由模式有两种

- hash  路径带有#符号  如 http://localhost/#/login
- history  路径不带#符号的  如 http：//localhost/login

修改路由配置，代码如下

```vue
export default new Router({
  mode:'history',
  routes: [{}]
})
```

<span style="pading:10px; background:blue;color:#fff; ">如何处理404页面</span>

step 1. 处理404创建一个名为`NotFound.vue`的视图组件，代码如下:

```vue
<template>
  <div>
    <h1>404，你的页面走丢了!</h1>
  </div>
</template>

<script>
export default {
  name: "NotFound"
}
</script>

<style scoped>

</style>
```

step 2. 然后就可以进入到index.js里面配置我们的路由

```javascript
{
    path: '*',
    name: 'NotFound',
    component: NotFound
  }
```

<span style="pading:10px; background:blue;color:#fff"> 路由钩子与异步请求</span>

beforeRouteEnter : 在进入路由前执行

beforeRouteLeave: 在离开路由前执行

上代码

```vue
<template>
  <div>
    <h1>我是个人信息页</h1>
    {{id}}
  </div>
</template>

<script>
export default {
  props:['id'],
  name: "UserProfile",
  //和java的拦截器，过滤器一样的作用,chain
  beforeRouteEnter:(to,from,next)=>{
    console.log("进入路由之前！")
    next();
  },
  beforeRouteLeave:(to,from,next)=>{
    console.log("离开路由之前！")
    next();
  }
}
</script>

<style scoped>
  
</style>
```

这样，当用户请求到没有的页面，就会进入到404页面

参数说明：

to: 路由将要跳转的路径信息

from: 路径条赚钱的路径信息

next: 路由的控制参数

	- next() 跳入下一个页面
	- next('/path')  改变路由的跳转方向，使其挑起到另一个路由
	- next(false)  返回原来的页面
	- nect((vm)=>{}) 仅在beforeRouteEnter 中可用，vm是组件实例

<span style="pading:10px; background:blue;color:#fff">在钩子函数中使用异步请求</span>

1、安装Axios cnpm install axios -s

2、在main.js中引用 Axios

![image-20210129185458887](F:\MD格式学习笔记库\狂神JAVA-17-VUE学习.assets\image-20210129185458887.png)

3 .进入到static目录中(只有static目录才能访问)，先建立一个文件夹,mock，然后在mock文件夹我们创建一个data.json，构造一个模拟的json数据

```json
{
  "name": "狂神说JAVA",
  "url": "http://blog.kuangstudy.com",
  "page": 1,
  "isNonProfit": true,
  "address": {
    "street": "含光门",
    "city": "陕西西安",
    "country": "中国"
  },
  "links": [
    {
      "name": "bilibili",
      "url": "https://space.bilibili.com/95256449"
    },
    {
      "name": "狂神说java",
      "url": "https://blog.kuangshenstudy.com"
    },
    {
      "name": "百度",
      "url": "https://www.baidu.com"
    }
  ]
}
```

添加完成后，我们可以尝试访问下，是否可以进入访问到：http://localhost:8080/static/mock/data.json

4、进入到Profile.vue组件中，我们先编写一个获取数据的方法 getData()

```vue
  methods:{
    getData:function (){
      this.axios({
        method:'get',
        url:'http://localhost:8080/static/mock/data.json'
      }).then(function (response){
        console.log(response)
      })
    }
  }
```

这句话是this.axios的使用方法，表示使用get方法请求url，请求完成后，把请求后的结果放入到response，然后执行console.log打印出来。

5、完善我们的钩子函数 beforeRouteEnter, 如下

```vue
  //和java的拦截器，过滤器一样的作用,chain
  beforeRouteEnter:(to,from,next)=>{
    console.log("进入路由之前！"); //加载数据
    next(vm => {
      vm.getData();//进入路由之前，执行我们的getData方法
    });
  },
```

在next中， vm是当前自己这个组件， 使用方法就是 vm=>{} 

这个语句的意思，就是用户在访问个人属性页，路由拦截之前，先调用自己的getData方法。

