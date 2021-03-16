# Vue学习笔记-1

## 1、 Vue简介和基础

### 1.1什么是Vue

​	是一套用于构建用户界面的渐进式的Js框架，发布于2014年2月，与其他大型框架不同的是，Vue被设计为可以自底向上逐层应用，Vue的核心只关注视图层，结合了HTML+CSS+JS,非常医用，并且有很好的生态系统。Vue体积很小，速度很快，优化很到位!

​	ssm整合，搞定后端一套，增删改查,

### 1.2 前端设计模式

> mvvm  Vue是MVVM技术开发模式的实现者!

- model 模型层，在Vue表示JavaScript对象
- view  视图层，在这里表示Dom元素
- viewmodel  

> 引入vue的方式

	- 引入工程内的Vue的js文件
	- 引入外部网络提供的vue的js文件

>  Vm 的实现原理

View Model内置另一个观察者

1) 观察视图的变化  ，视图变了，就通知数据进行变化

2)观察数据的变化，当数据变了，就通知视图进行变化

MVVM通过vm实现了双向的数据绑定

> 其他MVVM实现者

- AngularJS
- ReactJS
- 微信小程序

> 为什么用Vue.js

- 强良机
- 移动优先，更适合移动端，比如移动端的Touch时间
- 易上手
- 吸取了Angular和React的长处，还有自己独特功能
- 开源，社区活跃度高

### 1.3 快速开始

> 如何获得vue的cdn文件

- https://cdn.bootcdn.net/ajax/libs/vue/3.0.1/vue.cjs.js

> 如何在页面使用vue



> Vue对象里面有哪些东西？分别什么作用？

new Vue({

​	el:'', 该vue对象绑定到哪个div上

​	data:{

​		//提供的数据，里面存放是键值对

​	}

})

> 怎么在页面取得数据

在html的被vue绑定的元素中，通过使用插值表达式  {{变量名}} ，来获取vue对象中的数据

没有被vue绑定的html元素，是无法使用插值表达式的。

### 1.4 插(差)值表达式

插值表达式是用在html中被绑定的元素中的，目的是通过插值表达式来获取vue对象中的属性和方法。

vue对象的属性是哪里提供的？vue对象的data属性提供的

vue 对象中的方法是哪里提供的？ vue对象的methods属性提供的

语法格式 {{ vue的内容 }}  ,差值表达式只能用在html里面，不能用在html的标签里面 <a href="{{url}}">就是非法的

差值表达式的使用方法:

```vue
{{   变量  }}  //直接名字获取值

{{   [1,2,3,4][2]  }}  //数组形式，取出里面第二个

{{  {"name":"xiaoming","age":20}.age   }}  //取出来里面的age，通过.方式访问对象里面具体键值

{{    sayHi() }}      //差值表达式也可以直接调用方法
```

### 1.5 Vue中关键字

这些关键字都是做为html中标签的属性

> 1. v-model

mvvm双向数据绑定 v-model，

v-model用在标签的内部, 是将 标签的value值 与 vue实例中的data属性值 	进行绑定

```vue
<div id="app">
  请输入你的专业:<input type="text" v-model="major"/>
  <p>我是一位{{major}}的程序员</p>
</div>

const vm = new Vue({
  el:'#app',
  data:{
  major:'java'
  },
  methods:{
  sayHi:function () {
  	console.log("HelloVue");
	}
	}
});
```



> 2. v-on

通过配合具体的事件名，来绑定vue中定义的函数

```html
<div id="app">
    请输入你的专业:<input type="text" v-on:input="changeMajor"/>
    请输入你的专业:<input type="text" v-on:input="changeMajor2()"/>
    <p>我是一位{{major}}的程序员</p>
</div>

<script>
    const vm = new Vue({
        el:'#app',
        data:{
            major:'java'
        },
        methods:{
            changeMajor:function () {
               console.log("change title!");
            },
            changeMajor2:function () {
                console.log("change title!");
            }
        }
    });
</script>
```



注意点: 

1、data里面的数据和methods里面的方法名不要重名，不然会报错

2、v-on:事件名="方法名",这里面方法名既可以带()如changeMajor2()，也可以不带()如changeMajor，如果带参数，就需要带括号

> 3 v-on 增强

补充1、enent.target.value  v-on 绑定方法时，此时方法有个内置的默认参数对象 event,可以通过event.target.value获取当前事件对象的value值

```html
<div id="app">
  请输入你的专业:<input type="text" v-on:input="changeMajor"/>
  <p>我是一位{{major}}的程序员</p>
</div>

const vm = new Vue({
    el:'#app',
    changeMajor:function (event) {
       console.log("change title!");
       console.log(evetn.target.value）
       this.major = event.target.value
		}
}),
```

补充2、this this表示当前vue的实例本身，可以通过this.来调用当前vue对象的属性和方法，但是在axios()等衍生的代码部分就不能用this

补充3、v-on的简写   v-on:input=""  ==> @input=""  这两个写法是等价的

> 4 v-bind属性

我们知道插值表达式是不能写在html的标签的属性里面的，如果一定要用 vue中的属性 做为 html标签的属性 的内容，就可以通过v-bind进行属性绑定

html里面的所有属性，

```html
<div id="app">
  <a v-bind:href="link"></a>
</div>

const vm = new Vue({
        el:'#app',
        data:{
            major:'java',
            link:'http://www.baidu.com'
        },
});
```

说明1、上述式子,通过v-bind:将a标签的属性href同vue对象的link属性绑定

补充1、v-bind可简写为 :    v-bind:href="link" ==> :href="link"  连个表达式等价

> 5. v-once 属性

指明这个元素的数据只出现一次，数据内容的修改不影响此元素

```html
<div id="app">
    没有v-once:<p>{{title}}</p>

    有v-once:<p v-once>{{title}}</p>

    <input type="text" v-model="title">
</div>

    const vm = new Vue({
        el:'#app',
        data:{
            title:'膝盖元素',
            link:'http://www.baidu.com'
        }
    });
```

> 6、v-html 和v-text

v-html 会将vue中的属性的值，做为html的元素来使用

v-text 会将vue中的属性的值，只作为纯文本来使用输出

```javascript
<div id="app">
    v-html: <span v-html="mylink"></span><br>
    v-text: <span v-text="mylink"></span>
</div>

<script>
    const vm = new Vue({
        el: '#app',
        data: {
            link: 'http://www.baidu.com',
            mylink: '<a href="http://www.baidu.com">千峰</a>'
        }
    });
</script>
```

效果

![image-20210220230424624](/Users/luojingen/Library/Application Support/typora-user-images/image-20210220230424624.png)

### 1.6 vue事件

> 1、 vue如何使用事件

方式1、传递参数, v-on="addbtn(step)"  

```html
<div id="app">
    <button type="button" v-on:click="addbtnfn(2)">增加</button>
    <button type="button" v-on:click="subbtnfn(2)">减少</button>
    <input type="text" v-model="mystep">
    {{count}}
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            count: 0,
            mystep: 1
        },
        methods: {
            addbtnfn: function (step) {
                //this.count += 1;
                //this.count += this.mystep;
                this.count += step;
            },
            subbtnfn: function (step) {
                //this.count -= 1;
                //this.count -= this.mystep;
                this.count -= step;
            }
        }
    })
</script>
```

这里面有个小坑：

描述: 在input里面 v-model绑定到vue的mystep属性，但在绑定过程中，导致mystep变成字符串属性了

解决办法:在使用时候，如果我们需要作为数字计算，可以使用 this.count += this.mystep+0; 这样就可以让mystep变成数字形式参与计算

```java
<div id="app">
    <button type="button" v-on:click="addbtnfn">增加</button>
    <button type="button" v-on:click="subbtnfn">减少</button>
    <input type="text" v-model="mystep">
    {{count}}
</div>
</body>
<script>
    const vm = new Vue({
        el:'#app',
        data:{
            count:0,
            mystep:1
        },
        methods:{
            addbtnfn:function (){
                this.count += parseInt(this.mystep);
            },
            subbtnfn: function (){
                this.count -= parseInt(this.mystep);
            }
        }
    });
```



方式2、@mousemove="mymv" ,然后用鼠标移动捕获

```html
<div id="app">
      <p v-on:mousemove="mymo">
        今天是星期一: X坐标是{{x}},Y坐标是{{y}}
    </p>
</div>

let vm = new Vue({
    el: "#app",
    data: {
        count: 0,
        x:0,
        y:0
    },
    methods: {
        mymo: function (envet) {
            this.x = envet.clientX;
            this.y =envet.clientY;
            console.log(envet)
        }
    }
})
```

> 2、 事件中参数传递

设参   

```
<button type="button" v-on:click="addbtnfn(2)">增加</button>
```

传参

```
addbtnfn: function (step){}
```

接、用参

```
this.count += step;
```

> 3、 停止鼠标事件

```
<span v-on:mousemove="dummy">停止鼠标事件</span>

dummy: function (event) {
	event.stopPropagation();//停止鼠标事件
}

或者:
<span v-on:mousemove.stop>停止鼠标事件</span>


```

> 事件修饰符

@click.stop  阻止点击事件的传播

@mousemove.stop 阻止鼠标移动事件

@keyup.space 当空格弹起式触发事件

```
let vm = new Vue({
    el: "#app",
    data: {
    },
    methods: {
        mykeyupfn:function () {
            console.log("hello vue!");
        }
    }
})

//html
    <input type="text" @keyup.space="mykeyupfn">

```

### 1.7 vue改变内容和样式

我们在学习时候，主要学习是vue怎么改变内容，和vue如何改变样式，当学会使用vue改变内容或者样式，我们也就学会了vue

> 第一讲 vue改变内容

插值表达式，可以改变内容



### 1.8 计算属性computed

>  什么是计算属性？

计算属性的重点在 属性两个字(属性是名词)，首先它是属性，其次这个属性有计算的能力(计算是动词)，这个计算就是个函数，简单点数，他就是一个能够将计算结果缓存起来的属性(将行为转化成静态的属性)，仅此而已；

用法: 我们把不需要经常变换的东西，放入到computed, 从而提高效率。

> 计算属性和方法有什么区别

完整的html

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:v-bind="http://www.w3.org/1999/xhtml"
      xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>计算属性</title>
</head>
<body>

<div id="app">
    <p>调用当前时间的方法:{{ getCurrentTime()}}</p>
    <p>当前时间的计算属性:{{ getCurrentTime1 }}</p>
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: 'hello vue'
        },
        computed:{
            getCurrentTime1: function () {
                this.message;
                return Date.now(); //得到当前时间
            }
        },
        methods: {
            getCurrentTime: function () {
                return Date.now(); //得到当前时间
            }
        }
    })
</script>

</body>
</html>
```

说明

- methods 定义方法，调用方法使用currentTime()，需要带上括号
- computed： 定义计算属性，调用属性使用currentTime1，不需要带上括号，this.message是为了能够让currentTime1观察到数据变化而变化

注意: methods和computed里不能重名

结论: 

调用方法时，每次都需要进行计算，既然有计算过程则必定会产生系统开销，那如果这个结果是不经常变化的呢?此时就可以考虑将这个结果缓存起来，采用计算属性可以很方便的做到这一点；计算属性的主要特性就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销

### 1.9 watch监控属性

> 什么是监控属性

通过watch给属性绑定函数，当属性的值发生变化时，该函数就会调用。

调用时，可以接受两个参数，第一个是属性改变后的值，第二个参数是属性改变前的值；

```html
<div id="app">
    <input type="text" v-model="title">
      {{title}}
</div>

<script>
    const vm = new Vue({
        el: "#app",
        data: {
            title: 'hello vue'
        },
        watch: {
            title: function (newValue, oldValue) {//监控,当title发生改变，就会调用这个函数
                //可以给函数传参，内置参数有newValue和oldValue
                console.log("oldValue: "+ oldValue);
                console.log("newValue："+ newValue);
            }
        }
    })
</script>
```



### 1.10 vue改变样式

> 方法1、通过json格式数据改变样式

示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>change the css</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
    <style>
        .demo{
            width: 400px;
            height: 300px;
            background-color: deepskyblue;
        }
        .red{
            background-color: red;
        }
    </style>
</head>
<body>
<div id="app">
    <button @click="temp = !temp">改变颜色</button>
    <div class="demo" @click="temp=!temp" v-bind:class="{red:temp}"></div>
</div>

</body>
<script>
    const vm = new Vue({
        el:'#app',
        data:{
            temp : false
        },
        methods:{
            
        }
    })
</script>
</html>
```



说明：

1、通过给html元素的class属性，绑定vue中的属性值，得到样式的动态绑定;

2、上例，如果temp是true，>>>则 <div class ="red">  

如果temp是false，>>>则 <div class ="">

> 方法2、通过computed返回json对象改变样式

```html
<div>
  <div class="demo" :class="divClass"></div>
</div>
computed:{
    divClass: function () {
        return{//返回一个json对象
            red:this.temp,
            blud:!this.temp
        }
    }
}
```



> 方法3、双向数据绑定v-model  绑定到vue的data中一个属性

```html
<div>
    <div class="demo" :class="mycolor"></div>
    <input type="text" v-model="mycolor">
</div>

let vm = new Vue({
    el: "#app",
    data: {
        temp:false,
        mycolor: 'green'
    }
}
```



此时，在输入框输入对应的css样式，就实现了双向的数据绑定



改变样式3个方法的完整代码，供参考

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:v-bind="http://www.w3.org/1999/xhtml"
      xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>vue改变样式-computed</title>
    <style>
        .demo{
            width: 400px;
            height: 220px;
            background-color: gray;
            display: inline-block;
            margin: 10px;
        }
        .red{background-color: #ff3b61; }
        .green{background-color: green;}
        .blue{background-color: blue;}
    </style>
</head>
<body>

<div id="app">
    {{ attachRed }}
    <div class="demo" @click="temp=!temp" v-bind:class="{red:temp}"></div>
    <div class="demo" :class="divClass"></div>
    <div class="demo" :class="mycolor"></div>
    <input type="text" v-model="mycolor">
</div>

<!--导入vue-->
<script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            temp:false,
            mycolor: 'green'
        },
        methods: {
        },
        computed:{
            divClass: function () {
                return{//返回一个json对象
                    red:this.temp,
                    blud:!this.temp
                }
            }
        }
    })
</script>

</body>
</html>
```

> 方法4、多个样式操作

需求: 我们当前一个div，通过:class='' 想绑定多个属性，这个时候怎么做？

解决办法: 使用数组来解决   :class="[mycolor, mywidth]"

示例代码：

```shell
// style
<style>
.red{background-color: #ff3b61; }
.green{background-color: green;}
.blue{background-color: blue;}
.mywidth{ width: 600px;}
</style>

//html
<!--
    这个是错误的，无法被调用
    <div class="demo" :class="mycolor" :class="mw"></div>
-->
    <div class="demo" :class="[mycolor, mw, {red:temp}]"></div>
    
//vue
    let vm = new Vue({
        el: "#app",
        data: {
            temp:false,
            mycolor: 'green',
            mw:'mywidth'
        },
    }
```

注意: 不能这么写  <div class="demo" :class="mycolor" :class="mw"></div>

> 方法5、通过style设置样式

原来写法

```
<div style="background-color: aquamarine" class="demo"></div>
```

通过vue的写法

```html
<div :style="{backgroundColor: bc}" class="demo"></div>

<script>
    let vm = new Vue({
        el: "#app",
        data: {
            temp:false,
            mycolor: 'green',
            mw:'mywidth',
            bc:'aquamarine'
        },
    }
</script>
```

仍然绑定style属性，但是在style里面需要用键值对，需要大括号，对象的键background-color改为backgroundColor，bc为绑定vue对象的bc属性值

在内嵌的css样式里面指明style属性的值

> 方法6、通过computer计算设置样式

```
<p>计算属性</p>
<hr>
<div :style="myStyle" class="demo"></div>
```

```

computed:{
    divClass: function () {
        return{//返回一个json对象
            red:this.temp,
            blud:!this.temp
        }
    },
    myStyle:function () {
        return {
            backgroundColor:this.bc,
            width: this.mw+"px"
        }
    }
}
```

> 方法 7、设置style的多个样式

```
<p>设置style多个样式</p>
<hr>
<div :style="[myStyle,{height:width*2+'px'}]" class="demo"></div>
```



### 1.11 虚拟dom和diff算法

![image-20210221183900538](/Users/luojingen/Library/Application Support/typora-user-images/image-20210221183900538.png)

### 1.12 分支语句v-if

- v-if  满足条件才会显示，不满足条件就不显示
- v-else-if
- v-else
- v-show  实际上是让这个元素的display属性设为None,隐藏的效果，所以性能更好!

> v-if  满足条件才会显示，不满足条件就不显示

\<div v-if="temp">小苹果</div>

当temp的值为true时，div的内容才会被显示



示例代码

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:v-bind="http://www.w3.org/1999/xhtml"
      xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>v-if事件</title>
</head>
<body>

<div id="app">
    <button type="button" @click="temp=!temp">点我看到我</button>
    <div v-if="temp">Hello</div>
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            temp:false
        },
        methods: {
        }
    })
</script>

</body>
</html>
```

> v-else  当判断条件为false时候，才会显示

v-if的对立面

```html
<div v-else="temp">看到他了!</div>
```



> v-else-if   多次判断

多个判断条件，

```html
<div id="app">
    <button type="button" @click="temp=!temp">点我看到我</button>
    <div v-if="temp">看到我了!</div>
    <div v-else-if="temp1">!!!前方高能，串串咳嗽!</div>
    <div v-else="temp">看到他了!</div>
</div>
```

```vue
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            temp:false,
            temp1:true
        },
        methods: {
        }
    })
</script>
```

> v-show

用法和v-if相同的，也就是说v-show="布尔值变量"是true的时候，就会显示内容，是false的时候不会电视内容，但是v-show改变的是元素的演示，不显示内容时的样式 display 是noe,而 v-if是直接让元素消失或者直接添加元素，效率上，v-show更高。

```html
<body>

<div id="app">
    <button type="button" @click="temp=!temp">点我看看</button>
    <div v-if="temp">我是v-if</div>
    <div v-show="temp">我是v-show</div>
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            temp:false,
        }
    })
</script>

</body>
```

![image-20210221191116584](/Users/luojingen/Library/Application Support/typora-user-images/image-20210221191116584.png)

> template 模板标签的使用

在vue中经常会碰到这个标签，目前可以使用该标签配合v-if实现多个元素一起出现或者一起消失，注意，template不能和v-show一起使用。

```html
<div id="app">
    <button type="button" @click="temp=!temp">点我看看</button>
    <template v-if="temp">
        <div>
            <div>我是v-if</div>
            <div>我是v-show</div>
        </div>
    </template>
</div>
```

### 1.13 循环语句v-for

> 1、普通的v-for  

 是vue中循环的关键字，我们需要定义数据源，然后通过v-for来遍历数据源，再使用插值表达式输出数据

```html
<body>

<div id="app">
    <ul>
        <li v-for="a in args">{{a}}</li>
    </ul>
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            args: [1,2,3,4,5,6,7]
        }
    })
</script>

</body>
```

> 2、改进版带下标：for语句里面，key建议加上，做为标识，index是下标

```html
<div id="app">
    <ul>
        <li v-for="(str,index) in args" :key="index">
          {{index}} --- {{str}}
        </li>
    </ul>
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            args: [1,2,3,4]
        }
    })
</script>
```

> 3、遍历一个对象

```
<ul>
    <li v-for="(v, k ,i) in student">
       {{v}}-{{k}} -{{i}}
    </li>
</ul>

<script>
    let vm = new Vue({
        el: "#app",
        data: {
            args: [1, 2, 3, 4],
            student: {
                name: 'xiaoming',
                age: 20
            }
        }
    });
</script>
```

页面展示效果

![image-20210221212605225](/Users/luojingen/Library/Application Support/typora-user-images/image-20210221212605225.png)



说明:  

1)、 v 是value，k 是key， i是index;

2) 、k 是key，

3)、 i是index;

> 4、遍历增强版，遍历对象数组

```html
<div id="app">
    <ul>
        <li v-for="student in students">
            <span v-for="(v, k ,i) in student">{{i}}-{{v}} -{{k}}#####</span>
        </li>
    </ul>
</div>

<script>
    let vm = new Vue({
        el: "#app",
        data: {
            args: [1, 2, 3, 4],
            students: [{
                name: 'xiaoming',
                age: 20
            }, {
                name: 'xiaogang',
                age: 30
            }]
        }
    });
</script>
```



说明:



### 1.14 学生表练习题

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:v-bind="http://www.w3.org/1999/xhtml"
      xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>普通的for</title>
</head>
<body>

<div id="app">
    <table>
        <tr>
            <th>序号</th>
            <th>姓名</th>
            <th>年龄</th>
            <th>电话</th>
        </tr>
        <tr v-for="(user,index) in users">
            <td>{{index+1}}</td>
            <td v-for="value in user">{{value}}</td>
        </tr>

    </table>
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            users:[
                {
                    name:'小明',
                    age:20,
                    tel:'13888888888'
                },{
                    name:'小花',
                    age:17,
                    tel:'17788888888'
                },{
                    name:'小红',
                    age:26,
                    tel:'16666666666'
                }
            ]
        },
        methods: {
        }
    })
</script>

</body>
</html>
```

## 2 Vue进阶

通过前面学习，我们已经可以在一个页面使用vue的基本内容，vue对象的关键字、vue的分治、循环，我么去搭建一个简单的页面已经没有太大问题

### 2.1 vue实例(vue对象)基本特性

#### 2.1.1 一个vue实例可以操作另一个vue实例的属性和方法

**step1 新建一个页面，先创建一个vue的完整实例**

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:v-bind="http://www.w3.org/1999/xhtml"
      xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>普通的for</title>
</head>
<body>

<div id="app">
    <h1>{{title}}</h1>
    <button id="btn" @click="btnclick">show</button>
    <p v-show="showFlag">显示段落</p>
    {{lowercasetitle}}
</div>

<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: "#app",
        data: {
            title: '一个基本的vue实例',
            showFlag: false,
        },
        methods: {
            btnclick: function () {
                this.showFlag = !this.showFlag;
                this.updateTitle("this is new title");
            },
            updateTitle: function (d) {
                this.title = d;
            }
        },
        computed:{
            lowercasetitle:function () {
                return this.title.toLowerCase();
            }
        },
        watch: {
            title: function (newVal, oldVal) {
                console.log(newVal);
            }
        }
    })
</script>

</body>
</html>
```

**step 2 在同一个页面，再创建一个新的实例v2**

```javascript

<div id="app2">
    <button type="button" @click="changeTitle">改变app的标题</button>
</div>

var v2 = new Vue({
    el: "#app2",
    methods: {
        changeTitle: function () {
            vm.title="Study Vue!"
        }
    }
});
```

在这里，我们能够用v2的方法去改变vm的title属性



**step 3 除了可以调用另一个vue的属性，还可以调用另一个vue的方法**

```html
<div id="app2">
    <button type="button" @click="changeTitle">改变app的标题</button>
    <button type="button" @click="toVmMethod">调用vm的方法</button>
</div>

    var v2 = new Vue({
        el: "#app2",
        methods: {
            changeTitle: function () {
                vm.title="Study Vue!"
            },
            toVmMethod:function () {
                vm.btnclick();
            }
        }
    });
```

**总结**

1、可以通过一个vue对象操作另一个vue对象的属性、方法等,如上例中通过v2调用vm的title和btnclick()；

2、一个vue对象操作另一个vue对象的内容，维度有两个，一个是操作另一个的属性，一个是操作另一个的方法



####  2.1.2  什么是对象的实例属性，实例属性有什么特征和特性？

什么叫对象的实例属性？ 

vue实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 `$`，以便与代理的数据属性区分

在对象中带有$的叫做实例属性,类如 el,data,compute,methods等,他们并不是通过对象.的方式调用的属性，在vue中el,data等等这些键就是实例属性

```javascript
var data= {
    mytitle:"这是实例对象!"
}

var v2 = new Vue({
    el: "#app2",
    data:data, //此处是直接从外部引入data
    methods:{
        showValueObject:function () {
            console.log(vm.title)
        }
    }
});
```

实例属性还可以这么调用 v1.$data.title相当于v1.title

总结几个比较经常用的实例属性



this.$refs :用来访问v-ref指令的子组件

this.$data :  用来访问组件实例观察的数据对象

this.$el : 用来挂载当前组件实例的`dom`元素

this.$on:  监听实例的自定义事件

this.$emit : 触发事件

this.$mount('#app3')  ： 动态的将一个实例挂载到一个html元素上

#### 2.1.3 vue对象的data赋值

vue对象，除了自己定义data，还可以定义data变量然后赋值

```javascript
<script>
    var vm = new Vue({
        el: "#app",
        data:{
            title:"vuetitle"
        }
    });

    var data= {
        mytitle:"这是实例对象!"
    }
    var v2 = new Vue({
        el: "#app2",
        data:data,
        methods:{
            showValueObject:function () {
                console.log(vm.title)
            }
        }
    });
</script>
```

#### 2.1.4 实例属性ref的用法：相当于是id

```html
<div id="app">
    <button type="button" ref="mybtn1"  @click="btnclick">show</button>
    <button type="button" ref="mybtn2"  @click="btnclick">show</button>
</div>

    var vm = new Vue({
        el: "#app",
        methods: {
            btnclick: function () {
                this.$refs.mybtn1.innerText = "tttttbbbbbnnnnn";
            }
        }
    });
```

refs使用

1、在vue中，往往使用refs属性来代替id属性的舒勇，那么就可以快速的通过调用ref的值来获得页面中某个元素。

2、页面view中button，可以通过vue的实例属性 $refs来快速找到标记了ref的对应页面元素

#### 2.1.5 动态绑定vue实例到页面中

mount,是加载的意思,实现了页面的元素和vue对象的动态绑定，之前都是通过el的方式来绑定，我们也可以通过vue的mount实例属性来动态绑定。

```html

<div id="app3"></div>


<script>
    var vm = new Vue({
        el: "#app",
        methods: {
            btnclick: function () {

                this.$refs.mybtn1.innerText = "tttttbbbbbnnnnn";
            }
        }
    });
    
    var v3 =new Vue({
        template:"<h1>Hello,Vue!</h1>"
    });

    v3.$mount("#app3")
</script>
```

### 2.2 vue 组件

万事万物皆对象，vue的一大特性，组件化, 全局注册

#### 2.2.1 、如何全局化注册一个组件？ 

```vue
Vue.component("comp1", {
    template:"<h1>hello</h1>"
});
```

注册的形式，双引号里comp1是组件名字，{}里面是组件的实例, 其实里面就是一个vue的对象

如何使用一个组件？像使用一个html标签一样使用

```html
<div id="app">
    <comp1></comp1>
</div>
```

我们可以写的更加完善一些：

```javascript
    Vue.component("comp1",{
        template:"<div>" +
            "{{title}}" +
            "<button type='button' @click='btnfn'>hello</button>" +
            "</div>",//会显示在页面中
        data:function(){
            return {
                title:"hello"
            }
        },

        methods: {
            btnfn:function () {
                console.log("Hello Vue")
            }
        }
    });
```

总结:

1、可以将vue对象做为一个组件，被反复使用

2、要想使用组件化，就需要在页面中注册组件，注册的方式有两种，分别为全局注册和局部注册

- 全局注册： 通过Vue.component("组件名",{vue对象})
- 局部注册:  components:{  "my-comp":comp}  

3、使用组件，只有在被vue绑定了的html元素中才能使用组件,如果一个div没有被vue绑定，那么这个div就不能使用之前注册的组件，简单来讲，就是比如<div id="app"> 已经被vue绑定了，这个时候里面才能使用 <comp1>这个组件，如果 <div id="app2">没有绑定到vue上，那是不能使用<comp1>组件标签。

#### 2.2.2 、做为组件的vue对象和之前的vue对象的区别



**区别1、data实例属性发生了区别**

原来

```javascript
var vm = new Vue({ 
	data:{ 
		name:"小明"
	}
})
```

现在,作为组件

```javascript
Vue.component("com1",{
	data:function(){
		return {
			name:"小明"
		}
	}
})
```

**区别2、这种组件中，template的写法**

template 是将内容展现在页面上的一个键,这个值是一个字符串。一定要注意一个点，template里必须有且只能有一个根元素

错误的写法

```html
<template>
    <div>我是兄弟</div>
    <div>
        {{title}}
        <button type='button' @click='btnfn'>hello</button>
    </div>
</template>
```

正确的写法:

```html
<template>
    <div>
        <div>我是兄弟</div>
        <div>{{title}}<button type='button' @click='btnfn'>hello</button></div>
    </div>
</template>
```

#### 2.2.3 、vue的生命周期

一个vue对象会经历初始化、创建、绑定、更新和销毁等阶段，不同的阶段都会有相应的生命周期勾子函数(Hook)

可以参考官方文档的那个图：

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)



#### 2.2.4.一个页面只有一个div，其他都是组件

vue组件里的data必须使用 function(){ return {xxx:xxxx, yyy:yyyy}}的形式对对象进行封装，防止对其他数据的修改。

注意:template里面必须有一个根节点

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:v-bind="http://www.w3.org/1999/xhtml"
      xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>vue的组件第一讲</title>
</head>
<body>

<div id="app">
    <comp1></comp1>
</div>



<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    Vue.component("comp1",{
        template:"<div>我是兄弟</div><div>" +
            "{{title}}" +
            "<button type='button' @click='btnfn'>hello</button>" +
            "</div>",//会显示在页面中
        data:function(){
            return {
                title:"hello"
            }
        },

        methods: {
            btnfn:function () {
                console.log("Hello Vue")
            }
        }
    });

    const vm = new Vue({
        el: "#app",
        data: {},
        methods: {}
    })
</script>

</body>
</html>
```



#### 2.2.5、局部注册    改进版，提高代码的复用性, vue组件的本地注册

本地注册和全局注册的区别: 我们在下面的例子，my-comp这个组件不能被用到 <div id="app2">里面的，这就是本地注册和全局注册的区别。

全局注册; 可以在任意一个div中使用，采用 Vue.component("de",{})注册

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:v-bind="http://www.w3.org/1999/xhtml"
      xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>vue的组件本地定义</title>
</head>
<body>

<div id="app">
    <my-comp></my-comp>
</div>
<div id="app2">
    <you-comp></you-comp>
</div>



<!--导入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var comp={
        template:"<p>" +
            "{{status}}" +
            "<button @click='changebtn'>btn</button>" +
            "</p>",
        data(){
            return {
                status:"hello"
            }
        },

        methods: {
            changebtn:function () {
                console.log("Hello Vue");
                this.status ="被点击了!";
            }
        }
    };

    new Vue({
        el: "#app",
        components:{
            "my-comp":comp
        },
        data: {},
        methods: {}
    });
    new Vue({
        el: "#app2",
        components:{
            "you-comp":comp
        },
        data: {},
        methods: {}
    })
</script>

</body>
</html>
```





### 2.3.vue-cli

```shell
cli (command line interfaces)命令行接口，在进行vue项目开发时，可以选择不同的vue模板进行项目的搭建，比如simple,webpack-simple,webpack,browserify/browserify-simple等，vue-cli是官方提供的一个脚手架，用于快速生成一个vue的项目模板。

我们后端使用maven来创建项目，有两个目的，
1、通过maven的依赖机制，能够快速的管理依赖
2、通过maven来确定项目的结构，所谓的项目结构就是项目里面有哪些文件和文件夹，文件夹就是一个怎么样的层级关系。

问题来了？一个VUE项目里的项目结构应该是什么样的呢?我们能不能快速的获得这样的项目结构呢？
可以通过vue-cli这种脚手架工具来解决这样的问题。
vue-cli里面存放很多常用的项目骨架，直接拿来用就可以搭建出来一个拥有比较成熟的项目结构的项目。
```

#### 2.3.1 vue-cli怎么用？ 详细的步骤

step1 下载node.js  https://nodejs.org/en/download

node.js是一个机遇chrome v8引擎的，要想使用vue-cli，就得先安装node.js，node.js就是一个可以让前端运行在nodejs提供的服务器下的一个工具，换句话说，就是提供了一个服务器.



step 2、在node.js的cmd组件(command prompt) 中, 使用node.js安装vue-cli

```
npm -v # 如果看到版本就是安装成功了

npm install vue-cli -g
```

npm  知名使用node.js，install 安装，vue-cli 就是安装的东西，-g 就是全局使用



step 3 使用vue-cli 下载项目骨架模板，搭建我们的项目，创建vue项目文件夹并打开

```
vue list

mkdir d:/vuedemo

cd d:/vuedemo
```

step 4、使用vue list命令查看当前可用的vue骨架



step 5、使用vue命令创建基于vue-webpack-simple骨架的项目，vue-cli-demo是项目名，过程中需要输入一些参数，回车是使用提示的值

```
vue init webpack-simple vue-cli-demo
```

vue list

使用命令来搭建项目骨架

vue init 骨架名 项目名

例如: vue init webpack-simple  vue-cli-demo 

step 6、创建基于webpack骨架的项目

进入项目后，执行npm install 下载依赖

![image-20210223110805604](/Users/luojingen/Library/Application Support/typora-user-images/image-20210223110805604.png)

下载完依赖，执行npm run dev 就可以启动了，这时候就能去浏览器中看到当前项目了。

![image-20210223110827037](/Users/luojingen/Library/Application Support/typora-user-images/image-20210223110827037.png)

#### 2.3.2  一个vue-cli项目结构

在项目中， ./  当前路径  ../ 是上级路径



**分析一下vue-cli的 项目内结构**

1、index.html  不管项目多复杂，固定都是11行，实际内容已经被打包到了/dist/build.js了



2、main.js 文件，是整个vue项目的入口js

```
//把vue需要组件导入
import Vue from 'vue'   
// ./是当前路径 ../是上级路径 找到当前路径的App.vue
import App from './App.vue'


new Vue({
  // 让当前vue对象绑定页面上的id是app的那个div
  el: '#app',
  // 让app.vue的内容展现在该div中
  render: h => h(App)
});
```

3、App.vue，这种以.vue扩展名的内容，实际上就是一个vue对象，在这里这种文件也叫做vue组件

![image-20210223113902518](/Users/luojingen/Library/Application Support/typora-user-images/image-20210223113902518.png)

4、我们的idea需要配置一个vue插件，具体可以百度如何在idea安装vue插件，不安装vue插件，idea无法识别vue





#### 2.3.3 编写app.vue

**注意：**一个template是必须要有一个根节点的,下面这种形式会报错

```vue
<template>
  <div>
    {{title}}
  </div>
  <span></span>
</template>
```





**全局注册： 演示在App.vue组件中使用另一个vue组件**

step 1、 在项目中，创建一个components文件夹，然后在里面建home.vue和Header.vue

home.vue

```html
<template>
  <div>
    label:{{label}}
    <button @click="changeLabel">点我</button>
  </div>
</template>

<script>
  export default {
    name: "Home",
    data(){
      return{
        label:"i am a label"
      }
    },
    methods:{
      changeLabel: function () {
          this.label = "yes ,you are a label!"
      }
    }
  }
</script>

<style scoped>

</style>
```

Header.vue

```html
<template>
  <div>
    <h1>{{title}}</h1>
    <span></span>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        title: '这个是Header模板页'
      }
    }
  }
</script>

<style>

</style>
```



step 2、进入到main.js，我们进行全局注册这三个组件

```html
//把三个组件导入进来
import Header from './components/Header'
import Content from './components/Content'
import bottom from './components/bottom'

//全局注册组件，这样可以在整个项目把组件拿来当标签用
Vue.component("MyHeader",Header)
Vue.component("MyContent",Content)
Vue.component("MyBottom",bottom)
```



step 3、 进入到App.vue里，就可以使用我们的全局组件

```html
<template>
  <div id="app">
    <MyHeader></MyHeader>
    <MyContent></MyContent>
    <MyBottom></MyBottom>
  </div>
</template>

<script>
  export default {
    name: 'app',
  }
</script>

<style>

</style>
```



**小结:**

全局注册： 在main.js中通过import和Vue.component配合，来将一个.vue文件注册成为一个标签，。这个标签就可以在整个项目中全局使用



#### 2.3.4 本地注册,组件中嵌套组件



step 1、我们在上一个项目的基础上，进入到main.js，把全局注册的组件删除掉

step 2、进入到App.vue，本地注册三个组件

```html
<template>
  <div id="app">
    <MyHeader></MyHeader>
    <MyContent></MyContent>
    <MyBottom></MyBottom>
  </div>
</template>

<script>
  //把三个组件导入进来，局部注册
  import Header from './components/Header'
  import Content from './components/Content'
  import bottom from './components/bottom'


  export default {
    name: 'app',
    data() {},
    components: {
      "MyHeader": Header,
      "MyContent": Content,
      "MyBottom": bottom
    }

  }
</script>

<style>

</style>
```

这样注册的三个组件，称之为局部注册，这些组件只能在本地App.vue使用，不能去其他地方如Content.vue使用

### 2.6 组件之间传参

#### 2.6.1  组件之间传参-父传子

组件之间参数传递:父传子， 可以在子组件使用  props：[] 接收

step 1、,我们定义一个Content组件，进入子组件里面设参，如下图

```html
<template>
  <div>
    这是一个商品列表.....
    {{ mytitle }}
  </div>
</template>

<script>
  export default {
    name: "Content",
    props:[mytitle]
  }
</script>

<style scoped>

</style>
```

step 2、接下来，我们进入到父组件App.vue，我们传递一个参数给子组件，形式如下, ：mytitle为子组件里面的 props中参数, msg为App.vue的参数，

```html
<template>
  <div id="app">
    <MyContent :mytitle="msg"></MyContent>
  </div>
</template>

<script>
  //把三个组件导入进来，局部注册
  import Content from './components/Content'

  export default {
    name: 'app',
    data() {
      return {
        msg: "父传子传值案例！"
      }
    },
    components: {
      "MyContent": Content,
    }

  }
</script>

<style>

</style>
```



step3、经常上述步骤，参数从父组件传递到了子组件，下一步就是接参数

![image-20210223151049940](/Users/luojingen/Library/Application Support/typora-user-images/image-20210223151049940.png)

插值表达式获得vue属性的值有三种方式

1、data

2、computed  属性计算

3、props 其他组件传过来的属性



通过子组件的的props部分，来指明可以接收的参数，父组件通过在标签中写明参数的键值来传递参数。

**props 是表示一个组件的参数部分，那么props的写法有两种：**

1) props :[参数列表]  比如 props['title','myprice',.....]

2) props:{参数名:{type:String,required:true,default:'xx'},}，例如

```javascript
props:{
  myName:{
    type:String,  //类型
    required:true,  //是否必须
    default:'xx'  //默认值
  },
  myTitle:{
    type: String,
    required: true,
    default: '学习vue的props传参'
  }
}
```

#### 2.6.2 组件之间传参 -子传父

子组件可以反向调用父组件中的函数并传递参数，下面演示如何进行这种操作。

step 1、在父组件App.vue中，我们对子组件的调用，增加方法的绑定

```html
<template>
  <div id="app">
    <MyContent :myName="name" :ffn="changeName"></MyContent>
  </div>
</template>

<script>
  //把三个组件导入进来，局部注册
  import Content from './components/Content'

  export default {
    name: 'app',
    data() {
      return {
        name: "xiaoyu"
      }
    },
    methods:{
      changeName:function (name) {
        this.name=name
      }
    },
    components: {
      "MyContent": Content,
    }
  }
</script>

<style>

</style>
```



step 2、 在子组件Content.vue里面，我们定义ffn参数，并生命为方法，然后我们定义方法

```html
<template>
  <div>
    这是一个商品列表.....
    <h1>{{ myName }}</h1>
    <button type="button" @click="ffn('xiaoyu')">点我</button>
  </div>
</template>

<script>
  export default {
    name: "Content",
    props:{
      'myName':{
        type:String,
        required:true,
        default:'这是一个props的另一个形式!'
      },
      'ffn':{
        type: Function,
      }
    },
  }
</script>

<style scoped>

</style>
```



step 3、改进的参数传递，常用，从下往上的事件发射

```javascript
  doClick() {
    //this.ffn("xiaoyu");
    this.$emit('newName','xiaoyu');
  }
```


程序执行的过程

![image-20210223161938729](/Users/luojingen/Library/Application Support/typora-user-images/image-20210223161938729.png)

#### 2.6.3 组件之间传参-子传父优化

step 1、在Content.vue中，点击按钮，就会调用doClick方法，而doClick方法就是向上级发射一个方法，这个方法的关键字是 newName，传递的参数是xiaoyu

```javascript
<template>
  <div>
    这是一个商品列表.....
    <h1>{{ myName }}</h1>
    <button type="button" @click="doClick">点我</button>
  </div>
</template>

methods:{
      doClick() {
        //this.ffn("xiaoyu");
        this.$emit('newName','xiaoyu'); //第一个位置，是方法的关键字， 第二个位置是子组件传递给父组件的参数
      }
    }
```

step 2、在App.vue中，根据发射出来的标识，绑定到父组件具体方法上，

```javascript

<MyContent :myName="name" @newName=changeName($event)></MyContent>  // @ 为点击事件的缩写，newName 为子组件发射出来的方法关键字， changeName 为父组件的方法名

methods:{
      changeName:function (name) {
        this.name=name
      }
},
```

这样，当点击子组件的click，就会把事件按照关键字，键值的方式向上发射，上层捕获即可

在子组件中，使用的是this.$emig("键","值")

在父组件中，子组件的标签中使用@键=”msg=$event“  其中$event就能得到值，msg是父组件中的vue的属性



完整的代码

App.vue

```javascript
<template>
  <div id="app">
    <h1>Vue-Router</h1>
    <router-link to="/main">首页</router-link>
    <router-link to="/content">内容页</router-link>
    <hr>
    <MyContent :myName="name" @comnentAction="changeName($event)"></MyContent>
    <router-view></router-view>
  </div>
</template>

<script>
import Content from "./components/Content";
export default {
  name: 'App',
  data(){
    return{
      name:"xiaoyu from Father vue"
    }
  },
  methods:{
    changeName:function (name){
      this.name = name;
    }
  },
  components: {
    "MyContent": Content,
  }
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

Content.vue 

```html
<template>
  <div>
    这是一个商品列表内容页.....
    <h1>我是子组件Content:--{{ myName }}</h1>
    <button type="button" @click="doClick">点我会从把子组件的xiaoyu传递给父组件获</button>
  </div>
</template>

<script>
export default {
  name: "Content",
  props:{
    'myName':{type:String,required:true,default:'这是props的另一种形式'},
  },
  methods:{
    doClick: function (){
      this.$emit('comnentAction','xiaobai')
    }
  }
}
</script>

<style scoped> /*scoped代表这个样式只在这个页面生效*/

</style>
```

