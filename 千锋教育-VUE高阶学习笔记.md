# Vue学习笔记-高阶学习

## 1. 使用vue-element-admin



### 1.1 vue-element-admin 介绍

1. 在vue-element-template的vue.config.js，定义了在项目中@是 src的别名

```javascript
configureWebpack: {
  // provide the app's title in webpack's name field, so that
  // it can be accessed in index.html to inject the correct title.
  name: name,
  resolve: {
    alias: {
      '@': resolve('src')
    }
  }
},
```

2. 更改侧边栏的名称，可以通过修改/src/router/index.js,  找到对应的路径，然后修改里面的meta元素

```javascript
{
  path: '/',
  component: Layout,
  redirect: '/dashboard',

  children: [{
    path: 'dashboard',
    name: 'Dashboard',
    component: () => import('@/views/dashboard/index'),
    meta: { title: '后台管理平台', icon: 'dashboard' }
  }]
},
```

3.vue-element-admin,左边侧边栏，当只有一个子节点，父节点会自动屏蔽，如果不想屏蔽，就需要使用alwaysShow: true,

```javascript
{
  path: '/example',
  component: Layout,
  redirect: '/example/table',
  name: 'Example',
  alwaysShow: true,
  meta: { title: '商品管理', icon: 'el-icon-s-help' },
  children: [
    {
      path: 'table',
      name: 'Table',
      component: () => import('@/views/table/index'),
      meta: { title: '商品列表', icon: 'table' }
    },
  ]
},
```

4、因为vue-element-template每个router都会显示在侧边栏，如果不希望展示在侧边栏，就需要使用hidden: true



### 1.2 项目架构





### 1.3 进入商品列表

> 问题1 、如何修改控制台名称？ 在vue-element-admin 的router 路径，找到里面的index.js，修改里面的meta 元素，可以修改名称

```javascript
{
  path: '/',
  component: Layout,
  redirect: '/dashboard',
  children: [{
    path: 'dashboard',
    name: 'Dashboard',
    component: () => import('@/views/dashboard/index'),
    meta: { title: '商品管理后台', icon: 'dashboard' }
  }]
},
```

title:  修改标签名字，  

icon  修改图标的名字



> 问题2. 实现自定义一个tables

可以根据router/index.js，在里面找到对应的vue组件，component: () => import('@/views/table/index'), 

step1 ，我们可以进入到views/table/index里面，新建一个/product/index，在里面修改

step 2, 因为vue-element-admin, 只有一个子元素，会默认不显示菜单按钮，为了继续显示菜单，我们可以在父元素增加alwaysShow: true,

step 3、在项目安装axios 

```bash
npm install --save axios vue-axios

import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)
```

step 4、发起this.axios请求

```javascript
export default {

  data() {
    return {
      product: null
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      // eslint-disable-next-line no-unused-vars
      const vm = this
      this.axios({
        method: 'get',
        url: 'http://localhost:8080/prop/all'
      }).then(function(response) {
        console.log(response);
        vm.product = response.data
      })
    }
  }
}
</script>
```

1、在this.axios() 无法调用到vue实例，我们采用const vm = this，规避这个拿不到的问题



### 1.4 进入编辑商品页

step 1、先搞好前段的页面，有一个商品的编辑页面，在这个例子，我们使用form.vue组件作为商品的编辑页面

```vue
<template>
  <div class="app-container">
    {{this.$route.params.id}}
    <el-form ref="form" :model="form" label-width="120px">
      <el-form-item label="Activity name">
        <el-input v-model="form.name" />
      </el-form-item>

      <el-form-item label="Activity zone">
        <el-select v-model="form.region" placeholder="please select your zone">
          <el-option label="Zone one" value="shanghai" />
          <el-option label="Zone two" value="beijing" />
        </el-select>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="onSubmit">修改</el-button>
        <el-button @click="onCancel">取消修改</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: null
    }
  },
  methods: {
    onSubmit() {
      this.$message('submit!')
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped>
.line{
  text-align: center;
}
</style>
```

step 2、编辑完页面，配置路由，路由需要考虑用户点击后，将传递当前点击的id作为商品的id



```javascript
{
  path: '/product',
  component: Layout,
  redirect: '/product/list',
  name: 'Product',
  alwaysShow: true,
  meta: { title: '商品管理', icon: 'el-icon-s-help' },
  children: [
    {
      path: 'list',
      name: 'list',
      component: () => import('@/views/product/index'),
      meta: { title: '商品列表', icon: 'table' }
    },
    {
      path: 'edit/:id',
      name: 'edit',
      hidden: true,
      component: () => import('@/views/product/edit'),
      meta: { title: '编辑商品', icon: 'form' }
    }
  ]
},
```

 path: 'edit/:id',  路由传参

step 3、配置完路由后，我们需要进入到/product/list.vue中，用户点击编辑按钮，跳转到编辑页面，同时带入商品的id



```javascript
<el-button type="success" size="small" icon="el-icon-edit" @click="goEditPage(scope.row.id)">编辑</el-button>


  methods: {
    goEditPage(id){
      console.log("跳入编辑页面..."+id)
      this.$router.push("/product/edit/"+id);
    },
  }
```

this.$router.push("/example/edit/"+id);



step 4、 编写后端接口，我们根据用户传入的id来进行查询

```java
@RequestMapping("/{id}")
@ResponseBody
public Product selectByPrimaryKey(@PathVariable Long id){
    System.out.println("==>"+ id);
    return productService.selectByPrimaryKey(id);
}
```

在这里，我们使用路径传参，所以在方法参数里面，加上了@PathVariable注解



这样，当用户点击了编辑按钮，就可以带着商品的id，跳入到商品编辑页面，在商品编辑页面，在页面展现之前，先向后台获取这个id的商品数据，然后回填到对应数据上。

说明：

1、一个json在线编辑器: https://www.bejson.com/jsoneditoronline

2、实现一个编辑商品的需求，一般步骤是

​	- 在商品列表提供编辑按钮

​	- 创建一个商品编辑页(edit.vue)

​	- 在路由表中注册商品编辑页，在路由表，需要有传参能力 :id

​	 - 通过商品列表的编辑按钮跳转到商品编辑页	

​     - 路由时传递商品id的参数，通过之前的套路即可解决

在商品列表页的点击按钮传递参数，参数采用scope.id形式 editpro(scope.row.id)

```javascript
    goEditPage(id){
      this.$router.push("/product/edit/"+id);
    },
```



在路由表修改路由，使用:id 传递参数 ,

```javascript
  {
    path: '/product',
    component: Layout,
    redirect: '/product/list',
    name: 'Product',
    alwaysShow: true,
    meta: { title: '商品管理', icon: 'el-icon-s-help' },
    children: [
      {
        path: 'list',
        name: 'list',
        component: () => import('@/views/product/list'),
        meta: { title: '商品列表', icon: 'table' }
      },
      {
        path: 'edit/:id',
        name: 'edit',
        hidden: true,
        component: () => import('@/views/product/edit'),
        meta: { title: '编辑商品', icon: 'form' }
      }
    ]
  },
```

路由时传递商品参数，通过惯有的套路，在编辑商品页面，把id拿到

```javascript
this.$route.params.id
```



```javascript
created(){
    this.fetchDataById();
}
methods:{
    fetchDataById(){
      var vm = this;
      var id = this.$route.params.id
      this.axios({
        method: 'get',
        url: 'http://localhost:8080/product/'+id
      }).then(function(response){
        console.log(response.data);
        vm.product = response.data;
      })
    },
}
```



### 1.5 编辑商品提交

在商品编辑页面， 当用户点击了提交按钮，应该想后台提交当前用户的数据，后台接收接收到用户提交的数据，应该保存，并返回一个保存成功的标记，前台根据后台返回的结果，跳转到商品列表页面

注意:

1、对于post的数据，需要使用不同的格式，使用axios直接提交是无法接收的，因为axios默认提交的请求头内容为

```
Content-Type:application/json; charset=UTF-8
```

而后端没有使用@ResponseBody时候，需要的是

```
Content-Type:application/x-www-form-urlencoded
```

所以需要转换一下

```javascript
import Qs from 'qs'

this.axios({
    method:'post',
    url:'http://localhost:8080/regist',
    headers:{
        'Content-type': 'application/x-www-form-urlencoded'
    },
    transformRequest:[ function(data){
        return QS.stringify(data);
    }],
    data:{
        username:this.username,
        password:this.password
    }
}).then( function(response){
    alert(response.data)
})
```



2、对于前段传递Json，我们后端可以使用RequestBody来接收json数据并封装到对应对象

```javascript
public String login2(@RequestBody LoginUserDTO user)
```



对于完成商品的编辑页面，我们接下里的步骤是

step 1、 我们调整后台的接口，是的后台接收的是 RequestBody

```java
@RequestMapping(value = "/add", method = RequestMethod.POST)
@ResponseBody
public String addProduct(@RequestBody Product product){
    System.out.println("==>"+ product.toString());
    int i = productService.insert(product);
    if (i>0){
        return "success";
    }
    return "error";
}
```

注意:

- JQuery Ajax 以 application/x-www-form-urlencoded 上传 JSON对象, 后端用 @RequestParam 或者Servlet 获取参数
- JQuery Ajax 以 application/json上传 JSON字符串,后端用 @RquestBody 获取参数

>JSON.parse() 
>用于将一个 JSON 字符串转换为 JavaScript 对象。 
>JSON.stringify() 
>用于将 JavaScript 值转换为 JSON 字符串。





### 1.6 编辑商品的验证

step 1、 需要在表格中，增加 ref，也就是表格的唯一识别符，类似于id的作用， 添加:rules，绑定一个验证规则，在对应的验证字段上增加prop，

```
//1 在表头上
<el-form :model="product":rules="productRules"label-width="120px" ref="productForm">

//2 在字段上
 <el-form-item label="商品名称" prop="name">
```

step 2、在data()中绑定验证的规则，要放到return里面

```javascript
data() {
  return {
    product:{

    },
    productRules: {
      /*使用内置的*/
      name: [
        { required: true, message:"此空不能为空", trigger: 'blur'},
        { min: 2, max: 200, message: "长度在 2 到 200 个字符", trigger: "blur"}
        ],
      salePrice: [
        { required: true,message:"此空不能为空", trigger: 'blur'},
        { min: 2, max: 30, message: "长度在 2 到 30 个字符", trigger: "blur"}
        ],
      salePoint: [
        { required: true,message:"此空不能为空", trigger: 'blur'},
        { min: 2, max: 200, message: "长度在 2 到 200 个字符", trigger: "blur"}
      ]
    },
  }
},
```

step 3、需要调用验证规则，调用的形式为  this.$refs.表名

```javascript
onSubmit() {
  this.$refs.productForm.validate(valid => {
    if (valid) {

    } else {
      console.log('error submit!!')
      return false
    }
  });

  this.$message('submit!');
},
```

step 4、 在调用的按钮中，需要传递过来表名

```html
<el-form-item>
  <el-button type="primary" @click="onSubmit('productForm')">修改</el-button>
  <el-button @click="onCancel">取消修改</el-button>
</el-form-item>
```



### 1.7 实现删除商品



delepro(id){

```javascript
    delData(id){
      const vm = this;
      this.axios({
        method: 'delete',
        url:'http://localhost:8080/product/'+id,
      }).then(function(response){
        vm.fetchData();
        vm.$router.push('/product/list');
        vm.$message({
          message:'删除成功!',
          type:'success'
        });
      }).catch(function(response){
        vm.$router.push('/product/list');
        vm.$message({
          message:'删除失败!',
          type:'error'
        });
      })
    }
```

后台的接口

```java
@RequestMapping(value = "/{id}", method = RequestMethod.DELETE)
@ResponseBody
public String delProduct(@PathVariable Long id) {
    System.out.println("==>delProduct()" + id);
    int i = productService.deleteByPrimaryKey(id);
    if (i > 0) {
        return "success";
    }
    return "error";
}
```



说明:

1)使用vue，删除后，页面不会刷新，需要手动调用更新数据，才会刷新

2)后台采用restful风格





### 1.7 添加一个商品



创建一个页面的步骤

1、配置页面

2、配置路由

step 1. 首先，我们要在列表的页面，添加一个点击后会跳转到添加商品页面的按钮

```html
<template>
  <div class="app-container">
	<el-row style="display: flex; margin-bottom: 20px; padding-right:10px; height: 45px;float: right">
      <el-button type="success">添加新商品</el-button>
    </el-row>
    ...
    ...
    </div>
</template>
```



step 2、 配置页面，在 /src/product/目录下，我们拷贝edit.vue，命名为add.vue，并修改里面的相关输入项

```html
<template>
  <div class="app-container">
    <el-form :model="product"
             :rules="productRules"
             label-width="120px"
             ref="productForm">
      <el-form-item label="商品名称" prop="name">
        <el-input v-model="product.name" placeholder="请输入商品名称" />
      </el-form-item>

      <el-form-item label="商品售价" prop="salePrice">
        <el-input v-model="product.salePrice" placeholder="请输入商品价格"/>
      </el-form-item>
      <el-form-item label="商品卖点" prop="salePoint">
        <el-input  v-model="product.salePoint" placeholder="请输入商品的卖点" />
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="onSubmit('productForm')">提交</el-button>
        <el-button @click="onCancel">取消添加</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
```

step 3、进入到/router/index.js里面，配置新增页面的路由, 

```javascript
/*商品管理相关页面*/
{
  path: '/product',
  component: Layout,
  redirect: '/product/list',
  name: 'Product',
  alwaysShow: true,
  meta: { title: '商品管理', icon: 'el-icon-s-help' },
  children: [
    {
      path: 'list',
      name: 'list',
      component: () => import('@/views/product/list'),
      meta: { title: '商品列表', icon: 'table' }
    },
    {
      path: 'edit/:id',
      name: 'edit',
      hidden: true,
      component: () => import('@/views/product/edit'),
      meta: { title: '编辑商品', icon: 'form' }
    },
    {
      path: 'add/',
      name: 'add',
      hidden: true,
      component: () => import('@/views/product/add'),
      meta: { title: '添加商品', icon: 'form' }
    }
  ]
},
```

step 4、 将商品列表页面的点击按钮关联到跳转新增页面上

```javascript
// 进入新加商品
goAddPage(){
  this.$router.push('/product/add')
}
```

做完上面这些，基本页面是可以跳转进入了，下面就是要配置数据的提交和后台接口了



step 5、完善后台的接口，在ProductController类里面，增加一个添加商品的接口

```java
     @PostMapping(value = "/add")
    @ResponseBody
    public String addProduct(Product product) {
        System.out.println("==>addProduct()...");
        if (!StringUtils.isEmpty(product)) {
            product.setUpdateUser(1L);
            product.setCreateUser(1L);
            product.setCreateTime(new Date());
            product.setUpdateTime(new Date());
            product.setFlag(true);
            product.setTypeId(1L);
            product.setTypeName("phone");
            product.setImages("http://localhost:8080/images/1.jpg");

            int i = productService.insert(product);
            if (i > 0) {
                return "success";
            }
        }
        return "error";
    }
```

step 6、 进入前台的页面，在add.vue页面添加的函数

```javascript
<script>
import Qs from 'qs'
export default {
  data() {
    return {
      product:{

      },
      productRules: {
        /*使用内置的*/
        name: [
          { required: true, message:"此空不能为空", trigger: 'blur'},
          { min: 2, max: 200, message: "长度在 2 到 200 个字符", trigger: "blur"}
          ],
        salePrice: [
          { required: true,message:"此空不能为空", trigger: 'blur'}
          ],
        salePoint: [
          { required: true,message:"此空不能为空", trigger: 'blur'},
          { min: 2, max: 200, message: "长度在 2 到 200 个字符", trigger: "blur"}
        ]
      },
    }
  },
  created() {
  },
  methods: {
    //提交添加请求
    onSubmit(){
      this.$refs['productForm'].validate(valid => {
        var vm = this;
        if (valid) {
          //axios提交数据
          this.axios({
            method:'post',
            url:'http://localhost:8090/product/add',
            //=====解决post请求无法携带数据问题
            transformRequest:[ function(data){
              return Qs.stringify(data)
            }],
            data: vm.product
          }).then(function(response) {
            vm.$router.push('/product/list');
            vm.$message({ message:'一条新商品已添加成功!', type:'success' });
          }).catch( function(error){
            vm.$message({
              message:'添加失败!',
              type:'error'
            })
          });
        } else {
          vm.$message({
            message:'添加失败!',
            type:'error'
          })
          return false
        }
      });
    },
    onCancel() {
      this.$router.push('/product/list');
      this.$message({
        message: '取消修改!',
        type: 'warning'
      })
    }
  }
}
</script>
```



如果数据库所有字段都是必须填写，此时我们可以在插入时候，数据全部生成

```java
@RequestMapping(value='addpro', method=RequestMethod.Post)
@ResponseBody
public String addProduct(Tproduct prod){

   prod.setCreateTime(new Data());

   prod.setTypeId(1L);

   .....

   //把属性都给设置上去

     tProductMapper.insert(prod);
​    return "success";
}
```

这样我们就添加成功了一个商品了



## 2 pagination分页



分页原理，任何分页系统都要需要有5个部分

1、记录总条数

2、每页显示的记录条数

3、总页数

4、当前是第几页

5、当前页的所有记录



官网介绍地址 [Pagination 分页](https://panjiachen.github.io/vue-element-admin-site/zh/feature/component/pagination.html#%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F)



pagination实际上是一个组件，组件里面设置了分页常用到的参数，通过父组件使用pagination组件，向该组件传递参数，让pagination组件得到分页常用到的参数值，这就能够实现分页。这样的组件之间的参数传递(父传子) 是之前介绍过的知识点。



**分页功能的分类:**

真分页 ：

每一页都从后端发起请求，每次都需要建立后端连接请求数据，

优缺点：

适合数据量大的环境，但是每次都需要访问后台服务器，资源压力较大



假分页：

一开始从后端获取所有的数据，前端通过组件的方式对数据进行分页，在点击分页按钮式，数据已经在浏览器的缓存中了，不需要再请求后端接口，

优缺点： 

快速，但是后端数据量大不适合

### 2.1 pageHelper介绍及使用



如果我们想实现分页，我们得先实现后端的接口，，建议尝试PageHelper分页插件，这一定是最方便使用的分页插件。分页插件支持任何复杂的单表、多表分页



**安装方式：**

step 1 首先在F:\code\Vue\SSMBackProject这个ssm项目中，添加PageHelper的相关组件

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>最新版本</version>
</dependency>
```

step 2、在项目SSMBackProject的spring-dao.xml配置文件中，加入PageHelper的配置内容

```xml
<!-- 3 获取 SqlSessionFactory 工厂类-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!--绑定mybatis配置文件-->
    <property name="configLocation" value="classpath:mybaits-config.xml"/>
    <!--配置PageHelper-->
    <property name="plugins">
        <array>
            <bean class="com.github.pagehelper.PageInterceptor">
                <property name="properties">
                    <!--使用下面的方式配置参数，一行配置一个 -->
                        <value>
                            reasonable = true <!--分页合理化参数，默认值为false。当该参数设置为 true 时，pageNum<=0 时会查询第一页， pageNum>pages（超过总数时），会查询最后一页。默认false 时，直接根据参数进行查询-->
                            helperDialect = mysql <!--分页插件会使用的数据库链接-->
                        </value>
                </property>
            </bean>
        </array>
    </property>
</bean>
```



step 3、 编写后端请求分页接口

```java
/**
 *获得分页对象，里面封装了分页需要用到的所有信息
 * @param pageNum  指定当前是第几页
 * @param pageSize  指定每页显示多少条记录
 * @return  获得当前分页的对象
 */
@RequestMapping(value = "/pageInfo", method = RequestMethod.GET)
@ResponseBody
public PageInfo<Product> getPageInfo(int pageNum, int pageSize){
    logger.info("==> getPageInfo() coming in。。。");
    //1 .通过调用PageHelper的静态方法开始获取分页数据
    PageHelper.startPage(pageNum,pageSize);
    // 2. 获得所有的商品记录
    List<Product> lists = productService.selectAllProduct();
    // 3. 获得当前分页的对象
    PageInfo<Product> pageInfo = new PageInfo<Product>(lists);
    // 4. 返回我们的pageInfo
    return pageInfo;
}
```



做完上述的我们就可以去进行测试了，例如我们可以使用postman访问 http://localhost:8080/product/pageInfo?pageNum=1&pageSize=2 就可以返回到对应的数据

```json
{
  "total": 8,  //数据库中总的记录条数
  "list": [
    {
      "id": 1,
      "name": "Apple/苹果iPhone11(A2223)国行全新正品手机 绿色 全网通128GB",
      "images": "https://item.jd.com/100008348542.html",
      "price": 5299,
      "salePrice": 5099,
      "salePoint": "白条12期免息可选。赠【20W快充头】+无线充+壳膜套装+质保3年+运费险。关注店铺和商品额外加送15选1。",
      "typeId": 1,
      "typeName": "phone",
      "flag": true,
      "createTime": 1614408134000,
      "updateTime": 1614408137000,
      "createUser": 1,
      "updateUser": 1
    },
    {
      "id": 2,
      "name": "Redmi 9A 5000mAh??? ????????? 1300?AI?? ????? ???? 4GB+64GB ??? ?????? ?? ??",
      "images": "https://item.jd.com/100014348492.html",
      "price": 599,
      "salePrice": 589,
      "salePoint": "Redmi 9A 5000mAh??? ????????? 1300?AI?? ????? ???? 4GB+64GB ??? ?????? ?? ??",
      "typeId": 1,
      "typeName": "phone",
      "flag": true,
      "createTime": 1614408210000,
      "updateTime": 1614408213000,
      "createUser": 1,
      "updateUser": 1
    }
  ],
  "pageNum": 1, //当前第一页
  "pageSize": 2,  //每页获取的数据
  "size": 2,   //请求回来数据大小
  "startRow": 1,	//起始行第一行
  "endRow": 2,   //结束行 第二行
  "pages": 4,     // 一共有多少页
  "prePage": 0,   // 前一页是
  "nextPage": 2,  //下一页是
  "isFirstPage": true,  //是否是第一页
  "isLastPage": false,   //是否是最后一页 ?
  "hasPreviousPage": false,   //有前一页?
  "hasNextPage": true,   // 有后一页？
  "navigatePages": 8,  //
  "navigatepageNums": [
    1,
    2,
    3,
    4
  ],
  "navigateFirstPage": 1,
  "navigateLastPage": 4
}
```



### 2.2 一个典型SSM的配置





### 2.3 前端联调



我们先从综合项目，找到我们想要的东西，我们通过浏览，发现综合项目里面的一个表单有分页，

通过观察路由，我们找到这个table， 然后在tables里面，找到了分页，

进入table，发现他是通过本地注册引入一个子组件Pagination.vue的方式实现了分页功能，所以我们照着他的做法，一点点研究

我们从element-admin里面拷贝Pagination

step 1、在 /product/list.vue里面导入Pagination

1）components/Pagination

2) utils/scrollTo



step 2、按照使用规范,在list.vue使用Pagination

```html
<template>
  <pagination
    :total="total"
    :page.sync="listQuery.page"
    :limit.sync="listQuery.limit"
    @pagination="getList" />
</template>

<script>
import Pagination from '@/components/Pagination'

export default {
  components: { Pagination },
  data() {
    return {
      total: 0,
      listQuery: {
        page: 1,
        limit: 20
      }
    }
  },
  methods: {
    getList() {
      // 获取数据
    }
  }
}
</script>
```

|    参数     | 说明                                |   类型    |     默认值      |
| :---------: | :---------------------------------- | :-------: | :-------------: |
|    total    | 总条目数                            |  Number   |        /        |
|    page     | 当前页数  支持 .sync 修饰符         |  Number   |        1        |
|    limit    | 每页显示条目个数，支持 .sync 修饰符 |  Number   |       20        |
| page-sizes  | 每页显示个数选择器的选项设置        | Number [] | 10, 20, 30, 50] |
|   hidden    | 是否隐藏                            |  Boolean  |      false      |
| auto-scroll | 分页之后是否自动滚动到顶部          |  Boolean  |      true       |

| 事件名称   | 说明                                | 回调参数     |
| ---------- | ----------------------------------- | ------------ |
| pagination | 当 limit 或者 page 发生改变时会触发 | {page,limit} |



total  记录的总条数

listQuery>page 当前是第几页

listQuery.limit  每页显示的数量

getlist()  当前页的所有记录(数据)



总页数？ 没有要求提供



step 3. 我们进入到/src/product/list.vue里面，开始整合Pagination 

我们先插入 Pagination组件

```html
<template>  
    .....
<!--分页子组件,传递参数-->
    <pagination v-show="total>0"
                :total="total"
                :page.sync="listQuery.page"
                :limit.sync="listQuery.limit"
                @pagination="getList" />
    </template> 
```

然后，我们在script里面注册pagination，并声明和组装参数

```javascript
<script>
import Pagination from '@/components/Pagination/index'
export default {
  components: { Pagination },
  data() {
    return {
      product: null,
      listLoading:true,
      total: 0,  // 记录总条目数
      listQuery: {
        page: 1,  //当前页数  支持 .sync 修饰符
        limit: 10, // 每页显示条目个数，支持 .sync 修饰符
      },
    }
  },
</script>
```

为什么这样用？是因为我们通过vue-element-admin的综合平台里面，看到F:\code\Vue\vue-element-admin\src\views\table\complex-table.vue 这么实现，我们照着他的做



接着，我们声明getlist()函数，这个函数用于获取后端提供的PageInfo对象。在PageInfo里面封装了我们需要的分页数据。

```javascript
created() {
  this.getList()
},
methods: {
  //获取当前页的所有数据, 得到一个PageInfo对象
  getList(){
    var vm = this;
    this.axios({
      method:'GET',
      url:'http://localhost:8080/product/pageInfo?pageNum='+vm.listQuery.page+'&pageSize='+vm.listQuery.limit
    }).then(function(response){
      //得到一个PageInfo对象
      vm.total = response.data.total; //pageInfo中的total给vm的total
      vm.product = response.data.list
      // Just to simulate the time of the request
      setTimeout(() => {
        vm.listLoading = false
      }, 1 * 1000)
    })
  },
  }
```

注意

1、这时候我们在created就是使用 getList，不是fetchData了

2、另外，通过读源码，还需要scroll-to.js

3、分页显示中文，需要进入到main.js，把Vue.use(ElementUI, { locale }) 变更为Vue.use(ElementUI)，这样分页就显示中文了



### 2.4 一些小细节



细节1 ： pagination如何实现点击分页就像后台发送请求

认真研究下Pagination的源码，里面涉及 到 子传父 传值





## 3. 微信支付

### 3.1 尝试生成二维码

调试商家账户：

1、注册成为微信支付的商户，目前使用千峰提供的用于开发测试用的商户信息 [微信支付开发文档](https://pay.weixin.qq.com/wiki/doc/apiv3/open/pay/chapter2_7_0.shtml)

[微信支付开发文档-v2版本](https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=11_1)



2、下载微信支付的SDK和Demo [微信支付的sdk托管地址](https://github.com/wechatpay-apiv3/wechatpay-apache-httpclient)

[微信支付SDK下载地址-V2版本](https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=11_1)

3、使用IDEA打开拥有微信支付sdk的项目



​	就是把微信支付的sdk复制到自己的项目里面



4、在项目里面，创建一个配置类MyWXPayConfig，继承WXpayConfig类，在该类中，填写商家信息

```java
import java.io.InputStream;

/** 微信支付的配置类，包含收款商家的账户ID等信息
 * 注意：
 * 下面这些账户和ID是需要公司注册的
 * Created by Josh luo on 2021/3/4
 */
public class MyWxPayConfig extends WXPayConfig {
    /**
     * 商家注册微信支付后会生成一个应用ID
     * @return
     */
    @Override
    String getAppID() {
        return "wx632c8f211f8122c6";
    }

    /**
     * 商户ID
     * @return
     */
    @Override
    String getMchID() {
        return "1497984412";
    }

    /**
     * 固定的key
     * @return
     */
    @Override
    String getKey() {
        return "sbNCm1JnevqI36LrEaxFwcaT0hkGxFnC";
    }

    /**
     * 证书，可以为null
     * @return
     */
    @Override
    InputStream getCertStream() {
        return null;
    }

    @Override
    public int getHttpConnectTimeoutMs() {
        return super.getHttpConnectTimeoutMs();
    }

    @Override
    public int getHttpReadTimeoutMs() {
        return super.getHttpReadTimeoutMs();
    }

    /**
     * 获得一个微信域信息
     * @return
     */
    @Override
    IWXPayDomain getWXPayDomain() {
        MyWXPayDomain wxDomain = new MyWXPayDomain();
        return wxDomain;
    }

    @Override
    public boolean shouldAutoReport() {
        return super.shouldAutoReport();
    }

    @Override
    public int getReportWorkerNum() {
        return super.getReportWorkerNum();
    }

    @Override
    public int getReportQueueMaxSize() {
        return super.getReportQueueMaxSize();
    }

    @Override
    public int getReportBatchSize() {
        return super.getReportBatchSize();
    }
}
```

5、在项目中，创建一个类MyWXPayDomain,实现IWXPayDomain接口,配置支付的域名

```java
package com.github.wxpay.sdk;

/**
 * Created by Josh luo on 2021/3/4
 */
public class MyWXPayDomain  implements IWXPayDomain{
    public void report(String domain, long elapsedTimeMillis, Exception ex) {

    }

    /**
     * 返回微信的域名
     * @param config 配置
     * @return
     */
    public DomainInfo getDomain(WXPayConfig config) {
        /**
         * 参数1  微信支付域名
         * 参数2  是否主域名
         */
        DomainInfo domainInfo = new DomainInfo("api.mch.weixin.qq.com", true);
        return domainInfo;
    }
}
```

6、下单

```
a 在自己的系统中下单
b 在微信系统中下单
c 生成二维码

```

7、 编写测试类

```java
public class WXPayController {
    public static void main(String[] args) throws Exception {
        MyWxPayConfig config = new MyWxPayConfig();
        WXPay wxpay = new WXPay(config);

        Map<String, String> data = new HashMap<String, String>();
        data.put("body", "腾讯充值中心-QQ会员充值"); //要支付的产品标题
        data.put("out_trade_no", "2021030413465900000012");  //订单编号 年月日时间+随机码+订单编号
        data.put("device_info", ""); //设备信息
        data.put("fee_type", "CNY");  // 货币类型
        data.put("total_fee", "1"); // 货币最小单位
        data.put("spbill_create_ip", "123.12.12.123");
        // 最重要的点，需要有一个回调的接口，这个接口可以获取此次微信支付的信息
        data.put("notify_url", "http://www.example.com/wxpay/notify"); // 回调地址，当微信支付完会调用这个地址
        data.put("trade_type", "NATIVE");  // 此处指定为扫码支付
        data.put("product_id", "12"); //商品的id

        try {
            // 返回值: 下单成功之后的支付地址
            Map<String, String> resp = wxpay.unifiedOrder(data);
            System.out.println(resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
}
```

按照上述步骤，我们就可以生成一个支付二维码的地址，weixin://wxpay/bizpayurl?pr=bkUxnK7zz，用这个地址就能进行支付1分钱了。

微信服务器返回的内容：

{nonce_str=ztQIQJAaQOhaPiCh, code_url=weixin://wxpay/bizpayurl?pr=bkUxnK7zz, appid=wx632c8f211f8122c6, sign=88968EEBE4933C5E03FDB5D757E0C885EDCF814E44F2825A494B9CD4BB88D522, trade_type=NATIVE, return_msg=OK, result_code=SUCCESS, mch_id=1497984412, return_code=SUCCESS, prepay_id=wx0414034151831142a92ca9f7066c5d0000}



### 3.2 微信支付的步骤



1.注册商户Id

2 下载sdk

3 在sdk中进行开发  

   1)创建一个配置类MyWXPayConfig extends  WXPayConfig

​	2) 创建一个类实现 IWXPayDoamin ,指明一个域名

​	3） 编写场景类，实现下单



微信支付，用户从下单到微信支付的时序图关系

![image-20210304150626172](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210304150626172.png)

，刚才我们写的main就是一个发起下单请求的时序功能

接下来我们就需要设计一个回调接口，提供给微信后台进行查询通知

### 3.3 准备一个SSM的工作环境

step 1、 新建一个maven项目，选择maven-arche-webapp骨架，在src/main下新建 java, resources，刷新一下maven，即可被识别结构，然后更新web.xml为4.0

step 2、找一个ssm项目，拷贝web.xml, resources下的db.properties, log4j.properties, spring-dao.xml,spring-mvc.xml,spring-service.xml , mybatis-config.xml以及applicationContext.xml

step 3、我们把各个项目都配置完，就在controller建立一个测试接口，在WEN-INF/views/下面建立一个hello.jsp用来测试

```java
@Controller
public class UserController {
    @RequestMapping("/sayhi")
    public String sayHi(){
        return "hello, boy!";
    }
}
```

如果正确进入到这个页面，那么说明没有任何问题。





### 3.4 编写回调接口

首先，我们编写一个回调接口  http://localhost:8080/wxpay/notify_url

```java
@Controller
@RequestMapping("wxpay")
public class WXPayController {
    @RequestMapping("notify_url")
    @ResponseBody
    public String getNotifyURL(){
        return "success,微信支付接口可正确回调!";
    }
}
```

当前这个接口，微信支付是没法回调的 ，因为是内网接口，微信支付无法访问到的，为了能够被外网访问，我们就需要开启一个内网穿透



### 3.5 内网穿透

因为微信支付是外网，所以需要内网穿透才能实现正确的访问到，内网穿透是将本地服务器暴露在公网上，供公网上的用户访问

我们需要安装一个内网穿透工具，这样外网就可以访问到了，推荐使用netapp.cn https://natapp.cn/



1、注册登录后，购买免费隧道，此时本地端口一定是tomcat开启的那个端口地址，也就是说如果本机器tomcat服务器是8080端口，这里也要写8080

![image-20210304210556123](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210304210556123.png)





2、进入到我的隧道，复制token  c29995544153df19 

![image-20210304210720698](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210304210720698.png)

3、下载客户端，解压到对应文件夹 F:\Environment\natapp_windows_amd64_2_3_9，在这个文件夹执行cmd，命令

`natapp -authtoken=c29995544153df19` 启动natapp，启动后效果

![image-20210304211318433](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210304211318433.png)



4、这个时候，我们就可以通过 http://bhyjb6.natappfree.cc/wxpay/notify_url 访问到本机的 http://localhost:8080/wxpay/notify_url，此时就内网穿透成功了，这个时候就可以被微信支付回调了

我们把自己的主程序优化一下，把接口请求的内容接收打印一下，来看一下到底微信请求时候，给我们发过来什么内容

```java
@Controller
@RequestMapping("wxpay")
public class WXPayController {
    @RequestMapping("/notify")
    public void getNotifyURL(HttpServletRequest request, HttpServletResponse response) throws IOException {
        //获取微信支付的请求内容,从请求消息中获得数据
        ServletInputStream is = request.getInputStream();
        byte [] b = new byte[1024];
        int len=0;
        String str=null;
        while ((len=is.read(b))!=-1){
            str = new String (b,0,len);
        }
        //处理微信支付返回来的结果
        System.out.println(">>>+"+str);

        // 返回给微信一个标准格式的回信
        response.getWriter().write("<xml><return_code><![CDATA[SUCCESS]]></return_code><return_msg><![CDATA[OK]]></return_msg></xml>");
    }
}
```

微信给我们返回的内容

```xml
+<xml><appid><![CDATA[wx632c8f211f8122c6]]></appid>
<bank_type><![CDATA[OTHERS]]></bank_type>
<cash_fee><![CDATA[1]]></cash_fee>
<fee_type><![CDATA[CNY]]></fee_type>
<is_subscribe><![CDATA[N]]></is_subscribe>
<mch_id><![CDATA[1497984412]]></mch_id>
<nonce_str><![CDATA[Wv0gI9CTf0A7KlQ0miOjudLgkCPRzobT]]></nonce_str>
<openid><![CDATA[oUuptwpUTpZBQKVP7pBrsM6I6Elw]]></openid>
<out_trade_no><![CDATA[2021030422515900000012]]></out_trade_no>
<result_code><![CDATA[SUCCESS]]></result_code>
<return_code><![CDATA[SUCCESS]]></return_code>
<sign><![CDATA[4C5DEFDE0F79D325046D193AF6EED5A692600C41E3369FA06BE8F4F74B996C51]]></sign>
<time_end><![CDATA[20210304225223]]></time_end>
<total_fee>1</total_fee>
<trade_type><![CDATA[NATIVE]]></trade_type>
<transaction_id><![CDATA[4200000993202103048037784885]]></transaction_id>
</xml>
>>>+<xml><appid><![CDATA[wx632c8f211f8122c6]]></appid>
<bank_type><![CDATA[OTHERS]]></bank_type>
<cash_fee><![CDATA[1]]></cash_fee>
<fee_type><![CDATA[CNY]]></fee_type>
<is_subscribe><![CDATA[N]]></is_subscribe>
<mch_id><![CDATA[1497984412]]></mch_id>
<nonce_str><![CDATA[Wv0gI9CTf0A7KlQ0miOjudLgkCPRzobT]]></nonce_str>
<openid><![CDATA[oUuptwpUTpZBQKVP7pBrsM6I6Elw]]></openid>
<out_trade_no><![CDATA[2021030422515900000012]]></out_trade_no>
<result_code><![CDATA[SUCCESS]]></result_code>
<return_code><![CDATA[SUCCESS]]></return_code>
<sign><![CDATA[4C5DEFDE0F79D325046D193AF6EED5A692600C41E3369FA06BE8F4F74B996C51]]></sign>
<time_end><![CDATA[20210304225223]]></time_end>
<total_fee>1</total_fee>
<trade_type><![CDATA[NATIVE]]></trade_type>
<transaction_id><![CDATA[4200000993202103048037784885]]></transaction_id>
</xml>

```



5、回应消息给微信后台 ，我们可以进入官网，查看结果通知形式，这时候我们完善结果通知给微信服务器

```java
@Controller
@RequestMapping("wxpay")
public class WXPayController {
    @RequestMapping("/notify")
    public void getNotifyURL(HttpServletRequest request, HttpServletResponse response) throws IOException {
        //获取微信支付的请求内容,从请求消息中获得数据
        ServletInputStream is = request.getInputStream();
        byte [] b = new byte[1024];
        int len=0;
        String str=null;
        while ((len=is.read(b))!=-1){
            str = new String (b,0,len);
        }
        //处理微信支付返回来的结果
        System.out.println(">>>+"+str);

        // 返回给微信一个标准格式的回信
        response.getWriter().write("<xml><return_code><![CDATA[SUCCESS]]></return_code><return_msg><![CDATA[OK]]></return_msg></xml>");
    }
}
```



微信文档地址 https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=9_7&index=8

支付结果通知：

```xml
<xml>
  <return_code><![CDATA[SUCCESS]]></return_code>
  <return_msg><![CDATA[OK]]></return_msg>
</xml>
```



## 4 ssm项目整合微信支付

一个比较完整的微信支付流程

![image-20210304232243234](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210304232243234.png)

1、`用户`进入 `商户项目` 进行产品下单，并选择微信支付

2、`商户项目`向`微信后台`发起下单请求

3、微信平台收到请求后，返回给[商户项目] 一个回复，里面包含二维码连接；

4、[商户项目] 根据二维码连接，生成支付二维码，并返回给 `用户`

5、`用户`打开微信app扫码，扫码成功后`微信`通知 `微信平台`支付成功

6、`微信平台`通过调用回调接口，通知`商户项目` 支付结果

7、`商户项目` 应答`微信平台`已经收到支付结果，同时通知用户 `下单成功`

8、`用户` 此时跳转到下单成功页面

接下来，我们就需要根据这个时序图，来进行我们的项目开发

### 4.1 整合项目

step 1、把wxpay sdk里面的相关Java类和接口拷贝到top.aigoo目录下，如下图

![image-20210304234621516](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210304234621516.png)

step 2. 把wxpay sdk 的pom依赖，加入到ssm项目中，里面的profile等资料可以先不存进去



step3、进入到报错的类，因为我们导入了不同的包，所以对应的包地址改一下

![image-20210304234745699](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210304234745699.png)

整合后的pom.xml

```shell
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>SSMBackProject</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!--依赖
        junit
        数据库驱动 mysql-connector-java
        连接池 c3p0
        servlet servlet-api
        jsp jsp-api,
        jstl
        mybatis mybatis,
        mybatis-spring,
        spring  spring-webmvc,
        spring-jdbc
        jackson
        GoEasy依赖
        -->
        <!--GoEasy依赖-->
        <dependency>
            <groupId>io.goeasy</groupId>
            <artifactId>goeasy-sdk</artifactId>
            <version>0.3.16</version>
        </dependency>
        <!--Junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!--mysql数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--数据库连接池-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>
        <!--servlet-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <!--jsp-->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <!--jstl-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--mybatis-spring-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
        <!--spring-mvc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <!--spring-jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
        </dependency>
        <!-- jackson https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.8</version>
        </dependency>
        <!--https://github.com/pagehelper/Mybatis-PageHelper/blob/master/README_zh.md-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.2.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!--微信支付sdk-->
        <dependency>
            <groupId>com.github.wechatpay-apiv3</groupId>
            <artifactId>wechatpay-apache-httpclient</artifactId>
            <version>0.2.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore -->
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpcore</artifactId>
            <version>4.4.14</version>
        </dependency>
        <!--微信支付-->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.21</version>
        </dependency>
        <!--微信支付-->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.21</version>
        </dependency>

    </dependencies>
    <!--Goeasy长连接仓库-->
    <repositories>
        <repository>
            <id>goeasy</id>
            <name>goeasy</name>
            <url>http://maven.goeasy.io/content/repositories/releases/</url>
        </repository>
    </repositories>
    <!--微信支付相关-->
    <licenses>
        <license>
            <name>The BSD 3-Clause License</name>
            <url>https://opensource.org/licenses/BSD-3-Clause</url>
            <distribution>repo</distribution>
        </license>
    </licenses>

    <!--静态资源导出-->
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
</project>
```

### 4.2  完成 统一下单



统一下单的基本逻辑是，

 1、加载商家相关配置

 2、创建支付对象

 3、将支付相关元数据汇总到map

 4、向微信服务器发起下单请求



重点是，有一个回调接口在元数据

相关 代码如下:

```java
@RequestMapping("/doPay")
@ResponseBody
public String doPay() throws Exception {
    // 用于生成订单编号的随机数
    Date date = new Date();
    SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMddHHssmm");
    String orderIdPrefix = sdf.format(date);// 创建订单编号前缀
    String deviceId = "59000000"; //设备id
    String pId = String.valueOf(new Random().nextInt(1000)); //商品id
    String orderId = orderIdPrefix+deviceId+pId;    //订单号


    //1. 创建配置类对象
    MyWxPayConfig config = new MyWxPayConfig();
    // 2. 创建支付对象
    WXPay  wxpay= null;
    try {
        wxpay = new WXPay(config);
        // 3. 将一些元数据存入到map集合
        Map<String, String> data = new HashMap<String, String>();
        data.put("body", "腾讯充值中心-QQ会员充值"); //要支付的产品标题
        data.put("out_trade_no", orderId);  //订单编号 年月日时间+随机码+订单编号
        data.put("device_info", ""); //设备信息
        data.put("fee_type", "CNY");  // 货币类型
        data.put("total_fee", "1"); // 货币金额
        data.put("spbill_create_ip", "123.12.12.123");
        // 最重要的点，需要有一个回调的接口，这个接口可以获取此次微信支付的信息
        //data.put("notify_url", "http://www.example.com/wxpay/notify"); // 回调地址，当微信支付完会调用这个地址
        data.put("notify_url", "http://bhyjb6.natappfree.cc/wxpay/notify"); // 回调地址，当微信支付完会调用这个地址

        data.put("trade_type", "NATIVE");  // 此处指定为扫码支付
        data.put("product_id", pId); //商品的id

        try {
            Map<String, String> resp = wxpay.unifiedOrder(data);
            System.out.println(resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }catch (Exception e){
        e.printStackTrace();
    }
    return "ok";
}
```





### 4.3 整合Zxing生成支付二维码

1、首先，导入包

```
<!--二维码生成包-->
<dependency>
    <groupId>com.google.zxing</groupId>
    <artifactId>core</artifactId>
    <version>3.3.3</version>
</dependency>
<dependency>
    <groupId>com.google.zxing</groupId>
    <artifactId>javase</artifactId>
    <version>3.3.3</version>
</dependency>
```

2、在代码中使用Zxing，实现生成二维码

```java
/**
 * Created by Josh luo on 2021/3/5
 */
@Controller
public class QrcordController {
    @RequestMapping("/qrcode")
    public void qrcode(HttpServletRequest request, HttpServletResponse response){
        // 二维码需要包含的文本内容
        String uri = "http://www.baidu.com";
        HashMap<EncodeHintType, Object> hints = new HashMap<EncodeHintType, Object>();
        hints.put(EncodeHintType.CHARACTER_SET,"UTF-8");
        hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.M);
        hints.put(EncodeHintType.MARGIN, 2);

        try {
            BitMatrix bitMatrix = new MultiFormatWriter().encode(uri, BarcodeFormat.QR_CODE, 200, 200, hints);
            MatrixToImageWriter.writeToStream(bitMatrix,"PNG",response.getOutputStream());
            System.out.println("创建二维码完成");
        } catch (WriterException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```



### 4.4 编写微信支付接口

step 1、 完善我们的支付接口，我们需要让用户支付完成后，自动调用生成二维码并并进入到支付页面

```
/**
 * 向微信平台发起 下单请求接口
 */
@RequestMapping("/doPay")

public void doPay(HttpServletResponse response) throws Exception {
    // 用于生成订单编号的随机数
    Date date = new Date();
    SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMddHHssmm");
    String orderIdPrefix = sdf.format(date);// 创建订单编号前缀
    String deviceId = "59000000"; //设备id
    String pId = String.valueOf(new Random().nextInt(1000)); //商品id
    String orderId = orderIdPrefix+deviceId+pId;    //订单号


    //1. 创建配置类对象
    MyWxPayConfig config = new MyWxPayConfig();
    // 2. 创建支付对象
    WXPay  wxpay= null;
    try {
        wxpay = new WXPay(config);
        // 3. 将一些元数据存入到map集合
        Map<String, String> data = new HashMap<String, String>();
        data.put("body", "腾讯充值中心-QQ会员充值"); //要支付的产品标题
        data.put("out_trade_no", orderId);  //订单编号 年月日时间+随机码+订单编号
        data.put("device_info", ""); //设备信息
        data.put("fee_type", "CNY");  // 货币类型
        data.put("total_fee", "1"); // 货币金额
        data.put("spbill_create_ip", "123.12.12.123");
        // 最重要的点，需要有一个回调的接口，这个接口可以获取此次微信支付的信息
        //data.put("notify_url", "http://www.example.com/wxpay/notify"); // 回调地址，当微信支付完会调用这个地址
        data.put("notify_url", "http://bhyjb6.natappfree.cc/wxpay/notify"); // 回调地址，当微信支付完会调用这个地址

        data.put("trade_type", "NATIVE");  // 此处指定为扫码支付
        data.put("product_id", pId); //商品的id

        try {
            Map<String, String> resp = wxpay.unifiedOrder(data);
            // 二维码连接 code_url
            String uri = resp.get("code_url");
            //生成二维码
            QRcordController.qrcode(response,uri);
            System.out.println(resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }catch (Exception e){
        e.printStackTrace();
    }
}
```

在这里，我们主要完善了3点

1、从微信返回的map中，获取二维码连接 uri    String uri = resp.get("code_url");

2、我们调用二维码生成接口，  QRcordController.qrcode(response,uri); ，在这里，我们传递一个response变量，传递一个uri变量，则qrcode()会将生成二维码放入到response中



step 2、我们通过访问一个接口获取二维码，这个接口是显示一个页面

```
@RequestMapping("/showpay")
public String showPay(){
    return "mypay";
}
```

step 3、我们在进入到/WEB-INF/views/ 添加一个mypay.jsp，在这个jsp页面，我们发起请求支付并生成二维码的请求

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>支付页面</title>
</head>
<body>
    <h1>请扫码支持</h1>
    <img src="/wxpay/doPay">
</body>
</html>
```



做完上述步骤，启动服务器，在浏览器请求  http://localhost:8080/wxpay/showpay，可以发现已经可以生成支付二维码了。

### 4.5 使用GoEasy解决异步支付后跳转

当我们用户支付成功后，这个时候，从用户侧，是需要用户跳转到下单页面的，这个时候，我们就需要有一个能力，让用户的浏览器自动跳转到下单成功页面，这个时候就需要我们的goeasy能力



step 1、 先注册一个账号，登录后，创建一个应用，就能得到您的appkey  [goeasy官网](https://www.goeasy.io/cn/get-start.html)



![image-20210305204321821](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210305204321821.png)

step 2、服务器导包，加入goeasy的包

```
<!--GoEasy依赖-->
<dependency>
<groupId>io.goeasy</groupId>
<artifactId>goeasy-sdk</artifactId>
<version>0.3.16</version>
</dependency>
```



step 3、 进入到我们的后台，WXPayController中，增加异步跳转的功能

```java
@RequestMapping("/notify")
public void getNotifyURL(HttpServletRequest request, HttpServletResponse response) throws IOException {
    //获取微信支付的请求内容,从请求消息中获得数据
    ServletInputStream is = request.getInputStream();
    byte [] b = new byte[1024];
    int len=0;
    String str=null;
    while ((len=is.read(b))!=-1){
        str = new String (b,0,len);
    }
    //处理微信支付返回来的结果
    System.out.println(">>>+"+str);

    //@TODO 此处需要处理，增加从微信请求信息中获取返回回调信息，并判断是否为支付成功
    

    // 返回给微信一个标准格式的回信
    response.getWriter().write("<xml><return_code><![CDATA[SUCCESS]]></return_code><return_msg><![CDATA[OK]]></return_msg></xml>");
    
    //异步通知用户浏览器从支付页面跳转到支付成功页
    GoEasy goEasy = new GoEasy( "http://rest-hangzhou.goeasy.io","my_appkey");
    goEasy.publish("my_channel", "Hello, GoEasy!");
}
```



step 4、进入到我们的前台页面, WEB-INF/views/mypay.jsp，前端引入goeasy

```javascript
<%--引入goeasy--%>
<script type="text/javascript" src="https://cdn.goeasy.io/goeasy-1.2.1.js"></script>

<script type="text/javascript">
    const goEasy = GoEasy.getInstance({
        host: 'hangzhou.goeasy.io', //应用所在的区域地址: 【hangzhou.goeasy.io |singapore.goeasy.io】
        appkey: "BC-b43c1c01801040419914ced9f1afd583" //替换为您的应用appkey
    });
    
</script>
```



step 5、我们从后端推送goeasy

```
goEasy.publish("my_channel", "Hello, GoEasy!");  // 隧道名可以使用订单的编号，解决一次推送所有客户端都会被推送的问题
```

这里面

my_channel:  就是我们的频道， 以为前端监听同一频道，都能收到消息，所以在真实案例中，我们一般频道选择用订单号等类似唯一标识符作为频道

例如  “pay_success_go_nextpage_channel_202103052122”

Hello, GoEasy! :



step 6、在接收和发送消息之前，必须要先连接GoEasy，我们在mypay.jsp，连接GoEasy

```javascript
goEasy.connect({
    onSuccess: function () {  //连接成功
        console.log("GoEasy connect successfully.") //连接成功
    },
    onFailed: function (error) { //连接失败
        console.log("Failed to connect GoEasy, code:"+error.code+ ",error:"+error.content);
    },
    onProgress:function(attempts) { //连接或自动重连中
        console.log("GoEasy is connecting", attempts);
    }
});
```



step 7,在web客户端去接收消息

在发送消息之前，您需要先完成订阅操作, 来准备接收消息。

1.根据您的业务需求来设定，channel可以为任意字符串，除了不能包含空格，和不建议使用中文外，没有任何限制，只需要和消息的发送端保持一致，就可以收到消息。channel可以是您直播间的uuid,也可以是一个用户的唯一表示符，可以订阅多个channel，可以任意定义，channel不需要创建，可随用随弃。

```javascript
goEasy.subscribe({
    channel: "my_channel",//替换为您自己的channel
    onMessage: function (message) {
        console.log("Channel:" + message.channel + " content:" + message.content);
    },
    onSuccess: function () {
        console.log("Channel订阅成功。");
    },
    onFailed: function (error) {
        console.log("Channel订阅失败, 错误编码：" + error.code + " 错误信息：" + error.content)
    }
});
```



这样，当我们微信回调我们的回调函数，就会向前端告诉前端，跳转到支付成功页面



完整的前台请求页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>支付页面</title>
</head>
<body>
    <h1>请扫码支持</h1>
    <img src="/wxpay/doPay">
    <%--引入goeasy--%>
    <script type="text/javascript" src="https://cdn.goeasy.io/goeasy-1.2.1.js"></script>

    <script type="text/javascript">
        const goEasy = GoEasy.getInstance({
            host: 'hangzhou.goeasy.io', //应用所在的区域地址: 【hangzhou.goeasy.io |singapore.goeasy.io】
            appkey: "BC-b43c1c01801040419914ced9f1afd583" //替换为您的应用appkey
        });
        goEasy.connect({
            onSuccess: function () {  //连接成功
                console.log("GoEasy connect successfully.") //连接成功
            },
            onFailed: function (error) { //连接失败
                console.log("Failed to connect GoEasy, code:"+error.code+ ",error:"+error.content);
            },
            onProgress:function(attempts) { //连接或自动重连中
                console.log("GoEasy is connecting", attempts);
            }
        });


        // 接收消息
        goEasy.subscribe({
            channel: "pay_success_go_nextpage_channel_202103052122",//替换为您自己的channel
            onMessage: function (message) {
                console.log("Channel:" + message.channel + " content:" + message.content);
            },
            onSuccess: function () {
                alert("success");
                console.log("Channel订阅成功。");
            },
            onFailed: function (error) {
                console.log("Channel订阅失败, 错误编码：" + error.code + " 错误信息：" + error.content)
            }
        });

    </script>
</body>
</html>
```

完整的后端请求函数

```java
/**
 * 微信支付在发起下单请求后  支付结果通知
 * @param request  微信支付的请求内容
 * @param response  返回给微信支付平台的支付成功结果
 * @throws IOException
 */
@RequestMapping("/notify")
public void getNotifyURL(HttpServletRequest request, HttpServletResponse response) throws IOException {
    //获取微信支付的请求内容,从请求消息中获得数据
    ServletInputStream is = request.getInputStream();
    byte [] b = new byte[1024];
    int len=0;
    String str=null;
    while ((len=is.read(b))!=-1){
        str = new String (b,0,len);
    }
    //处理微信支付返回来的结果
    System.out.println(">>>+"+str);

    //@TODO 此处需要处理，增加从微信请求信息中获取返回回调信息，并判断是否为支付成功


    // 返回给微信一个标准格式的回信
    response.getWriter().write("<xml><return_code><![CDATA[SUCCESS]]></return_code><return_msg><![CDATA[OK]]></return_msg></xml>");

    //异步通知用户浏览器从支付页面跳转到支付成功页
    GoEasy goEasy = new GoEasy( "http://rest-hangzhou.goeasy.io","BC-b43c1c01801040419914ced9f1afd583");

    goEasy.publish("pay_success_go_nextpage_channel_202103052122", "Hello, GoEasy!");
}
```



## 5. 整合业务项目

### 5.1 项目需求分析

一个标准开发步骤：

1、定需求

2、提取数据实体

3、设计数据库

4、编码

5、单元测试系统测试

6、上线  bug  上线  bug

**第一步，先分析需求**

本次项目需求，实现一个下单的需求， 用户浏览商品，点击商品，选择购买，选择下单，支付，然后成功，业务流程图如下：

![image-20210305233900285](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210305233900285.png)



**第二步，我们就要开始提取实体了，**

实体在 java叫做对象，在数据库就是一张表

在上述业务，我们会碰到的实体可能如下的

1、商户、 商户钱包、

2、用户、用户钱包、用户的收货地址、

3、商品、在售商品、已售商品、历史商品价格表、

4、订单、订单详情、

初步整理，我们需要11张彪，接下来我们就要使用powerdesigner画出ER图

**第三步：使用powerdesigner设计ER图**

3.1  我们在工作空间，创建一个conceptual data model  (概念数据模型)

3.2 我们点击entity， 在面板上点击创建一个实体，我们还需要选择tools - model options，在notation里面选择Entity/Releationship 

3,3 双击实体就可以开始编辑了

	- 做表的时候， 对于价格，我们使用bigint, 这样如果12.29 我们在数据库就是1229，往外用，就除以100，往里存就乘以100
	- 对于主键和商品pid，我们一般让商品主键自主增加，商品pid供查询使用，这样速度快
	- 对于字符串，我们使用varchar

经过一系列操作，我们生成的ER图如下

![image-20210306010733427](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306010733427.png)



**第四步：做完ER图，我们下一步就是生成physical data model(物理数据模型)**

根据我们的概念数据模型，得到我们的物理数据模型，物理模型可以生成数据库表

step 1. 点击 tools--generate physical data model， 进入生成物理数据模型引导窗口

step 2. 在引导窗口，选择DBMS为mysql 5.0 ,

![image-20210306011418479](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306011418479.png)



生成后的物理数据模型如下

![image-20210306011808778](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306011808778.png)







**五、在生成了物理数据模型后，就是生成数据库**

step1、 点击两个实体之间的关系，去掉所有物理数据模型的关系，并点击表，在主键列，双击，点击identity，让主键自增

![image-20210306013030442](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306013030442.png)



![image-20210306013045932](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306013045932.png)



step 2、点击database ---generate database, 我们把这个sql文件保存到我们的Java后台项目文件中

![image-20210306120725322](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306120725322.png)

step 3、 打开navicat，先创建一个编码格式为 utf8mb4 -- UTF-8 Unicode 的数据库，名称命名为qf-shop-v1，然后执行excute sql file，就可以生成我们的表了

![image-20210306120852597](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306120852597.png)



至此，我们就完成了我们的项目需求和数据库工作

### 5.2 业务分析和设计



#### 5.2.1 mybatis逆向工程插件

mybatis逆向工程是一个可以快速根据数据库表帮我们生成pojo实体类和mapper接口及mapper映射文件的一个插件，需要下载这个项目，需要注意的是：只能支持单表操作(单表的增删改查等sql可以帮助我们生成), 关联查询需要自己写。

国内有一个叫tk-mybatis，也是具有这个功能，建议查阅，已经更加好用的工具



首先, 使用逆向工程插件，来获得mybatis需要的内容,在SSM项目中我们需要三个部分来实现DAO的操作，分别是

1） 数据库表对应的实体 entity

2） DAO层的接口 mapper

3) 实体和表之间的映射文件  .xml



接下来，我们将通过使用逆向工程插件来获得这三个部分



使用的步骤：

step 1、引入mybatis-generator-core 依赖

```xml
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.3.7</version>
</dependency>
```



step2 配置文件的配置 ,在项目的resources目下下，新建generator文件夹，在里面新建generatorConfig.xml

```xml
<!DOCTYPE generatorConfiguration PUBLIC
        "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--指定了驱动jar包的位置，这个是针对下载jar包的方式,因为用了maven所以这个就用不上了-->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <commentGenerator>
            <property name="suppressDate" value="false"/>
            <!--是否去除自动生成的注释 true 是  false  否-->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!--数据库连接url 用户名 密码-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/qf-shop-v1?serverTimeZone=Asia/ShanghaiuseSSL=false&amp;characterEncoding=utf8&amp;useUnicode=True"
                        userId="root"
                        password="123456">
            <!--<property name="serverTimeZone" value="UTC"/>-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>
        <!--指定生成entiry实体类的具体位置-->
        <javaModelGenerator targetPackage="top.aigoo.pojo" targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
            <!--去掉前后空格-->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <!--指定生成mybatis映射xml文件的包名和位置-->
        <sqlMapGenerator targetPackage="top.aigoo.dao" targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>
        <!--指定生成mapper接口的具体位置-->
        <javaClientGenerator type="xmlmapper" targetPackage="top.aigoo.dao" targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>
        <!--
        要生成entity/mapper的表名及自定义的DO名
        <table tableName="users" domainObjectName="User"/>
        -->
        <table tableName="t_bis_wallet" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_business" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_historyprice" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_order" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_orderinfo" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_product" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_saledproduct" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_saleproduct" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_user" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_user_addr" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />

        <table tableName="t_wallet" enableCountByExample="false"
               enableUpdateByExample="false" enableDeleteByExample="false"
               enableSelectByExample="false" selectByExampleQueryId="false" />
    </context>
</generatorConfiguration>
```

step 3、在pom.xml里面我们还要配置插件

```xml
<plugins>
    <!--mybatis逆向工程-->
    <plugin>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>1.3.7</version>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.1.47</version>
            </dependency>
        </dependencies>
        <configuration>
            <!--配置文件的路径-->
            <configurationFile>src/main/resources/generator/generatorConfig.xml</configurationFile>
            <overwrite>true</overwrite>
        </configuration>
    </plugin>
</plugins>
```

step 4、使用maven中的plugins中的mybatis-generator-maven-plugin来进行生成

![image-20210306144610770](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306144610770.png)



为了保证这些是没问题，我们最好做一个单元测试

```java
public class TestTProduct {
    @Test
    public void testAddTProduct(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        TProductService productServiceImpl = context.getBean("productServiceImpl", TProductService.class);
        TProduct product = new TProduct();
        product.setPid(10011L);
        product.setPname("hua wei P 30");
        product.setPrice(6777L);

        int result = productServiceImpl.addProduct(product);

        if (result>0){
            System.out.println("插入成功");
        }else {
            System.out.println("插入失败");
        }

    }
}
```



经过上述的操作，我们就把数据库的相关工作已经完成了，接下来，我们接下来实现业务流程



### 5.3 实现业务流程

![image-20210306220843172](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210306220843172.png)



分析过程：

1、用户下单，一次订单包含着的信息有订单基本信息，还有这个订单的购买商品，所以从这儿可以看出来一个订单将影响到订单表、订单项目表、已售商品表

2、因为多张表内容，我们就需要一个OrderVO的实例来包含订单及附带的商品；

3、在OrderService里面，通过传递一个OrderVo来实现添加，所以我们可以设计一个类如 public void addOrder(OrderVO orderVO) 的方法

4、在这个方法中，当接收到OrderVo对象，分开进行插入操作，其中Order部分，通过OrderMapper.insert(Order order) 实现插入，因此需要 使用@Autowired 注入OrderMapper

5、在这个方法中，将OrderVo里面的 List<Product> 拼装成一个 List<OrderInfo> , 在设计上我们一般不调用OrderInfoMapper来插入，所以我们使用OrderInfoService来实现插入

6、这样我们就反过来需要设计 public int addOrderInfo(List<TOrderinfo> orderinfoList)，再向上在OrderInfoMapper，设计 int insertOrderInforos(List<TOrderinfo> orderinfoList);

7、而OrderInfoMapper，就需要从接口的方法--mapper.xml映射文件---OrderInfoService接口---OrderInfoserviceImpl实现，再最后注入到咱们方法中，供插入List<OrderInfo>使用

当完成这些设计，我们还需要去测试类里面，执行测试一下看是否可以成功生成。



有几个重要的节点：

1、OrderInfoMapper.xml里面，通过使用<foreach >实现插入一个列表数据

```xml
<insert id="insertOrderInforos">
  insert into t_orderinfo (id, order_id, pid)
  values
  <foreach collection="list" item="orderinfo" separator=",">
    (#{orderinfo.id,jdbcType=BIGINT}, #{orderinfo.orderId,jdbcType=BIGINT}, #{orderinfo.pid,jdbcType=BIGINT})
  </foreach>
</insert>
```

2、OrderInfoServiceImpl 需要通过OrderInfoMapper来执行插入List<Orderinfo>的能力

```java
@Service
public class OrderInfoServiceImpl implements IOrderInfoService {
    @Autowired
    TOrderinfoMapper orderinfoMapper;
    /**
     *要添加的orderInfo，将一个订单的多条记录添加到订单详情表，因为一个订单会有多个商品
     * @param orderinfoList 要添加的orderinfo的集合
     */
    public int addOrderInfo(List<TOrderinfo> orderinfoList) {
        return orderinfoMapper.insertOrderInforos(orderinfoList);
    }
}
```

3、对于一个订单含有多张表内容，一般需要用VO来解决

```java
public class OrderVO {

    /**
     * 订单实例
     */
    private TOrder tOrder;

    /**
     * 当前订单包含商品的集合
     */
    private List<TProduct> products;
}
```

4、在OrderService中，是直接处理相关操作的区域

```java
@Service
public class OrderServiceImpl implements IOrderService {

    @Autowired
    TOrderMapper orderMapper;

    @Autowired
    OrderInfoServiceImpl orderInfoServiceImpl;

    /**
     * OrderVO
     * 往订单表填数据
     * 往订单详情填数据
     * 往已售商品填数据
     *
     * @param orderVO
     */
    public void addOrder(OrderVO orderVO) {
        // 从OrderVo中获得order对象
        TOrder tOrder = orderVO.gettOrder();
        // 从OrderVo中获得所有商品的集合
        List<TProduct> products = orderVO.getProducts();
        // 将order存到数据库的订单表中
        int order_result = orderMapper.insert(tOrder);

        //=============== 接下来封装订单详情TOrderInfo
        //需要封装一个List<TOrderInfo>

        List<TOrderinfo> orderinfoList = new ArrayList<TOrderinfo>();
        //1 . 遍历
        Iterator<TProduct> iterator = products.iterator();
        Long orderId = tOrder.getOrderId();

        while (iterator.hasNext()) {
            TProduct product = iterator.next();

            TOrderinfo tOrderinfo = new TOrderinfo();
            tOrderinfo.setPid(product.getPid());
            tOrderinfo.setOrderId(orderId);

            orderinfoList.add(tOrderinfo);
        }
        // 得到list,调用TOrderInfo的service实现订单详情列表的插入
        orderInfoServiceImpl.addOrderInfo(orderinfoList);
    }
}
```



5、在OrderVO里面只有List<Product>， 但是我们进行OrderInfoService.addOrderInfo 需要的是List<OrderInfo > ,所有需要把List<product>封装为 <OrderInfo>

```java
//需要封装一个List<TOrderInfo>

List<TOrderinfo> orderinfoList = new ArrayList<TOrderinfo>();
//1 . 遍历
Iterator<TProduct> iterator = products.iterator();
Long orderId = tOrder.getOrderId();

while (iterator.hasNext()) {
    TProduct product = iterator.next();

    TOrderinfo tOrderinfo = new TOrderinfo();
    tOrderinfo.setPid(product.getPid());
    tOrderinfo.setOrderId(orderId);

    orderinfoList.add(tOrderinfo);
}
```



最终我们设计一个测试

```java
public class TestOrder {


    @Test
    public void addOrder(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        IOrderService orderService = context.getBean("orderServiceImpl",IOrderService.class);
        TProductService productService = context.getBean("productServiceImpl",TProductService.class);

        // 需要编写数据
        OrderVO orderVO = new OrderVO();

        TOrder tOrder = new TOrder();

        tOrder.setUserId(1L);
        tOrder.setOrderId(Long.parseLong(MyUtils.getCurrentTime()));
        tOrder.setOrderPrice(99887L);
        tOrder.setOrderFlag((short)0);
        tOrder.setOrderAddr("北京市海淀区福缘门社区5排3号301");
        tOrder.setOrderTel("13801232323");
        tOrder.setOrderUser("李杨");
        tOrder.setCreateTime(new Date());
        tOrder.setUpdateTime(new Date());

        orderVO.settOrder(tOrder);

        //======

        List<TProduct> products = productService.getProducts();
        orderVO.setProducts(products);

        //==========orderVO封装完毕，调用service
        orderService.addOrder(orderVO);
    }
}
```



### 5.4 事务传播的问题提出

事务传播：当事务A中包含了事务B，如果事务B出现了回滚，导致事务A也会产生回滚，此时事务A和事务B成为了一个事务整体，如何实现事务B的回滚不影响事务A呢？ 这就是事务传播的问题。

![image-20210307205145552](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210307205145552.png)



springmvc的声明式事务--事务并发、传播性、隔离级别(重难点)



导读，本届重点在于多线程并发环境下事务处理、和数据库在并发环境下的表锁和行锁

案例：在新增图书的时候，肯定需要先新增作者。

springmvc声明式事务，

事务分为两种    编程式事务和 声明式事务

Connection conn

conn.setAutoCommit(false)

conn.commit()

conn.rollback()

shangshu 这种就是编程式事务

**我们使用spring mvc的声明式事务，需要做的步骤**



step1 、导入事务包  

```xml
<!--spring-tx-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>
```

step 2、配置，在sprin-service.xml配置加入事务

```xml
    <!--step 3.配置声明式事务管理器，指明哪个数据源采用哪个事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源， 表明dataSource数据源使用DataSourceTransactionManager-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--允许事务以注解方式进行配置-->
    <tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/> <!--指定使用哪个事务管理器-->
```

一个小坑：

1） proxy-target-class="true  这个属性代表Java动态代理是以类，而不是以接口，没加这个时候，使用注解事务，就会报错，找不到bean



step 3、使用事务注解

```java
@Transactional(readOnly=true, rollBack=Exception.class)  //次注解表示此方法的事务交给spring管理
public void  save(TbBook book){
    // 新增作者新增，并且需要返回主键
    int authorId=authorService.save(book.getAuthor);
    if(1==1){
        throw new NullPointerException("故意的");
    }

    book.setAuthorId(authorId);
    //完成新增图书操作
    bookMapper.save(book);
}
```



注意：

1、我们在使用的时候不喜欢使用物理外键





#### 5.4.1 事务的传播

```
导读: 我们知道,mvc模型中一般将事务放在service层控制，那么问题时，service层方法A调用service层方法B，那方法A和方法B是否在同一个事务中？
```

![image-20210308214351942](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210308214351942.png)

```
案例1：
方法A和方法B上都有事务注解，方法A调用方法B，此时方法B中出现异常，会否会导致方法A的回滚

答案： 会回滚

分析：方法A和方法B在同一个事务中，证明方法A的事务会传递给方法B，这就是spring 声明式事务的传播性
案例2  方法A调用方法B，方法B出现异常，不要影响到方法A的成功事务执行

```

**spring中的事务传播性**

涉及到事务的作用域，session和事务件的关系，事务的传播行为有如下几种：

```
1 PROPAGATION_REQUIRED 支持当前事务，没有事务就新建一个，默认的
比如 方法A调用方法B，方法A有事务，于是方法B会沿用方法A的使用，而不会重新创建，
	如果方法A没有事务，方法B有事务，方法B会新建事务

2 PROPAGATION_SUPPORTS  支持当前事务，如果没有事务，不新建事务
比如  方法A 调用方法B，方法A由事务，方法B会沿用方法A的事务，而不会重新创建，
       如果方法A不支持事务，方法B即使有事务，方法B也不会新建事务

3 PROPAGATION_MANDATORY   支持当前事务，没有事务会抛出异常
比如   方法A调用方法B，方法A有事务，则方法B也有事务，
	   如果方法A没有事务，方法B需要事务就会抛出异常

4 PROPAGATION_REQUIRES _NEW  使用新建事务，如果当前存在事务，把当前事务挂起，
比如 方法A 调用方法B，不管方法A是否支持事务，方法B都会新建事务，挂起方法A 的事务

5 PROPAGATION_REQUIRES_NOT_SUPPORTED  不支持事务操作，有事务则挂起
比如  方法A 调用方法B，不论方法A是否有事务，方法B都不支持事务

6 PROPAGATION_NEVER  不支持事务，有事务则挂起
比如  方法A 调用方法B，不论方法A 是否有事务，方法B 都不支持事务

7 PROPAGATION_NESTED  如果当前存在事务，则在嵌套事务内执行，如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作
比如   方法A调用方法B，方法B产生异常，回滚，但是方法A没有异常，方法A的数据插入成功 

我们经常使用的事务是  PROPAGATION_NESTED
```

注意：



1） 内层事务的异常可能影响到外层事务，即使内层开启的新事物，原因：内层方法出现异常，会导致被挂起的异常无法正常执行完成

比如方法B产生异常，但在方法A没捕获，必然导致方法A的事务回滚，即使事务B使用NEST方法

2） 经常使用的两个事务传播级别  PROPAGATION_NESTED 和 PROPAGATION_REQUIRED

3） 默认情况下，spring方法中事务的传播特性为  如果方法的上一级方法存在事务，则方法会沿用上一级方法的事务

4） PROPAGATION_REQUIRES _NEW 开启新的session



#### 5.4.2 事务的隔离

```
导读
事务的四大特性中隔离性的实现会带来一定的性能效果，要实现事务的隔离性，就需要避免线程同步操作同一条数据。事务隔离级别问题分类如下：
1、脏读，事务A读取了事务B没有提交的数据
2、不可重复读，事务a读取了数据后，事务b对此会数据进行修改，事务a再次读取发现不一致
3、幻读，事务a读取了一个范围的数据后，事务b又修改了一条数据进来，导致事务a再次读取时发现不一样，多处一行数据，好像出现幻觉一样

```



隔离级别是事务四大特性之一的隔离性的级别，隔离分为不同级别的隔离

读未提交 read uncommitted   没有隔离

度已提交  read committed

可重复读  repeatable read 默认

串行化  serializable

事务隔离级别的事先方式是枷锁，隔离级别越高，锁影响的范围越大，效率也就越低

脏读演示

解决脏读：配置事务隔离级别为read committe

现象：一个事务读取了另一个事务没有提交的内容

模拟：

1、开启两个客户端，都开启事务

2、一个客户端用来读取数据，一个客户端用来写数据

3、写操作进行事务回滚



避免脏读

数据库默认是避免脏读的

```
select @@tx_isolation;
## 设置数据的事务隔离级别
## read uncommitted 能读取其他事务没有提交的内容，最低的隔离级别
set session transaction isolation level repeatable read;

start transaction;
select * from tb_account where account_id =1 for update;

update tb_account set account_money=200 where account_id =1;

commit;
insert into tb_account(account_money,account_user_id) values(100,2);
rollback

select @@tx_isolation;

select * from tb_account;
start transaction
```



锁：

数据库引擎常见分2中 myisam和innodb

数据库引擎 是一种数据库操作机制，目前数据库默认存储引擎，innodb

区别

innodb 支持事务，行锁

myisam 不支持事务，表锁



InnoDB  行锁，共享锁和排它锁 写锁



select .... lock in share mode  共享锁，乐观锁，读锁  特点 对读操作共享，写操作排队



select.... for update;  排它锁



Myisam

lock table tb_user read

unlock tables;















### 5.5 使用element-ui设计首页



**准备工作： 准备一个VUE工作坏境，安装vue-router, axios, element-ui**

```bash
## element-ui
/*element-ui*/
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);

## vue-router
npm install vue-router --save-dev
// 在index 引入vue-router*/
import Router from 'vue-router'
Vue.use(Router);
//在main.js加载router
//简单示例

## axios
npm install --save axios vue-axios

import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)
```

通过上述操作，我们就能得到一个干净的webpack工作环境。



**step 1、首页布局，先搭建项目骨架，完成整体项目的大视图,**



**step 2、完成banner的设计**

 图片可以找一个千图网找几个好看的banner

图片可放在static文件夹，然后在vue里面使用data()里面放入 bannerImages :{}



**step 3、完成商品列表的设计**

我们想放入3个商品，呈现一个商品列表，

1、先设计假数据，使用easymock平台，设计几个假数据, 此处需要完善一下商品，我们需要增加一个商品图片的字段pimg

```json
{
    products:[
        {
        "id": 1,
        "pid": 10011,
        "pname": "hua wei p30",
        "price":6788,
        "pimg":"./static/p1.jpg"
        },
        {
        "id": 2,
        "pid": 10012,
        "pname": "hua wei p40",
        "price":6788,
        "pimg":"./static/p2.jpg"
        },
        {
        "id": 3,
        "pid": 10013,
        "pname": "xiao mi p100",
        "price":3788,
        "pimg":"./static/p3.jpg"
        }
    ]
}
```

2、先把数据放到vue的data里面，试试前台页面能否根据静态数据读取到并显示出来

```html
<div class="products">
    <div v-for="pro in props" class="product">
        <img :src="pro.pimg"/>
        <span>{{pro.name}}</span>
        <h4>
            {{pro.price}}
        </h4>
    </div>
</div>
```



3、调整细节，添加按钮，调整商品div大小，调整位置等信息

4、发送axios，动态获取到数据

npm install axios -s

在main.js引入

import axios from 'axios'

Vue.prototype.axios=axios



```javascript
created(){
    this.getList();
}
methods:{
    getlist: function(){
        var vm = this;
        this.axios({
            method:'get',
            url:'http:/xxxx.xxx/prodeuct/getlist',
        }).then( function(response){
            vm.pros = response.data.pros;
        });
    }
}
```



成功的话，我们就成功的实现了商品首页，接下来我们就可以设计后台接口

5、一般首页前端开发完，就应该彻底把后台也给完成，所以进入到后台，设计后台的接口

此处有一个技巧，我们用Mybatis-geneter，在数据库表字段改动，我们可以从pojo,到mapper接口到mapper映射都删掉，然后在generator配置文件把其他表都先注释，然后再生成一次





6、分页，我们可以使用element-ui的pagination，可以研究下





### 5.6 element-ui的详情页设计

首先 商品详情的简单结构

![image-20210308150109457](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210308150109457.png)



step 1，首先，详情作为一个新的组件 product.vue

1、要知道哪个商品，

2、在router/index.js里面增加 一个mode:"history" 可以去掉url里面的#，配置一个/的路径，让他 redirect到 /home





### 5.7 搭建购物车

#### 5.7.1 需求分析和筹备

一般，用户从产品详情页，点击加入购物车，此时，就需要知道，哪个`用户`，添加`几个`什么`商品`到购物车, 这个时候，我们就需要知道3个参数，

user_id， 可以从登陆用户获取，如果用户没有登陆，需要用户登录

pid  商品id 这个产品详情页已经有

num 计划购买的商品数量



#### 5.7.2 产品详情页加入购物车功能

一、**设计从产品详情跳转到购物车页面** 在产品详情页面，我们需要传递的参数，此时还需要知道接下来购物车的页面情况，这样前后考虑设计

1.1 设置a链接点击不出现url的跳动 ：<a class="btn-add" onclick="false" @click="setAmountAdd" href="javascript:void(0)" >+</a>  [A标签触发onclick事件而不跳转的多种解决方法](https://www.jb51.net/article/39180.htm)

​	-在详情页，点击添加按钮实现增加1个商品，点击 '-' 按钮实现减少1个商品

``` javascript
<div class="wrap-input">
    <input class="text buy-num" id="buy-num" v-model="num" data-max="200">
    <a class="btn-reduce" onclick="false" @click="setAmountReduct" href="javascript:void(0)" >-</a>
    <a class="btn-add" onclick="false" @click="setAmountAdd" href="javascript:void(0)" >+</a>
</div>
//在methods定义方法
    setAmountReduct(){
      if (this.num>0){
        console.log("nums"+this.num);
        this.num -= parseInt(1);
      }else {
        this.num=0;
      }
    },
    setAmountAdd(){
      if (this.num>=500){
        this.num=500;

      }else {
        this.num += parseInt(1);
      }
    },
```

`说明`:

​	1、从方法接收到的参数是string类型，如果参与运算需要使用parseInt(val) 这样的形式



1.2 设计路由，用户点击[加入购物车]，跳转到 购物车列表页面，

 在productDetail.vue的方法中.

```javascript
//方法1  传递参数，通过path匹配路由 ，在path后面跟上对应的值
//index.js配置路由
path: '/cart/list/:userId/:pid/:num', name:'/cartList',component: Cart
//传递方式，通过path匹配路由
goCartPage(userId,pid,num){
  this.$router.push({
    path: `/cart/list/${userId}/${pid}/${num}`  // -> /cart/list/10011/10011/2
  })
}
//获取参数
$route.params.userId
//传递后形成的路径，刷新页面，参数不丢失

/path/userOd值/pid值/num值


//方法2  传递参数，通过name匹配路由 
// 配置路由 
path: '/cart/list/', name:'cartList',component: Cart
//传值方式  通过name匹配参数
goCartPage(){
	var p = {
		userId=10011,
		pid=this.pro.pid
		pcount=this.pro.p_count
	}
  this.$router.push({name:'Cart',params:p})
}
//获取参数
$route.query.name
//传递后形成的路径，刷新页面，参数丢失
/path
```



1.3 调用方法，点击跳入到购物车页面: 

```html
<a class="elbtn btn-margin" @click="goCartPage">加入购物车</a>
```



#### 5.7.3 实现购物车后台接口



二、用户进入购物车，从详情带进来的参数是user_id, pid, num，这个时候应该有如下的过程

1、根据user_id 先要获取到用户的购物车信息

2、把添加的商品和购物车的商品进行比较，如果已在购物车就把商品数量增加num个，如果没在购物车，就在购物车里面添加一条数据;

3、把封装好的购物车信息返回给前端vue，前端vue根据购物车数据，展现在前端页面上。



业务分析：

思考： 怎么拿商品数据，我们只有UserID,Pid,count，我们需要的是，pname,pcount,pay_price, 我们需要的是这个商品的购物车的list，购物车信息，

在在页面展示的，有pid，商品名称pname、商品单价pay_price，购买数量 num，

在数据库t_cart中,有 id,cid, user_id,pid,num,pay_price,status

解决:

t_cart 要用到字段: id , pay_price , user_id, num;

t_product 要用到字段 : pid,pname,price

我们展示t_cart的数据，我们就需要连表 t_cart和t_product 进行查询，同时需要构建一个CartVO的对象

难点：

单独做都不难，但是我们需要整合一起，所以我们先做步骤1



![image-20210311150511627](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210311150511627.png)





首先 需要字段 cid  user_id  pid pname price num, 所以我们需要连表查询



```sql
select  t1.cart_id t1.user_id.t1.pid ,t1.pcount ,t2.pname,t2.price, from t_cart as t1 join t_product as t2 on t1.pid=t2.pid where t1.user_id='10011'
```



后端业务的业务流程

![image-20210311154603016](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210311154603016.png)

用户添加商品到购物车的业务流程NS图



step 1、访问购物车接口，我们需要   cart_id  user_id  pid  所以我们需要连表查询

​	1) 根据uid获取所有的购物车数据

​	2) 更新购物车表，有则更新数量，无则插入

​	3) 封装cartVO



step 1、连表查询关键语句

```sql
select t1.cid, t1.user_id,t1.pid,t1.num,t1.pay_price,t2.pname,t2.price 
from t_cart as t1 JOIN t_product as t2 
on t1.pid=t2.pid
where t1.user_id='10011'
```



step 2、，我们写一个VO类，创建CartVo 由 TCart和Tproduct两部分组成, 最好把属性都写出来，不要直接用TProduct和TCart做属性，因为后期，这两个对象由很多字段，但不是所有字段都是CartVO需要的，这样就会造成冗余字段

```java
public class CartVo{
    // t1.cart_id,t1.user_id,t1.pid,t2.pname,t2.price,t1.pcount 
    private Long cartId;
    private Long UserId;
    private Long pid;
    private String pname;
    private BigDecimal price;
    private  Long pcount;
}
```

step 3、按照前面业务流程，先实现根据user_id，取得当前数据库的购物车数据

```java
@RequestMapping("list")
public List<TCart> findCartList(Long userId) {
    return cartService.findCartListByUserId(userId);
}
```

step 4、实现添加用户商品到购物车，根据传递的user_id, pid,num

```java
@RequestMapping("addCart")
    public ServerResponse < List<CartVO> > findCartList(Long userId, Long pid, int num) {
        // step 1. 获取当前用户购物车数据
        if (userId == null|| pid==null || num<=0) {
            return ServerResponse.createByErrorCodeMessage(ResponseCode.NEED_LOGIN.getCode(), ResponseCode.NEED_LOGIN.getDesc());
        }
        List<TCart> carts = findCartList(userId);
        // step 2. 判断当前的pid是否在cartLists里面,如果在就把flag更新为false
        boolean flag = true;
        for (TCart cart : carts) {
            if (cart.getPid().equals(pid)) {
                //2.1 pid已存在
                cart.setNum(cart.getNum() + num);
                // 增加商品数量
                //int result = cartService.updateByPrimaryKey(cart);
                flag = false;
            }
        }
        // 2.2 pid没在，新增,封装数据
        if (flag) {
            TCart tCart = new TCart();
            tCart.setCid(MyUtils.getCurrentTimeForCartId()); //使用工具生成购物车cid
            tCart.setUserId(userId);
            tCart.setPid(pid);
            tCart.setNum(num);
            tCart.setStatus((byte) 1); //记录状态. 1: 正常 ,0:禁用,-1：删除
            tCart.setCreateTime(new Date());
            tCart.setUpdateTime(new Date());
            // 得需要根据pid去获取pay_price价格
            TProduct product = tProductService.findByPid(pid);
            tCart.setPayPrice(product.getPrice());
            // 添加到数据库
            carts.add(tCart);
            cartService.insert(tCart);  //写入数据库
        }

        //3. 封装数据CartVo
        //carts = findCartList(userId);
        List<CartVO> cartVOList = new LinkedList<CartVO>();

        for (TCart cart : carts) {
            CartVO cartVO = new CartVO();

            cartVO.setCid(cart.getCid());
            cartVO.setUserId(cart.getUserId());
            cartVO.setPid(cart.getPid());
            cartVO.setPayPrice(cart.getPayPrice());
            cartVO.setNum(cart.getNum());
            cartVO.setPname(tProductService.findByPid(cart.getPid()).getPname());
            cartVOList.add(cartVO);
        }

        return ServerResponse.createBySuccess(cartVOList);
    }
```



完成上述的两个操作，获得两个接口

/cart/list?user_id=xxxxx  获取指定用户购物车数据

/cart/addCart?userId=10011&pid=10015&num=3    提交商品数据并返回CartVO



完成上述的步骤，实现了在商品详情页，点击商品添加到购物车的需求



```java
// 老师教材是这么写的，到时候看看为什么这么写

@RestController
public class CartController {

    @RequestMapping("cartlist")
    public List<CartVO> findCartList(Long userId){
        return null;
    }
}
```









#### 5.7.4 实现购物车页面及前后联调

```javascript
<template>
  <div>
    <Header></Header>
    <div class="g-container-width clearfix">
      <div class="cart-pannel">
        <div class="panel-heading">
          <div class="panel-title">购物车</div>
          <div>
            <button type="button" class="btn btn-lg" @click="delSelect()">批量删除</button>
          </div>
        </div>

        <div class="panel-body">
          <table class="table table-hover">
            <thead>
            <tr>
              <th>
                <input type="checkbox"
                       @click="selectAll()"
                       :checked="goods.length===allSelectedGoods.length&&goods.length">
              </th>
              <th>商品名称</th>
              <th>商品单价</th>
              <th>购买数量</th>
              <th>小计</th>
              <th>操作</th>
            </tr>
            </thead>

            <tr v-for="(v,k) in goods" v-show="goods.length!==0">
              <td>
                <input type="checkbox" @click="selectSingle(k)" :checked="allSelectedGoods.indexOf(v.cid)>=0">
              </td>
              <td style="width: 50% ;text-overflow:ellipsis">{{ v.pname }}</td>
              <td>￥{{ v.payPrice }}</td>
              <td>
                <button type="button" style="width:25px;text-align: center" @click="changeNum(k,v.num,-1)">
                  -
                </button>
                <input type="text" v-model="v.num" style="width:25px;text-align: center">
                <button type="button" style="width:25px;text-align: center" @click="changeNum(k,v.num,1)">
                  +
                </button>
              </td>
              <td>{{ v.num * v.payPrice }}</td>
              <td>
                <button type="button" class="btn btn-danger btn-xs" @click="del(k)">删除</button>
              </td>
            </tr>
          </table>

          <span v-show="goods.length===0" style="text-align: center">
            <h1>购物车为空</h1>
          </span>
        </div>
        <div class="panel-footer" style="text-align:right">
          共计￥<span>{{ totalPrice }}</span>元
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import Header from "../../components/Header";

export default {
  name: "Cart",
  components: {Header},
  data() {
    //商品数据
    return {
      goods: [
        {
          cid: 1,
          userId: '',
          pid: '',
          pname: '【详情页领券！到手价2099元起】Xiaomi/小米小米8年度旗舰全面屏骁',
          payPrice: 2299.00,
          num: 1
        },
      ],
      //控制全选
      allChecked: true,
      //存放被选择的数据
      allSelectedGoods: [],
      userId: this.$route.query.userId,
      pid: this.$route.query.pid,
      num: this.$route.query.num
    }
  },
  methods: {
    // 请求后台服务器，填充数据
    getCartData() {
      const vm = this;
      this.axios({
        method: 'get',
        url: 'http://localhost:8080/cart/addCart?userId=' + vm.userId + '&pid=' + vm.pid + '&num=' + vm.num
      }).then(function (resp) {
        vm.goods = resp.data.data;
      })

    },
    //删除购物车元素
    //删除是一定也要记得从allSelectGoods数组中删除对应的id
    del(k) {
      if (!confirm("确认删除吗")) {
        window.event.returnValue = false;
      } else {
        //如果该条信息已被选中
        if (this.allSelectedGoods.indexOf(this.goods[k].id) !== -1) {
          var index = this.allSelectedGoods.indexOf(this.goods[k].id);
          this.allSelectedGoods.splice(index, 1);
        }
        this.goods.splice(k, 1);
        console.log(this.allSelectedGoods)
      }
    },
    //增减数量
    changeNum(k, num, type) {
      //如果是减少，要判断>1，最少是1
      if (type === -1) {
        if (this.goods[k].num > 1) {
          this.goods[k].num = this.goods[k].num + type;
        }
      } else {
        this.goods[k].num = this.goods[k].num + type;
      }
    },
    //点击全选按钮
    selectAll() {
      //event.currentTarget.checked表示点击完后该选择框的状态
      if (!event.currentTarget.checked) {
        this.allSelectedGoods = [];
      } else {
        this.allSelectedGoods = [];//先置空，然后再重新添加，不然数组里会有重复！因为有可能点击全选之前已经选择了几个单选按钮。也就是数组里已经添加过了对应的id。
        this.goods.forEach((v, k) => {
          this.allSelectedGoods.push(v.cid)
        })
      }
      console.log(this.allSelectedGoods)
    },
    //点击单选按钮
    selectSingle(k) {
      if (event.currentTarget.checked) {
        this.allSelectedGoods.push(this.goods[k].cid)
      } else {
        for (var i = 0; i < this.allSelectedGoods.length; i++) {
          if (this.goods[k].cid === this.allSelectedGoods[i]) {
            this.allSelectedGoods.splice(i, 1);
            this.allChecked = false;
            break;
          }
        }
      }
      console.log(this.allSelectedGoods)
    },
    //批量删除
    delSelect() {
      if (confirm("确认删除" + this.allSelectedGoods.length + "条信息吗？")) {
        for (var i = this.goods.length - 1; i >= 0; i--) {
          if (this.allSelectedGoods.indexOf(this.goods[i].cid) !== -1) {
            //从allSelectedGoods数组中也需要删除
            var index = this.allSelectedGoods.indexOf(this.goods[i].cid);
            this.allSelectedGoods.splice(index, 1);
            //删除goods数组中的信息
            /*var index = this.goods.indexOf(v.id);*/
            this.goods.splice(i, 1);
          }
        }
        //这种写法是错误的，因为数组每次删除一条数据以后索引值会发生变化，所以用上述的倒叙删除，先删除索引大的数据
        /* this.goods.forEach((v, k) => {
             if (this.allSelectedGoods.indexOf(v.id)!==-1) {
             //从allSelectedGoods数组中也需要删除
                 var index = this.allSelectedGoods.indexOf(this.goods[k].id);
                 this.allSelectedGoods.splice(index,1);
             //删除goods数组中的信息
                /!*var index = this.goods.indexOf(v.id);*!/
                console.log(k);
                 this.goods.splice(k,1);
             }
         });*/
      }
      console.log(this.allSelectedGoods)
    }
  },
  computed: {
    //totalPrice计算总价
    totalPrice() {
      let totalprice = 0;
      //未加入选择框时的计算总价
      /*
          //方法一
        /!*  for(var i=0;i<this.goods.length;i++){
            totalprice += this.goods[i].num*this.goods[i].price
          }*!/
          //方法二
          this.goods.forEach( (v,k)=>{
            totalprice += v.num*v.price;
          });*/
      //加入选择框以后的计算总价
      this.goods.forEach((v, k,index) => {
        console.log("this.allSelectedGoods.indexOf(v.cid)>>"+this.allSelectedGoods.indexOf(v.cid))
        if (this.allSelectedGoods.indexOf(v.cid) !== -1) {
          totalprice += v.payPrice * v.num;
        }
      });
      return totalprice
    },
  },
  created() {
    this.getCartData();
  }
}
</script>
```



注意点：

1、vue中对于多选择处理

```javascript
// 1. 定义一个 allSelectedGoods[] 数组，里面放入被选中的商品pid 
//存放被选择的数据
allSelectedGoods: [],
// 2. 对于单个选择框的:checked属性，判断 allSelectedGoods[]数组里面是否有对应这条记录的pid
<input type="checkbox" @click="selectSingle(k)" :checked="allSelectedGoods.indexOf(v.pid)>=0">
//3 .对于全选 ，在表头放一个checkbox，通过判断goods的长度是否和allSelectedGoods[]两个数组长度一致，判断是否标记checked
:checked="goods.length===allSelectedGoods.length&&goods.length">
//4. 对于全选按钮，添加点击方法，先判断是否选中，如果checked是已选中，点击后就将allSelectedGoods数组清空，否则，就遍历goods表，把pid取出来放到
// allSelectedGoods
    selectAll() {
      //event.currentTarget.checked表示点击完后该选择框的状态
      if (!event.currentTarget.checked) {
        this.allSelectedGoods = [];
      } else {
        this.allSelectedGoods = [];//先置空，然后再重新添加，不然数组里会有重复！因为有可能点击全选之前已经选择了几个单选按钮。也就是数组里已经添加过了对应的id。
        this.goods.forEach((v, k) => {
          this.allSelectedGoods.push(v.pid)
        })
      }
    },
//5. 对于单选，方法逻辑是 如果当前checked是选中，就把这条记录的pid添加到 数组，否则就从数组去除掉
    selectSingle(k) {
      if (event.currentTarget.checked) {
        this.allSelectedGoods.push(this.goods[k].pid)
      } else {
        for (var i = 0; i < this.allSelectedGoods.length; i++) {
          if (this.goods[k].pid === this.allSelectedGoods[i]) {
            this.allSelectedGoods.splice(i, 1);  //将删除位于 index i就是古 的1个 元素
            this.allChecked = false;
            break;
          }
        }
      }
      console.log(this.allSelectedGoods)
    }

```

2、购物车对删除的处理

```javascript
// 1. 获取allSelectGoods长度，然后判断当前goods.pid是不是在allSelectGoods里面，在里面，就删掉allSelectGoods和good数组，    
delSelect() {
      if (confirm("确认删除" + this.allSelectedGoods.length + "条信息吗？")) {
        for (var i = this.goods.length - 1; i >= 0; i--) {
          if (this.allSelectedGoods.indexOf(this.goods[i].pid) !== -1) {
            //从allSelectedGoods数组中也需要删除
            var index = this.allSelectedGoods.indexOf(this.goods[i].pid);
            this.allSelectedGoods.splice(index, 1);
            //删除goods数组中的信息
            /*var index = this.goods.indexOf(v.id);*/
            this.goods.splice(i, 1);
          }
        }
      }
}
// 2. 单独删掉一条 K是key {cid:1111} 里面的cid  v-for=“(v,k ) in goods” 因为goods是数组，所以k是index，v是对应index的值
    del(k) {
      if (!confirm("确认删除吗")) {
        window.event.returnValue = false;
      } else {
        //如果该条信息已被选中
        if (this.allSelectedGoods.indexOf(this.goods[k].pid) !== -1) {
          var index = this.allSelectedGoods.indexOf(this.goods[k].pid);
          this.allSelectedGoods.splice(index, 1);
        }
        this.goods.splice(k, 1);
        console.log(this.allSelectedGoods)
      }
    }
```







####  5.7.5 知识点：swagger使用

我们需要向前端程序员提供接口文档，我们可以用word，

接口文档中需要有，

​	请求方式  get post put delete 

​	接口地址：(url) 请求参数(参数名、参数类型)  

​	返回对象的格式  写清自己返回的是{usernmae;''  ,password:''  }  或者 数组[]



方法二：使用showdoc.cc 在线编辑我们的接口文档 

[showdoc.cc] (https://www.showdoc.cc)







### 6 提交订单

前面完成了购物车的相关信息，接下来需要实现提交订单

提交订单还未涉及到支付

我们观察以下TOrder数据库，发现如下的字段

![image-20210313224814355](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210313224814355.png)

其中有些东西需要前端传过来，

根据我们之前微信支付的TOrderVO，我们知道一个TOrderVO包含有TProducts和TOrder，所以我们需要封装数据，整体NS流程图如下

![image-20210313225219430](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210313225219430.png)

`TOrder表数据`

【id  order_id  u**ser_id 前端** **order_price 前端** order_flag **order_user 前端** **order_tel前端**  **order_addr前端** create_time update_time】

`TProduct集合`

只需要前端把对应的pid发过来就行

#### 6.1 前端页面

1。如何获得被选中的行的数据?

```
@selection-change="handleSelectionChange">


    handleSelectionChange(val) {
      console.log(val)
      this.allSelectedGoods = val;
      this.multipleSelection = val;
    },
```

2. 如何计算出来订单的总价格

```javascript
handleSelectionChange(val) {
  console.log(val)
  this.multipleSelection = val;

  this.multipleSelection.forEach(row=>{
    this.totalMoney += row.payPrice*row.num;
  })
},
```

3、订单中是需要有联系人、收件地址，收件电话的，我们用表单完成

```javascript
<!--收货人信息-->
<div>
  <h3>收货信息</h3>
  <el-form :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
    <el-form-item label="收件人">
      <el-input v-model="orderUser"></el-input>
    </el-form-item>

    <el-form-item label="联系人电话" >
      <el-input v-model="orderTel"></el-input>
    </el-form-item>

    <el-form-item label="收件地址" >
      <el-input v-model="orderAddr"></el-input>
    </el-form-item>
  </el-form>
</div>

  data() {
    //商品数据
    return {
      goods: [
        {
          cid: 1,
          userId: '',
          pid: '',
          pname: '【详情页领券！到手价2099元起】Xiaomi/小米小米8年度旗舰全面屏骁',
          payPrice: 2299.00,
          num: 1
        },
      ],


      //控制全选
      allChecked: true,
      //存放被选择的数据
      allSelectedGoods: [],
      userId: this.$route.query.userId,
      pid: this.$route.query.pid,
      num: this.$route.query.num,

      totalMoney:0,
      multipleSelection: [],

      orderUser:'',
      orderTel:'',
      orderAddr:'',
    }
  },
```

4、前端需要包装出来一个Tproeduct列表

思考：因为每次选择框变更后，都会让multipleSelection更新，所以我们可以如下实现

```javascript
    //提交订单,
    submitOrder() {
      //1. 封装OrderVo数据，根据后台的OrderVO，TOrder和TProduct两个实例
      var orderData = {
        userId: this.userId,
        orderPrice: this.totalMoney,
        orderUser: this.orderUser,
        orderTel: this.orderTel,
        orderAddr: this.orderAddr,
        pids:[]
      }
      // 2. 获取选中的每一行的商品，封装Tproduct
      this.multipleSelection.forEach((row,index) => {
        orderData.pids[index] = row.pid;
      })
      //console.log(orderData);
      // 3. 向后台发送数据 使用axios
      
    },
```

说明：

1、row是view,index是索引，所以我们可以遍历一下，让这些pid放到pids里面



5、我们使用axios的post方法，有两套，需要和后端开发进行配套使用

方法1): 如果我们使用axios。post,此时默认发送的就是json格式，此时后台接收参数形式：public void addOrder(@RequestBody OrderDTO orderDTO)

方法2)如果我们使用axios，使用transformRequest将格式转换一下  此时后台接收参数的形式 public void addOrder(OrderDTO orderDTO)

```javascript
import Qs from 'qs';
// 3. 向后台发送数据 使用axios
this.axios({
  method:'post',
  url:'http://localhost:8080/order/addOrder',
  /** 默认axios发送的是json格式,我们需要把格式转换一下 application/x-www-form-urlencode */
  transformRequest: [function (data) {
    return Qs.stringify(data)
  }],
  data: orderData,
})
```





#### 6.2 后端接口设计

前端提交数据给后端接口，后端将订单更新到数据库，

`DTO` 

编写后端接口来获取前端请求的数据时，发现前端请求的数据有很多个参数，因此可以提供一个DTO，来封装前端提交的多个数据

DTO   data transfer object 数据传输对象



1、我么设计一个OrderDTO, 这个用来存放前端提交订单时候的参数，

```java
public class OrderDTO {
    private Long userId;
    private Long orderPrice;
    private String orderUser;
    private String orderTel;
    private String orderAddr;
    private List<Long> pids;
}
```



2、根据DTO我们设计OrderController接口，负责接收前端请求的数据

```java
@RestController
@RequestMapping("order")
public class OrderController {

    /**
     * @RequestBody
     * @PathVariable
     * @RequestParam
     *
     * @param orderDTO
     */
    @RequestMapping(value = "addOrder",method = RequestMethod.POST)
    public void addOrder(OrderDTO orderDTO){
        System.out.println(orderDTO);
    }
}
```

通过打印出来的结果，可以发现数据已经被正确的封装进入了OrderDTO中

3、接下来，把orderDTO封装到OrderVO和OrderItem中，这种工作在controller是不适合的，因为这是业务逻辑相关，我们需要把这些业务放到Service中

提交订单分两方面工作，一个是写入Order数据库，一个是写入OrderItem数据库，

根据之前的逻辑，已经封装出来了 OrderVO, OrderVO包含Torder和TProducts,

我们计划通过调用 addOrder(orderVo)实现添加订单，所以接下来流程，整体上应该是，

根据OrderDTO数据封装出来 Order

根据OrderDTO数据封装出来TProducts 数据



3.1 封装OrderVO中需要的 TOrder数据

```java
public void addOrder(OrderDTO orderDTO) {
    //1. 根据OrderDTO封装orderVO
    //1.1 封装OrderVO中的TOrder数据
    OrderVO orderVO = new OrderVO();

    TOrder tOrder = new TOrder();
    tOrder.setOrderId(MyUtils.getCurrentTimeForCartId());
    tOrder.setUserId(orderDTO.getUserId());
    tOrder.setOrderPrice(orderDTO.getOrderPrice());
    tOrder.setOrderUser(orderDTO.getOrderUser());
    tOrder.setOrderTel(orderDTO.getOrderTel());
    tOrder.setOrderAddr(orderDTO.getOrderAddr());
    tOrder.setOrderFlag((short)1);
    tOrder.setCreateTime(new Date());
    tOrder.setUpdateTime(new Date());
    // 1.2 放入到OrderVO
    orderVO.settOrder(tOrder);
}
```

3.2 封装OrderVo中需要的List<TProduct>

`step 1. 完善TProductMapper的接口和mapper`

```java
public interface TProductMapper {
   /**
     * 根据传入的pids列表查出来全部的TProduct
     * @param pids  商品pid集合
     * @return TProduct列表
     */
    List<TProduct> findProductListByPids(List<Long> pids);
}
```

`step 2. 完善我们的TProductMapper.xml的映射文件`

```xml
  <!--根据传入的pids列表查找全部的-->
  <select id="findProductListByPids" resultMap="BaseResultMap">
    select
    <include refid="Base_Column_List" />
    from t_product
    where pid in (
    <foreach item="pid" collection="list"  separator=",">
      #{pid, jdbcType=BIGINT}
    </foreach>
    )
  </select>
```



`step 3、在OrderServiceImpl注入 TProductMapper，并加入接口`

```java
public class OrderServiceImpl implements IOrderService {

    @Autowired
    private TOrderMapper orderMapper;

    @Autowired
    private IOrderInfoService orderInfoService;

    @Autowired
    private TProductMapper productMapper;
    
    
     public void addOrder(OrderDTO orderDTO) {
        //1. 根据OrderDTO封装orderVO
        //1.1 封装OrderVO中的TOrder数据
        OrderVO orderVO = new OrderVO();

        TOrder tOrder = new TOrder();
        tOrder.setOrderId(MyUtils.getCurrentTimeForCartId());
        tOrder.setUserId(orderDTO.getUserId());
        tOrder.setOrderPrice(orderDTO.getOrderPrice());
        tOrder.setOrderUser(orderDTO.getOrderUser());
        tOrder.setOrderTel(orderDTO.getOrderTel());
        tOrder.setOrderAddr(orderDTO.getOrderAddr());
        tOrder.setOrderFlag((short)1);
        tOrder.setCreateTime(new Date());
        tOrder.setUpdateTime(new Date());
        // 1.2 放入到OrderVO
        orderVO.settOrder(tOrder);

        // 2. 封装OrderVO中的ProductList
        List<TProduct> products = new LinkedList<TProduct>();
        List<Long> pids = orderDTO.getPids();

        // 2.1根据pids获取响应的商品集合
        products = productMapper.findProductListByPids(pids);

        orderVO.setProducts(products);

        //3.===========orderVO封装完毕，进行插入
        addOrder(orderVO);
        // 4. 对结果进行处理
    }
}
```

这样我们就完成了当用户从购物车提交订单，把商品写入到TOrder和TOrderItem的功能



#### 6.3.封装统一返回结果，

在真实项目中，为了和前端沟通更加效率，我们一般是将结果封装到ServerResponse中，这个类一般包含 status,msg及data

step 1、编写一个工具类，统一服务器返回类

```java
@JsonInclude(JsonInclude.Include.NON_NULL)

public class ServerResponse<T> implements Serializable {
    private int status; //响应状态码
    private String msg; //响应消息
    private T data; //请求的数据

    // 构造器,状态码
    private ServerResponse(int status) {
        this.status = status;
    }

    // 构造器 状态码和消息
    private ServerResponse(int status, String msg) {
        this.status = status;
        this.msg = msg;
    }

    //构造器  状态码和数据
    private ServerResponse(int status, T data) {
        this.status = status;
        this.data = data;
    }

    //构造器 状态码+消息+数据
    private ServerResponse(int status, String msg, T data) {
        this.status = status;
        this.msg = this.msg;
        this.data = data;
    }
    //    使之不在JSON序列化结果当中
    @JsonIgnore
    // 可以快速进行成功与否的条件判断
    public boolean isSuccess() {
        return this.status == ResponseCode.SUCCESS.getCode();
    }

    @JsonIgnore
    // 可以快速进行成功与否的条件判断，判断false时，不用加!。囧
    public boolean isFail() {
        return this.status != ResponseCode.SUCCESS.getCode();
    }

    public int getStatus() {
        return status;
    }

    public String getMsg() {
        return msg;
    }

    public T getData() {
        return data;
    }

    // 快速构建返回结果,成功时的调用
    public static <T> ServerResponse<T> createBySuccess() {
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode());
    }

    public static <T> ServerResponse<T> createBySuccessMessage(String msg) {
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode(), msg);
    }

    public static <T> ServerResponse<T> createBySuccess(T data) {
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode(), data);
    }

    // 快速构建返回结果,失败时的调用
    public static <T> ServerResponse<T> createBySuccess(String msg, T data) {
        return new ServerResponse<T>(ResponseCode.SUCCESS.getCode(), msg, data);
    }


    public static <T> ServerResponse<T> createByError() {
        return new ServerResponse<T>(ResponseCode.ERROR.getCode(), ResponseCode.ERROR.getDesc());
    }


    public static <T> ServerResponse<T> createByErrorMessage(String errorMessage) {
        return new ServerResponse<T>(ResponseCode.ERROR.getCode(), errorMessage);
    }

    public static <T> ServerResponse<T> createByErrorCodeMessage(int errorCode, String errorMessage) {
        return new ServerResponse<T>(errorCode, errorMessage);
    }
}
```

step 2、完善OrderController、OrderServiceImpl, 

```java
//OrderServiceImpl
public ServerResponse addOrder(OrderDTO orderDTO)
// Controller
public ServerResponse addOrder(OrderDTO orderDTO)

```

step 3  前端对结果进行处理，当结果正确跳转到订单详情页面

```javascript
submitOrder() {
  //1. 封装OrderVo数据，根据后台的OrderVO，TOrder和TProduct两个实例
  var orderData = {
    userId: this.userId,
    orderPrice: this.totalMoney,
    orderUser: this.orderUser,
    orderTel: this.orderTel,
    orderAddr: this.orderAddr,
    pids:[]
  }
  // 2. 获取选中的每一行的商品，封装Tproduct
  this.multipleSelection.forEach((row,index) => {
    orderData.pids[index] = row.pid;
  })
  console.log(orderData);
  // 3. 向后台发送数据 使用axios
  var vm = this;
  this.axios({
    method:'post',
    url:'http://localhost:8080/order/addOrder',
    /** 默认axios发送的是json格式,我们需要把格式转换一下 application/x-www-form-urlencode */
    transformRequest: [function (data) {
      return Qs.stringify(data)
    }],
    data: orderData,
  }).then(function (resp){
      if (resp.data.status===0){
          vm.$message({
            message:resp.data.msg,
            type:'success'
       });
      // 定时调到一个页面
      setTimeout(function (){
        vm.$router.push("/orderDetail");
      },2000);

    }else {
      vm.$message.error(resp.data.msg);
    }
  })
},
```







实现登录功能

思考 如何实现既可以邮箱 又可以用户名 又可以手机号，： 后台sql模糊查询



登录成功后的处理-拦截器

先记住登录状态， 使用sessionStorage实现前端浏览器的数据存储，模拟后端的session技术

sessionStorage.setItem("isLogin", "true");



判断用户是否登录

修改Login.vue





修改main.js

利用理由狗子BeforeEach方法判断用户是否登录，关键代码如下

```javascript
//在跳转钱执行 to 要去的地方，from 来自哪儿  next 下一个去到那里，放行到下一个过滤器
router.beforeEach((to,from,next)=>{
  //获取用户的登录状态
  let isLogin = sessionStorage.getItem("isLogin");
  //注销
  if (to.path=='/logout'){
    //清空
    sessionStorage.clear();
    //跳转到登录页
    next({path:'/login'});
  }
  //如果请求是登录页
  else if (to.path=='/login'){
    if (isLogin!=null){
      //跳转到首页
      next({path:'/main'});
    }

  }
  //如果不是登录状态
  else if (isLogin==null){
    //跳转到登录页
    next({path:'/login'})
  }

  //下一个路由 放行过滤器
  next();

})
```







### 7. 用户登录



#### 7.1 前端页面

step 1、编写Login组件

```javascript
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
          {required: true, message: '账号不可为空', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '密码不可为空', trigger: 'blur'}
        ]
      },
      //对话框显示和隐藏
      dialogVisible: false
    }
  },
  methods: {
    onSubmit(formName) {
      // 为表单绑定验证功能
      this.$refs[formName].validate((valid) => {
        if (valid) {
          //使用vue-router路由到指定页面，该方式称之为编程式导航
          this.$router.push("/main");
        } else {
          this.dialogVisible = true;
          return false
        }
      });
    },
    handleClose: function () {
      console.log("Handle Close，空函数");
    }

  }
}
</script>

<style scoped>

.login-box {
  border: 1px solid #dcdfe6;
  width: 350px;
  margin: 180px auto;
  padding: 35px 35px 15px 35px;
  border-radius: 5px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  box-shadow: 0 0 25px #909399;
}

.login-title {
  text-align: center;
  margin: 0 auto 40px auto;
  color: #303133;
}
</style>
```

step 2、进入router/index.js 配置路由

```
{path: '/login',name: 'Login',component: Login} //登录页
```

这样前端页面的样式就完成了



#### 7.2 后端接口实现登录

 1、模糊查询实现 手机号、邮箱、登录的实现，后台sql模糊查询实现

step 1、设计UserController, 增加login方法

```java
@Autowired
private UserService userService;

@RequestMapping(value = "/login", method = RequestMethod.GET)
public ServerResponse login(String userTel, String userPwd) {

    return userService.login(userTel,userPwd);

}
```

step 2、设计UserServiceImpl

```java
@Service("userService")
public class UserServiceImpl implements UserService {
    // 操作user表，是狂神教学使用的user表
    @Autowired
    private UserMapper userMapper;
    // 操作t_user表，是千峰教学使用的user表
    @Autowired
    private TUserMapper tUserMapper;

    public List<User> selectAllUser() {
        return userMapper.selectAllUser();
    }

    public ServerResponse login(String userTel, String userPwd) {
        // 因为需要向tUserMapper传递两个参数，因此把参数存入到MAP中
        Map<String, String> map = new HashMap<String, String>();
        map.put("userTel",userTel);
        map.put("userPwd",userPwd);

        TUser user = tUserMapper.findByTelAndPwd(map);
        // 判断user是否为空，来生成结果
        if (user!=null){
            return ServerResponse.createBySuccess(user);
        }else {
            return ServerResponse.createByErrorMessage("用户不存在");
        }
    }
}
```

注意：

1、如果我们需要Mapper执行多个查询，就需要使用Map来进行传递参数



step 3、完善TUserMapper接口

```java
public interface TUserMapper {
    int deleteByPrimaryKey(Long id);

    int insert(TUser record);

    int insertSelective(TUser record);

    TUser selectByPrimaryKey(Long id);

    int updateByPrimaryKeySelective(TUser record);

    int updateByPrimaryKey(TUser record);

    TUser findByTelAndPwd(Map<String, String> map);
}
```

step 4 ，进入TUserMapper.xml，编写findByTelAndPwd 的sql脚本

```xml
  <!--findByTelAndPwd-->
  <select id="findByTelAndPwd" resultMap="BaseResultMap">
    select
    <include refid="Base_Column_List" />
    from t_user
    where user_tel = #{userTel,jdbcType=VARCHAR} and user_pwd=#{userPwd,jdbcType=VARCHAR}
  </select>
```



这样我们就实现了后端接口，可以通过访问，http://localhost:8080/user/login?userTel=13801268603&userPwd=123456来获得对应的值

![image-20210315190239528](F:\MD格式学习笔记库\千锋教育-VUE高阶学习笔记.assets\image-20210315190239528.png)

#### 7.3 前后联调

step 1、进入到前端login.vue组件，完善axios请求，对后端返回数据进行处理

```javascript
onSubmit(formName) {
  // 为表单绑定验证功能
  var vm = this;
  this.$refs[formName].validate((valid) => {
    if (valid) {
      // 表单验证成功，向后台发送登录请求
      this.axios({
        method:'get',
        url:'http://localhost:8080/user/login?userTel='+vm.form.user_tel+'&userPwd='+vm.form.user_pwd
      }).then(function (resp){
          if (resp.data.status===0){
            console.log(resp.data.data)
            vm.$router.push("/home");
          }else {
            console.log(resp.data.msg)
            vm.$router.push("/login")
          }

      })
      //使用vue-router路由到指定页面，该方式称之为编程式导航

    } else {
      this.dialogVisible = true;
      return false
    }
  });
},
```



step 2、我们还可以使用表单验证，通过vue对前端表单数据进行验证后提交

```javascript
// 1。在表单里面写上:rules，绑定rules属性
<el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">

//2 .在需要验证的字段，加上prop
 <el-form-item label="电话" prop="user_tel">
//3 .在 data()方法中写上rules规则
      //表单验证，需要在el-form-item元素增加prop属性
      rules: {
        user_tel: [
          {required: true, message: '账号不可为空', trigger: 'blur'}
        ],
        user_pwd: [
          {required: true, message: '密码不可为空', trigger: 'blur'}
        ]
      },
//4. 调用表单验证
this.$refs[formName].validate((valid) => {
if (valid) {
    // 通过干的事
} 
else{
	//没通过干的事儿
    return false
}
```



这样，当用户登录后就跳转进入到home页面





### 5.8 sessionStorage



登录完成后，我们就需要记住用户的登录信息，这个时候我们在vue使用sessonStorate处理，使用的方式步骤时。



#### 5.8.1 记录session

通过sessionStorage.setItem("isLogin","true");  对登录状态进行保存

```java
this.axios({
            method:'get',
            url:'http://localhost:8080/user/login?userTel='+vm.form.user_tel+'&userPwd='+vm.form.user_pwd
          }).then(function (resp){
              if (resp.data.status===0){
                sessionStorage.setItem("isLogin","true");

                console.log(resp.data.data)
                //使用vue-router路由到指定页面，该方式称之为编程式导航
                vm.$router.push("/home");
              }else {
                console.log(resp.data.msg)
                vm.$router.push("/login")
              }

})
```



#### 5.8.2 修改main.js

```javascript
// 在跳转前执行
router.beforeEach((to,from,next)=>{
  // 获取用户登录状态
  let isLogin = sessionStorage.getItem('isLogin');
  //注销
  if (to.path == '/logout'){
    //清空
    sessionStorage.clear()
    //跳转到登录
    next({path:'/login'});
  }

  //如果请求的是登录页
  else if (to.path =='/login'){
    if (isLogin!=null){
      //跳转到首页
      next({path:'/home'});
    }
  }
  // 如果是非登录状态
  else if (isLogin == null){
    //跳转到登录页
    next({path:'/login'});
  }
  //下一个路由
  next();
})
```

