# JavaScript最新教程

## 1. JavaScript简介

### 1.1 历史

JavaScript是一门世界上最流行的脚本语言! 简单的来说

Java 、JavaScript  这个作者用了10天就给开发出来了，工程师是网景公司的员工

**== 一个合格的后端人员，必须精通JavaScript==**

(关于历史)[https://blog.csdn.net/kese7952/article/details/79357868] 



ECMAScript  可以理解为javaScript的一个标准，最新版本是ECMAScript 6, 但是大部分浏览器还停留在支持es5代码上，开发环境和生成线上环境版本不一致。

可能写的代码在线上不能用。



关键字、变量、闭包等，有兴趣的同学可以去查查为什么叫javaScript



### 1.2 快速入门

> 内部标签
>
> ```
>     <!--script标签，写javascript脚本-->
>     <!--直接引入,直接写入脚本-->
>     <script>
>         //这是一个注释，和Java是一样的
>         alert("Hello,World!");
>     </script>
> ```

> 2.外部引入
>
> abs.js
>
> ```
> <!--引入方式2 外部引入，必须这里script必须成对出现-->
> <script src="js/abs.js"></script>
> <!--不是必须要声明type类型-->
> <script type="text/javascript"></script>
> ```



示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--script标签，写javascript脚本-->
    <!--直接写入脚本-->
    <script>
        alert("Hello,World!");
    </script>
    <!--引入方式2 外部引入-->
    <script src="js/qj.js"></script>
</head>
<body>
    <input type="button">
</body>
</html>
```



## 2. 基本语法

### 2.1 定义变量

```javascript
//定义变量  变量类型 变量名 = 变量值;
var num =1;
var name = "秦僵";
var msg = "hello,world java!";
alert(msg);
```

### 2.2 基本条件控制

```javascript
//2.条件控制
var score = 90;

if(score>60&&score<70){
    alert("你及格了");
}else if (score>70&&score<80) {
    alert("你是中技生");
}else if (score>=90&&score<=100){
    alert("你好牛逼啊");
}
```

> ```
> /*JavaScript严格区分大小写*/
> 
> ```
>
> 在浏览器调试打印js的方法
>
> console {debug: *ƒ*, error: *ƒ*, info: *ƒ*, log: *ƒ*, warn: *ƒ*, …}
>
> console.log(score) 	//在console控制台打印变量



### 2.3 浏览器必备调试js

​	0.Elements 页面元素，一般用来扒网站

​	1.console 控制台 调试使用	比如可以使用console.log("测试输出")，就可以在控制台输出语句，类似System.out.print()

	2. source 源代码  打断点使用,可以做调试，类似ide的debug
![image-20201114102214913](F:\MD格式学习笔记库\狂神JAVA-10-JavaScript学习.assets\image-20201114102214913.png)

   	1. network  查看网络用
            	2. performance
         	3. application   应用
                	4. memory
               	5. Security
                      	6. Audits

### 2.4 数据类型

数值，文本，图形，音频，视频....都属于数据类型

数值 number  js不区分小数和整数

```javascript
/*数值表示*/
123 //定义整数123
123.10 //浮点数123.1
1.123e4	//科学计数法
Nan  //not a number
Infinity //无限大的数
/*字符串*/
'abc',"abc"
/*布尔值*/
true  false
/*l逻辑运算符*/
&& || ！
/*比较运算符*/

=
== 等于，类型不一样，值一样，也会判断为true  例如 1和“1” 在js使用==会判断为真
=== 等于 类型一样，值一样，才会判断为true
须知：
	Nan===Nan  这个与所有的数值都不相等，包括自己
    只能通过isNaN(NaN)来判断这个数是否为NaN
    浮点数问题 console.log((1/3)===(1-3/3))	不相等，尽量避免使用浮点数进行运算，存在精度的问题!
	可以使用绝对值进行判断 console.log(Math.abs(1/3-(1-2/3))<0.00001)  结果是true
/*其他*/
null	空的意思
undefined	未定义

/*数组*/
var arr =[1,2,3,4,5,'hello',null, true]
arr
[1, 2, 3, 4, 5, "hello", null, true];
new Array(1,2,3,4,5,'hello',null, true);
arr[1];	//保证代码可读性，尽量使用[]
java的数值必须是相同类型的~js中不需要这样!
取数组下标，如果越界了，就会 undefined

/*对象*/

数组是中括号，对象是大括号
//Person person = new Person(1,3,4,5,6) 在java使用方法
var person = {
    name:"秦僵",
    age:3,
    sex:"男",
    tags:['js','java','web','.....']
}

每个属性使用,号隔开，最后一个不需要使用,
取对象的值	：	console.log(person.name)

/*	变量	*/
不能使用数字开头定义变量定义，其他和java相同


```



### 2.5 严格检查模式

'use strict'  严格检查模式，必须卸载js的第一行，预防javascript的随意定义导致产生一些问题

局部变量 let，必须  IDEA需要设置支持ES6语法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>

        // 'use strict'  严格检查模式，必须卸载js的第一行，预防javascript的随意定义导致产生一些问题
        //局部变量 let，必须  IDEA需要设置支持ES6语法
        'use strict'
        //全局变量
         let i = 1;
        //ES6  局部变量使用let定义
    </script>
</head>
<body>
    测试严格检查模式
</body>
</html>
```



## 3.数据类型

### 3.1 字符串及方法

- 正常字符串我们使用单引号或者双引号包裹

- 注意转义字符 \  例如console.log("b\.\'a");  \' \n \t \r \u4e2d  例如  console.log('\u4e2d')

- 多行字符，使用tab上面那个符号

```javascript
var msg = `你好 你叫是么么 
你叫什么名字`
```

- 模板字符串,

```javascript
let name="qinjiang";
let age = 3;

let msg = `你好呀，${name} ,${age}`
```

- 字符串长度

```
var student ="student";
console.log(student.length)
console.log(student.index)
console.log(student.substring(1)) //从第一个字符串截取到最后一个字符串
console.log(student.substring(1，3)) //从第一个字符串截取到第三个字符串  //[1,3)
```

### 3.2 数组

Array可以包含任意的数据类型

> ​	var arr=[1,2,3,4,5,6]

长度

> console.log(arr.length)

注意.假如给array.length赋值，数组大小就会发生变化~ 如果赋值过小，元素就会丢失，如果赋值过大，就会添加几个空的元素

通过元素获得下标索引，indexOf

> ​	arr.indexOf(2)	
>
> var arr=[1,2,3,4,5,6,"1","2"]
>
> arr.indexOf(1)	 //0
>
> arr.indexOf("1")  //6
>
> ​	字符串的1和数字1是不同的

slice() 截取Array的一部分，返回一个新的数组,类似于String的substring

> 	arr.slice(3)
> (7) [4, 5, 6, empty × 4]
> arr.slice(3,7)
> (4) [4, 5, 6, empty]

数组丢值	使用push pop，尾部弹出压入元素

push: 压入到尾部

pop：弹出尾部的一个元素

```javascript
arr = [0, 2, 3, 4, 5, 6, "1","b"]
(8) [0, 2, 3, 4, 5, 6, "1", "b"]
arr
(8) [0, 2, 3, 4, 5, 6, "1", "b"]
arr.push('c',"d");
10
arr
(10) [0, 2, 3, 4, 5, 6, "1", "b", "c", "d"]
arr.pop()
"d"
arr
(9) [0, 2, 3, 4, 5, 6, "1", "b", "c"]
```

unshift()  shift() 头部弹出压入元素

unshift: 压入到头部

shift：弹出头部的一个元素



排序 array.sort()  

> arr.sort()
> (9) [0, "1", 2, 3, 4, 5, 6, "b", "c"]

元素反转  array.reverse()  

> arr.reverse()
> (9) ["c", "b", 6, 5, 4, 3, 2, "1", 0]



### 3.3 对象

若干个键值对,对象的定义方式如下：

```javascript
var 对象名 = {
	属性名:属性值，
	属性名:属性值，
	属性名:属性值，
	属性名:属性值，
}
var person = {
name:"张萨姆",
age:100,
sex:"男",
email:"2324242@qq.com",
score:0
}
```

js中对象 {.......}表示一个对象，键值对描述属性 xxxx:xxxx，多个属性之间使用逗号隔开，最后一个属性不用加, 



1.对象赋值

```javascript
person.name="罗岭峰"
"罗岭峰"
person.name
"罗岭峰"
person
{name: "罗岭峰", age: 100, sex: "男", email: "2324242@qq.com", score: 0}
```



2、使用一个不存在的对象属性，是不会报错的

```javascript
person.haha
undefined
```

3、动态的添加、删减属性

```javascript
person
{name: "罗岭峰", age: 100, sex: "男", email: "2324242@qq.com", score: 0}
person.haha
undefined
delete person.name
添加
person.hah="haha"
"haha"
person
{age: 100, sex: "男", email: "2324242@qq.com", score: 0, name: "罗岭峰", …}

```

4.判断属性值是否在这个对象中! xxx in xxxx!

```javascript
javascript中的所有的键都是字符串，值是任意对象!
   'age' in person
true
toString' in person
true
```

5. 查看是否有对象

>  person.hasOwnProperty('age')
>
> true

### 3.4 流程控制

if语句

```javascript
var age =3;
if (age>3){//第一个判断
    console.log("haha");
}else if(age<4) {//第二个判断
    console.log("kuwa");
}else {//否则
    console.log("kuwa");
}
```

while 语句，避免死循环

```javascript
while (age<100){
    age+=1;
    console.log("age-->"+age);
}


do{age+=1;
    console.log("age-->"+age);

}  while (age<100)
```

for 循环，

```javascript
for (let i = 0; i < 100; i++) {
    console.log("age-->"+age+i)
}
```

数组循环

```javascript
demo = [1,2,3,4,5,6,7,8,9,10]
demo.forEach(function (value) {
    console.log(value)
})


方式2：
// for (Type str: Elements){} //java方式

// for(var num in age){}  //javascript方式

//for(var index in object)  使用的是索引
for (var num in demo){
    console.log("for in 方式遍历数组-->"+num);
}
方式3：
for of 可以适用 数组、字符串、Map、Set对象
for(var chr of uniquewords){ console.log(chr)}
```

### 3.5 Map和Set

这两个在ES6才出现，要解决的问题

例如 我既想统计学生成绩，又想统计学生的名字

**Map:**

var names = ['tom','jack','haha'];

var scores = [100,90,80];

为了解决这个查询，所以就提出了Map，这个类似python的字典

```javascript
var map = new Map([['tom', 100], ['jack', 90], ['tom', 80]]);
var name = map.get('tom');
console.log("tom的成绩是:" + map.get('tom')) //通过key获得value
map.set('admin','123456')

```

**Set 是一个无序不重复集合**

```javascript
var set = new Set([3,1,1,1,1]);//set 可以去重
set.add(2);
set.delete(2);

>>>>>>console
set
Set(2) {3, 1}
set.add(7)  //添加!
Set(3) {3, 1, 7}
set.delete(7)  //删除!
true
set
Set(2) {3, 1} 

console.log(set.has(3)) //是否包含元素
```



**Iterator**

通过for of 我们可以实现打印出来具体数组的值，ES6的新特性

```javascript
var arr2 = [1,2,3,4,5]
for (let ar of arr2) {
    console.log("for.... of...-->"+ar);
}
```

也可以实现map和set的迭代遍历

```javascript
var map2 = new Map([['tom',100],['jack',100],['haha',100]])

for (let x of map2) {
    console.log("map--->"+x)
}

>>>>>>
map--->tom,100
map--->jack,100
map--->haha,100
```



## 4.函数及面向对象

方法： 对象里面，(属性，方法)

函数:  方法的本质还是函数，放在对象里面就是方法

### 4.1 定义函数

> **定义方式1**

```javascript
public 返回值类型 方法名(参数列表){return 返回值}

//定义一个绝对值函数

function abs(x){
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}
```

一旦执行到return代表函数结束，返回结果!

如果没有执行retrun,函数执行完也会返回结果，结果就是undefined

> **定义方式二  这是一种匿名函数定义方式，但是可以把结果赋值给前面的变量，通过abs2就可以进行调用!**

```javascript
var abs2 = function (x) {
    if (x>0){
        return x;
    }else {
        return -x;
    }
}
```

>  **调用函数**

直接调用就行 

```javascript
abs(10) //10
abs(-10) //10
```

参数问题:JavaScript可以传递任意个参数，也可以不传递参数~

参数进来之后是否存在的问题？

假设不存在参数，那如何来规避？

```javascript
abs(-10,123,123,123123,123123,312)  //10
```

> **arguments()**

`arguments` 是js免费赠送的关键字，代表传递进来的所有参数，是一个数组, 利用arguments我们可以拿到所有的参数.

```javascript
var abs3 = function (x){
    console.log("x--->"+x);
    for (let i = 0; i < arguments.length; i++) {
        console.log("arguments-->"+arguments[i]);
    }
    if (x>0){
        return x;
    }else {
        return -x;
    }
}
>>>>输出结果

abs3(13123,1231231,3123123,12312312,32131231,321321312,312312312,312312312,312312312,3123123)
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:24 x--->13123
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 arguments-->13123
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 arguments-->1231231
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 arguments-->3123123
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 arguments-->12312312
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 arguments-->32131231
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 arguments-->321321312
3定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 arguments-->312312312
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:26 
```

问题：arguments包含所有的参数，我们有时候想使用多余的参数来进行附加操作，需要排除已有的参数~

> **rest参数，在es6引入，接收参数之外的参数**

```javascript
var abs4 = function (x,y,...rest){
    console.log("x->"+x);
    console.log("y->"+y);
    console.log("rest->"+rest);
}
>>>>
abs4(10,20,10,20,20)
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:36 x->10
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:37 y->20
定义函数.html?_ijt=6cq4ea7994jbep8e299a5ndnbb:38 rest->10,20,20    
```

rest参数只能卸载最后面的参数，必须用...标识，就是抄袭java的可变长参数

### 4.2 变量的作用域

在javascript中，var定义的变量实际是有作用域的。假设在函数体内声明，则在函数体外不可使用~（闭包）

```javascript
<script>
    'use strict'
function qj() {
    var x =1;
    x=x+1;
}
x= x+2;//Uncaught ReferenceError  x is not defined
</script>
```

> 内部函数可以访问外部函数的成员，反之则不行

```javascript
function outerfun() {
    var x =1;
    function innerfun() {
        var x='a';
        console.log("inner-->"+x);
    }
    console.log("outer-->"+x);
    innerfun();
}
```

> 假设在javaScript中，函数查找变量从自身函数开始~由"内"到"外"查找, 假设外部存在这个同名的函数变量，则内部函数会屏蔽外部函数的变量。

`提升变量作用域，s所以我们都是在函数最前面进行变量声明`

```javascript
function qj5() {
    var x = "x"+y;
    console.log(x);
    var y = 'y';
}  //x undefined
```

> 全局函数

定义在脚本起始,定义的就是全局变量

```javascript
    var dem= 5;
    function demf() {
        console.log(dem);
    }
    demf();
    console.log(dem);
</script>
```

JavaScript实际上只有一个全局作用域，任何变量(函数也可以视为变量)，假设没有再函数作用范围内找到，就会向外查找，如果在外面找到了就可以直接使用，最终会找到window下面。如果在全局作用域都没找到，报错`RefrenceError`



> 规范
>
> 由于我们所有的全局变量都会绑定到我们的window上，如果不同的js文件，使用了相同的全局变量，就会产生冲突~ 如何减少冲突

```javascript
//唯一的全局变量
var kuangApp ={};
//定义全局变量
kuangApp.name = 'kuangshen';
//定义全局方法
kuangApp.add = function (a,b) {
    return a+b;
}
```

把自己的代码全部放入自己定义的唯一空间名字中，降低全局命名冲突的问题~



> 局部作用域let

```javascript
function fff() {
    for (let i = 0; i < 100; i++) {
        console.log(i);
    }
    console.log(i);//问题。 i出了作用域，在外面仍然可以使用
}
```

Es6 let关键字，解决局部作用域冲突的问题，建议大家都使用`let`去定于局部作用域的变量。

> 常量 const

在ES6之前，var定义 的变量都可以修改，只会口头约定只要大写字母命名的变量，就是常量，建议不要修改这样的值，但是仍然可以修改

在ES6,引入了`const`，定义为常量（只读变量）

### 4.3 方法

方法就是把函数放在对象里面，对象只有两个东西: 属性和方法。

```javascript
var kuangshen = {
    name:"秦僵",
    birth:2020,
    //方法
    age:function () {
        //今年-chusheng de出生的年
        var now = new Date().getFullYear();
        return now-this.birth;
    }
}

//属性
kuangshen.name
//方法，使用时候一定要带()
kuangshen.age()

```

this. 代表的是什么？ 拆开上面的代码看看, 可以发现this是始终指向调用他的那个人, this是无法指向的，是默认指向调用它的那个对象；

```javascript
function getAge() {
//今年-chusheng de出生的年
var now = new Date().getFullYear();
return now-this.birth;
}
var kuangshen = {
name:"秦僵",
birth:2020,
//方法
age:getAge
}
```

在js中可以控制this的指向，使用apple()

```javascript
getAge().apply(kuangshen,[])//传递空参
```



## 5.内部对象

标准对象 测试对象类型 typeof

typeof 123 --->number

typeof NaN ---> number

typeof [] --->object

typeof true --->boolean

typeof Math.abs --->function

typeof 'string' ---->string

### 5.1 Date

Date是个日期类型，使用直接使用 new Date()

基本使用

```javascript
<script>
    var now = new Date();
console.log(now.getFullYear()); //年
now.getMonth(); //月
now.getDate(); //日

now.getHours(); //时
now.getMinutes();//分
now.getSeconds();//秒

now.getDay();// 星期几
now.getTime(); //时间戳  全世界统一 ，1970 1.1 0:00:00到现在的时间
console.log(new Date(1321312312312312)) //时间戳转为时间
now.toLocaleString()  //转化为本地时间
</script>
```

### 5.2 Json对象

Json是什么？早期所有的数据传输使用xml文件!

在JavaScript，一切皆为对象，任何js支持的类型都可以使用json来进行表示：number string....

格式：

	- 对象都用大括号{}来进行
	- 数组都是使用[]
	- 所有的键值对都是使用key:value

```javascript
<script>
    'use strick';
    var user = {
        name: "qinjiang",
        age: "3",
        sex: "男"
    }
    //对象转换为json字符串
    var jsonUser = JSON.stringify(user);
    //json字符串解析为对象,参数为json字符串
    var obj = JSON.parse('{"name":"qinjiang","age":"3","sex":"男"}');
</script>
```

上述例子是JSON字符串和JS对象的转化，JSON和JS对象的区别

```javascript
var user = {name: "qinjiang", age: "3", sex: "男"}   //这个是js对象
var obj = JSON.parse('{"name":"qinjiang","age":"3","sex":"男"}');	//这个是json字符串
```

### 5.3 Ajax(自己查资料，自己学)

- 原生的js写法，xhr一步请求
- JQuery封装好，方法$("#name").ajax("")
- axios请求





## 6.面向对象编程



### 6.1 什么是面向对象

JavaScript,Java,c#。。。。这些语言都是面向对象，javascript有一些区别!  

普通上。一个叫做类，一个叫做对象! 类是对象的模板，对象的抽象，对象是类的具体表现

在JavaScript这个需要大家换一下思维模式：

最开始js有个原型，__proto__

```javascript
<script>
    'use strick';
var user = {
    name: "qinjiang",
    age: "3",
    sex: "男",
    run:function () {
        console.log(this.name+"run......");
    }
};
var xiaoming = {
    name: "xiaoming",
};
//小明的原型指向user,这样小明就具有了user的相关属性
//这个时候小明的原型是user
xiaoming.__proto__=user;

var Brid =  {
    fly:function f() {
        console.log(this.name+'可以飞了!')
    }
}
//这个时候小明的原型被只想了Brid，小明可以飞了
xiaoming.__proto__=Brid;

</script>
```

### 6.2 class继承

`class`关键字，是在ES6引入的,还有老的浏览器还不支持

1、定义一个类，属性，方法，直接使用class

```javascript
<script>
    class Student{
        constructor(name) {
            this.name =name;
        }
        hello(){
            console.log('hello');
        }
    }
    </script>
```

2、在js里面使用类

```javascript
//使用创建一个实例
var xuesheng = new Student("xuesheng");
var xiaoming = new Student("xiaoming");
var xiaohong = new Student("xiaohong");
xiaoming.hello();
```

3、在js里面进行类继承

```javascript
    <script>
        class Student {
            constructor(name) {
                this.name = name;
            }

            hello() {
                console.log('hello');
            }
        }

        //使用创建一个实例
        var xuesheng = new Student("xuesheng");
        var xiaoming = new Student("xiaoming");
        var xiaohong = new Student("xiaohong");
        xiaoming.hello();
        class Xiaoxuesheng extends Student{
            constructor(name,grade) {
                super(name);
                this.grade = grade;
            }

            hello() {
                console.log('hello,xiaoxuesheng');
            }
            myGrade(){
                console.log("我是一名小学生")
            }
        }
        var daxuesheng = new Xiaoxuesheng("daxuesheng");
        daxuesheng.myGrade();
        
    </script>
```

本质：查看对象原型，本质还是和原来一样的__proto__，只不过写法发生了改变

查询一个单词，**原型链**  在js中所有的对象最终都是指向object





## 6.操作Dom对象(重点)

DOM Document Object Model  文档对象模型。浏览器网页就是一个Dom树形结构!

	- 更新 更新Dom节点
	- 遍历Dom节点  得到Dom节点
	- 删除  删除一个Dom节点
	- 添加  添加一个新的Dom节点

要操作一个Dom节点，就必须先获得这个Dom节点

### 6.1 获取Dom节点

```javascript
<body>
    <div id="father">
        <h1>标题一</h1>
        <p id="p1">p1</p>
        <p class="p2">p1</p>
    </div>
    <script>
        //对应css选择器
        var h1 = document.getElementsByTagName('h1')//标签选择器
        var p1 = document.getElementById('p1') //id选择器
        var p2 = document.getElementsByClassName('p2') //类选择器
        var father = document.getElementById('father') //id选择器
        var childrens = father.children; //获取父节点的孩子节点
    </script>
```



### 6.2 更新节点信息

```javascript
<body>
    <div id="123"></div>
    <script>
        var id1 = document.getElementById('123');
        id1.innerText='123';
        id1.innerHTML='<strong>123</strong>';

        id1.style.color ='red';
        id1.style.fontSize ='200px';
        id1.style.border ='10px solid red';
    </script>
</body>
```



操作文本

	-	` id1.innerText='123';`  修改文本的值
	-	`id1.innerHTML='<strong>123</strong>';` 可以解析HTML文本标签

操作js

```javascript
id1.style.color ='red';	//属性使用字符串，包裹
id1.style.fontSize ='200px';	//下划线转驼峰命名
id1.style.border ='10px solid red';
```



### 6.3 删除节点

删除节点的步骤，先获取到父节点，再通过父节点删除自己

```javascript
<body>
    <div id="father">
        <h1>标题</h1>
        <p id="p1">我是p1</p>
        <p class="p222">我是p2</p>
    </div>
    <script>
        var h1 = document.getElementsByTagName('h1');
        var p1 = document.getElementById('p1');
        var p2 = document.getElementsByClassName('p222')[0]
        var father = document.getElementById('father')

        var childrens = father.children; //获取父节点下的所有子节点
        father.removeChild(p2)

    </script>
</body>
```

基本上我们在项目中使用删除节点方法是如下的

```javascript
<script>
    var self = document.getElementById('p1');
    var father = self.parentElement;
    father.removeChild(self)
</script>
```



注意：删除多个节点时候，节点实在动态变化的，删除节点的时候一定要注意

```javascript
father.removeChild(father.children[0]);
father.removeChild(father.children[1]);
father.removeChild(father.children[2]); //删除了0之后，2就没了。
```



### 6.4 添加/插入 节点

我们获得了某个Dom节点，假设这个dom节点是空的，我们通过innerHTML就可以增加一个元素了，但是如果这个dom节点已经存在元素了，我们就不能这么干了，因为会产生覆盖!

我们一般使用追加的函数  append()

```javascript
<body>
    <p id="js">JavaScript</p>
    <div id="list">
        <p id="javase">JavaSE</p>
        <p id="javaee">JavaEE</p>
        <p id="javame">JavaME</p>
    </div>
    <script>
        var js = document.getElementById('js');
        var list = document.getElementById('list');
        list.appendChild(js);  //追加到后面
    </script>
</body>
```

执行了list.appendChild(js) 就把js这个元素插入到list的子元素中去了。

![image-20201115163331243](F:\MD格式学习笔记库\狂神JAVA-10-JavaScript学习.assets\image-20201115163331243.png)



**通过js创建一个新的节点**

```javascript
var newP = document.createElement("p") //创建一个p标签
newP.id = 'newP';
newP.innerText = "Hello,kuangshen";
list.appendChild(newP);
```



## 7. 操作Bom元素(重点)

BOM浏览器对象模型，B:Browser  O Object M Model

浏览器介绍

javascript和浏览器的关系？ js的诞生就是为了能够让他能够在浏览器中运行! 所以我们学习js，

目前的主流浏览器内核

	- IE 6-11	Windows系统用的多，默认
	- Chrome   Windows系统用的多
	- Safari   Mac系统的默认浏览器
	- Firefox   Linux用的多，默认就是Firefox浏览器

### 7.1 window对象

window代表我们的浏览器窗口

```javascript
window.alert(1)
undefined
window.innerWidth
742
window.innerHeight
571
window.outerHeight
1179
window.outerWidth
758
window.innerHeight
317
window.innerWidth
1026
window.innerWidth
1964
window.innerWidth
2563
//大家可以调整浏览器的窗口试试....
```



### 7.2 navigator对象

Navigator,代表浏览器封装了浏览器的信息

```javascript
window.navigator
Navigator {vendorSub: "", productSub: "20030107", vendor: "Google Inc.", maxTouchPoints: 0, hardwareConcurrency: 12, …}
navigator.appVersion
"5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36"
navigator.userAgent
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36"
navigator.appName
"Netscape"
navigator.platform
"Win32"
navigator.getUserMedia
ƒ () { [native code] }
navigator.storage
StorageManager {}
navigator.connection
NetworkInformation {onchange: null, effectiveType: "4g", rtt: 50, downlink: 10, saveData: false}
```

大多数的时候，我们不会使用`navigator`对象，因为会被认为修改!

不建议使用这些属性来判断和编写代码!

### 7.3 screen对象

代表屏幕的尺寸

```javascript
screen
Screen {availWidth: 2560, availHeight: 1400, width: 2560, height: 1440, colorDepth: 24, …}
screen.width
2560  //代表屏幕宽度2560px
screen.height
1440	//代表屏幕高度1440px
```



### 7.4 location(重要)

location 代表当前页面的URL信息

```javascript
location
Location {
    href: "https://www.baidu.com/", 
    ancestorOrigins: DOMStringList, 
    origin: "https://www.baidu.com", 
    protocol: "https:", 
    host: "www.baidu.com",
    hostname: "www.baidu.com"
    pathname: "/"
    protocol: "https:"协议
    reload: ƒ reload()//刷新网页
   	location.assign('http://www.aigoo.top') //重新定位到一个地址
    …}
```



### 7.5 Document

document代表当前页面，HTML  DOM文档树

```javascript
document.title
"百度一下，你就知道"
document.title="金根说"
"金根说"
```

可以直接获取页面文档树节点

`var dl = document.getElementById('app');`

可以获取cookie

>document.cookie
>"PSTM=1600411949; BD_UPN=12314753; BIDUPSID=1D639F0E11F0B036BE51C714906845B4; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BAIDUID=4AD7A56C0D4FCC6548188ED8BDBE69EF:FG=1; H_WISE_SIDS=154758_153758_160799_159578_156287_150775_159614_148855_160935_159383_158927_154173_150772_157264_127969_160765_146732_160274_159740_131423_114550_132548_159703_155395_107315_160868_160319_155344_155255_159954_157792_144966_159797_159950_154619_158717_158642_159588_147551_160708_159157_159092_110085_157006; plus_cv=1::m:49a3f4a6; sug=3; sugstore=0; ORIGIN=2; bdime=0; H_PS_PSSID=32818_1436_33102_32944_33061_31254_32723_33098_33100_32962_26350; H_PS_645EC=019bhFQ5sv6H7OBPdS6a9FVZZ%2FzG4fhCE%2FfXXNjzUD3rksS5xWKHrs8Vt2FC%2BkTjp0Bg; delPer=0; BD_CK_SAM=1; PSINO=1; MCITY=-%3A; BD_HOME=1"

可以劫持cookie原理，获取你的cookie然后上传到他的服务器



### 7.6 History

history代表浏览器的历史记录	

​	history.back()  //后退

​	history.forward() //前进



## 8.操作表单

表单是什么，from DOM 树

1. 文本框

```javascript
<body>
<form action="" method="post">
    <span>用户名:</span><input type="text" id="username">
</form>
<script>
    var input_text = document.getElementById('username');
</script>
</body>

输出结果：input_text.value
""
input_text.value
"12313"
input_text.value='12312312'
"12312312"
```

1. 下拉框     <select>
2. 复选框，单选框   <radio> <checkbox>

```javascript
<form action="" method="post">
    <p>
        <span>用户名:</span><input type="text" id="username">
    </p>
    <p>
        <span>用户名:</span><input type="password" id="username">
    </p>
    <p>
        <span>性别:</span>
        <input type="radio" name="sex" value="man" id="boy">男
        <input type="radio" name="sex" value="women" id="girl">女
    </p>
</form>

<script>
    var input_text = document.getElementById('username');

    //对于单选框
    var boy_radio=document.getElementById("boy");
    var girl_radio=document.getElementById("girl");
    //对于单选框，多选框等固定的值，boy_radio.value只能取到当前值，需要用checked
    boy_radio.checked;//查看返回的结果，是否为true，如果是true就是选中~
    girl_radio.checked=true;
</script>
```

1. 隐藏域   hidden
2. 密码框   password

表单目的，提交信息，获得要提交的信息



MD5加密

```javascript
<form action="" method="post">
    <p>
        <span>用户名:</span><input type="text" id="username" name="username"/>
    </p>
    <p>
        <span>密码:</span><input type="password" id="password" name="password"/>
    </p>
    <!--绑定时间，当按钮被点击，我们要支持的动作-->
    <button type="submit" onclick="aaa()" name="" id="">提交</button>


</form>
<script>
    function aaa() {
        var uname = document.getElementById('username');
        var pwd =document.getElementById('password');
        console.log("用户名:"+uname.value);
        console.log("密码:"+pwd.value);
    }
</script>
```



采用隐藏输入框进行加密行为

```javascript
<body>
<form action="" method="post" onsubmit="return aaa()">
    <p>
        <span>用户名:</span>
        <input type="text" id="username" name="username">
    </p>
    <p>
        <span>密码:</span>
        <input type="password" id="password" name="password">
    </p>
        <input type="hidden" id="md5password" name="md5_password">
    <!--绑定事件-->
    <button type="submit">提交</button>
</form>
<script>
    function aaa() {
        var uname = document.getElementById('username');
        var pwd = document.getElementById('password');
        var md5pwd = document.getElementById('md5password');
        md5pwd.value = md5(pwd.value);

        console.log("用户名:" + uname.value);
        console.log("input_pwd密码:" + pwd.value);
        console.log("md5_pwd密码:" + md5pwd.value);
        return  false;
    }
</script>
</body>
```







## 9 JQuery初步入门

JavaScript和Jquery的关系: 库，存在大量的Javascript方法函数

> 获取jquery

$(选择器).action()

```javascript
    $('#testid').click(function () {
        alert("答复进口加咖啡!");
    });
```



9.1 jQuery选择器

可以去这个网站，这里内容非常多!   https://jquery.cuishifeng.cn/



# JavaScript：基础语法

### 注释

JavaScript的语法和Java语言类似，每个语句以`;`结束，语句块用`{...}`。但是，JavaScript并不强制要求在每个语句的结尾加;浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上;。JavaScript严格区分大小写，如果弄错了大小写，程序将报错或者运行不正常。

**注释:**

```javascript
// 这是一行注释
alert('love qinjiang'); // 这也是注释

/* 从这里开始是块注释
仍然是注释
仍然是注释
注释结束 */
```

### 变量

变量的概念基本上和小学的方程变量是一致的，只是在计算机程序中，变量不仅可以是数字，还可以是任意数据类型。

变量在JavaScript中就是用一个变量名表示，变量名是大小写英文、数字、$和_的组合，且**不能用数字开头**。变量名也不能是**JavaScript的关键字**，如if、while等。申明一个变量用var语句，比如：

```javascript
var a; // 申明了变量a，此时a的值为undefined
var $b = 1; // 申明了变量$b，同时给$b赋值，此时$b的值为1
var s_007 = '007'; // s_007是一个字符串
var Answer = true; // Answer是一个布尔值true
var t = null; // t的值是null
```

在JavaScript中，使用等号=对变量进行赋值。可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量，但是要注意只能用var申明一次，例如：

```javascript
var a = 123; // a的值是整数123
a = 'ABC'; // a变为字符串
```

这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。例如Java是静态语言，赋值语句如下：

```javascript
int a = 123; // a是整数类型变量，类型用int申明
a = "ABC"; // 错误：不能把字符串赋给整型变量
```

和静态语言相比，动态语言更灵活，就是这个原因。所以很容易出错~emmmm....

**strict模式**

JavaScript在设计之初，为了方便初学者学习，并不强制要求用var申明变量。这个设计错误带来了严重的后果：如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为全局变量：

```javascript
i = 10; // i现在是全局变量
```

在同一个页面的不同的JavaScript文件中，如果都不用var申明，恰好都使用了变量i，将造成变量i互相影响,不容易维护;

为了修补JavaScript这一严重设计缺陷，ECMA在后续规范中推出了strict模式，在strict模式下运行的JavaScript代码，强制通过var申明变量，未使用var申明变量就使用的，将导致运行错误。

启用strict模式的方法是在JavaScript代码的第一行写上：

```javascript
'use strict';
```

如果浏览器支持strict模式，下面的代码将报ReferenceError错误:

```html
<script>
    'use strict';
    abc = 'Hello, world';
    console.log(abc);
</script>
```

## 数据类型

计算机顾名思义就是可以做数学计算的机器，因此，计算机程序理所当然地可以处理各种数值。但是，计算机能处理的远不止数值，还可以处理文本、图形、音频、视频、网页等各种各样的数据，不同的数据，需要定义不同的数据类型。在JavaScript中定义了以下几种数据类型：

**Number** , JavaScript不区分整数和浮点数，统一用Number表示，以下都是合法的Number类型;

```javascript
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```

Number可以直接做四则运算，规则和数学一致：

```javascript
1 + 2; // 3
(1 + 2) * 5 / 2; // 7.5
2 / 0; // Infinity
0 / 0; // NaN
10 % 3; // 1
10.5 % 3; // 1.5
```

**字符串**是以单引号'或双引号"括起来的任意文本，比如`'abc'`，`"xyz"`等等。字符串常见的操作如下：

```javascript
var s = 'Hello, world!';
s.length; // 13
```

要获取字符串某个指定位置的字符，使用类似Array的下标操作，索引号从0开始：

```javascript
var s = 'Hello, world!';

s[0]; // 'H'
s[6]; // ' '
s[7]; // 'w'
s[12]; // '!'
s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined
```

JavaScript为字符串提供了一些常用方法，注意，调用这些方法本身不会改变原有字符串的内容，而是返回一个新字符串：

- toUpperCase()把一个字符串全部变为大写
- toLowerCase()把一个字符串全部变为小写
- indexOf()会搜索指定字符串出现的位置
- substring()返回指定索引区间的子串

```javascript
var s = 'Hello';
s.toUpperCase(); // 返回'HELLO'
var lower = s.toLowerCase(); // 返回'hello'并赋值给变量lower

var b = 'hello, world';
b.indexOf('world'); // 返回7
b.indexOf('World'); // 没有找到指定的子串，返回-1
b.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
b.substring(7); // 从索引7开始到结束，返回'world'
```

**布尔值**和布尔代数的表示完全一致，一个布尔值只有true、false两种值，要么是true，要么是false，可以直接用true、false表示布尔值，也可以通过布尔运算计算出来：布尔值经常用在条件判断中

```javascript
true; // 这是一个true值
false; // 这是一个false值
2 > 1; // 这是一个true值
2 >= 3; // 这是一个false值
```

`&&`运算是与运算，只有所有都为true，&&运算结果才是true：

```javascript
true && true; // 这个&&语句计算结果为true
true && false; // 这个&&语句计算结果为false
false && true && false; // 这个&&语句计算结果为false
```

`||`运算是或运算，只要其中有一个为true，||运算结果就是true：

```javascript
false || false; // 这个||语句计算结果为false
true || false; // 这个||语句计算结果为true
false || true || false; // 这个||语句计算结果为true
```

`!`运算是非运算，它是一个单目运算符，把true变成false，false变成true：

```javascript
! true; // 结果为false
! false; // 结果为true
! (2 > 5); // 结果为true
```

**比较运算符**,当我们对Number做比较时，可以通过比较运算符得到一个布尔值：

```javascript
2 > 5; // false
5 >= 2; // true
7 == 7; // true
```

实际上，JavaScript允许对任意数据类型做比较：

```javascript
false == 0; // true
false === 0; // false
```

要特别注意相等运算符==。JavaScript在设计时，有两种比较运算符：

- 第一种是==比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；
- 第二种是===比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。
- 由于JavaScript这个设计缺陷，不要使用==比较，始终坚持使用===比较。

另一个例外是NaN这个特殊的Number与所有其他值都不相等，包括它自己：

```javascript
NaN === NaN; // false
```

唯一能判断NaN的方法是通过isNaN()函数：

```javascript
isNaN(NaN); // true
```

**null和undefined**

`null`表示一个“空”的值，它和0以及空字符串''不同，0是一个数值，''表示长度为0的字符串，而null表示“空”。在其他语言中，也有类似JavaScript的null的表示，例如Java也用null，Swift用nil，Python用None表示。但是，在JavaScript中，还有一个和null类似的`undefined`，它表示“未定义”。JavaScript的设计者希望用null表示一个空的值，而undefined表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用null。

## 数组

**数组**是一组按顺序排列的集合，集合的每个值称为元素。JavaScript的数组可以包括任意数据类型。例如：

```javascript
[1, 2, 3.14, 'Hello', null, true];
```

上述数组包含6个元素。数组用[]表示，元素之间用,分隔。

另一种创建数组的方法是通过Array()函数实现：

```javascript
new Array(1, 2, 3); // 创建了数组[1, 2, 3]
```

然而，出于代码的可读性考虑，强烈建议直接使用[]。

数组的元素可以通过索引来访问。请注意，索引的起始值为0：

```javascript
var arr = [1, 2, 3.14, 'Hello', null, true];
arr[0]; // 返回索引为0的元素，即1
arr[5]; // 返回索引为5的元素，即true
arr[6]; // 索引超出了范围，返回undefined
```

要取得Array的长度，直接访问`length`属性：

```javascript
var arr = [1, 2, 3.14, 'Hello', null, true];
arr.length; // 6
```

请注意，直接给Array的length赋一个新的值会导致Array大小的变化：

```javascript
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;
arr; // arr变为[1, 2]
```

大多数其他编程语言不允许直接改变数组的大小，越界访问索引会报错。然而，JavaScript的Array却不会有任何错误。在编写代码时，不建议直接修改Array的大小，访问索引时要确保索引不会越界。

**indexOf** : 与String类似，Array也可以通过indexOf()来搜索一个指定的元素的位置：注意了，数字30和字符串'30'是不同的元素。

```javascript
var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10); // 元素10的索引为0
arr.indexOf(20); // 元素20的索引为1
arr.indexOf(30); // 元素30没有找到，返回-1
arr.indexOf('30'); // 元素'30'的索引为2
```

`slice()`就是对应String的substring()版本，它截取Array的部分元素，然后返回一个新的Array：

```javascript
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3); // 从索引3开始到结束: ['D', 'E', 'F', 'G']
```

`push()`向Array的末尾添加若干元素，`pop()`则把Array的最后一个元素删除掉;

```javascript
var arr = [1, 2];
arr.push('A', 'B'); // 返回Array新的长度: 4
arr; // [1, 2, 'A', 'B']
arr.pop(); // pop()返回'B'
arr; // [1, 2, 'A']
arr.pop(); arr.pop(); arr.pop(); // 连续pop 3次
arr; // []
arr.pop(); // 空数组继续pop不会报错，而是返回undefined
arr; // []
```

如果要往Array的头部添加若干元素，使用`unshift()`方法，`shift()`方法则把Array的第一个元素删掉：

```javascript
var arr = [1, 2];
arr.unshift('A', 'B'); // 返回Array新的长度: 4
arr; // ['A', 'B', 1, 2]
arr.shift(); // 'A'
arr; // ['B', 1, 2]
arr.shift(); arr.shift(); arr.shift(); // 连续shift 3次
arr; // []
arr.shift(); // 空数组继续shift不会报错，而是返回undefined
arr; // []
```

`sort()`可以对当前Array进行排序，它会直接修改当前Array的元素位置，直接调用时，按照默认顺序排序：

```javascript
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
```

`reverse()`把整个Array的元素给掉个个，也就是反转：

```javascript
var arr = ['one', 'two', 'three'];
arr.reverse(); 
arr; // ['three', 'two', 'one']
```

`splice()`方法是修改Array的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：

```javascript
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

## 对象

**JavaScript的对象**是一组由键-值组成的无序集合，例如：

```javascript
var person = {
    name: 'qinjiang',
    age: 3,
    hobby: ['code', 'music', 'girl'],
    city: 'Xian'
};
```

JavaScript对象的键都是字符串类型，值可以是任意数据类型。其中每个键又称为对象的属性,后面则是对象的值!

要获取一个对象的属性，我们用对象变量.属性名的方式

```javascript
person.name; //qinjiang
person.age; //3
```

由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性：

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
xiaoming.age; // undefined
delete xiaoming['name']; // 删除name属性
xiaoming.name; // undefined
delete xiaoming.school; // 删除一个不存在的school属性也不会报错
```

如果我们要检测xiaoming是否拥有某一属性，可以用in操作符：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
'name' in xiaoming; // true
'grade' in xiaoming; // false
```

不过要小心，如果in判断一个属性存在，这个属性不一定是xiaoming的，它可能是xiaoming继承得到的：

```javascript
'toString' in xiaoming; // true
```

因为toString定义在object对象中，**而所有对象最终都会在原型链上指向object**，所以xiaoming也拥有toString属性。
要判断一个属性是否是xiaoming自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法：

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```



# JavaScript：流程控制、集合

## 条件判断

JavaScript使用if () { ... } else { ... }来进行条件判断。例如，根据年龄显示不同内容，可以用if语句实现如下：

```javascript
var age = 20;
if (age >= 18) { // 如果age >= 18为true，则执行if语句块
    alert('adult');
} else { // 否则执行else语句块
    alert('teenager');
}
```

其中else语句是可选的。如果语句块只包含一条语句，那么可以省略{},不建议这样写

```javascript
var age = 20;
if (age >= 18)
    alert('adult');
else
    alert('teenager');
```

如果还要更细致地判断条件，可以使用多个if...else...的组合：

```javascript
var age = 3;
if (age >= 18) {
    alert('adult');
} else if (age >= 6) {
    alert('teenager');
} else {
    alert('kid');
}
```

请注意，if...else...语句的执行特点是二选一，在多个if...else...语句中，如果某个条件成立，则后续就不再继续判断了。

**JavaScript把`null`、`undefined`、`0`、`NaN`和空字符串`''`视为false，其他值一概视为true**

## for循环

要计算1+2+3，我们可以直接写表达式：

```javascript
1 + 2 + 3; // 6
```

要计算1+2+3+...+10，勉强也能写出来。但是，要计算1+2+3+...+10000，直接写表达式就不可能了。为了让计算机能计算成千上万次的重复运算，我们就需要循环语句。JavaScript的循环有两种，一种是for循环，通过初始条件、结束条件和递增条件来循环执行语句块：

```javascript
var x = 0;
var i;
for (i=1; i<=10000; i++) {
    x = x + i;
}
x; // 50005000
```

让我们来分析一下for循环的控制条件：

- i=1 这是初始条件，将变量i置为1；
- i<=10000 这是判断条件，满足时就继续循环，不满足就退出循环；
- i++ 这是每次循环后的递增条件，由于每次循环后变量i都会加1，因此它终将在若干次循环后不满足判断条件i<=10000而退出循环。

**for循环最常用的地方是利用索引来遍历数组：**

```javascript
var arr = ['Apple', 'Google', 'Microsoft'];
var i, x;
for (i=0; i<arr.length; i++) {
    x = arr[i];
    console.log(x);
}
```

for循环的3个条件都是可以省略的，如果没有退出循环的判断条件，就必须使用break语句退出循环，否则就是死循环：

```javascript
var x = 0;
for (;;) { // 将无限循环下去
    if (x > 100) {
        break; // 通过if判断来退出循环
    }
    x ++;
}
```

for循环的一个变体是for ... in循环，它可以把一个对象的所有属性依次循环出来：

```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    console.log(key); // 'name', 'age', 'city'
}
```

### while

for循环在已知循环的初始和结束条件时非常有用。而上述忽略了条件的for循环容易让人看不清循环的逻辑，此时用while循环更佳。

while循环只有一个判断条件，条件满足，就不断循环，条件不满足时则退出循环。比如我们要计算100以内所有奇数之和，可以用while循环实现：

```javascript
var x = 0;
var n = 99;
while (n > 0) {
    x = x + n;
    n = n - 2;
}
x; // 2500
```

最后一种循环是do { ... } while()循环，它和while循环的唯一区别在于，不是在每次循环开始的时候判断条件，而是在每次循环完成的时候判断条件：至少 执行一次!

```javascript
var n = 0;
do {
    n = n + 1;
} while (n < 100);
n; // 100
```

在编写循环代码时，务必小心编写初始条件和判断条件，尤其是边界值。特别注意i < 100和i <= 100是不同的判断逻辑。同时也要避免死循环的产生!

## Map

JavaScript的默认对象表示方式{}可以视为其他语言中的Map或Dictionary的数据结构，即一组键值对。

但是JavaScript的对象有个小问题，就是键必须是字符串。但实际上Number或者其他数据类型作为键也是非常合理的。

为了解决这个问题，最新的ES6规范引入了新的数据类型Map。需要浏览器支持,我这个360支持es6的规范!

**Map是一组键值对的结构，具有极快的查找速度。**举个例子，假设要根据同学的名字查找对应的成绩，如果用Array实现，需要两个Array：

```javascript
var names = ['Michael', 'Bob', 'Tracy'];
var scores = [95, 75, 85];
```

给定一个名字，要查找对应的成绩，就先要在names中找到对应的位置，再从scores取出对应的成绩，Array越长，耗时越长。

如果用Map实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。用JavaScript写一个Map如下：

```javascript
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```

初始化Map需要一个二维数组，或者直接初始化一个空Map。Map具有以下方法：

```javascript
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉：

```javascript
var m = new Map();
m.set('Adam', 67);
m.set('Adam', 88);
m.get('Adam'); // 88
```

## Set

Set和Map类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在Set中，没有重复的key。

要创建一个Set，需要提供一个Array作为输入，或者直接创建一个空Set：

```javascript
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```

重复元素在Set中自动被过滤：

```javascript
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
```

通过add(key)方法可以添加元素到Set中，可以重复添加，但不会有效果：

```javascript
s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}
```

通过delete(key)方法可以删除元素：

```javascript
var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```

**最后:Map和Set是ES6标准新增的数据类型，请根据浏览器的支持情况决定是否要使用。**



# JavaScript：函数、标准对象

## 初识函数

函数就和Java中的方法是一样的,说白了,就是一系列语句的集合,我们可以提取出来实现复用!

**在JavaScript中，定义函数的方式如下：**

```javascript
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

上述abs()函数的定义如下：

- `function`指出这是一个函数定义；
- `abs`是函数的名称；
- `(x)`括号内列出函数的参数，多个参数以,分隔；
- `{ ... }`之间的代码是函数体，可以包含若干语句，甚至可以没有任何语句。

请注意，函数体内部的语句在执行时，一旦执行到return时，函数就执行完毕，并将结果返回。因此，函数内部通过条件判断和循环可以实现非常复杂的逻辑。

如果没有return语句，函数执行完毕后也会返回结果，只是结果为undefined。

由于JavaScript的函数也是一个对象，上述定义的abs()函数实际上是一个函数对象，而函数名abs可以视为指向该函数的变量。

因此，第二种定义函数的方式如下：

```javascript
var abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```

在这种方式下，function (x) { ... }是一个匿名函数，它没有函数名。但是，这个匿名函数赋值给了变量abs，所以，通过变量abs就可以调用该函数。

上述两种定义完全等价，注意第二种方式按照完整语法需要在函数体末尾加一个;，表示赋值语句结束。

**调用函数时，按顺序传入参数即可：**

```javascript
abs(10); // 返回10
abs(-9); // 返回9
```

由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：

```javascript
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```

传入的参数比定义的少也没有问题：

```javascript
abs(); // 返回NaN
```

要避免收到undefined，可以对参数进行检查：

```javascript
function abs(x) {
    //类型比较,和抛出异常~
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

**arguments** 它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。利用arguments，你可以获得调用者传入的所有参数。也就是说，即使函数不定义任何参数，还是可以拿到参数的值：

```javascript
function abs() {
    if (arguments.length === 0) {
        return 0;
    }
    var x = arguments[0];
    return x >= 0 ? x : -x;
}

abs(); // 0
abs(10); // 10
abs(-9); // 9
```

**rest参数**

ES6标准引入了rest参数 ：

```javascript
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```

rest参数只能写在最后，前面用...标识，从运行结果可知，传入的参数先绑定a、b，多余的参数以数组形式交给变量rest，所以，不再需要arguments我们就获取了全部参数。

如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是undefined）。

## 变量作用域与解构赋值

在JavaScript中，用var申明的变量实际上是有作用域的。

如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量：

```javascript
'use strict';

function foo() {
    var x = 1;
    x = x + 1;
}

x = x + 2; // ReferenceError! 无法在函数体外引用变量x
```

如果两个不同的函数各自申明了同一个变量，那么该变量只在各自的函数体内起作用。换句话说，不同函数内部的同名变量互相独立，互不影响：

```javascript
'use strict';

function foo() {
    var x = 1;
    x = x + 1;
}

function bar() {
    var x = 'A';
    x = x + 'B';
}
```

由于JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行：

```javascript
'use strict';

function foo() {
    var x = 1;
    function bar() {
        var y = x + 1; // bar可以访问foo的变量x!
    }
    var z = y + 1; // ReferenceError! foo不可以访问bar的变量y!
}
```

如果内部函数和外部函数的变量名重名怎么办？来测试一下：

```javascript
function foo() {
    var x = 1;
    function bar() {
        var x = 'A';
        console.log('x in bar() = ' + x); // 'A'
    }
    console.log('x in foo() = ' + x); // 1
    bar();
}

foo();
```

这说明JavaScript的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。

### 变量提升

JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部：

```javascript
'use strict';

function foo() {
    var x = 'Hello, ' + y;
    console.log(x);
    var y = 'Bob';
}

foo();
```

虽然是strict模式，但语句var x = 'Hello, ' + y;并不报错，原因是变量y在稍后申明了。但是console.log显示Hello, undefined，说明变量y的值为undefined。这正是因为JavaScript引擎自动提升了变量y的声明，但不会提升变量y的赋值。

对于上述foo()函数，JavaScript引擎看到的代码相当于：

```javascript
function foo() {
    var y; // 提升变量y的申明，此时y为undefined
    var x = 'Hello, ' + y;
    console.log(x);
    y = 'Bob';
}
```

由于JavaScript的这一怪异的“特性”，我们在函数内部定义变量时，请严格遵守“在函数内部首先申明所有变量”这一规则。最常见的做法是用一个var申明函数内部用到的所有变量：

```javascript
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
    // 其他语句:
    for (i=0; i<100; i++) {
        ...
    }
}
```

**全局作用域**

不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象window，全局作用域的变量实际上被绑定到window的一个属性：

```javascript
'use strict';

var course = 'Learn JavaScript';
alert(course); // 'Learn JavaScript'
alert(window.course); // 'Learn JavaScript'
```

因此，直接访问全局变量course和访问window.course是完全一样的。

你可能猜到了，由于函数定义有两种方式，以变量方式var foo = function () {}定义的函数实际上也是一个全局变量，因此，顶层函数的定义也被视为一个全局变量，并绑定到window对象：

```javascript
'use strict';

function foo() {
    alert('foo');
}

foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用
```

进一步大胆地猜测，我们每次直接调用的alert()函数其实也是window的一个变量：

```javascript
window.alert('调用window.alert()');
// 把alert保存到另一个变量:
var old_alert = window.alert;
// 给alert赋一个新函数:
window.alert = function () {}
alert('无法用alert()显示了!');
// 恢复alert:
window.alert = old_alert;
alert('又可以用alert()了!');
```

**局部作用域**

由于JavaScript的变量作用域实际上是函数内部，我们在for循环等语句块中是无法定义具有局部作用域的变量的：

```javascript
'use strict';

function foo() {
    for (var i=0; i<100; i++) {
        //
    }
    i += 100; // 仍然可以引用变量i
}
```

为了解决块级作用域，ES6引入了新的关键字let，用let替代var可以申明一个块级作用域的变量：

```javascript
'use strict';

function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError:
    i += 1;
}
```

**常量**

由于var和let申明的是变量，如果要申明一个常量，在ES6之前是不行的，我们通常用全部大写的变量来表示“这是一个常量，不要修改它的值”：

```javascript
var PI = 3.14;
```

ES6标准引入了新的关键字const来定义常量，const与let都具有块级作用域：

```javascript
'use strict';

const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
PI; // 3.14
```

## 方法

在一个对象中绑定函数，称为这个对象的方法。

在JavaScript中，对象的定义是这样的：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了
```

绑定到对象上的函数称为方法，和普通函数也没啥区别，但是它在内部使用了一个this关键字，这个东东是什么？

在一个方法内部，this是一个特殊变量，它始终指向当前对象，也就是xiaoming这个变量。所以，this.birth可以拿到xiaoming的birth属性。

## 标准对象

在JavaScript的世界里，一切都是对象。

但是某些对象还是和其他对象不太一样。为了区分对象的类型，我们用typeof操作符获取对象的类型，它总是返回一个字符串：

```javascript
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```

### Date

在JavaScript中，Date对象用来表示日期和时间。要获取系统当前时间，用:

```javascript
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳
```

注意，当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值。

如果要创建一个指定日期和时间的Date对象，可以用：

```javascript
var d = new Date(2015, 5, 19, 20, 15, 30, 123);
d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)
```

你可能观察到了一个非常非常坑爹的地方，就是JavaScript的月份范围用整数表示是0~11，0表示一月，1表示二月……，所以要表示6月，我们传入的是5！这绝对是JavaScript的设计者当时脑抽了一下，但是现在要修复已经不可能了。

Date对象表示的时间总是按浏览器所在时区显示的，不过我们既可以显示本地时间，也可以显示调整后的UTC时间：

```
var d = new Date();
d.toLocaleString(); 
d.toUTCString(); 
```

### JSON

JSON是JavaScript Object Notation的缩写，它是一种数据交换格式。

在JSON出现之前，大家一直用XML来传递数据。因为XML是一种纯文本格式，所以它适合在网络上交换数据。XML本身不算复杂，但是，加上DTD、XSD、XPath、XSLT等一大堆复杂的规范以后，任何正常的软件开发人员碰到XML都会感觉头大了，最后大家发现，即使你努力钻研几个月，也未必搞得清楚XML的规范。

终于，在2002年的一天，道格拉斯·克罗克福特（Douglas Crockford）同学为了拯救深陷水深火热同时又被某几个巨型软件企业长期愚弄的软件工程师，发明了JSON这种超轻量级的数据交换格式。

并且，JSON还定死了字符集必须是UTF-8，表示多语言就没有问题了。为了统一解析，JSON的字符串规定必须用双引号""，Object的键也必须用双引号""。

由于JSON非常简单，很快就风靡Web世界，并且成为ECMA标准。几乎所有编程语言都有解析JSON的库，而在JavaScript中，我们可以直接使用JSON，因为JavaScript内置了JSON的解析。

把任何JavaScript对象变成JSON，就是把这个对象序列化成一个JSON格式的字符串，这样才能够通过网络传递给其他计算机。

如果我们收到一个JSON格式的字符串，只需要把它反序列化成一个JavaScript对象，就可以在JavaScript中直接使用这个对象了。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

```json
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

- JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

  ```javascript
  var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
  var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
  ```

**JSON 和 JavaScript 对象互转**

- 要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法：

  ```javascript
  var obj = JSON.parse('{"a": "Hello", "b": "World"}'); 
  //结果是 {a: 'Hello', b: 'World'}
  ```

- 要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法：

  ```javascript
  var json = JSON.stringify({a: 'Hello', b: 'World'});
  //结果是 '{"a": "Hello", "b": "World"}'
  ```

**上代码:**

```
"en"``>````  ```"UTF-8"``>``  ``JSON_秦疆````` ``"text/javascript"``>``  ``//编写一个js的对象``  ``var` `user = {``    ``name:``"秦疆"``,``    ``age:3,``    ``sex:``"男"``  ``};``  ``//将js对象转换成json字符串``  ``var` `str = JSON.stringify(user);``  ``console.log(str);``  ` `  ``//将json字符串转换为js对象``  ``var` `user2 = JSON.parse(str);``  ``console.log(user2.age,user2.name,user2.sex);` `` 
```



# JavaScript：操作 BOM 和 DOM

### 浏览器说明

由于JavaScript的出现就是为了能在浏览器中运行，所以，浏览器自然是JavaScript开发者必须要关注的。

目前主流的浏览器分这么几种：

- IE 6~11：国内用得最多的IE浏览器，历来对W3C标准支持差。从IE10开始支持ES6标准；
- Chrome：Google出品的基于Webkit内核浏览器，内置了非常强悍的JavaScript引擎——V8。由于Chrome一经安装就时刻保持自升级，所以不用管它的版本，最新版早就支持ES6了；
- Safari：Apple的Mac系统自带的基于Webkit内核的浏览器，从OS X 10.7 Lion自带的6.1版本开始支持ES6，目前最新的OS X 10.11 El Capitan自带的Safari版本是9.x，早已支持ES6；
- Firefox：Mozilla自己研制的Gecko内核和JavaScript引擎OdinMonkey。早期的Firefox按版本发布，后来终于聪明地学习Chrome的做法进行自升级，时刻保持最新；
- 移动设备上目前iOS和Android两大阵营分别主要使用Apple的Safari和Google的Chrome，由于两者都是Webkit核心，结果HTML5首先在手机上全面普及（桌面绝对是Microsoft拖了后腿），对JavaScript的标准支持也很好，最新版本均支持ES6。

其他浏览器如Opera等由于市场份额太小就被自动忽略了。

另外还要注意识别各种国产浏览器，如某某安全浏览器，某某旋风浏览器，它们只是做了一个壳，其核心调用的是IE，也有号称同时支持IE和Webkit的“双核”浏览器。

不同的浏览器对JavaScript支持的差异主要是，有些API的接口不一样，比如AJAX，File接口。对于ES6标准，不同的浏览器对各个特性支持也不一样。

在编写JavaScript的时候，就要充分考虑到浏览器的差异，尽量让同一份JavaScript代码能运行在不同的浏览器中。

### 浏览器对象

JavaScript可以获取浏览器提供的很多对象，并进行操作。

**window**

window对象不但充当全局作用域，而且表示浏览器窗口。

window对象有innerWidth和innerHeight属性，可以获取浏览器窗口的内部宽度和高度。内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。

对应的，还有一个outerWidth和outerHeight属性，可以获取浏览器窗口的整个宽高。

```javascript
// 可以调整浏览器窗口大小试试:
console.log('window inner size: ' + window.innerWidth + ' x ' + window.innerHeight);
```

**navigator 对象表示浏览器的信息，最常用的属性包括：**

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的User-Agent字符串

```javascript
console.log('appName = ' + navigator.appName);
console.log('appVersion = ' + navigator.appVersion);
console.log('language = ' + navigator.language);
console.log('platform = ' + navigator.platform);
console.log('userAgent = ' + navigator.userAgent);
```

请注意，navigator的信息可以很容易地被用户修改，所以JavaScript读取的值不一定是正确的。

**screen对象表示屏幕的信息，常用的属性有：**

- screen.width：屏幕宽度，以像素为单位；
- screen.height：屏幕高度，以像素为单位；
- screen.colorDepth：返回颜色位数，如8、16、24。

```javascript
console.log('Screen size = ' + screen.width + ' x ' + screen.height);
```

**location对象表示当前页面的URL信息。例如，一个完整的URL：**

可以用location.href获取。要获得URL各个部分的值，可以这么写：

```javascript
location.protocol; // 'http'
location.host; // 'www.example.com'
location.port; // '8080'
location.pathname; // '/path/index.html'
location.search; // '?a=1&b=2'
location.hash; // 'TOP'
```

如果要重新加载当前页面，调用location.reload()方法非常方便。

**document对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，document对象就是整个DOM树的根节点。**

document的title属性是从HTML文档中的<title>xxx</title>读取的，但是可以动态改变：

```javascript
document.title = '努力学习JavaScript!';
```

- 用document对象提供的getElementById()和getElementsByTagName()可以按ID获得一个DOM节点和按Tag名称获得一组DOM节点!
- JavaScript可以通过document.cookie读取到当前页面的Cookie：

**history**

history对象保存了浏览器的历史记录，JavaScript可以调用history对象的back()或forward ()，相当于用户点击了浏览器的“后退”或“前进”按钮。

这个对象属于历史遗留对象，对于现代Web页面来说，由于大量使用AJAX和页面交互，简单粗暴地调用history.back()可能会让用户感到非常愤怒。

新手开始设计Web页面时喜欢在登录页登录成功时调用history.back()，试图回到登录前的页面。这是一种错误的方法。

任何情况，你都不应该使用history这个对象了。

## DOM

由于HTML文档被浏览器解析后就是一棵DOM树，要改变HTML的结构，就需要通过JavaScript来操作DOM。

始终记住DOM是一个树形结构。操作一个DOM节点实际上就是这么几个操作：

- 更新：更新该DOM节点的内容，相当于更新了该DOM节点表示的HTML的内容；
- 遍历：遍历该DOM节点下的子节点，以便进行进一步操作；
- 添加：在该DOM节点下新增一个子节点，相当于动态增加了一个HTML节点；
- 删除：将该节点从HTML中删除，相当于删掉了该DOM节点的内容以及它包含的所有子节点。

在操作一个DOM节点前，我们需要通过各种方式先拿到这个DOM节点。最常用的方法是`document.getElementById()`和`document.getElementsByTagName()`

由于ID在HTML文档中是唯一的，所以document.getElementById()可以直接定位唯一的一个DOM节点。document.getElementsByTagName()和document.getElementsByClassName()总是返回一组DOM节点。要精确地选择DOM，可以先定位父节点，再从父节点开始选择，以缩小范围。

```javascript
// 返回ID为'test'的节点：
var test = document.getElementById('test');

// 先定位ID为'test-table'的节点，再返回其内部所有tr节点：
var trs = document.getElementById('test-table').getElementsByTagName('tr');

// 先定位ID为'test-div'的节点，再返回其内部所有class包含red的节点：
var reds = document.getElementById('test-div').getElementsByClassName('red');

// 获取节点test下的所有直属子节点:
var cs = test.children;

// 获取节点test下第一个、最后一个子节点：
var first = test.firstElementChild;
var last = test.lastElementChild;
```

### 更新DOM

拿到一个DOM节点后，我们可以对它进行更新。

可以直接修改节点的文本，方法有两种：

一种是修改`innerHTML`属性，这个方式非常强大，不但可以修改一个DOM节点的文本内容，还可以直接通过HTML片段修改DOM节点内部的子树：

```javascript
// 设置文本为abc:
p.innerHTML = 'ABC'; 
```

第二种是修改innerText或textContent属性，这样可以自动对字符串进行HTML编码，保证无法设置任何HTML标签;

```javascript
// 设置文本:
p.innerText = '<script>alert("Hi")</script>';
// HTML被自动编码，无法设置一个<script>节点:
// <p id="p-id">&lt;script&gt;alert("Hi")&lt;/script&gt;</p>
```

### 插入DOM

当我们获得了某个DOM节点，想在这个DOM节点内插入新的DOM，应该如何做？

如果这个DOM节点是空的，例如，<div></div>，那么，直接使用innerHTML = `child`就可以修改DOM节点的内容，相当于“插入”了新的DOM节点。

如果这个DOM节点不是空的，那就不能这么做，因为innerHTML会直接替换掉原来的所有子节点。

有两个办法可以插入新的节点。一个是使用appendChild，把一个子节点添加到父节点的最后一个子节点。例如：

```html
<!-- HTML结构 -->
<p id="js">JavaScript</p>
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
</div>
```

把<p id="js">JavaScript</p>添加到<div id="list">的最后一项：

```javascript
var
    js = document.getElementById('js'),
    list = document.getElementById('list');
list.appendChild(js);
```

更多的时候我们会从零创建一个新的节点，然后插入到指定位置：

```javascript
var
    list = document.getElementById('list'),
    haskell = document.createElement('p');
haskell.id = 'haskell';
haskell.innerText = 'Haskell';
list.appendChild(haskell);
```

动态创建一个节点然后添加到DOM树中，可以实现很多功能。举个例子，下面的代码动态创建了一个<style>节点，然后把它添加到<head>节点的末尾，这样就动态地给文档添加了新的CSS定义：

```javascript
var d = document.createElement('style');
d.setAttribute('type', 'text/css');
d.innerHTML = 'p { color: red }';
document.getElementsByTagName('head')[0].appendChild(d);
```

### 删除DOM

删除一个DOM节点就比插入要容易得多。

要删除一个节点，首先要获得该节点本身以及它的父节点，然后，调用父节点的removeChild把自己删掉：

```javascript
// 拿到待删除节点:
var self = document.getElementById('to-be-removed');
// 拿到父节点:
var parent = self.parentElement;
// 删除:
var removed = parent.removeChild(self);
removed === self; // true
```

注意到删除后的节点虽然不在文档树中了，但其实它还在内存中，可以随时再次被添加到别的位置。







