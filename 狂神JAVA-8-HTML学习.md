# HTML 详解

## 初识HTML

超文本标记语言  Hyper Text Markup Language(超文本标记语言)  -超文本包含文字、图片、音频、视频、动画等



世界知名浏览器厂商对HTML5的支持

微软、Google、苹果、Opera、Mozilla

市场的需求

跨平台

W3C 万维网联盟 World Wide Web Consortium ,成立于1994年，web技术领域最权威和具有影响力的国际中立性技术标准结构



W3C的标准包括:

- 结构化标准语言-html，xml
- 表现标准语言 css
- 行为标准 Dom ECMAScript（JavaScript）

常见的IDE

记事本，Dreamweaver，IDEA，WebStorm



## 网页基本标签

自闭和标签、开放标签  <meta charset="UTF-8">

闭合标签  <title>我的第一个页面</title>

```
 <!--DOCTYPE  告诉浏览器，我们要使用什么规范-->
```

idea快捷注释 ctrl +//

示例代码注释

```html
<!--DOCTYPE  告诉浏览器，我们要使用什么规范-->
<!DOCTYPE html>
<!--html 这是一个总标签，所有页面内容要包含在这里-->
<html lang="en">
<!--head  代表网页的头部-->
<head>
    <!--meta 描述性标签，用于描述网站的一些信息，一般用来做seo使用-->
    <meta charset="UTF-8">
    <meta name="keywords" , content="狂神说Java，西部开源">
    <meta name="description" , content="来这儿可以学习Java">

    <!-- title 网站的标题，在标签头部显示-->
    <title>我的第一个页面</title>
</head>
<!---->
<body>
    Hello,World！
</body>
</html>
```

标题标签 	 <title>标题标签</title> <h1>关闭标签</h1> <h2>关闭标签</h2> <h3>关闭标签</h3> <h4>关闭标签</h4> <h5>关闭标签</h5> <h6>关闭标签</h6>

段落标签	<p></p>  快捷键 p+tab键

换行标签	<br/>

水平线标签  <hr/>

字体样式标签  

```html
<strong>粗体: I love you！ </strong>
 <em>斜体标签</em>
```

注释和特殊符号标签

 

```html
<!--特殊符号-->
    <!--特殊符号-->
    空&nbsp;格   &nbsp; 表示空格
    &gt; 大于号
    &lt;  小于号
    &copy; 版权所有狂神
    &female;&male;
    <br>
```



## 图像、超链接

```html
 <!--
	a标签
	href必填，表示要跳转到那个页面
	target： 表示窗口在哪儿打开
		_blank  在新标签打开页面
		_self  在自己网页中打开
	锚链接
		1. 需要一个标记
		2.使用 #name
	功能性连接：
		带有功能
-->

	<a href="https://www.baidu.com" target="_blank">点击我跳转新页面
        <img src="../resources/image/1.jpg" alt="狂神头像" title="悬停蚊子" width=300 heidht=300>
	</a>


	<a href="#top" target="_blank">点击我跳转锚链接</a>
	功能连接
	QQ
	<a href="mailto:xxxx@xxx.com" target="_blank">点击我跳转锚链接</a>
	
```

## 块元素和块内元素





## 列表、表格、媒体元素

块元素，无论内容多少。该元素独占一行叫做块元素

行元素  能放在一行叫做行内元素

**列表**

无序列表	，一般用在导航上

```html
<ul> 导航用无序
    <li>java</li>
</ul>
```

**有序列表**

```html
<ol>
    <li>java</li>
</ol>
```

**自定义列表**  公司网站底部经常用自定义列表

```
<!--自定义标签-->
<dl>
    <dt>学科</dt>
    <dd>java</dd>
    <dd>python</dd>
    <dd>C</dd>
</dl>
```

**表格**，简单通用，应用广泛 ，基本单元格，行，列

```html
<table border="0.5px">
<!--clospan 跨列 rowspan跨列 -->
    <tr>
        <td colspan="4">1-1</td>
        <td rowspan="2">1-1</td>
        <td></td></tr>
    <tr><td>1-1</td><td></td><td></td></tr>
    <tr><td>1-3</td><td></td><td></td></tr>
    
</table>
```

**视频+音频**

```html
    <!--src  资源路径   controls    控制条  autoplay 自动播放-->
    <video src="../resourse/video/1.mp4" autoplay controls></video>
    <autio src="../resourse/video/1.mp3" autoplay controls></autio>
```

**页面的结构**

Header	页面头部

Footer	页面脚步

section	web页面中一块独立的区域

article	独立文章内容

aside	相关内容或者应用，常用语侧边栏

nav	导航类辅助内容



网页里面内嵌网页（内联）

Iframe

```html
<!--内联框架-->
<iframe src="path" name="mainFrame" frameborder="0"></iframe>
```



## 表单及表单应用(重点)

```html
<form action="result.html" method="post">
        <p>姓名: <input type="text" name="name"></p>
        <p>密码: <input type="password" name="pass"></p>
        <p>
            <input type="submit" name="Button" value="提交">
            <input type="reset" name="Reset" value="重填">
        </p>
</form>
```

action:     表单提交的位置，可以是网站，也可以是一个请求的地址

method： post，get提交方式  get方式提交可以在url中看到我们提交的信息，不安全但是高效    post 比较安全，并且可以传输大文件



type  指定元素类型，text,password,checkbox,radio,submit,reset,file,hidden,image,button ,

name	指定表单元素的名称

value  元素初始值，type为radio时，必须指定一个值 默认初始值

size	指定表单元素的初始宽度，设置输入框长度限制，文本框长度

maxlength ="8"  最长能写几个字符

checked  type为radio或者checkbox时，指定按钮是否被选中

**单选框**

```html
<p>
    <input type="radio" value="boy" name="sex">男
    <input type="radio" value="boy" name="sex">女
</p>
<!--注意 在type=radio时候，name是起到分组的作用， name需要一样-->
```

**多选框**

```html
<p>
    <input type="checkbox" value="sleep" name="hobby">睡觉
    <input type="checkbox" value="code" name="hobby" checked>敲代码
    <input type="checkbox" value="chat" name="hobby">聊天
    <input type="checkbox" value="game" name="hobby">游戏
</p>
<!--注意 在type=radio时候，name是起到分组的作用	name需要一样--> 默认选中checked
```

**下拉框**

```html
    <p>
        <select name="列表名称" id="">
            <option value="1">中国</option>
            <option value="2">瑞士</option>
            <option value="3">美国</option>
            <option value="4">英国</option>
            <option value="5" selected>日本</option>
        </select>
    </p>
```



**文本域**

```html
<p>
    <textarea name="textarea" id="1110" cols="30" rows="10">这是一个文本域</textarea>
</p>
```



**滑块**

```html
<!--滑块-->
<p>音量:
    <input type="range" name="voice" min="0" max="100" step="2">
</p>
```

**搜索框**

```html
<!--搜索框-->
<p>搜索:
    <input type="search" name="search">
</p>
```



disable  hidden	readonly	checked	



**增强鼠标可用性**

```html
<!--    增强鼠标的可用性-->
    <p>
        <label for="mark">你点我试试:</label>
        <input type="text" id="mark">
    </p>
```







## 表单初级验证(重点)



为什么需要表单验证？

​	减轻服务器压力并保证数据安全!



required 不能为空，必须要填写 可用在文本框中

placeholder	默认提示信息	可用在控件上

pattern	正则表达式，可以自定义一个正则表达式	

```html
<!--正则表达式去查找-->
<p>自定义邮箱:
    <input type="text" name="diyname" pattern="^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$">
</p>
```

