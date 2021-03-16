# Vue学习笔记-2

## 0 Vue中的表单使用

### 0.1 v-model修饰符

创建一个表单，通过v-model绑定data中的属性

先创建的页面模板

```
<template>
  <div id="app">

    <div style="width: 50%" class="container">
      <div>
        <h3>Register</h3>
        <h5>Email</h5>
        <input type="text" class="form-control" v-model="mail"/><br/>

        <h5>Password</h5>
        <input type="text" class="form-control" v-model="password"/><br/>

        <h5>Gender</h5>
        <input type="radio" name="gender" v-model="gender" value="false"/>男
        <input type="radio" name="gender" v-model="gender" value="male"/>女 <br/>

        <h5>hobby</h5>
        <input type="checkbox" name="hobby" v-model="hobby" value="music">音乐
        <input type="checkbox" name="hobby" v-model="hobby" value="movie">电影
        <input type="checkbox" name="hobby" v-model="hobby" value="sport">运动 <br/>

        <button type="button" class="btn btn-success">(成功)Success</button>

      </div>
    </div>
  </div>
</template>
```

index.html

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>myvuedemostudy</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet">
  </head>
  <body>
    <div id="app"></div>
    <script src="/dist/build.js"></script>



    <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
    <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
    <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js"></script>
  </body>
</html>
```



## 3.vue发射ajax需求

### 3.0 axios一篇优秀博客

https://www.imooc.com/article/287900



### 3.1 什么是axios

```bash
Axios 是一个基于 promise 的 HTTP 库，简单的讲就是可以发送get、post请求。说到get、post，大家应该第一时间想到的就是Jquery吧，毕竟前几年Jquery比较火的时候，大家都在用他。但是由于Vue、React等框架的出现，Jquery也不是那么吃香了。也正是Vue、React等框架的出现，促使了Axios轻量级库的出现，因为Vue等，不需要操作Dom，所以不需要引入Jquery.js了。
```

### 3.2 为什么要使用axios





### 3.3 怎么使用axios

> step  1 首先，安装我们的axios   npm install --save axios vue-axios 或者直接引用
>
> <script src="https://unpkg.com/axios/dist/axios.min.js"></script>

axios的示例代码如下

```
// GET
axios.get('/user', {
  params: {
    ID: 12345
  }
})
.then(function (response) {
  console.log(response);
})
.catch(function (error) {
  console.log(error);
});

// POST
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```



> step 2、在项目main.js里面，激活axios,引入进来

```
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios);
```

> step 3、发送ajax请求

```javascript
this.axios({
  method:'get',
  url:'http://localhost:8080/register',
  data:{}
}).then(function (response) {
  console.log(response.data)
});
```

发送post请求

```vue
this.axios({
  method:'post',
  url:'http://bit.1y/2mTM3ny',
  data:{
    mail: this.mail,
    password: this.password
  }
}).then(function (response) {
  console.log(response.data)
  this.$data =response.data
});
```



> step 4、服务器端解决跨域问题

```
<mvc:cors>
	<mvc:mapping path "/**"
			alllowed-origins="*"
			allowed-methods="post,GET,OPTIONS,DELETE,PUT",
			allowed-headers="Content-Type,Access-Control-Allow-Headers,Authorization,X-Requested-With",
			allowed-credentials="true" />
</mvc:cors>
```

在srping-mvc.xml中加入上述一段，其中allowed-origins指的是允许的访问源的域名，*表示任何人都可以访问，也可以指明具体在域名

> stpe 5、解决axios无法传递data中的参数问题

原因:默认情况下发送axios时，请求头中的内容类型为

Content-Type:application/json;charset=UTF-8

*作业: 实现一个商品列表，后台是ssm脚手架，前台请求获取商品列表*

### 3.4 什么是跨域问题

在浏览器运行ajax请求时会出现跨域的问题，那什么是跨域，什么又是域？如何解决跨域问题。

例如

我们vue项目部署在的服务器

http://localhost:8080

后端接口部署在的服务器

http://localhost:8090

什么是跨域问题？

跨域，指浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制。

什么是同源？

所谓同源就是指协议、域名、端口都相同的

如何解决跨域

CORS是一个W3C标准，全程为跨域资源共享(Cross origin resource sharing) 它允许浏览器向跨院服务器发出ajax请求

2) 使用jsonp解决跨域问题，用于解决主流浏览器跨域访问问题

cors和jsonp目的想动，但比jsonp更强大，因为cors食指所有请求

3) 还可以使用nginx反向代理

> springmvc配置cors

```xml
<mvc:cors>
    <mvc:mapping path="localhost:8090/*"
    allowed-origins="*"
    allowed-methods="GET,POST,OPTIONS,DELETE,PUT,PATCH"
    allowed-headers="Content-Type,Access-Control-Allow-Headers,Authorization,X-Requested-With"
    allow-credentials="true"/>
</mvc:cors>
```



> 配置拦截器解决跨域

```java
public class HeaderFilter implements Filter {

    private static final String OPTIONS_METHOD = "OPTIONS";

    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain chain) throws IOException, ServletException {
        System.out.println("==>HeaderFilter");

        HttpServletRequest req = (HttpServletRequest)servletRequest;
        HttpServletResponse res = (HttpServletResponse) servletResponse;

        String origin = req.getHeader("Origin");

        if(!StringUtils.isEmpty(origin)) {
            // 允许客户端的域
            res.addHeader("Access-Control-Allow-Origin", origin);
            // 允许客户端提交的Header
            String requestHeaders = req.getHeader("Access-Control-Allow-Headers");
            if (!StringUtils.isEmpty(requestHeaders)) {
                res.addHeader("Access-Control-Allow-Headers", requestHeaders);
            }
            // 允许客户端访问的Header
            res.addHeader("Access-Control-Allow-Headers", "Cache-Control, Content-Language, Content-Type, Expires, " +
                    "Last-Modified, Pragma");
            // 允许客户端携带凭证信息
            res.addHeader("Access-Control-Allow-Credentials", "true");
            // 允许客户端请求方法
            res.addHeader("Access-Control-Allow-Methods","GET, POST, PUT, OPTIONS, DELETE");
            if (OPTIONS_METHOD.equalsIgnoreCase(req.getMethod())) {
                res.setStatus(HttpServletResponse.SC_NO_CONTENT);
                res.setContentType(MediaType.TEXT_HTML_VALUE);
                res.setCharacterEncoding("utf-8");
                res.setContentLength(0);
                res.addHeader("Access-Control-Max-Age", "1800");
                return;
            }
        }
        chain.doFilter(req, res);
    }

    public void destroy() {

    }
}

```

> springboot配置cors

1、使用java配置方式



2、使用注解配置方式

### 3.5 解决axios无法传递data中的参数问题

原因:默认情况下发送的axios时，请求头中的内容类型为：

```
Content-Type:application/json;charset=UTF-8
```

而实际上服务器需要的是

```
Content-Type:application/x-www-form-urlencoded
```

因此，使用axios的内置中的方法进行内容类型的转换

```javascript
import Qs from 'qs'
onSubmit:function () {
  this.axios({
    method:'post',
    url:'http://localhost:8081/register',
    transformRequest: [function (data) {
      return Qs.stringify(data)
    }]，
    data:{
      email: this.email
  }
  }),then(function (response){
      
  })
}
```



注意： 如果前端发过去的是json格式的数据，那么后端就要用@RequestBody来接收，下面是例子

```java
@RequestMapping(value = "login". method = ReqeustMethod.POST)
public String login(@RequestBody LoginUserDTO user){
    String username = user.getName();
    String passwrod = user.getPassword();
    if ("xiaoming".equeals(username) && "123456".equals(password)){
        return "success";
    }
    return "error";
}
```



此时前端的Login.vue的请求

```javascript
onSubmit(formName){
    this.$refs[formName].validate((valid) =>{
        var vm = this;
        if (valid){
            alert(‘submit!’);
            //发送axios请求
            this.axios({
                
                methods:'post',
                url:'http://localhost:8090/login/',
                data:{
                    //在axios里面不能用this调用到vue对象
                    name:'vm.form.name',
                    password:'vm.form.password'
                }
            }).then( function(resp){
              if（resp.data=="successs"){
                  alert("登录成功");
              }else{
                  vm.$message.error("用户名或者密码错误")
              }
            })
        }else{
            this.$message.error("用户名或密码格式错误!");
            return false;
        }
    }
}
```



## 4、vue-router路由

### 4.1 什么是路由

路由的功能，在数据通信时，帮你选择通信的路线。

我们的vue中路由，能够帮助我们在一个vue组件中实现与其他组件之间的相互切换

可以通过路由模块，将指定的组件显示在路由视图中，如何实现的路由跳转呢？



### 4.2 vue-router的安装和使用

路由由第三方提供

Step 1、安装路由, 安装后最好执行以下npm install ,安装以下依赖

```
npm install vue-router -S
```

step 2、设计路由的界面

在src文件夹下创建components文件夹，文件夹内分别创建user,home组件

user.vue

```
<template>
    <div>
      <h1>用户列表</h1>
    </div>
</template>

<script>
    export default {
        name: "user"
    }
</script>

<style scoped>

</style>
```

home.vue

```
<template>
  <div>
    <h1>我是首页</h1>
    label:{{label}}
    <button @click="changeLabel"></button>
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



step3、创建路由表：在src下创建routes.js，这个名字不能变，在webpack-simple的模板下，就得叫routes.js

```
import home from './views/Home'
import user from './views/user'

export const routes = [
  {path: '/home', component: home},
  {path: '/user', component: user}
];
```



stpe4、引入路由模板并使用 

在main.js中引入路由模块并使用，注册路由表

```
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

import VueRouter from 'vue-router'  //1.引入路由模块
import {routes} from "./routes";  //2.引入静态路由表
Vue.use(VueRouter);   //3.引入路由模块
//4.创建一个VueRouter模块的实例
const router = new VueRouter({
  routes:routes
});

new Vue({
  el: '#app',
  //5 把router实例放到vue实例中
  router,

  render: h => h(App)
});
```



step5、创建路由连接和路由视图，路由体验, 我们可以去体验下

App.vue

```
<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <span>
          <router-link to="/home">首页</router-link>
        </span>
        <span>
          <router-link to="/user">用户列表</router-link>
        </span>
      </div>
      <div class="row">
        <div class="col-xs-12 col-sm-8">
          <router-view></router-view>
        </div>
      </div>
    </div>
  </div>
</template>
```

改变url，发现<router-view><router-view>中的内容发生改变

- http://localhost:8080/#/		显示home
- http://localhost:8080/#/user    显示user

向router实例添加mode属性：

	- 值 hash: url带 #  适用于调试模式
	- 值history  url不带#

### 4.3 路由间参数传递

下面是一个例子，先看看效果，我们把bootcss的相关css,js引入加到index.html，然后我们在App.vue里， 写成这个样子

```html
<template>
  <div id="app">
    <div class="container">
      <!--导航-->
      <nav class="navbar navbar-default navbar-static-top">
        <div class="container">
          <div class="navbar-header">
            <router-link class="navbar-brand" to="/">宝淘商城</router-link>
          </div>
          <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav">
              <li class="active">
                <router-link to="/home">首页</router-link>
              </li>
              <li>
                <router-link to="/user">用户</router-link>
              </li>
              <li>
                <router-link to="/home">Contact</router-link>
              </li>
            </ul>
          </div><!--/.nav-collapse -->
        </div>
      </nav>

      <div class="row">
        <div class="col-xs-12 col-sm-8">
          <router-view></router-view>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {}
    }

  }
</script>

<style>

</style>
```



什么叫做参数传递？

传统传递参数 :

1、路由表：{path: '/user/:id', component: user},

2、在user.vue里面:        <p>userId: {{id}}</p>  关键语句是 id:this.$router.params.id

```
<template>
    <div>
      <h1>用户列表</h1>
      <p>userId: {{id}}</p>
    </div>
</template>

<script>
    export default {
      name: "user",
      data(){
          return{
            id:this.$router.params.id
          }
      }
    }
</script>

<style scoped>

</style>
```



**总结：**

如何设参？  通过在路由表，设置路由参数   `{path: '/user/:id', component: user},`

如何传参?  通过router-link-to的访问路径时携带参数  `<router-link to="/user/10">用户</router-link>`

如何接参?  在目标vue中，我们找到通过vue中data(),在里面return $route.params.id就行  id=this.$route.params.id

还可以通过props来进行接参，但props有的时候可能接不到参数

> 扩展，当我们能够拿到id，我们就可以从后台拿到商品列表，然后填充到前台页面

step 1、在模板vue中，例如本例子中的User.vue中，加入created(){}方法，在这里，我们通过axios，请求后端接口，获得对应用户的列表，填充到data(){ return中}



step 2、在data()中我们就可以完善下设计,加入一个users， 这个时候，我们在view里面就可以获取到users了

```
<script>
    export default {
        name: "user",
      data(){
          return{
            id:this.$router.params.id,
            users:[],
          }
      },

      created() {
        this.axios({
          method:'get',
          url:'http://localhost:8080/register',
          data:{}
        }).then(function (response) {
          console.log(response.data)
          this.uses = response.data;
        })
      }
    }
</script>
```

### 4.4 路由之间跳转方式

方式1、通过html中的路由连接 <router-link to ="路由地址“> </router-link>进行跳转

```
<li><router-link to="/user/1">用户</router-link></li>
```

方式2、通过js实现路由的跳转,如下例

```html
view部分
<button type="button" @click="btnfn">点我</button>


js部分
    methods:{
      btnfn: function () {
        this.$router.push("/header/1");
      }
    }
```

## 5. vue资源管理

### 5.1 scoped

如果组件有scoped ,代表这个样式表，只在本vue里面生效，不会全局生效，如果没有标上scoped，则代表这个样式是全部页面生效



### 5.2 关于资源打包

开发一个vue项目，标准步骤

1- 用vue-cli 拉取一个项目骨架 vue init webpack myvuedemo3

2- 安装依赖   npm install

3- 使用npm run dev 进入开发者模式

此时在开发者模式中，各种修改都能看到实时效果，那么这些内容实际上是由vue-cli进行打包并发布在node.js上的，那么最后开发完成后，要上生产线，这些资源是需要我们手动部署在自己的服务器上的。

​	那么哪些资源需要我们手动部署? 因此，通过npm run build命令来构建资源。

4- 使用npm run build 命令来构建资源

会产生一个dist文件夹，这个文件夹里面就包含静态资源



## 6 webpack 项目+elementui

### 6.1 新建一个webpack骨架

**1、先做项目,使用webpack模板  vue init webpack myvuedemo3, 需要确认的项目都是否**

![image-20210225124237923](/Users/luojingen/Library/Application Support/typora-user-images/image-20210225124237923.png)

**2、安装依赖 vew-router、  elemetn-ui 、sass -loader  node-sass axios 五个插件，用--save-dev模式安装， 安装完插件,我们再安装依赖**

```bash
# 进入工程目录
cd myvuedemo3
# vue-router
npm install vue-router --save-dev

# element-ui
npm install element-ui -S

#  axios 
npm install --save axios vue-axios

# 安装SAAS加载器
npm install sass-loader node-sass --save-dev

```



**3、运行起来看效果，**

执行完插件安装后，进入到项目文件中，安装依赖包

```
cnpm install

npm run dev
```



**4、配置路由,在webpack里面使用vue-router**

step 1,创建路由表，在src目录下创建router文件夹，新建index.js，此时这个文件就是一个路由表，会被自动扫描到

```javascript
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router);

import Hello from '../components/Hello'

//创建 router 实例，然后传'routers'配置
const router = new Router({
  routes: [
    {path: "/hello", name: "hello", component: Hello}
  ]
});

//导出路由
export default router
```

step 2, 在main.js.配置我们的路由表，全局使用路由模块

```javascript
import Vue from 'vue'
import App from './App'
// 引入创建的路由表
import router from './router'

new Vue({
  el: '#app',
  router,
  // 让app.vue的内容展现在该div中
  render: h => h(App)
});
```

step 3、在src目录下，搞一个views文件夹，新建一个Hello.vue，在App.vue中创建路由视图

```javascript
##  App.vue
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

## Hello.vue
<template>
  <div>
    <h1>Hello Vue!</h1>
  </div>
</template>

<script>
  export default {
    name: "Hello"
  }
</script>

<style scoped>

</style>
```

5、配置elementUI,在webpack里面使用elementUI

**5、在项目中使用elementUI**

step 1、之前我们已经安装了elementUI, 接着在main.js里，全局引入elementUI插件

```html
import Vue from 'vue'
import App from './App'
import router from './router' //引入vue-router
import ElementUI from 'element-ui'; //引入elementUI
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);


new Vue({
  el: '#app',
  router,
  // 让app.vue的内容展现在该div中
  render: h => h(App)
});
```

step 2、接着我们到src/views/Hello.vue里面，尝试试用一下elementUI的效果

```bash
<template>
  <div>
    <h1>Hello Vue!</h1>
    <el-button @click="visible = true">Button</el-button>

    <el-dialog :visible.sync="visible" title="登录错误">
      <p>这是一个对话框，效果太牛逼了!</p>
    </el-dialog>

  </div>

</template>

<script>
  export default {
    name: "Hello",
    data() {
      return {
        visible: false
      }
    }
  }
</script>

<style scoped>

</style>
```

说明: 这里面有一个visible 控制着dialog是否显示，里面有一个 :visible.synv="visible" 就可以控制点击就弹出对话框

**6、项目中使用axios,在webpack里面使用axios的项目**

step 1、我们需要先安装axios，因为axios么有install,所以不能使用vue.use()方法，要使用就必须每个文件都引用一次，为了解决这个问题，有3种方法，1)我们可以结合vue-axios使用，2)axios改写为vue的原型属性，3)结合vue的action

```
import Vue from 'vue'
import App from './App'
import router from './router' //引入vue-router
import ElementUI from 'element-ui'; //引入elementUI
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
import axios from 'axios'//引入axios
import VueAxios from 'vue-axios'
Vue.use(VueAxios,axios);

new Vue({
  el: '#app',
  router,
  // 让app.vue的内容展现在该div中
  render: h => h(App)
});
```

step 2、操作之后，我们就可以使用了，在组件文件的methods里面去使用

```kotlin
getNewsList(){
      this.axios.get('api/getNewsList').then((response)=>{
        this.newsList=response.data.data;
      }).catch((response)=>{
        console.log(response);
      })
},
```

至此我们就搭建了一个比较完备的vue项目骨架，后续再需要什么插件，我们可以单独学习使用

### 6.2 用element创建登录页

step 1、在上面的项目骨架基础上，在src目录下新建一个views, 里面增加Login.vue

```html
<template>
    <div>
      这是一个登录页面
    </div>
</template>
```

step 2、安装路由模块，在项目骨架基础上，在src目录下新建一个 src/router的目录，专门放置我们的路由配置代码，在src/router目录下创建一个名为 index.js的路由配置文件，在main.js里面，把router引入

```javascript
import Vue from 'vue'
//引入理由创建
import Router from 'vue-router'
//安装路由
Vue.use(Router);


//配置路由
const router = new Router({
  routes: [
		 {path: "/login", name: "login", component: login}
  ]
});

//导出路由
export default router
```

实现字段的验证

在main.js里面引入router

```javascript
import Vue from 'vue'
import App from './App'
// 引入上面创建的路由配置目录
import router from './router' 

new Vue({
  el: '#app',
  //配置路由
  router,
  // 让app.vue的内容展现在该div中
  render: h => h(App)
});
```

在App.vue中,删掉HelloVue等初始项，修改后的App.vue

```
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

<style>
  body{
    font-family: "微软雅黑";  /*  设置字体 */
    margin: 0px auto /* 去除上下的边距*/
  }
</style>
```

step 3、在项目创建一个src/views文件件，新建一个Login.vue，使用elementUI完善一个登录页面

```
<template>
  <div class="login-container">
    <el-form class="login-form"
             :label-position="labelPosition"
             ref="ruleForm"
             label-width="80px"
             :model="ruleForm">
      <h2 class="login-title">管理系统登录</h2>

      <el-form-item label="用户名" prop="username">
        <el-input v-model="ruleForm.username" type="text" placeholder="请输入用户名"></el-input>
      </el-form-item>


      <el-form-item label="手机号" prop="phone">
        <el-input v-model="ruleForm.phone" type="text" placeholder="请输入手机号"></el-input>
      </el-form-item>

      <el-form-item label="密码" prop="password">
        <el-input v-model="ruleForm.password" type="password" placeholder="请输入登录密码"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="myLoginSubmit">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data() {
      return {
        labelPosition: 'right',
        ruleForm: {
          username: '',
          password: '',
          phone: ''
        }
      }
    },

    methods: {
      myLoginSubmit: function (formName) {
        
      }
    }
  }
</script>

<style scoped>
  .login-container {
    position: absolute;
    width: 100%;
    height: 100%;
    background: url("../../static/login.jpg") no-repeat;
    background-size: 100% 100%;
  }

  .login-form {
    width: 350px;
    margin: 160px auto; /* 上下间距160px，左右自动居中*/
    background-color: rgba(255, 255, 255, 0.8);; /* 透明背景色 */
    padding: 30px;
    border-radius: 10px; /* 圆角 */
    border-shadow: 0px 0px 20px #dcdfe6; /*阴影*/
  }

  /* 标题 */
  .login-title {
    color: #303133;
    text-align: center;
  }
</style>
```

说明：

详细参阅  博客 [vue实现登录页面](https://www.cnblogs.com/youth-man/p/14448292.html)

### 6.3 登录页表单验证

step 1、在表单的title增加 :rules="rules"，指定当前表单将适配的验证规则, 同时，我们还需要在字段上加入prop属性

```html
<el-form class="login-form"
         :label-position="labelPosition"
         :rules="rules"
         ref="ruleForm"
         label-width="80px"
         :model="ruleForm">
         
<el-form-item label="用户名" prop="username">
   <el-input v-model="ruleForm.username" type="text" placeholder="请输入用户名"></el-input>
</el-form-item>
```

step 2、在script中，将rules的规则写到data()中，这样当失去焦点就触发提示，对应的验证规则，可以百度下

```
data() {
  return {
    labelPosition: 'right',
    ruleForm: {
      username: '',
      password: '',
      phone: ''
    },
    rules: {
      username: [
        {required: true, message: '请输入用户名', trigger: 'blur'},
        {min: 5, max: 12, message: '长度在 5 到 12 个字符', trigger: 'blur'}
      ],
      password: [
        {required: true, message: '请输入密码', trigger: 'blur'},
        {min: 5, max: 12, message: '长度在 5 到 12 个字符', trigger: 'blur'}
      ],
      phone: [
        {required: true, message: '请输入手机号码', trigger: 'blur'},
        {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
      ]
    }
  }
},
```

step3、在点击方法中，调用验证规则，当用户点击按钮，需要把表单名传递过去，而表单名就是表单定义的ref字段值

在方法中，通过调用vue对象的实例属性 $refs就可以获取到form

```
myLoginSubmit: function (formName) {

  this.$refs[formName].validate((valid) => {
    // console.log(valid) 验证通过为true，有一个不通过就是false
    if (valid) {
      // 这里通过的逻辑
      this.axios({
        method: 'get',
        url: 'http://localhost:8090/a',
        headers: {
          'Content-type': 'application/x-www-form-urlencoded'
        },
        transformRequest: [function (data) {
          let ret = ''
          for (let it in data) {
            ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
          }
          return ret
        }]

      }).then(function (response) {
        console.log(response.data);
      }).catch(function (error) {
        console.log(error);
      });
      console.log("值验证通过!")
    } else {
      console.log('error submit!!');
      return false;
    }
  });
```

完整的页面代码如下：

```
<template>
  <div class="login-container">
    <el-form class="login-form"
             :label-position="labelPosition"
             :rules="rules"
             ref="ruleForm"
             label-width="80px"
             :model="ruleForm">
      <h2 class="login-title">管理系统登录</h2>

      <el-form-item label="用户名" prop="username">
        <el-input v-model="ruleForm.username" type="text" placeholder="请输入用户名"></el-input>
      </el-form-item>


      <el-form-item label="手机号" prop="phone">
        <el-input v-model="ruleForm.phone" type="text" placeholder="请输入手机号"></el-input>
      </el-form-item>

      <el-form-item label="密码" prop="password">
        <el-input v-model="ruleForm.password" type="password" placeholder="请输入登录密码"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="myLoginSubmit('ruleForm')">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data() {
      return {
        labelPosition: 'right',
        ruleForm: {
          username: '',
          password: '',
          phone: ''
        },
        rules: {
          username: [
            {required: true, message: '请输入用户名', trigger: 'blur'},
            {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '请输入密码', trigger: 'blur'},
            {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
          ],
          phone: [
            {required: true, message: '请输入手机号码', trigger: 'blur'},
            {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
          ]
        }
      }
    },

    methods: {
      myLoginSubmit: function (formName) {

        this.$refs[formName].validate((valid) => {
          // console.log(valid) 验证通过为true，有一个不通过就是false
          if (valid) {
            // 这里通过的逻辑
            this.axios({
              method: 'get',
              url: 'http://localhost:8090/a',
              headers: {
                'Content-type': 'application/x-www-form-urlencoded'
              },
              transformRequest: [function (data) {
                let ret = ''
                for (let it in data) {
                  ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
                }
                return ret
              }]

            }).then(function (response) {
              console.log(response.data);
            }).catch(function (error) {
              console.log(error);
            });
            console.log("值验证通过!")
          } else {
            console.log('error submit!!');
            return false;
          }
        });

      }
    }
  }
</script>

<style scoped>
  .login-container {
    position: absolute;
    width: 100%;
    height: 100%;
    background: url("../../static/login.jpg") no-repeat;
    background-size: 100% 100%;
  }

  .login-form {
    width: 350px;
    margin: 160px auto; /* 上下间距160px，左右自动居中*/
    background-color: rgba(255, 255, 255, 0.8);; /* 透明背景色 */
    padding: 30px;
    border-radius: 10px; /* 圆角 */
    border-shadow: 0px 0px 20px #dcdfe6; /*阴影*/
  }

  /* 标题 */
  .login-title {
    color: #303133;
    text-align: center;
  }
</style>
```



### 6.4 Login.vue发起一个登录请求给后台

step 1、首先，在点击方法中，我们把当前的vm定义一个

```javascript
var vm = this;
```

step 2、我们通过使用axios进行发起ajax请求，取值使用vm取值

```javascript
onSubmit: function (formName) {
  var vm = this;
  this.$refs[formName].validate((valid) => {
    // console.log(valid) 验证通过为true，有一个不通过就是false
    if (valid) {
      // 这里通过的逻辑
      this.axios({
        method: 'get',
        url: 'http://localhost:8090/login?username='+vm.ruleForm.username+'&password='+vm.ruleForm.password,
      }).then(function (response) {
        console.log(response.data);
      }).catch(function (error) {
        console.log(error);
      });

    } else {
      console.log('error submit!!');
      return false;
    }
  });
```

注意，我们在这里，之前，没有写 var vm=this；直接使用this.ruleForm.username是取不到值的，因为在axios里面，this获取的值是axios

对于使用post带参数，我们将在下一讲讲解

### 6.5 用elementUI创建首页

step 1、在src/views下面，新建Home.vue

step 2、进入index.js, 注册首页的路由信息



step3、我们在登录成功，让用户跳转进入首页,这里我们使用vm.$router.push("")

```
onSubmit: function (formName) {
        var vm = this;
        ...
        ...
      this.axios({....}).then(function (response) {
        console.log(response.data);
        if (response.data=="success"){
          vm.$router.push("/home");
        }
      }).catch(function (error) {
        console.log(error);
      });
}
```

这样，当用户登录成功，返回结果是success的时候，就会跳转到首页



### 6.6 Mock数据及axios请求

0、准备easymock，可以去easymock官网看看



提供假数据可以让前端不需要等待后端接口，而直接进行下一步的开发。一个常用的工具 easy mock

1、如果想页面渲染数据，需要在created() 钩子函数实现数据填充

```
<script>
  export default {

    name: "Home",

    data() {
      return {
        users: []
      }
    },

    created() {
    },

  }
</script>
```

2、发送axios时，我们需要指明vm对象

```
created() {
  /**
   * 因为axios的then内部,是不能通过this调用到vue实例的,then里面的this是当前的axios对象
   * 因此把当前vue对象用vm指明，这样this就不会冲突了
   * @type {default}
   */
  var vm =this;
  this.axios({
    method:'get',
    url:'http://localhost:8090/user/all',

  }).then(function (resp) {
    console.log(resp.data)
  })
},
```

3、再把数据绑定上去

```
<script>
  export default {

    name: "Home",

    data() {
      return {
        users: []
      }
    },

    created() {
      /**
       * 因为axios的then内部,是不能通过this调用到vue实例的,then里面的this是当前的axios对象
       * 因此把当前vue对象用vm指明，这样this就不会冲突了
       * @type {default}
       */
      var vm =this;
      this.axios({
        method:'get',
        url:'http://localhost:8090/user/all',

      }).then(function (resp) {
        console.log(resp.data);
        vm.$data.users = resp.data
      })
    },

}
```

4、在view视图里面把数据绑定上去

```
<el-container>
  <el-header style="text-align: right; font-size: 12px">
    <el-dropdown>
      <i class="el-icon-setting" style="margin-right: 15px"></i>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item>查看</el-dropdown-item>
        <el-dropdown-item>新增</el-dropdown-item>
        <el-dropdown-item>删除</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
    <span>王小虎</span>
  </el-header>

  <el-main>
    <el-table :data="users">
      <el-table-column prop="uid" label="用户id" width="140">
      </el-table-column>
      <el-table-column prop="username" label="用户名" width="120">
      </el-table-column>
      <el-table-column prop="nickname" label="昵称" width="120">
      </el-table-column>
      <el-table-column prop="age" label="年龄">
      </el-table-column>
    </el-table>
  </el-main>
</el-container>
```



## 7 嵌套路由（子路由）

### 7.1 什么是嵌套路由

嵌套路由也叫子路由，在实际应用中，通常由多层嵌套的组件组合而成，同样滴，URL中隔断动态路径也按照某种嵌套结构对应嵌套的各层组件，例如：

/user/foo/profile



简单说，就是在路由显示的组件内部，又嵌套新的路由，叫做子路由





### 7.2 创建嵌套视图组件

1)  用户信息组件





2） 用户列表组件



### 7.3 配置嵌套路由表

到index.js里面，把两个组件引入到路由表，然后就开始配置子路由，在哪个组件有子路由，就在哪个组件配置

children

```
const router = new Router({
  routes: [
    {path: "/home", name: "Home", component: Home,
      children:[
        {path:"/all", name:"all", component:UserList}
      ]

    },
    {path: "/login", name: "login", component: login}
  ]
```

注意，使用子路由时，就是 /all，不用加/home/all

### 7.4 修改首页视图使用嵌套路由

修改时候，需要增加点击链接<router-link>和显示的<router-view>

1、<router-link>里面的to可以直接路由路径  

```
<template>
  <div>
    <el-container>
      <el-header>Header</el-header>
      <el-main>
        <el-container>
          <router-link to="/user/all">用户列表</router-link>
        </el-container>
        <el-container>
          <router-view></router-view>
        </el-container>
      </el-main>
      <el-footer>Footer</el-footer>
    </el-container>
  </div>
</template>
```

2、方式2 ，还可以使用button，点击事件的方式 this.$router.push(/user/all)



## 8 组件重定向

重定向的意思大家都明白，但VUE的重定向是作用在路径不同但组件相同的情况下

### 8.1、配置重定向

1-1 修改路由的配置

```
{path: "/gohome", redirect: "/home"},
```

说明，这里定义两个路径，一个是/gohome，一个是/gohome,其中访问/gohome会重定向到/home



1-2 重定向到组件



### 8.2、带参数的重定向

2-1 修改路由的配置



2-2 重定向到组件



## 9 参数传递

### 9.1 使用路径匹配方式

step 1、在理由表里面修改路径配置

```
{path: '/user/profile/:id', name :"userProfile" , component: UserProfile}
```

说明 主要在path属性中增加了:id这样的占位符

step 2、传递参数

​	1) router-link

```
<router-link :to={name:"userProfile", params:{id:1}}>个人信息</router-link>
```

说明: 此时我们把to改成了:to ，是为了将这一属性当成对象使用，注意router-link中的name属性名一定要和路由中的属性名称匹配，因为这样vue才能找到对应的路由路径；

​	2) 代码方式

```
this.$router.push({name:"userProfile", params:{id:1}});
```



step 3、接受参数

在组件里面，使用 {{this.$route.param.id}} 接收参数

### 9.2 使用props的方式

step 1、 修改路径的配置

```
{path: '/user/profile/:id', name :"userProfile" , component: UserProfile, props:true}
```

说明: 主要增加了props:true 这个属性

step 2、传递参数

同上，没有太大变化



step 3、接收参数

为目标组件增加props属性，代码如下

```
<script>
    export default {
      props:['id'],
      name: "UserList"
    }
</script>
```

在模板中，就是使用

```
{{id}}
```

来接收参数





## 10 路有钩子和异步请求

### 10.1 路由中的勾子函数

- beforeRouteEnter  在进入路由前执行
- beforeRouteLeave  在离开路由前执行

案例代码如下：

```
<script>
    export default {
      props:['id'],
      name: "UserList",
      beforeRouteEnter:(to,from,next)=>{
        console.log("准备进入个人信息页");
        next();
      },
      beforeRouteLeave:(to,from,next)=>{
        console.log("准备离开个人信息页");
        next();
      },

    }
</script>
```

参数说明

to  路由将要跳转的路径信息

from 路由跳转前的路径信息

next 路由的控制函数

​	next() 跳入下一个页面

​	next('/path') 改变卤藕的跳转方向，使其跳到另一个路由

​	next(false) 返回原来的页面

​	next((vm)=>{})  仅在beforRouteEnter  中可用，vm是组件实例

### 10.2 在勾子函数中使用异步请求















