# JavaWeb入门到实战

## 1.JAVAWEB基本概念

### 1.1 前言

web开发：

	- web 网页的意思
 - 静态web
   	- html， css，
      	- 提供给所有人看的数据始终不会发生变化
 - 动态web
   	- 提供给所有人看的数据，会发生变化，每个人在不同的时间不同的地点看到的信息各不相同
      	- 淘宝，几乎所有的网站都是
      	- 技术栈  servlet/jsp  asp  php

在java中，动态web资源开发的技术统称为javaweb；



### 1.2 web应用程序

web应用程序：可以提供浏览器访问的程序

​	a.html  b.html 多个web资源被整合起来就可以对外提供web服务

​	一个web应用由多个部分组成(静态web,动态web)

	-	html css js
	-	jsp servlet
	-	java程序
	-	jar包
	-	配置文件(Properties)

web应用程序编写完毕后，若想提供给外界访问，需要一个服务器来统一管理；



### 1.3. 静态web

- *.htm *.html 这些都是网页文件的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取
- 静态web存在的缺点
  - web页面无法动态更新，所有用户看到的都是同一个页面
    - 轮播图，点击特效，伪动态
    - javascript[实际开发，他用的最多]
    - vbscript
  - 它无法和数据库交互(数据无法持久化，用户无法交互)

### 1.4 动态web

页面会动态展示: web的页面展示的效果因人而异

![image-20201121184950790](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201121184950790.png)

缺点：

 - 加入服务器的动态web资源出现了错误，我们需要重新编写我们的后台程序，重新进行发布；
   	- 停机维护

优点：

	- web页面可以动态更新，所有用户看到的都可以定制化
	- 可以和数据库进行交互，数据可以持久化(可以和数据库进行访问)



新手村:--->魔鬼训练(分析原理，看源码)--->pk场



## 2. web服务器

### 2.1 web的一些开发技术

ASP:  微软的，早些年非常流行，在HTML中嵌入了VB的脚本，ASP+COM，在ASP开发中，基本一个页面都有几千行的业务代码，页面及其混乱，维护成本特别高！ 主要使用C#语言

jsp/servlet:

- sun公司主推的B/S架构(浏览器和服务器)，C/S架构(客户端和服务器)
- 主要的实现语言是基于JAVA语言(所有的大公司或一些开源的组件，都是用JAVA写)
- 可以承载高并发，高可用，高性能
- 语法结构像asp，加强市场的强度; ASP-->JSP



PHP  

- Php开发速度很快，功能很强大，跨平台，代码很简单(70% wordpress);
- 无法承载大访问量情况, 有一些局限性!



### 2.2 web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息。

Tomcat

百度一下tomcat的资料

IIS : 微软的公司，主要跑一些ASP的程序，Windows自带的



## 3.Tomcat安装及配置

### 3.1 Tomcat安装

![image-20201121194019952](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201121194019952.png)

tomcat官网：https://tomcat.apache.org/



### 3.2 Tomcat启动和配置

文件夹作用和信息

![image-20201121193937523](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201121193937523.png)





**启动.关闭 Tomcat**

启动 startup.bat

关闭 shutdown.bat

访问http://localhost:8080

可能遇到的问题：

​	1、java环境变量么有配置，tomcat会闪退，需要配置java_home,java_path

​	2、闪退问题

​	3、 乱码问题

### 3.3 Tomcat配置

![image-20201121200020206](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201121200020206.png)

**可以配置启动的端口号 port**

    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
tomcat 默认端口号 8080

mysql默认端口号3306

HTTPS 默认端口 443

HTTP 默认端口 80

**可以配置启动的appBase:webapps，可以配置启动的Host主机名称:localhost** 

```xml
  <Host name="localhost"  appBase="webapps"
        unpackWARs="true" autoDeploy="true">
        
   <Host name="www.qinjiang.com"  appBase="webapps"
        unpackWARs="true" autoDeploy="true">       
        
```
高难度面试题：

请你谈谈网站是如何进行访问的! 百度作业! DNS解析流程

用户----> 本机浏览器----------------用户发起请求--------------------------->查询本机etc/hosts



本机浏览器----------------3.用户发起请求------------------------------------->LocalDNSServer

-

LocalDNSServer-----------4.请根域名发起解析请求----------------------------->Root DNS Server

LocalDNSServer<----------5.返回gTLD域名解析服务器地址---------------------Root DNS Server



LocalDNSServer-----------6.向gTLD域名解析服务器发起解析请求--------------->gTLD Server

LocalDNSServer<-----------7.返回NameServer的服务器地址--------------------gTLD Server



LocalDNSServer-----------8.查询请求的域名对应的IP地址----------------------->Name Server

LocalDNSServer<-----------9.返回域名对应的IP地址------------------------------Name Serverr



LocalDNSServer-----------10.返回域名对应的IP地址----------------------------->本机浏览器





### 3.4 配置tomcat环境变量(可选项)

```properties
##先配置环境变量
CATALINA_BASE =F:\Environment\apache-tomcat-9.0.38
CATALINA_HOME = F:\Environment\apache-tomcat-9.0.38

## 配置path
%CATALINA_HOME%\lib
%CATALINA_HOME%\bin
```



### 3.5 发布一个web网站

不会就先模仿

 - step1 将自己写的网站，放到服务器tomcat中指定的web应用的文件夹(webapps)下，就可以出来了；

 - step2 网站应该有的结构如下图

   ```java
   --webapps : Tomcat服务器的web目录
   	---Root
   	---kuangstudy:网站的目录名
   		- WEB-INF		放网站的程序
           	--class  java程序
           	--lib     web应用锁依赖的jar包
   			--web.xml  网站的配置文件
   		- index.html 默认的首页
   		- static
   			-css
   				-style.css
   			- js
   			- img
   		- .....
   ```

   

## 4. HTTP

### 4.1 什么是HTTP

百度查找



### 4.2 两个时代

- http1.0
  - http1.0  客户端可以与服务器连接后，只能获得一个web资源，断开连接
- http2.0
  - http2.0  客户端可以与服务器连接后，只能获得一个web资源，断开连接

### 4.3 http请求(Request)

[http协议讲解](https://blog.csdn.net/sinat_41620463/article/details/81019962)

http协议有个request，分为request 和response 
从浏览器向服务器发起请求一般用resquest，从服务器返回一般是response
每个resquest和response 都由三部分组成，分别为 Line、header 、body
line 在request 中可能是 get: ![img](file:///C:\Users\admin\AppData\Roaming\Tencent\QQTempSys\[5UQ[BL(6~BS2JV6W}N6[%S.png)http://www.baidu.com http 1.1
    在response中可生是 status:200 ok
header： 即可由服务器设置，告诉浏览器按照什么约束执行
             也可由服务器设置，告诉服务器提供什么约束格式返回



客户端--发起请求---服务器

```
General 请求体:
Request URL  请求地址 https://www.baidu.com
Request Method get/post方法
Status Code ：200 ok  状态码 200
Remote（远程） Address 14.215.177.59:443
```

```java
Request headers 请求头:
Accept:text/html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: max-age=0
Connection: keep-alive
```

1、请求体	

	- 请求行中的请求方式，get  
 - 请求方式 get,post,head,delete,put,tract......(restful)
   	- get 请求能够携带的参数比较少，大小有限制，会在浏览器的url地址栏显示出数据内从，不安全，但是高效
    - post 请求能够携带的参数比较多，大小无限制，不会在浏览器的url地址栏显示出数据内容，安全，但不是高效

2、请求头

```
Accept  浏览器端可以接受的媒体类型
Accept-Encoding	浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法,支持哪种编码方式 gbk, utf-8 gb2312 iso8859-1
Accept-Language	浏览器申明自己接收的语言,告诉浏览器，他的语言环境
Cache-control	这个是非常重要的规则。 这个用来指定Response-Request遵循的缓存机制 Cache-Control:no-cache  所有内容都不会被缓存 缓存控制
connection	Connection: keep-alive   当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接,
告诉浏览器，请求完成是断开还是保持连接
HOST  发送请求时，该报头域是必需的）主机
```

### 4.4 http响应（Response）

```
响应头：
Cache-Control: private 	//缓存控制
Connection: keep-alive   //连接 保持连接
Content-Encoding: gzip		// 编码
Content-Type: text/html;charset=utf-8  //类型
Date: Sun, 22 Nov 2020 03:24:17 GMT
Expires: Sun, 22 Nov 2020 03:24:17 GMT
Server: BWS/1.1
Set-Cookie: BDSVRTM=313; path=/   
Set-Cookie: BD_HOME=1; path=/
Set-Cookie: H_PS_PSSID=32818_1436_33102_33119_33061_31254_33098_33100_32962_26350; path=/; domain=.baidu.com
Strict-Transport-Security: max-age=172800
Traceid: 1606015457060115431411151445472357802237
Transfer-Encoding: chunked
X-Ua-Compatible: IE=Edge,chrome=1
```



2、响应体，和消息一样

```
Accept  告诉浏览器，服务器锁支持的数据类型
Accept-Encoding	支持哪种编码方式 gbk, utf-8 gb2312 iso8859-1
Accept-Language	告诉浏览器，他的语言环境
Cache-control	缓存控制
connection	告诉浏览器，请求完成是断开还是保持连接
HOST  主机
Refresh  告诉客户端，多久刷新一次
Location  让网页重新定位:
```



响应状态码(重点)

200 请求响应成功

404 找不到资源

​	4XX 资源不存在

3** 请求重定向

	- 重定向 你重新定位到我给你的新位置去；

5xx  服务器代码错误  500 502网关错误



## 5. Maven框架

我为什么要学习这个技术？

​	在javaweb开发中，需要导入大量的jar包，我们手动导入，很繁琐，如何让一个东西自动帮我们导入和配置这个jar包，由此Maven诞生了，Maven是一个工具，帮助我们来管理jar包，是一个项目架构管理工具! 我们用他就是方便我们导入jar包!

​	Maven核心思想：约定大于配置!

	-	有约束，不要去违反
	-	Manven会规定好你改如何去编写我们的Java代码，必须要按照这个规范去走!

安装Maven: 官网地址：https://maven.apache.org/index.html

![image-20201122120620436](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122120620436.png)

Maven对应的目录解释:



小狂神友情建议，下载后所有的文件放到一个文件夹下面管理

### 5.1 配置Maven环境变量

step1. 进入我们的系统环境变量中，配置如下的配置：

M2_HOME   **F:\Environment\apache-maven-3.6.3\bin**

MAVEN_HOME  **F:\Environment\apache-maven-3.6.3**

![image-20201122121306352](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122121306352.png)

step2 .进入系统的path，配置MAVEN_HOME

​	%MAVEN_HOME%\bin

![image-20201122121429863](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122121429863.png)

经过上述步骤，就配置成功了,可以在命令行执行 mvn -version进行测试

![image-20201122121751759](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122121751759.png)

注意：配置M2_HOME的原因是因为后续学习spring_boot， spring等框架都需要用到这个目录

### 5.2 配置阿里云镜像

镜像 mirrors 加速下载作用

添加步骤：

​	1、 进入自己的maven安装目录，找到F:\Environment\apache-maven-3.6.3\conf文件夹，

​	2、打开 settings.xml 并在文件的  <mirrors></mirrors>中间添加如下配置

```xml
  <mirrors>
     <!-- 阿里云仓库 -->
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
  </mirrors>
```

### 5.3 配置本地仓库

本地仓库 远程仓库

建立一个本地仓库，存储网络下载下来的jar包 

操作步骤：

1、 进入自己的maven安装目录，找到F:\Environment\apache-maven-3.6.3\conf文件夹，

​	2、打开 settings.xml 并在文件的   <localRepository></localRepository>中间添加如下配置

```xml
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>F:\Environment\apache-maven-3.6.3\maven-repo</localRepository>
  -->
  <localRepository>F:\Environment\apache-maven-3.6.3\maven-repo</localRepository>
```



### 5.4 在IDEA中使用MAVEN

step1 启动IDEA，创建一个Maven项目

![image-20201122125403471](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122125403471.png)

step2点击Next, 配置项目的信息

![image-20201122125651754](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122125651754.png)

​		上述填写 

​			- groupid: top.aigoo

​			- Artifactid: javaweb-01-maven

​			- name: javaweb-01-maven

step3 点击Next，配置maven的本地地址;

![image-20201122130348157](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122130348157.png)

至此就创建成功，进入IDEA等待项目初始化完毕

![image-20201122130919974](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122130919974.png)

Step4. IDEA中的MAVEN设置;

​	注意事项:创建完项目，需要进入到Setting里面看一眼Maven配置，确认是否是本地的配置。进入方式是 File--->settings--->Build Execution Deployment-->build tools--->Maven

![image-20201122131540259](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122131540259.png)



Step5. 到这里，maven在idea中的配置和使用就OK了



### 5.5 创建一个普通的Maven项目

我们可以通过创建一个meven，但是不选择模板的方式创建一个干净的Maven项目

![image-20201122134806617](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122134806617.png)



所以，使用模板创建的项目需要补充java 和resources文件夹，如下图

![image-20201122140114318](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122140114318.png)



### 5.6 在IDEA中配置TOMCAT

Step1 先点击Add Configuration

![image-20201122140611198](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122140611198.png)

Step2 。选择Tomcat Server

![image-20201122140652058](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122140652058.png)

Step3 .填写对应的tomcat配置信息

![image-20201122141423459](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122141423459.png)

Step4.新建一个tomcat artifacts

![image-20201122141809214](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122141809214.png)

解决警告的问题，为什么会有这个问题：我们访问一个网站，需要指定一个文件夹的名字，而且必须要配置一个artifacts

Step 5.关于虚拟路径的设置

![image-20201122142255188](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122142255188.png)



### 5.7 pom文件

pom.xml是maven的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--Maven的版本和头文件-->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
<!--  这里是我们刚才配置的GAV-->
  <groupId>top.aigoo</groupId>
  <artifactId>javaweb-01-maven</artifactId>
  <version>1.0-SNAPSHOT</version>
<!--  package：项目的打包方式
    jar:java应用
     war javaweb应用-->
  <packaging>war</packaging>

  <name>javaweb-01-maven Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>
  <!--配置-->
  <properties>
    <!--项目的默认构建编码-->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--编码的版本-->
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
  <!--项目依赖-->
  <dependencies>
    <!--具体依赖的jar包的配置文件-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
    </dependency>

  </dependencies>
<!--项目构建用的东西-->
  <build>
    <finalName>javaweb-01-maven</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```

**maven 由于他的约定大于配置，我们之后可能会遇到我们写的配置文件，无法被导出或者生效的问题，而我们解决方案：**

```xml
<!--在build中配置resources,来防止我们资源导出失败的问题-->
    <build>
      <resources>
        <resource>
            <directory>src/main/resources</directory>
            <excludes>
                <exclude>**/*.properties</exclude>
                <exclude>**/*.xml</exclude>
             </excludes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
    </build>
```

### 5.8 解决遇到的问题

1、Maven3.6.2 版本问题，所有依赖无法导入

2、Tomcat闪退

3、IDEA中每次都需要重复配置Maven

4、Maven项目中Tomcat无法配置

5、maven默认web项目中web.xml的版本问题，替换为tomcat中ROOT下xml一样的文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">

  
</web-app>
```

7、Maven仓库的使用

直接百度Maven Responsity  https://mvnrepository.com/



### 5.9 编写自己第一个HttpServlet

step1.首先，我们创建一个Maven项目，如上述步骤，并在src/main/java下新建一个包 top.aigoo，在这个包里面建立一个HelloServlet的java文件

![image-20201122165118470](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122165118470.png)

学习小技巧:

 - 如何快速知道去写一个servlet程序

   在tomcat的examples，有很多的例子，我们可以启动tomcat，然后访问examples里面的文件，然后找到自己需要的例子就可以

 - 如何去快速把项目的依赖包加载到Maven中

   可以在项目中，引入包的文件，点下 ctrl+enter ，选择Add to Maven，这样可以在本地的Maven库进行查找

- 如何快速定位HttpServlet所来自的包?

  首先，我们看tomcat可以启动这个例子，所以这个jar包在tomcat中肯定有，我们可以去tomcat/lib目录找一些相似的，进入里面可以发现有个名字叫做servlet-api.jar

  然后，我们可以去maven仓库，搜索httpservlet，但是没有搜索到，然后尝试搜索servlet-api，我们能够找到很多个servlet-api，这个时候参照tomcat里面例子代码，可以知道是来自于javax.servlet.XXX，所以我们选择一个类似的

  我们找到这个包，把dependce加入到自己项目的pom.xml中，并刷新；

  在这个过程中，我们还发现加了这个包并未正常运行，所以我们可以选择ctrl+enter 然后直接add

Step2. 编写我们的servlet文件

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        PrintWriter out = response.getWriter();
        out.println("<html>");
        out.println("<head>");
        out.println("<title>大家是个好学生!</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("<h1>大家是个好学生!</h1>");
        out.println("</body>");
        out.println("</html>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

step3. 编辑我们的web.xml，这个是项目的主要文件，我们需要进去配置我们的路由

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
        <!--首先我们需要配置servlet，name可以随便写，class需要和自己的servlet对应-->
        <servlet>
            <servlet-name>helloServlet</servlet-name>
            <servlet-class>top.aigoo.HelloServlet</servlet-class>
        </servlet>
        <!--还需要配置servlet和自己路由的配对， 这个意思从浏览器http://localhost:8080/kuang/kuang2 就可以访问，-->
        <servlet-mapping>
            <servlet-name>helloServlet</servlet-name>
            <url-pattern>/kuang2</url-pattern>
        </servlet-mapping>
</web-app>

```



备注：

```
<!--首先我们需要配置servlet，name可以随便写，class需要和自己的servlet对应-->
 <!--还需要配置servlet和自己路由的配对， 这个意思从浏览器http://localhost:8080/kuang/kuang2 就可以访问，其中/Kuang/ 是在tomcat里面配置的Deployment里面的Application context，-->
```

![image-20201122170221397](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122170221397.png)

### 5.10 关于父子项目遇到的一些坑

- 问题1、 安装了父子项目后，子项目无法继承父项目的依赖，同时子pom.xml变为灰色

  解决办法：进入settings-->Maven, 找到ignoredFiles, 将父pom.xml前面的√取消掉

- 问题2.生成子项目时候，刚开始的parent属性被自动刷掉了

  解决办法：待生成项目时候，拷贝下来，等子pom稳定后，在覆盖回去，或者直接手打 

  ```
    <parent>
      <artifactId>javaweb-02-servlet</artifactId>
      <groupId>top.aigoo</groupId>
      <version>1.0-SNAPSHOT</version>
    </parent>
  ```

- 问题3 删掉子项目，重新建立Module,在输入项目名，groupid和artifactid显示红色

  因为有缓存，关闭项目，重新打开就可以了

- 问题4. 在子项目的target---[WEB-INF]里面没有lib目录

  通过上述问题，勾选掉父pom.xml文件，然后这个问题得到了解决了!

## 6. servlet

### 6.1 servlet简介

Sun公司开发动态web的一门技术

Sun在这些API中提供一个接口叫做Servlet，如果你想开发一个动态web的程序，只需要完成两个小步骤

	- 编写一个类，实现servlet的接口
	- 把开发好的servlet类，部署到web服务器中

我们会把实现了servlet接口的java程序叫做Servlet

### 6.2 helloservlet

Servlet接口在Sun公司有一两个默认的实现类，HttpServlet，GenericServlet



**step1. 构建一个普通的maven项目，删掉里面的src目录，以后我们的学习就在里面建立module**

项目名称：javaweb-03-servlet

**step2. 关于maven父子工程的简介**

	- 在父项目中会多一个modules

```xml
    <modules>
        <module>servlet-01</module>
    </modules>
```



	- 在子项目会多一个parent（在idea 202.1版本没有自动生成这个目录，但是删掉groupid就可以正常用了）

```xml
  <parent>
    <artifactId>javaweb-02-servlet</artifactId>
    <groupId>top.aigoo</groupId>
    <version>1.0-SNAPSHOT</version>
  </parent>
```

父项目中的java,子项目可以直接使用

> son extends father

注意，如果是子类的项目，那么在pom.xml里面，就可以不要存在<groupId>top.aigoo</groupId> 属性，不然子项目会找不到父项目的jar包



**step3. Maven的环境优化**

	1. 将web.xml的配置更新为最新的版本，我们可以到tomcat/webapps/ROOT里面找到对应的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">
    
</web-app>
```

2. 添加java和resources文件夹

![image-20201122192133551](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122192133551.png)



3. 将maven的结构搭建完整



step4. 编写一个Servlet程序

	- 编写一个普通类
	- 实现Servlet接口,这里直接继承HttpServlet，Servlet的继承关系如下图

 ![image-20201122193141687](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122193141687.png)



```java
package top.aigoo.servlet;


import javax.servlet.ServletException;
import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletInputStream inputStream = req.getInputStream();
        System.out.println("doGet()");
        PrintWriter writer = resp.getWriter();
        writer.println("Hello World ,Servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



step5 编写servlet的映射

编写完成后，我们需要进入到web.xml里面，去编写servlet映射

```xml
<!--注册servlet-->
<servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>top.aigoo.servlet.HelloServlet</servlet-class>
</servlet>
<!--servlet的请求路径-->
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

step6 配置tomcat



![image-20201122194748111](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122194748111.png)

step7 启动项目

### 6.3 Servlet原理

Servlet是由web服务器调用，web服务器在收到浏览器请求之后，会：

![image-20201122215837769](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201122215837769.png)





### 6.4 Mapping问题

1、1个Servlet可以指定一个映射路径

```xml
    <!--servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello1</url-pattern>
    </servlet-mapping>
```



2、1个Servlet可以指定多个映射路径

```xml
    <!--servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello1</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello2</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello3</url-pattern>
    </servlet-mapping>
```



3、1个Servlet可以指定通用的映射路径

```xml
    <!--servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello/*</url-pattern>
    </servlet-mapping>
```

如果用/* 那就是默认请求路径，包括Index.html都会转移到这个请求地址



4、可以指定一些后缀实现请求映射

```xml
    <!--servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>*.hello</url-pattern>
    </servlet-mapping>
```

注意，*前面不能加项目的映射路径   /hello/\*.hello和     /\*.qinjiang 这个就会报错

6、优先级问题

指定了固有的mapping映射路径，优先级最高，如果找不到就会走默认的处理路径

```xml
<url-pattern>/hello</url-pattern>   //访问http://localhost:8080/s1/hello  会走这个
<url-pattern>/*</url-pattern>     
```

常用的一些方式：自定义404



解决Tomcat 在IDEA里面显示乱码的问题

- 1.Tomcat 容器下Conf文件夹logging.properties

```properties
如果是GBK改为UTF-8

java.util.logging.ConsoleHandler.encoding = UTF-8

```

- 2. 如果是IDEA2019.3以后版本，在工具栏help -》 Edit Custom VM Options，加上

> -Dfile.encoding=UTF-8

- 3、在tomcat启动参数中，加入参数

> -Dfile.encoding=UTF-8

- 在IDEA-setting-editr-fileEncodings，里面所有都改为utf-8  如下图

![image-20201123132711460](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201123132711460.png)

### 6.5 ServletContext 应用

 getServletContext（）

web容器在启动的时候，他会为每个web程序都创建一个ServletContext对象，他代表当前的web应用

- **案例1.共享数据 ，在一个servlet放置的数据，可以通过ServletContext被另一个servlet获取**

```java
//1. HelloServlet  ServletContext里面写入数据
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Hello");
        //this.getInitParameter()   //初始化参数
        //this.getServletContext()  上下文
        //this.getServletConfig()  //servlet配置
        ServletContext servletContext = this.getServletContext(); //获取当前Servlet的上下文对象
        String userName ="秦僵"; //定义一个准备写入到ServletContext中的变量
        servletContext.setAttribute("userName", userName); //添加到ServletContext

        resp.setContentType("text/html");  
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().println("刚才已经向ServletContext里面写入了一个userName:秦僵");
    }
}

//2. GetCharServlet  从ServletContext里面读数据

public class GetCharServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        String userName = (String)servletContext.getAttribute("userName");

        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().println("姓名:"+userName);
    }
}

//3. 对应的xml文件

    <!--注册servlet-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>top.aigoo.servlet.HelloServlet</servlet-class>
    </servlet>

    <servlet>
        <servlet-name>getc</servlet-name>
        <servlet-class>top.aigoo.servlet.GetCharServlet</servlet-class>
    </servlet>

    <!--映射servlet-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
    <servlet-mapping>
        <servlet-name>getc</servlet-name>
        <url-pattern>/getc</url-pattern>
    </servlet-mapping>
```

- **案例2 获取初始化参数，主要使用使用ServletContext的getInitParameter()**

```java
> 在项目servlet-02/web.xml配置初始化参数
<web-app>
    .....
    <!--配置servlet初始化参数-->
    <context-param>
        <param-name>jdbcDriver</param-name>
        <param-value>com.mysql.jdbcDriver</param-value>
    </context-param>
    <context-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost:3306/mysql</param-value>
	</context-param>
    <context-param>
        <param-name>user</param-name>
        <param-value>root</param-value>
    </context-param>
    <context-param>
        <param-name>passpord</param-name>
        <param-value>123456</param-value>
    </context-param>
    .....
</web-app>   
>>>>编写Servlet,使用ServletContext的getInitParameter()可以读取到web.xml文件的配置
 /*学习使用ServletContext获取初始化参数*/
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String url = this.getServletContext().getInitParameter("url");
        String jdbcDriver = this.getServletContext().getInitParameter("jdbcDriver");
        String user = this.getServletContext().getInitParameter("user");
        String passpord = this.getServletContext().getInitParameter("passpord");
        
        resp.getWriter().println("getInitParameter:jdbcDriver-->"+jdbcDriver);
        resp.getWriter().println("getInitParameter:url-->"+url);
        resp.getWriter().println("getInitParameter:user-->"+user);
        resp.getWriter().println("getInitParameter:passpord-->"+passpord);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}   


```

- **案例3 请求转发，主要使用使用ServletContext的getRequestDispatcher()**

```java
>> 实现ServletDemo3，使用getRequestDispatcher()请求转发
    public class ServletDemo3 extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("ServletDeme03执行!");
        ServletContext servletContext = this.getServletContext();
        RequestDispatcher requestDispatcher = servletContext.getRequestDispatcher("/sd2");
        requestDispatcher.forward(req,resp);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

>> 配置web.xml
    <servlet>
        <servlet-name>sd2</servlet-name>
        <servlet-class>top.aigoo.servlet.ServletDemo2</servlet-class>
    </servlet>

    <servlet>
        <servlet-name>sd3</servlet-name>
        <servlet-class>top.aigoo.servlet.ServletDemo3</servlet-class>
    </servlet>
            
     <servlet-mapping>
        <servlet-name>sd2</servlet-name>
        <url-pattern>/sd2</url-pattern>
    </servlet-mapping>

    <servlet-mapping>
        <servlet-name>sd3</servlet-name>
        <url-pattern>/sd3</url-pattern>
    </servlet-mapping>          
```

注意，请求转发和重定向是不同的

请求转发是在服务器进行处理，直接转发到另一个请求，在前台看到的url不变，

重定向，是返回新的url给浏览器，浏览器再次去请求新的url地址



- **案例4 读取资源文件，项目大量用到Properties**

properties

	-	在java路径下新建properties
	-	在resources目录下新建properties

发现:都被大宝到了同一个路径下:classes,我们俗称这个路径为类路径

思路：需要一个文件流!

**做这题时候碰到一个坑： 因为之前在系统的环境变量没有配置tomcat，然后再这儿使用maven遇到问题，maven可以将resources里面资源打包到war包，但是用tomcat启动生成的war包，就没有resources资源，只有 src/main/java下的资源**

解决办法： 设置系统的环境变量

CATALINA_BASE  =F:\Environment\apache-tomcat-9.0.38

CATALINA_HOME = F:\Environment\apache-tomcat-9.0.38

PATH添加

%CATALINA_HOME%\lib

%CATALINA_HOME%\bin



注意： 

第一: 我们的db.properties需要放到resources，因为我们执行编译后，maven会把这个文件直接放入到类路径下 /WEB-INF/classes下面；

第二：我们如果把db.properties里面，如果没有在子项目servlet-02的web.xml里面放入build参数，会导致无法将文件构建到项目中

```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```

```properties
jdbcDriver=com.mysql.jdbcDriver
url=jdbc:mysql://localhost:3306/mysql
user=root
passpord=123456
```

```java
public class ServletDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        InputStream resourceAsStream = servletContext.getResourceAsStream("/WEB-INF/classes/db.properties");
        Properties prop = new Properties();
        prop.load(resourceAsStream);

        resp.getWriter().println("jdbcDriver---->"+prop.getProperty("jdbcDriver"));
        resp.getWriter().println("url---->"+prop.getProperty("url"));
        resp.getWriter().println("user---->"+prop.getProperty("user"));
        resp.getWriter().println("passpord---->"+prop.getProperty("passpord"));

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

### 6.6 HttpServletRequest

#### 1.什么是HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过HTTP协议访问服务器时候，HTTP请求中的所有信息会被封装到HttpServletRequest,通过HttpServletRequest的方法，获得客户端的所有信息。



#### 2、获取前端参数并请求转发

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;
import java.util.Set;

public class RequestDemoServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("我进来这个方法了!");
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] checkboxes = req.getParameterValues("hobbys");

        System.out.println("=====================");
        System.out.println("username:"+username);
        System.out.println("password:"+password);
        System.out.println("checkboxes"+Arrays.toString(checkboxes));
        System.out.println("=====================");

        System.out.println(req.getContextPath());
        Set<String> realPath = this.getServletContext().getResourcePaths("/");
        for(String s: realPath){
            System.out.println("---:"+s);
        }
        System.out.println("realPath"+realPath);
        //通过请求转发
        //这了的/代表的是当前web的应用
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



### 6.7 HttpServletResponse响应

web服务器接收到客户端的http请求，会针对这个请求分别创建一个代表请求的HttpServletRequest对象和一个代表响应的HttpServletResponse对象；我们只需要

	- 如果要获取客户端请求过来的参数， 找HttpServletRequest
	- 如果要给客户端响应一些信息，找HttpServletResponse

#### 1 .对HttpServletResponse的方法

第1类、负责向浏览器发送数据的方法

```java
ServletOutputStream getOutputStream() throws IOException  //写其他字符
PrintWriter getWriter() throws IOException  //写中文
```

第2类、负责向浏览器发送一些响应头的方法

```java
void setCharacterEncoding(String charset)
void setContentLength(int len)
void setContentLengthLong(long len)
void setContentType(String type)
void setBufferSize(int size)
void setHeader(String name, String value)
void setIntHeader(String name, int value)
void setStatus(int sc)
void setStatus(int sc, String sm)
void setDateHeader(String name, long date)
```

> 3. 常见应用

1、向浏览器输出消息getOutputStream()  getWriter()，前面一直在讲

#### 2、下载文件应用，一般步骤如下

	- 1.要获取下载文件的路径
	- 2.下载的文件名是什么
	- 3.设置想办法让浏览器能够下载我们需要的东西
	- 4.获取下载文件的输入流
	- 5.创建缓冲区
	- 6.获取OutPutStream对象
	- 7.将FileOutputStream流写入到buffer缓冲区
	- 8.使用OutputStream将缓冲区中的数据输出到客户端

```java
public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1.要获取下载文件的路径
        String realPath = this.getServletContext().getRealPath("/WEB-INF/classes/1.png");
        Set<String> resourcePaths = this.getServletContext().getResourcePaths("/WEB-INF/classes/");
        Iterator<String> iterator = resourcePaths.iterator();
        while (iterator.hasNext()){
            String str = iterator.next();
            System.out.println("getResourcePaths(\"/WEB-INF/classes/\""+str);

        }
        System.out.println("下载的文件路径-->" + realPath);

        // 2.下载的文件名是什么
        String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
        // 3.设置想办法让浏览器能够下载我们需要的东西
        resp.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(fileName,"utf-8"));
        // 4.获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);

        // 5.创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];

        // 6.获取OutPutStream对象
        ServletOutputStream outputStream = resp.getOutputStream();
        // 7.将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端
        while ((len = in.read(buffer)) > 0) {
            outputStream.write(buffer, 0, len);
        }

        // 8.关闭操作
        in.close();
        outputStream.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
```

> 几个知识点：
>
> ​	1、通过 this.getServletContext().getResourcePaths("/WEB-INF/classes/")，能够把classes下面的文件路径都给获取到
>
> ​	2、获取路径下的文件名可以使用	realPath.substring(realPath.lastIndexOf("\\") + 1);
>
> ​	3、实现下载需要在服务器设置响应头信息  resp.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(fileName,"utf-8"));
>
> ​	URLEncoder.encode(fileName,"utf-8")); 这个的作用是保证中文的名字也可以支持处理
>
> ​	4、从文件读取文件并发送出去的方式
>
> ​	先读文件流
>
> ​	FileInputStream in = new FileInputStream(realPath);
>
> ​	建立缓冲区
>
> ​	int len =0
>
> ​	byte [] byte =new byte[1024];
>
> ​	建立输出流
>
> ​	ServletOutputStream st = resp.getOutputStream() 
>
> ​	开始输出
>
> ​	while((len=in.read(byte)>0){
>
> ​			st.write(byte,0,len);
>
> ​		}
>
> ​	最后关闭流
>
> ​	st.close()
>
> ​	in.close()

#### 3、通过HttpServletResponse实现验证功能

验证码怎么来的

```java
public class ImageCodeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setHeader("refresh", "3");//设置浏览器3秒刷新一次
        //在内存中得到图片,
        BufferedImage image = new BufferedImage(100, 50, BufferedImage.TYPE_INT_RGB); //获得画板

        Graphics2D g = (Graphics2D) image.getGraphics(); //获得画板的笔
        //把图片的背景色画成白色
        g.setColor(Color.white);
        g.fillRect(-20, 0, 100, 20);
        //给图片协商数据
        g.setColor(Color.red);
        g.setFont(new Font(null, Font.BOLD, 20));
        g.drawString(makeNum(), 0, 20);
        //设置浏览器支持图片无缓存
        resp.setContentType("image/jpeg");
        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires", -1);
        resp.setHeader("Cache-Control", "no-cache");
        resp.setHeader("pragma", "no-cache");

        //发送图片给浏览器
        boolean send = ImageIO.write(image, "jpg", resp.getOutputStream());
        if (send)
            System.out.println("发送成功了!");

    }
	//生成7位数字
    private String makeNum() {
        Random random = new Random();
        String num = random.nextInt(999999) + "";
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < 7 - num.length(); i++) {
            sb.append("0");
        }
        num = sb.toString() + num;
        return num;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```





#### 4、实现重定向



![image-20201124131114748](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201124131114748.png)



```java
public class RequestTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("进入这个请求了!");
        //处理请求
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        System.out.println("username:"+username+"   password:"+password);
        //重定向的时候一定要注意，路径问题，否则就会404
        resp.sendRedirect("/s/success.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

```jsp
<html>
    <body>
        <%--这里提交的路径需要寻找到项目的路径，项目的路径怎么找，用${}--%>
        <%--${pageContext.request.contextPath}  这个代表当前的项目地址--%>
        <form action="${pageContext.request.contextPath}/login" method="get">
            用户名:<input type="text" name="username"><br>
            密码:<input type="password" name="password"><br>
            <input type="submit">
        </form>
	</body>
</html>

```





重定向和转发的区别：面试题

相同点

​	-页面都可以实现跳转

不同点

	- 请求转发的时候。url不会产生裱花
	- 重定向时候，url地址栏会发生变化

#### 5.[获取资源路径](https://www.cnblogs.com/ncy1/articles/8811238.html)

```
1、xxx.class.getClassLoader().getResource(“”).getPath(); 
获取src资源文件编译后的路径（即classes路径） 


2、xxx.class.getClassLoader().getResource(“文件”).getPath(); 
获取classes路径下“文件”的路径 


3、xxx.class.getResource(“”).getPath()； 
缺少类加载器，获取xxx类经编译后的xxx.class路径 


4、this.getClass().getClassLoader().getResource(“”).getPath()； 
以上三种方法的另外一种写法 


5、request().getSession().getServletContext().getRealPath(“”)； 
获取web项目的路径 
```

![image-20201223221014038](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201223221014038.png)

#### 6 web读取路径的问题

项目结构

![image-20201223224624625](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201223224624625.png)



```java
public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("=======================");

        String path0 = this.getClass().getClassLoader().getResource("").getPath();
        String path01 = this.getClass().getClassLoader().getResource("1.png").getPath(); //Null
        String path011 = this.getClass().getClassLoader().getResource("/1.png").getPath(); //Null
        //String path0111 = this.getClass().getClassLoader().getResource("/WEB-INF/classes/11.png").getPath(); //Null

        String path2 = this.getClass().getResource("").getPath();
        String path21 = this.getClass().getResource("/1.png").getPath();

        //其实通过ServletContext().getRealPath来拼接路径只是绝对硬盘路径的升级版
        String path3 = this.getServletContext().getRealPath("");
        String path4 = this.getServletContext().getRealPath("resources/r1.png");


        System.out.println("获取src资源文件编译后的路径（即classes路径） "+path0);
        System.out.println("path01="+path01);
        System.out.println("path011="+path011);
        //System.out.println("path0111="+path0111);
        System.out.println("缺少类加载器，获取xxx类经编译后的xxx.class路径 "+path2);
        System.out.println("path21 "+path21);
        System.out.println("获取web项目的路径  "+path3);
        System.out.println("获取1.png的路径 "+path4);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

=======================
获取src资源文件编译后的路径（即classes路径） /F:/Environment/apache-tomcat-9.0.38/webapps/s/WEB-INF/classes/
path01=/F:/Environment/apache-tomcat-9.0.38/webapps/s/WEB-INF/classes/1.png
path011=/F:/Environment/apache-tomcat-9.0.38/webapps/s/WEB-INF/classes/1.png
缺少类加载器，获取xxx类经编译后的xxx.class路径 /F:/Environment/apache-tomcat-9.0.38/webapps/s/WEB-INF/classes/com/kuang/servlet/
path21 /F:/Environment/apache-tomcat-9.0.38/webapps/s/WEB-INF/classes/1.png
获取web项目的路径  F:\Environment\apache-tomcat-9.0.38\webapps\s\
获取1.png的路径 F:\Environment\apache-tomcat-9.0.38\webapps\s\resoutces\r1.png
```



## 7 Session Cookie

### 7.1 什么是session和cookie

回话：用户打开一个浏览器，然后点击了很多超链接，访问了多个web资源，关闭浏览器，这个过程就叫做一个绘画

有状态回话：

你能怎么证明你是西开的学生？

1、发票	西开

西开给你发票，

2、学校登记	西开标记你来过来



一个网站如何证明你来过？

客户端		服务端

​	1、服务器给客户端一个新建，客户端下载访问服务端带上新建就可以了； cookie

​	2、服务器登记你来过了，下次你来得时候我来匹配你 session

### 7.2 保存会话的两种技术

cookie

​	- 客户端技术，通过响应和请求

session		

	- 服务器技术，利用这个技术，可以保存用户的会话信息？我们可以吧信息或者数据保存在session

常见的用力：  网站登录后，你下次就不用继续登录了



### 7.3 示例代码

```java
/*服务器，告诉你，你来得时间，把这个时间封装成为一个新建，你下次带来，我就知道你来了*/
public class CookieDemo01Servlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决中文的乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8"); //不加这个，在浏览器访问时候会显示为乱码

        PrintWriter out = resp.getWriter();
        //
        Cookie[] cookies = req.getCookies();
        if (cookies!=null){
            //如果存在怎么办？我们取出来数据
            out.write("你上一次访问的时间是：");
            for (int i = 0; i < cookies.length; i++) {
                Cookie cookie = cookies[i];
                if (cookie.getName().equals("lastLoginTime")){
                    //获取cookie的值
                    String value = cookie.getValue(); //拿出来的是字符串，要转换成长整型才能变成时间
                    System.out.println(value);
                    long lastLoginTime = Long.parseLong(value); //使用Long包装类转换成长整型
                    Date date = new Date(lastLoginTime); //使用Date转化为时间 变为date对象
                    out.write(date.toLocaleString());
                }
            }
        }else {
            out.write("你是第一次访问!");
        }

        //服务器给客户端响应一个cookie
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis()+"");
        cookie.setMaxAge(24*60*60);
        resp.addCookie(cookie);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

1、从请求中获取cookie信息

2、服务器响应诶客户端cookie

```java
Cookie[] cookies = req.getCookies();  //获取cookie
cookie.getName()  //获取cookie中的key
cookie.getValue()  //获取cookie中的value
Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis()+""); //新建一个cookie
cookie.setMaxAge(24*60*60); //设置cookie的有效期
resp.addCookie(cookie);  //响应给客户端添加一个cookie
```



一个网站cookie是否存在上限!聊聊细节问题

- 一个cookie只能保存一个信息
- 一个web站点可以给浏览器发送多个，最多存20个
- cookie大小限制为4kb
- 300个cookie浏览器上线

删除cookie

- 不设置有效期，关闭浏览器，自动失效，设置有效期为0，就可以实现这个目标

```java
/*通过另一个请求删除掉cookie*/
public class CookieDemo02Servlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //创建一个cookie，名字必须和要删除的cookie一样
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis()+"");
        //将cookie有效期设置为0，立马过期
        cookie.setMaxAge(0);
        resp.addCookie(cookie);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

案例：遇到中文乱码问题如何解决

>```
>URLDecoder.decode(cookie.getValue(),"utf-8") 解码
>```
>
>```
>URLEncoder.encode("秦僵","utf-8")  编码
>```

```java
/*服务器，告诉你，你来得时间，把这个时间封装成为一个新建，你下次带来，我就知道你来了*/
public class CookieDemo03Servlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决中文的乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8"); //不加这个，在浏览器访问时候会显示为乱码

        PrintWriter out = resp.getWriter();
        //
        Cookie[] cookies = req.getCookies();
        if (cookies!=null){
            //如果存在怎么办？我们取出来数据
            out.write("你上一次访问的时间是：");
            for (int i = 0; i < cookies.length; i++) {
                Cookie cookie = cookies[i];
                if (cookie.getName().equals("name")){
                    //获取cookie的值  ,因为读取是乱码，所以先进行解码
                    String value = URLDecoder.decode(cookie.getValue(),"utf-8"); //拿出来的是字符串，要转换成长整型才能变成时间
                    System.out.println(value);
                    out.write(value);
                }
            }
        }else {
            out.write("你是第一次访问!");
        }

        //服务器给客户端响应一个cookie
        Cookie cookie = new Cookie("name", URLEncoder.encode("秦僵","utf-8"));
        cookie.setMaxAge(24*60*60);
        resp.addCookie(cookie);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

### 7.4 session(重点)

什么是session

	- 服务器会给没一个用户（浏览器）创建一个Session对象
	- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在
	- 用户登录之后，整个网站都可以访问他-->保存购物车的信息

![image-20201125123038714](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201125123038714.png)

HttpSession的相关重点方法

![image-20201125115044125](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201125115044125.png)

Session和cookie的区别：

	- cookie是把用户数据写给用户的浏览器，浏览器保存(可以保存多个)
	- session是把用户的数据写到用户独占的session中，在服务器端保存(保存重要的信息，减少服务器资源浪费)；
	- session对象由服务器创建，

session使用场景：

​	保存用户的登录信息，只要用户不关闭浏览器，信息都存在

​	购物车信息也可以保存在session

​	经常在整个项目中被使用的数据，我们将它保存在Session中

使用session的示例代码：

```java
public class SessionDemoServlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html; charset=utf-8");
        //得到session
        HttpSession session = req.getSession();
        //给Session中存东西
        session.setAttribute("name", new Person("秦僵",18));
        //获取session的id
        String sessionId = session.getId();
        //判断是否是新创建的Session
        if (session.isNew()) {
            resp.getWriter().write("Session创建成功了! Session的ID：" + sessionId);
        } else {
            resp.getWriter().write("Session 已经在服务器中存在了!sessionId: " + sessionId);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```java
public class SessionDemoServlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html; charset=utf-8");
        //得到session
        HttpSession session = req.getSession();
        //给Session中存东西
        Person person = (Person)session.getAttribute("name");
        //获取session的id
        String sessionId = session.getId();
        //判断是否是新创建的Session
        if (session.isNew()) {
            resp.getWriter().write("Session创建成功了! Session的ID：" + sessionId+"\n");
        } else {
            resp.getWriter().write("Session 已经在服务器中存在了!sessionId: " + sessionId);
            resp.getWriter().write("session.getAttribute(\"name\"):"+person.toString());
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```java
public class SessionDemoServlet3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html; charset=utf-8");
        //得到session
        HttpSession session = req.getSession();
        //移除name这个属性
        session.removeAttribute("name");
        //手动注销session，一旦注销sessionid就么有了，浏览器再访问就是生成新的sessionid
        session.invalidate();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



会话自动过期，Web.xml配置

```xml
<!--    设置Session默认失效时间-->
    <session-config>
        <!--15分钟后，session自动失效，以分钟为单位  24*60=1440=一天-->
        <session-timeout>15</session-timeout>
    </session-config>
```



## 8.jsp

### 8.1 什么是JSP

java server pages  JAVA服务器端页面，也和servlet一样，用来实现动态web站点的

最大的特点

	- 写jsp就像在写Html
	- html只给用户提供静态的数据，jsp页面中可以嵌入java代码，为用户提供动态数据；

### 8.2 jsp原理

思路:kankan JSP到底如何执行的？

- 代码层面没有任何问题

- 服务器内部工作

  - tomcat中有一个work目录

  - 我们用IDEA中使用TOMCAT会在IDEA的TOMCAT中生成一个work目录，发现页面装备成了JAVA程序

  - ![image-20201125124204236](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201125124204236.png)

    ​	![image-20201125124305277](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201125124305277.png)

- 浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet

- JSP最终也会转译成一个JAVA类，从源码上，她也是继承HttpServlet，也就是一个Servlet

在JSP的生成java文件C:\Users\admin\AppData\Local\JetBrains\IntelliJIdea2020.2\tomcat\Unnamed_javaweb-01-maven\work\Catalina\localhost\kuang\org\apache\jsp\index_jsp.java中，可以看到下面三个方法

```java
//初始化  
public void _jspInit() {
  }
//销毁
  public void _jspDestroy() {
  }
//jspService
  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
      throws java.io.IOException, javax.servlet.ServletException {
```

1、判断请求

```java

    if (!javax.servlet.DispatcherType.ERROR.equals(request.getDispatcherType())) {
      final java.lang.String _jspx_method = request.getMethod();
      if ("OPTIONS".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        return;
      }
      if (!"GET".equals(_jspx_method) && !"POST".equals(_jspx_method) && !"HEAD".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED, "JSP 只允许 GET、POST 或 HEAD。Jasper 还允许 OPTIONS");
        return;
      }
    }
```

2\定义内置的对象

```java
    final javax.servlet.jsp.PageContext pageContext; //页面上下文
    javax.servlet.http.HttpSession session = null;  //session
    final javax.servlet.ServletContext application;  //ServletContext 改名为applicationContext
    final javax.servlet.ServletConfig config; //ServletConfig 改为了config
    javax.servlet.jsp.JspWriter out = null;   //out的输出对象
    final java.lang.Object page = this;		//page代表了当前页
    javax.servlet.jsp.JspWriter _jspx_out = null; 
    javax.servlet.jsp.PageContext _jspx_page_context = null;
final javax.servlet.http.HttpServletRequest request, // 请求
final javax.servlet.http.HttpServletResponse response  //响应
```

3、输出页面之前，增加代码进行页面处理

```java
      response.setContentType("text/html");  //设置响应的页面类型为text/html
      pageContext = _jspxFactory.getPageContext(this, request, response,null, true, 8192, true);
      _jspx_page_context = pageContext; 
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;

      out.write("<html>\n");
      out.write("<body>\n");
      out.write("<h2>Hello World!</h2>\n");
      out.write("</body>\n");
      out.write("</html>\n");
```

以上的这些对象，我们可以在jsp页面中直接使用!

在JSP中，可以直接使用 < % 编写java代码%>

![image-20201125131539624](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201125131539624.png)



在JSP页面中，只要是JAVA代码就会原封不动的输出出来

如果是HTML代码，就会被转换为：



> out.write("<html>\r\n");

这样的格式输出到前端!



### 8.3 jsp基础语法

任何语言都有自己的语法,JAVA中有，JSP作为JAVA技术的一种应用，也有自己扩充的语法

我们创建一个普通的MAVEN项目，通过支持WEB，修改为WEB项目的过程

step1.在IDEA中，选择NewProject,不用勾选模板创建，

step2.待Maven项目加载完成后，光标定位到项目，右键选择[Add Framework Support] ，添加架构支持，选择WEB，即可!



接下来的语法学习，先搭建项目依赖，依赖包配置

```xml
pom.xml
    <dependencies>
        <!--        Servlet依赖-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
        <!--        JSP依赖-->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>javax.servlet.jsp-api</artifactId>
            <version>2.3.3</version>
        </dependency>
        <!-- JSTL表达式依赖 -->
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <!-- Standard标签库依赖 -->
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>

    </dependencies>
```

**1.JSP表达式**

表达式：<%= new java.util.Date() %> 用来将程序的输出，输出到客户端



**2.JSP脚本片段**

表达式:

<% 

​	int sum=0;

​	for(int i=1;i<100;i++){

​		sum+=i;	

}

​	out.println("<h1>Sum="+sum+>"</h1>");

%>

**3.脚本片段再实现**

```jsp
<%--jsp脚本片段--%>
<%
    int sum = 0;
    for (int i = 0; i < 100; i++) {
        sum += i;
    }
    out.print("<h1>Sum=" + sum + "</h1>");
%>
<h1>下面输出日期</h1>

<%=new java.util.Date()%>

<h1>下面还可以把java脚本片段进行分离</h1>
<%
    for (int i = 0; i < 5; i++) {
%>
<h1>Hello World!<%= i%>
</h1>
<%}%>
```

对应jsp生成的servlet

```java
      out.write("\n");
      out.write("\n");
      out.write("<html>\n");
      out.write("<head>\n");
      out.write("    <title>$Title$</title>\n");
      out.write("</head>\n");
      out.write("<body>\n");
      out.write('\n');

    int sum = 0;
    for (int i = 0; i < 100; i++) {
        sum += i;
    }
    out.print("<h1>Sum=" + sum + "</h1>");

      out.write("\n");
      out.write("<h1>下面输出日期</h1>\n");
      out.print(new java.util.Date());
      out.write("\n");
      out.write("<h1>下面还可以把java脚本片段进行分离</h1>\n");

    for (int i = 0; i < 5; i++) {

      out.write("\n");
      out.write("  <h1>Hello World!");
      out.print( i);
      out.write("</h1>\n");
}
      out.write("\n");
      out.write("</body>\n");
      out.write("</html>\n");
    }
```

4.JSP声明<%! %>

```java
<%!
  static {
    System.out.println("Loading servlet!");
  }
  static int globalVar =0;

  public void kuang(){
    System.out.println("进入了方法kuang()狂神秦僵!");
  }
%>
```

编译后生成的jsp.java

![image-20201126095029604](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201126095029604.png)



JSP声明 <%!%>： 会被编译到jsp生成的java类的类中

jsp脚本片段<%%>和jsp表达式<%=%>， 会被编译生成到jsp的 _jspService()中

上述的写法，使用起来不方便，所以衍生出来使用EL表达式 比如  <%=%> 用el表达式就是 ${}

```jsp
<%%>
<%=%>
<%! %>
<%--注释--%>
```

jsp的注释，不会在客户端显示，html就会

### 8.4 jsp指令

1、@page  errorPage="error/500.jsp"	这个设置500页面

2、@page import java.util.* 这个在当前页面引用库

关于设置错误页面的两种方法：

​	准备，在web目录下创建error文件夹，放入404.jsp和500.jsp， 在web目录下，创建img目录

​	方法1： 在相关页面使用 @page  errorPage="error/500.jsp"这样出现500的错误就会进入到error/500.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@page errorPage="error/500.jsp" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    int i = 1 / 0;
%>
</body>
</html>
```



​	方法2:  在web.xml配置

```
<error-page>
    <error-code>404</error-code>
    <location>/error/404.jsp</location>
</error-page>
<error-page>
    <error-code>500</error-code>
    <location>/error/500.jsp</location>
</error-page>
```

```
<@page encoding="utf-8">
<@include file=""> 包含，一般网站头尾都一样，作为共用的，可以直接提取公共页面，其他页面include

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%@include file="common/header.jsp" %>
<h1>网页</h1>
<%@include file="common/footer.jsp" %>
<%--    jsp标签--%>
<jsp:include page="common/header.jsp"/>
<h1>网页</h1>
<jsp:include page="common/footer.jsp"/>
</body>
</html>
```

两种区别

![image-20201126113140834](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201126113140834.png)

方法1，可以直接写写代码，写对象会有错误，方法2 ，不能重复定义变量

### 8.5 9大内置对象

```java
    final javax.servlet.jsp.PageContext pageContext; //页面上下文
    javax.servlet.http.HttpSession session = null;  //session
    final javax.servlet.ServletContext application;  //ServletContext 改名为applicationContext
    final javax.servlet.ServletConfig config; //ServletConfig 改为了config
    javax.servlet.jsp.JspWriter out = null;   //out的输出对象
    final java.lang.Object page = this;		//page代表了当前页
    javax.servlet.jsp.JspWriter _jspx_out = null; 
    javax.servlet.jsp.PageContext _jspx_page_context = null;
final javax.servlet.http.HttpServletRequest request, // 请求
final javax.servlet.http.HttpServletResponse response  //响应
```



- PageContext (PageContext) 存东西
- Request  存东西
- Response
- Session  这个存东西
- Application(ServletContext)  存储东西
- config(ServletConfig) 
- out
- page
- exception

不同对象存储东西，作用域的不同

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--内置对象--%>
<%
    pageContext.setAttribute("name1", "秦僵1号"); //保存的数据只在一个页面中有效
    request.setAttribute("name2", "秦僵2号"); //保存的数据只在一次请求中有效，请求转发会携带这个数据
    session.setAttribute("name3", "秦僵3号"); //保存在一次会话中有效，从打开浏览器，到关闭浏览器
    //统计在线人数
    application.setAttribute("name4", "秦僵4号"); //凌驾所有数据之上，保存的数据，在服务器存续期间都会有效，从打开服务器到关闭服务器
%>
<%--脚本片段的代码，会被原封不动的生成到到jsp.java中
要求：脚本片段的代码，必须保证java语法的正确性--%>
<%--<%--%>
<%--//通过pageContext取出我们保存的值--%>
<%--    Object name1 = pageContext.getAttribute("name1");--%>
<%--    Object name2 = request.getAttribute("name2");--%>
<%--%>--%>
<%
    //通过pageContext寻找的方法
    //从底层到高层(作用域)
    String name1 = (String)pageContext.findAttribute("name1");
    String name2 = (String)pageContext.findAttribute("name2");
    String name3 = (String)pageContext.findAttribute("name3");
    String name4 = (String)pageContext.findAttribute("name4");
    String name5 = (String)pageContext.findAttribute("name5");
%>
<%--使用EL表达式输出${} 等价于<%=%>--%>
    <h1>取出的值为:</h1>
    <h3>${name1}</h3>
    <h3>${name2}</h3>
    <h3>${name3}</h3>
    <h3>${name4}</h3>
    <h3>${name5}</h3>
</body>
</html>
```



```
    public void setAttribute(String name, Object attribute, int scope) {
        switch(scope) {
        case 1:
            this.mPage.put(name, attribute);
            break;
        case 2:
            this.mRequest.put(name, attribute);
            break;
        case 3:
            this.mSession.put(name, attribute);
            break;
        case 4:
            this.mApp.put(name, attribute);
            break;
        default:
            throw new IllegalArgumentException("Bad scope " + scope);
        }

    }
    
    public abstract class PageContext extends JspContext {
    public static final int PAGE_SCOPE = 1;
    public static final int REQUEST_SCOPE = 2;
    public static final int SESSION_SCOPE = 3;
    public static final int APPLICATION_SCOPE = 4;
```

request:客户端向服务器发送其你去，产生的数据，用户看完就没用了，比如新闻；

session:客户端向服务器发送请求，产生的数据，用户用完了一会还会用，比如购物车

application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用比如聊天数据

### 8.6 JSP标签、JSTL标签、EL表达式

EL（Expression Language）表达式：${}

	- 获取数据
	- 执行运算
	- 获取web开发的常用对象
	- 调用java方法

JSP标签

```
<%--http://localhost:8080/jsptag.jsp?name=kuangshen&age=12--%>
<jsp:forward page="jsptag2.jsp">
    <jsp:param name="name" value="kuangshen "></jsp:param>
    <jsp:param name="age" value="12"></jsp:param>
</jsp:forward>
    
    <body>
    名字:<%= request.getParameter("name")%>
    年龄:<%= request.getParameter("age")%>
    </body>   
```

JSTL标签



JSTL标签库的使用就是为了弥补HTML标签的不足，它自定义许多标签，可以供我们使用，标签的功能和JAVA代码一样

**核心标签(掌握)**

> 要导入  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>



示例代码1： c:if 

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>coreif</title>
</head>
<body>
<h4>测试</h4>
<form action="coreif.jsp" method="get">
    <%--El表达式，获取表单数据，使用${param.参数名}--%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit">
</form>
<%--判断如果用户提交用户名是管理员，则登录成功--%>
<c:if test="${param.username.equals('admin')}" var="isAdmin">
    <c:out value="${param.username}"/>
</c:if>


<c:out value="${isAdmin}"/>
</body>
</html>
```

示例代码2： c:choose 

```jsp
<head>
    <title>c:choose标签实例</title>
</head>
<body>
<%--在pageContent里面设置一个salary=4000--%>
<c:set var="salary" scope="page" value="${2000*2}"/>
<p>你的工资是:<c:out value="${salary}" /></p>

<c:choose>
    <c:when test="${salary <= 0}">
        太惨了!
    </c:when>
    <c:when test="${salary >= 1000}">
        不错的薪水,还能生活!
    </c:when>
    <c:otherwise>
        什么都没有!
    </c:otherwise>
</c:choose>
</body>
</html>
```



示例代码3： c:forEach

```jsp
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>coreforeach实例代码</title>
</head>
<body>
    <%
        ArrayList<String> people = new ArrayList<String>();
        people.add(0,"张三");
        people.add(1,"李四");
        people.add(2,"王五");
        people.add(3,"赵六");
        people.add(4,"钱七");
        request.setAttribute("list",people);
    %>
<%--
var 每次遍历的变量
items 要遍历的数组
begin 开始位置
end 结束的位置
step 步进 默认为1
--%>
<c:forEach var="people" items="${list}" begin="" end="" step="">
    <c:out value="${people}"/><br>
</c:forEach>
</body>
</html>
```





**格式化标签**

**SQL标签**

**XML标签**

### 8.7 JAVA BEEN

实体类

JAVA BEAN 与特定的写法

	- 必须有一个无参构造
	- 属性必须私有化
	- 必须有对应的get/set方法

一般用来和数据库的字段做映射ORM

ORM 对象关系映射

	- 数据库一张表对应java一个类
	- 表里面的字段-对应类的一个属性
	- 表里面的行记录，对应一个类的一个对象；

People表

| id   | name      | age  | address |
| ---- | --------- | ---- | ------- |
| 1    | 秦僵1号   | 3    | 西按    |
| 2    | 秦僵2号18 | 3    | 西安    |
| 3    | 秦僵3号   | 100  | 西安    |

```
class People{
	private int id;
	private String name;
	private int age;
	private String address;
}

class A{
	new People(1,"秦僵1号",3，"西安");
}
```

```xml
<body>
<jsp:useBean id="people" class="top.aigoo.pojo.People" scope="page"/>
<%--设置属性值--%>
<jsp:setProperty name="people" property="id" value="1"/>
<jsp:setProperty name="people" property="name" value="王丽"/>
<jsp:setProperty name="people" property="age" value="25"/>
<jsp:setProperty name="people" property="address" value="北京市顺义区高丽营镇金马工业区18号"/>

<%--得到属性值并显示出来--%>
主键id:<jsp:getProperty name="people" property="id"/><br>
姓名:<jsp:getProperty name="people" property="name"/><br>
年龄:<jsp:getProperty name="people" property="age"/><br>
地址:<jsp:getProperty name="people" property="address"/><br>


</body>
```



com.kuang.pojo

- 过滤器
- 监听器  GUI
- 文件上传
- 邮件发送
- JDBC复习:如何使用JDBC，JDBC CRUD JDBC事务

### 8.8.三层架构 MVC

什么是MVC  Model View Controller  模型视图控制器

Servlet和jsp都可以写java代码，为了易于维护和使用，servlet专注于处理请求，以及控制视图跳转，jsp专注于显示数据

用户直接访问控制层，控制层就可以直接操作数据库：

>servlet--curd--数据库
>
>弊端：程序十分臃肿，不利于维护，servlet的代码中，处理请求，响应，视图跳转，处理JDBC，处理业务代码，处理逻辑代码
>
>架构：没有什么是加一层解决不了的!
>
>程序员调用!
>
>|
>
>JDBC
>
>|
>
>Mysql oralcle sqlserver.......
>
>2 三层架构![image-20201127223837589](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201127223837589.png)

Model:

​	-业务处理  业务逻辑Service

	- 数据持久层CRUD DAO

View

	- 展示数据

- 提供链接发起Servlet请求(a,form,.img.....)

Controller(servlet)

	- 接收用户请求(req 请求参数session信息....)
	- 交给业务层处理对应的代码
	- 控制视图的跳转

> 登录--->接收用户的登录请求---->处理用户的请求(获取用户的登录的参数 username password)---->交给业务层处理登录业务(判断用户名密码是否正确)--->Dao层查询数据库用户名密码是否正确--->数据库





## 9.过滤器(Filter)

Filter：过滤器，用来过滤网站的数据；

![image-20201127232944733](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201127232944733.png)

	- 处理中文乱码
	- 登录、验证



Filter开发步骤

	-	导入包，导入包不要错

> ```
> <artifactId>javax.servlet-api</artifactId>
> <artifactId>javax.servlet.jsp-api</artifactId>
> <artifactId>jstl</artifactId>
>  <artifactId>standard</artifactId>
>  <artifactId>mysql-connector-java</artifactId>
> ```

	-	开发Filter,编写过滤器

```java
public class JavaFilterDemo1 implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("JavaFilterDemo1 初始化.....");
    }

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        System.out.println("+++++++++++过滤器执行前+++++++++++");
        chain.doFilter(request,response);
        System.out.println("<--------过滤器执行后---------->");
    }

    public void destroy() {
        System.out.println("JavaFilterDemo1 销毁.....");
    }
}
```



- 配置过滤器

```xml
    <filter>
        <filter-name>JavaFilterDemo1</filter-name>
        <filter-class>top.aigoo.filter.JavaFilterDemo1</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>JavaFilterDemo1</filter-name>
        <url-pattern>/servlet/*</url-pattern>
    </filter-mapping>
### 上述式子代表所有/servlet/后面的相关页面都会被过滤器捕获到
```



1、过滤中的所有代码，在过滤特定请求的时候都会执行

2、必须要让过滤器继续通行，chain.doFilter(request, response) //过滤器放行



## 10.监听器及过滤器使用案例

实现一个监听器接口，有很多种监听器

监听器作用：

监听器实现过程：

1、编写一个监听器，实现监听器接口

```java
public class OnlineCountListener implements HttpSessionListener {
    public void sessionCreated(HttpSessionEvent se) {
        //一旦session创建成功就会启动这个监听
        ServletContext servletContext = se.getSession().getServletContext();
        System.out.println(se.getSession().getId());
        int count = 0;
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");

        if (onlineCount == null) {
            onlineCount = new Integer(1);
        } else {
            count = onlineCount.intValue() ;
            onlineCount = new Integer(count + 1);
        }

        servletContext.setAttribute("OnlineCount", onlineCount);
    }

    public void sessionDestroyed(HttpSessionEvent se) {
        //一旦session销毁成功就会启动这个监听
        ServletContext servletContext = se.getSession().getServletContext();
        int count = 0;
        Integer onlineCount = (Integer) servletContext.getAttribute("OnlineCount");

        if (onlineCount == null) {
            onlineCount = new Integer(0);
        } else {
            count = onlineCount.intValue() ;
            onlineCount = new Integer(count - 1);
        }
      
        servletContext.setAttribute("OnlineCount", onlineCount);
        // session的销毁方式
        // 手动销毁  se.getSession().invalidate();
        // 自动销毁
    }
}
```



2、配置监听器

```xml
<listener>
    <listener-class>top.aigoo.listener.OnlineCountListener</listener-class>
</listener>
```



3、看情况是否使用

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>index</title>
</head>
<body>
<h1>当前网站有<span style="color: red"> <%= application.getAttribute("OnlineCount")%></span>人</h1>
</body>
</html>
```

监听器常见应用：

GUI编程常用，写游戏使用

**Filter实现权限的拦截：用户登录之后才能进入主页，用户注销后就不能进入到主页!**

业务逻辑：

1、用户进入登录页，提交自己的用户名，点击submit提交请求，如果用户输入的是admin，则进入到登录成功页，如果用户输入的不是admin，则进入登录失败页

2、在登录失败页，展示用户登录失败，并可以点击进入到登录成功页；

3、登录成功页，对应登录成功的用户可以正确跳转进入登录成功页，对于未登录用户，则进入登录失败页；

设计逻辑：

1、对于登录页login.jsp，设计一个servlet, 首先接收username,然后判断是否是admin,如果是，则设置session到Constant.USER_SESSION，如果不是，则跳转到失败页

设置session：req.getSession().setAttribute(Constant.USER_SESSION,req.getSession().getId())

跳转使用: resp.sendRedirect("/sys/success.jsp")

2、对于登录成功页，我们需要有一个退出登录的功能，我们退出登录的逻辑为判断当前的Constant.USER_SESSION是否是空，如果不为空，则把session的Constant.USER_SESSION 删除掉

   同时， 对于成功页，需要判断一下，如果是未登录用户直接访问，则跳转到错误页，如果是登录用户则可以访问

代码组织形式：

![image-20201128222234143](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201128222234143.png)

对应的web.xml

```xml
<servlet>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>top.aigoo.servlet.LoginServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>LoginServlet</servlet-name>
    <url-pattern>/servlet/login</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>LogoutServlet</servlet-name>
    <servlet-class>top.aigoo.servlet.LogoutServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>LogoutServlet</servlet-name>
    <url-pattern>/servlet/logout</url-pattern>
</servlet-mapping>

<!--过滤器-->
<filter>
    <filter-name>SysFilter</filter-name>
    <filter-class>top.aigoo.filter.SysFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>SysFilter</filter-name>
    <url-pattern>/sys/*</url-pattern>
</filter-mapping>
<filter>
    <filter-name>JavaFilterDemo1</filter-name>
    <filter-class>top.aigoo.filter.JavaFilterDemo1</filter-class>
</filter>
<filter-mapping>
    <filter-name>JavaFilterDemo1</filter-name>
    <url-pattern>/servlet/*</url-pattern>
</filter-mapping>

```

对应的jsp：

```jsp
<body>
    <h1>登录</h1>
    <form action="/servlet/login" method="post">
        用户名:<input type="text" name="username">
        <input type="submit">
    </form>
</body>

error
<body>
    <h1>错误</h1>
    <h3>没有权限,用户名错误</h3>
    <a href="/login.jsp">返回登录页面</a>
</body>
</html>

成功页
<body>
    <h1>主页</h1>
    <p><a href="/servlet/logout">注销</a></p>
</body>


```

示例代理

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //获取前端请求的参数
        String username = req.getParameter("username");

        if (username.equals("admin")){
            System.out.println("你是一个管理员，你还会跳转到登录成功页!");
            req.getSession().setAttribute(Constant.USER_SESSION,req.getSession().getId());
            resp.sendRedirect("/sys/success.jsp");
        }else {//登录失败
            resp.sendRedirect("/error.jsp");
        }
    }
    
public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object user_session = req.getSession().getAttribute("USER_SESSION");
        if (user_session!=null){
            req.getSession().removeAttribute(Constant.USER_SESSION);
            resp.sendRedirect("/login.jsp");
        }else {
            resp.sendRedirect("/login.jsp");
        }
    }

    //过滤器    
public class SysFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse resp = (HttpServletResponse) response;

        if (req.getSession().getAttribute(Constant.USER_SESSION)==null){
            resp.sendRedirect("/error.jsp");
        }

        chain.doFilter(request,response);
    }
    
public class Constant {
    public final static String USER_SESSION = "USER_SESSION";
}    
```



### JDBC复习

Java连接数据库 

需要的jar包

- java.sql
- javax.sql
- mysql-conneter-java....连接驱动(必须要导入)

事务：

```java
开启事务  connection.setAutoCommit(false)

事务提交  commit()

事务回滚  rollback()

关闭事务
```

Junit单元测试

依赖：

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
</dependency>
```



简单实用

@Test注解只有在方法上面有效，只有加了这个注解的方法，就可以直接运行!

```java
public class TestInsertDemo3 {
    @Test
    public void testDe() {
        System.out.println("demo");
    }
}
```

示例代码

```java
public class TestInsertDemo3 {
    @Test
    public void test() {
        //1.配置信息
        String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=UTF-8&useSSL=false";
        String username = "root";
        String password = "123456";
        Connection conn =null;

        try {
            //1.加载驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.连接数据库，代表数据库
            conn = DriverManager.getConnection(url, username, password);
            String sql = "update account set money=money-100 where name='A'";
            conn.prepareStatement(sql).executeUpdate();
            //3.通知数据库开启事务，false 开启
            conn.setAutoCommit(false);
            //制造错误
            int i = 1/0;
            String sql2 = "update account set money=money+100 where name='B'";
            conn.prepareStatement(sql2).executeUpdate();

            conn.commit();//以上两条SQL都执行成功了，就执行事务!
            System.out.println("插入成功");
        } catch (Exception e) {
            try {
                conn.rollback();//执行失败，事务回滚
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

    }
}

```







## 11.SMBMS超市订单管理系统

### 11.1 搭建开发环境

step 1、搭建一个干净的maven web项目 [在main下面添加java\resources包,更新web.xml为4.0版本，更新pom.xml文件]

step 2、配置tomcat	[配置vm optinons 为 -Dfile.encoding=UTF-8，配置Deployment]

step 3、测试项目是否能够跑起来

step 4、导入jar包 [jsp,servlet,mysql,jstl-api,standard]

step 5、创建项目包结构

step 6、编写实体类  ORM映射：表-类映射

step 7、编写基础功能类 

​		1、 编写数据库配置文件

》》》db.properties

> ```properties
> driver=com.mysql.jdbc.Driver
> url=jdbc:mysql://localhost:3306/smbms?useUnicode=true&characterEncoding=utf-8&useSSL=false
> username=root
> password=123456
> ```

​	2、读提取数据库配置

```java
static {
    //新建一个Properties类，这个类来加载db.properties
    Properties prop = new Properties();
    //通过类加载器读取对应资源
    InputStream is = BaseDao.class.getClassLoader().getResourceAsStream("db.properties");
    try {
        prop.load(is);
    } catch (Exception e) {
        e.printStackTrace();
    }
    driver = prop.getProperty("driver");
    url = prop.getProperty("url");
    username = prop.getProperty("username");
    password = prop.getProperty("password");
}
```

​	

​		3、基础的数据库操作工具包BaseDao

```java
public class BaseDao {
    private static String driver ;
    private static String url ;
    private static String username ;
    private static String password;

    //静态代码块，类加载的时候，就开始初始化了
    static {
        //新建一个Properties类，这个类来加载db.properties
        Properties prop = new Properties();
        //通过类加载器读取对应资源
        InputStream is = BaseDao.class.getClassLoader().getResourceAsStream("db.properties");
        try {
            prop.load(is);
        } catch (Exception e) {
            e.printStackTrace();
        }
        driver = prop.getProperty("driver");
        url = prop.getProperty("url");
        username = prop.getProperty("username");
        password = prop.getProperty("password");
    }

    //获取数据库的连接
    public static Connection getConnection() {
        Connection conn = null;
        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(url, username, password);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        return conn;
    }
    //编写查询公共方法
    public static ResultSet executeQuery(Connection conn, PreparedStatement prep,
                                         ResultSet resultSet, Object[] params, String sql) {
        //String sql = "select * from `account` where name='?' and account="?";

        try {
            prep = conn.prepareStatement(sql);
            for (int i = 0; i < params.length; i++) {
                prep.setObject(i + 1, params[i]);
            }
            resultSet = prep.executeQuery();

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return resultSet;
    }
    //编写增删改公共方法
    public static int executeUpdate(Connection conn, PreparedStatement prep,
                                    Object[] params, String sql) throws SQLException {
        //String sql = "insert INTO `account`(`name`,`money`) VALUES(?,?);

        prep = conn.prepareStatement(sql);
        for (int i = 0; i < params.length; i++) {
            prep.setObject(i + 1, params[i]);
        }
        return prep.executeUpdate();
    }

    public static boolean closeResource(Connection conn, PreparedStatement prst, ResultSet rs) {
        boolean flag = true;
        if (rs != null) {
            try {
                rs.close();
                //召唤垃圾收集器回收
                rs = null;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                flag = false;
            }
        }

        if (prst != null) {
            try {
                prst.close();
                //召唤垃圾收集器
                prst = null;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                flag = false;
            }
        }

        if (conn != null) {
            try {
                conn.close();
                //召唤垃圾收集器
                conn = null;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                flag = false;
            }
        }

        return flag;
    }
}
```



​		4、编写字符编码过滤器

```xml
    <!--字符编码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>top.aigoo.filter.CharacterEncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

step 8、导入静态资源包

把项目的calendar \css\images\js等文件夹放入到webapp下，不要放到resources里面

### 11.2 登录功能实现

![image-20201130105209763](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201130105209763.png)

1、编写前端页面

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>系统登录 - 超市订单管理系统</title>
    <link type="text/css" rel="stylesheet" href="${pageContext.request.contextPath }/css/style.css" />
    <script type="text/javascript">
	/* if(top.location!=self.location){
	      top.location=self.location;
	 } */
    </script>
</head>
<body class="login_bg">
    <section class="loginBox">
        <header class="loginHeader">
            <h1>超市订单管理系统</h1>
        </header>
        <section class="loginCont">
	        <form class="loginForm" action="${pageContext.request.contextPath }/login.do"  name="actionForm" id="actionForm"  method="post" >
				<div class="info">${error}</div>
				<div class="inputbox">
                    <label for="userCode">用户名：</label>
					<input type="text" class="input-text" id="userCode" name="userCode" placeholder="请输入用户名" required/>
				</div>	
				<div class="inputbox">
                    <label for="userPassword">密码：</label>
                    <input type="password" id="userPassword" name="userPassword" placeholder="请输入密码" required/>
                </div>	
				<div class="subBtn">
                    <input type="submit" value="登录"/>
                    <input type="reset" value="重置"/>
                </div>	
			</form>
        </section>
    </section>
</body>
</html>
```

2、设置欢迎页

```xml
<!--    设置欢迎页-->
    <welcome-file-list>
        <welcome-file>login.jsp</welcome-file>
    </welcome-file-list>
```

3、编写dao层登录用户登录的接口

在UserDao中，只进行获取登录用户的任务

```java
public interface UserDao {
    //得到登录的用户
    public User getLoginUser(Connection connection, String userCode);
}
```



4、编写dao接口的实现类

在实现类，我们通过使用BaseDao的的executeQuery()来获取数据库的用户，查到用户，就返回给用户，没查到，就返回NUll

```java
public class UserDaoImpl implements UserDao {
    //根据userCode，查询数据库,得到登录用户
    public User getLoginUser(Connection conn, String userCode) throws SQLException {
        PreparedStatement prep = null;//查询句柄
        ResultSet rs = null;//查询结果
        User user = null;//user

        if (conn != null) {
            String sql = "select * from smbms_user where userCode=?";//查询语句
            Object[] params = {userCode}; //查询参数
            rs = BaseDao.executeQuery(conn, prep, rs, params, sql);
            if (rs.next()) {
                user = new User();
                user.setId(rs.getInt("id"));
                user.setUserCode(rs.getString("userCode"));
                user.setUserName(rs.getString("userName"));
                user.setUserPassword(rs.getString("userPassword"));
                user.setGender(rs.getInt("gender"));
                user.setBirthday(rs.getDate("birthday"));
                user.setPhone(rs.getString("phone"));
                user.setAddress(rs.getString("address"));
                user.setUserRole(rs.getInt("userRole"));
                user.setCreatedBy(rs.getInt("createdBy"));
                user.setCreationDate(rs.getDate("creationDate"));
                user.setModifyBy(rs.getInt("modifyBy"));
                user.setModifyDate(rs.getDate("modifyDate"));
            }
            BaseDao.closeResource(null, prep, rs);
        }
        return user;
    }
}
```



5、编写业务层接口

在业务层，主要是通过获取的用户名，密码，对用户的登录进行校验，如果是正确的就返回给user，如果不成功就返回null

```java
public interface UserService {
    //用户登录
    public User login(String userCode, String password);
}
```



6、编写业务层实现类UserServiceImpl

业务层实现逻辑，通过查询获取用户，然后对用户的密码进行校验，验证通过返回User，不通过给null

```java
public class UserServiceImpl implements UserService {
    //业务层都会调用DAO层，所以我们要引入Dao层；
    private UserDao userDao;

    public UserServiceImpl() {
        this.userDao = new UserDaoImpl();
    }

    public User login(String userCode, String password) {
        Connection connection = null;
        User user = null;
        try {
            connection = BaseDao.getConnection();
            //通过业务层调用对应的具体的数据库操作
            user = userDao.getLoginUser(connection, userCode);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            BaseDao.closeResource(connection, null, null);
        }
        //判断密码是否相同
        if (!user.getUserPassword().equals(password)) {
            return null;
        }
        return user;

    }
```

7、编写LoginServlet，并对前端的get请求进行处理，获取username,和password，然后请求UserServiceImpl的login进行处理，如果获取到的user，就跳转到登录成功页，如果不成功，就返回到登录页，并给出出错信息

```java
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("LoginServlet----start.....");
        //获取用户名和密码
        String userCode = req.getParameter("userCode");
        String userPassword = req.getParameter("userPassword");
        //和数据库中的密码进行对比，调用业务层；
        UserServiceImpl userService = new UserServiceImpl();
        User user = userService.login(userCode, userPassword);//到这里已经把登录的人给查出来了

        if (user!=null){//查有此人，可以登录
            //将用户的信息放到Session中
            req.getSession().setAttribute(Constants.USER_SESSION,user);
            //跳转到内部主页
            resp.sendRedirect("jsp/frame.jsp");
        }else {//查无此人，无法登录
            //转发会登录页面，顺带提示它： 用户名或者密码错误
            req.setAttribute("error","用户名或者密码不正确!");
            req.getRequestDispatcher("login.jsp").forward(req, resp);
        }
    }
```



8、在web.xml注册Servlet

```xml
    <!--注册loginServlet-->
    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>top.aigoo.servlet.user.LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/login.do</url-pattern>
    </servlet-mapping>
```



9、测试访问，要确保以上功能实现

总结，上述一条线的逻辑结构如下：

![image-20201130141101496](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201130141101496.png)

![image-20201130195923317](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201130195923317.png)







### 11.3 用户注销

注销功能： 移除session，增加所有页面对登录权限的验证

```java
编写页面代码
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //注销就是将session中保存的userSession移除掉
    if (req.getSession().getAttribute(Constants.USER_SESSION)!=null){
        req.getSession().removeAttribute(Constants.USER_SESSION);
    }
    resp.sendRedirect(req.getContextPath()+"/login.jsp");
}
```

```xml
    <!--注册logoutServlet-->
    <servlet>
        <servlet-name>LogoutServlet</servlet-name>
        <servlet-class>top.aigoo.servlet.user.LogoutServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LogoutServlet</servlet-name>
        <url-pattern>/jsp/logout.do</url-pattern>
    </servlet-mapping>
```



实现对权限的过滤功能，思路，通过实现Filter， 对所有的请求，检查session里面是否含有Constant.USER_SESSION,含有USER_SESSION代表有登录，就跳转到登录后的页面，如果没有，就重定向到Error页面。

```java
##SysFilter.java
public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
    HttpServletRequest request= (HttpServletRequest)req;
    HttpServletResponse response= (HttpServletResponse)resp;

    System.out.println("request.getContextPath():"+request.getContextPath());


    if (request.getSession().getAttribute(Constants.USER_SESSION)==null){
        response.sendRedirect(request.getContextPath()+"/error.jsp");
    }
    chain.doFilter(req,resp);
}
```

注册加载SysFilter

```xml
<filter>
    <filter-name>SysFilter</filter-name>
    <filter-class>top.aigoo.filter.SysFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>SysFilter</filter-name>
    <url-pattern>/jsp/*</url-pattern>
</filter-mapping>
```





测试：登录、注销、权限都要保证正确

### 11.4 用户密码修改

1、导入前端素材

jsp/pwdmodify.jsp



2、



3、UserDao接口

```java
public interface UserDao {
    //得到登录的用户
    public User getLoginUser(Connection connection, String userCode) throws SQLException;
    // 修改当前用户密码
    public int updatePwd(Connection connection,int id, String password) throws SQLException;
}
```





4、UserDaoImpl实现

```java
public class UserDaoImpl implements UserDao {
    //根据userCode，查询数据库,得到登录用户
    public User getLoginUser(Connection conn, String userCode) throws SQLException {
        PreparedStatement prep = null;//查询句柄
        ResultSet rs = null;//查询结果
        User user = null;//user

        if (conn != null) {
            String sql = "select * from smbms_user where userCode=?";//查询语句
            Object[] params = {userCode}; //查询参数
            rs = BaseDao.executeQuery(conn, prep, rs, params, sql);
            if (rs.next()) {
                user = new User();
                user.setId(rs.getInt("id"));
                user.setUserCode(rs.getString("userCode"));
                user.setUserName(rs.getString("userName"));
                user.setUserPassword(rs.getString("userPassword"));
                user.setGender(rs.getInt("gender"));
                user.setBirthday(rs.getDate("birthday"));
                user.setPhone(rs.getString("phone"));
                user.setAddress(rs.getString("address"));
                user.setUserRole(rs.getInt("userRole"));
                user.setCreatedBy(rs.getInt("createdBy"));
                user.setCreationDate(rs.getDate("creationDate"));
                user.setModifyBy(rs.getInt("modifyBy"));
                user.setModifyDate(rs.getDate("modifyDate"));
            }
            BaseDao.closeResource(null, prep, rs);
        }
        return user;
    }

    //修改当前用户密码,参数.id,newpassword
    public int updatePwd(Connection conn, int id, String password) throws SQLException {

        PreparedStatement prep = null;
        Object[] params = {password, id};
        int execute = -1; //返回状态符

        if (conn != null) {
            String sql = "update smbms_user set userPassword=? where id=?";//查询语句
            prep = conn.prepareStatement(sql);
            //执行结果
            execute = BaseDao.executeUpdate(conn, prep, params, sql);
            //关闭资源
            BaseDao.closeResource(null, prep, null);
        }

        return execute;
    }
}
```

5、UserService接口

```java
//用户登录
public User login(String userCode, String password);
//根据用户id修改密码
public boolean updatePwd(int id, String password);
```



6、UserServicImple实现

```java
//根据用户id修改密码
public boolean updatePwd(int userCode, String newpassword) {
    Connection conn = null;
    boolean flag = false;
    try {
        conn = BaseDao.getConnection(); //获取我们的数据库连接
        if (userDao.updatePwd(conn, userCode, newpassword) > 0) {
            flag = true;
        }
    } catch (SQLException throwables) {
        throwables.printStackTrace();
    }finally {
        BaseDao.closeResource(conn, null, null);
    }
    return flag;
}
```

7、创建UserServlet类

```java
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //从session里面获取我们的user id
        Object obj = req.getSession().getAttribute(Constants.USER_SESSION);
        String password = req.getParameter("newpassword");

        System.out.println("UserServlet:-->" + password);

        boolean flag = false;

        //如果 session有对象，并且password这个不为空或者Empty
        if (obj != null && !StringUtils.isNullOrEmpty(password)) {
            UserService userService = new UserServiceImpl();
            flag = userService.updatePwd(((User) obj).getId(), password);

            if (flag) {
                req.setAttribute("message", "密码修改成功!请用新密码重新登录!");
                //密码修改成功，移除当前session中的usersession
                req.getSession().removeAttribute(Constants.USER_SESSION);

            } else {
                req.setAttribute("message", "密码修改失败");
                //密码修改失败,跳转到密码修改页
            }

        } else {
            req.setAttribute("message", "新密码有问题");
        }
        req.getRequestDispatcher("pwdmodify.jsp").forward(req, resp);

    }
```





8、记得实现复用，我们需要提取出方法！

```java
//实现servlet复用
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getParameter("method");
        if (method != null && method.equals("savepwd")) {
            this.updatePwd(req, resp);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }

    public void updatePwd(HttpServletRequest req, HttpServletResponse resp) {
        //从session里面获取我们的user id
        Object obj = req.getSession().getAttribute(Constants.USER_SESSION);
        String password = req.getParameter("newpassword");

        System.out.println("UserServlet:-->" + password);

        boolean flag = false;

        //如果 session有对象，并且password这个不为空或者Empty
        if (obj != null && !StringUtils.isNullOrEmpty(password)) {
            UserService userService = new UserServiceImpl();
            flag = userService.updatePwd(((User) obj).getId(), password);

            if (flag) {
                req.setAttribute("message", "密码修改成功!请用新密码重新登录!");
                //密码修改成功，移除当前session中的usersession
                req.getSession().removeAttribute(Constants.USER_SESSION);

            } else {
                req.setAttribute("message", "密码修改失败");
                //密码修改失败,跳转到密码修改页
            }

        } else {
            req.setAttribute("message", "新密码有问题");
        }

        try {
            req.getRequestDispatcher("pwdmodify.jsp").forward(req, resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```





9、注册UserServlet到web.xml

```xml
<!--注册UserServlet-->
<servlet>
    <servlet-name>UserServlet</servlet-name>
    <servlet-class>top.aigoo.servlet.user.UserServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>UserServlet</servlet-name>
    <url-pattern>/jsp/user.do</url-pattern>
</servlet-mapping>
```

**在实现基础功能后，我们还需要对旧密码进行校验，旧密码的校验工作，我们通过AJAX的异步请求实现**

设计思路：

1、前端ajax，当用户输入完旧密码后，既发起连接后台请求，带上method，olderpassword,以及连接 path+"/jsp/user.do" 进行密码的验证；

2、UserServlet通过对method=pwdmodify 进行判断，进行方法分发，如果方法是pwdmodify则分发到public void pwdModify(HttpServletRequest req, HttpServletResponse resp)

3、在方法中，先获取原来的密码，和前端提交的密码， 从session中获取到user对象，从req.getParamter()获取前端提交的密码

4、因为前端有四种情况，分别是密码相同(result:true) 密码不同(result:false) session过期(result:sessionerror) 及密码为空(result:error),通过构建HashMap实现

​	HashMap<String, String> resultMap = new HashMap<String, String>();

5、为了返回Json格式，需要设置setContentType=application/json

6、使用 JSONArray.toJSONString()转换HashMap，并用write.write()写出

7、关闭流

8、需要配置下Session的有效期设置





问题:

1、修改密码页面提示乱码： js引入到页面的时候未设置编码导致的问题!

2、解决idea控制台输入中文乱码问题

在 Tomcat文件夹目录下的 conf 中，logging.properties这个文件将其中的UTF-8改成GBK，准确一点应该是改里面的logging. consolehandler那一项的



### 11.5 用户管理实现

功能分析：

1、当前页面存在用户表和角色表两张表的内容

2、当前页面有pagesize,有分页 ，有总数

结论1、1个页面的东西可以从不同的数据库进行获取，根据这个功能分析，我们可以设计的实现逻辑如下

![image-20201203091607412](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201203091607412.png)

实现步骤

#### 1、开发准备

导入分页工具类、用户列表页面导入

#### 2、获取用户数量

UserDao--UserDaoImpl---UserService---UserServiceImpl

```sql
select count(1)
```

```java
接口>
      public int getUserCount(Connection conn, String userName, int userRole) throws SQLException;
>dao实现类
        public int getUserCount(Connection conn, String userName, int userRole) throws SQLException {
        /*
         * String sql = select count(1) as count from smbms_user as u, smbms_role as r where u.userRole=r.id
         * and u.userName LIKE "%李%"and u.userRole  =2
         * */

        //初始化
        PreparedStatement prep = null;
        ResultSet rs = null;
        int count = 0;   //返回的数据，不要用Integer，因为可能为null

        if (conn != null) {
            //参数数组
            List<Object> list = new ArrayList<Object>();
            //构造sql语句
            StringBuffer _sql = new StringBuffer();
            _sql.append("select count(1) as count from smbms_user as u, smbms_role as r where u.userRole=r.id");

            //userName 可能会不存在
            if (!StringUtils.isNullOrEmpty(userName)) {
                _sql.append(" and u.userName LIKE ?");
                list.add("%" + userName + "%");
            }
            //userRole 可能会不存在
            if (userRole > 0) {
                _sql.append(" and u.userRole = ?");
                list.add(userRole);
            }

            System.out.println("UserDaoImpl.getUserCount().sql-> " + _sql.toString());
            Object[] params = list.toArray();

            //调用jdbc，执行查询
            rs = BaseDao.executeQuery(conn, prep, rs, params, _sql.toString());
            while (rs.next()) {
                count = rs.getInt("count");
            }
            //关闭资源
            BaseDao.closeResource(null, prep, rs);
        }
        return count;
    }
>service接口
        //查询记录数
    public int getUserCount(String queryUserName, int queryUserRole);
>service实现类
    	public int getUserCount(String queryUserName, int queryUserRole) {
        //初始化
        Connection conn = null;
        int count =0;
        System.out.println("queryUserName ---- > " + queryUserName);
        System.out.println("queryUserRole ---- > " + queryUserRole);

        try {
            if (StringUtils.isNullOrEmpty(queryUserName)){
                queryUserName="";
            }
            if (queryUserRole<0){
                queryUserRole=0;
            }
            conn = BaseDao.getConnection();
            count = userDao.getUserCount(conn, queryUserName, queryUserRole);

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            BaseDao.closeResource(conn,null,null);
        }
        return count;
    }
```



#### 3、获取用户列表

1、接口

```java
    public List<User> getUserList(Connection conn, String username, int userRole, int currentPageNo, int pageSize) throws SQLException;
```

2、实现类

```java
public List<User> getUserList(Connection conn, String userName, int userRole, int currentPageNo, int pageSize) throws SQLException {
        /*
        select u.*,r.roleName from smbms_user as u, smbms_role as r where u.userRole=r.id
        and u.userName LIKE "%李%" and r.id =2
        order by creationDate desc limit 0,5
         * */
        PreparedStatement prep = null;
        ResultSet rs = null;
        List<User> userList = new ArrayList<User>();

        if (conn != null) {
            //拼接sql查询语句
            StringBuffer _sql = new StringBuffer();
            List<Object> list = new ArrayList<Object>();

            _sql.append("select u.*,r.roleName from smbms_user as u, smbms_role as r where u.userRole=r.id");
            if (!StringUtils.isNullOrEmpty(userName)) {
                _sql.append(" and u.userName LIKE ?");
                list.add("%" + userName + "%");
            }
            if (userRole > 0) {
                _sql.append(" and u.userRole = ?");
                list.add(userRole);
            }
            _sql.append(" order by creationDate desc limit ?,?");

            //组装params
            currentPageNo = (currentPageNo - 1) * pageSize;//mysql offset 例如currentPageNo=3，pageSize=5,则当前偏移量为从limit 10,5
            list.add(currentPageNo);
            list.add(pageSize);
            System.out.println("UserDaoImpl.getUserCount().sql-> " + _sql.toString());

            Object[] params = list.toArray();
            //调用jdbc，执行查询
            rs = BaseDao.executeQuery(conn, prep, rs, params, _sql.toString());

            while (rs.next()) {
                User user = new User();
                user.setId(rs.getInt("id"));
                user.setUserCode(rs.getString("userCode"));
                user.setUserName(rs.getString("userName"));
                user.setUserPassword(rs.getString("userPassword"));
                user.setGender(rs.getInt("gender"));
                user.setBirthday(rs.getDate("birthday"));
                user.setPhone(rs.getString("phone"));
                user.setAddress(rs.getString("address"));
                user.setUserRole(rs.getInt("userRole"));
                user.setCreatedBy(rs.getInt("createdBy"));
                user.setCreationDate(rs.getDate("creationDate"));
                user.setModifyBy(rs.getInt("modifyBy"));
                user.setModifyDate(rs.getDate("modifyDate"));
                user.setUserRoleName(rs.getString("roleName"));
                userList.add(user);
            }

        }
        return userList;
    }
```

3、Service接口

```java
    public List<User> getUserList(String queryUserName, int queryUserRole, int currentPageNo, int pageSize);
```

4、Service实现类

```java
    public List<User> getUserList(String queryUserName, int queryUserRole, int currentPageNo, int pageSize) {
        //初始化
        Connection conn = null;
        List<User> userList = new ArrayList<User>();
        System.out.println("queryUserName ---- > " + queryUserName);
        System.out.println("queryUserRole ---- > " + queryUserRole);
        System.out.println("currentPageNo ---- > " + currentPageNo);
        System.out.println("pageSize ---- > " + pageSize);

        try {
            conn = BaseDao.getConnection();
            userList = userDao.getUserList(conn, queryUserName, queryUserRole, currentPageNo, pageSize);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            BaseDao.closeResource(conn,null,null);
        }
        return userList;
    }
```





#### 4、获取角色列表

为了我们职责统一，可以把角色的操作单独放在一个包中，和POJO类对应

1、role dao层接口

```java
    public List<Role> getRoleList(Connection conn) throws SQLException;
```

2、role dao层实现

```java
    public List<Role> getRoleList(Connection conn) throws SQLException {
        //初始化
        String sql = "select * from smbms_role";
        PreparedStatement prep =null;
        ResultSet rs = null;
        Object [] params = {};
        List<Role> roleList = new ArrayList<Role>();
        //调用jdbc
        prep = conn.prepareStatement(sql);
        rs = BaseDao.executeQuery(conn, prep, rs, params, sql);
        //读取结果插入到list
        while (rs.next()){
            Role role = new Role();
            role.setId(rs.getInt("id"));
            role.setRoleCode(rs.getString("roleCode"));
            role.setRoleName(rs.getString("roleName"));
            role.setCreatedBy(rs.getInt("createdBy"));
            role.setCreationDate(rs.getDate("creationDate"));
            role.setModifyBy(rs.getInt("modifyBy"));
            role.setModifyDate(rs.getDate("modifyDate"));
            roleList.add(role);
        }
        //关闭资源
        BaseDao.closeResource(null, prep,rs);
        return roleList;
    }
```

3、service层接口

```java
    public List<Role> getRoleList();
```

4、Service层实现

```java
    //查询Role列表
    public List<Role> getRoleList() {
        Connection conn = null;
        List<Role> roleList = null;

        try {
            conn = BaseDao.getConnection();
            roleList = roleDao.getRoleList(conn);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            BaseDao.closeResource(conn, null, null);
        }
        return roleList;
    }
```



#### 5、用户显示的Servlet类(重点,难点)

1、获取的用户前端输入数据(用来查询)

2、判断请求是否需要执行，看参数的值来判断

3、为了实现分页，需要计算出当前页面和总页码，以及页面的大小。。。。

4、用户列表展示

5、返回前端

```java
   public void queryUser(HttpServletRequest req, HttpServletResponse resp) {
        // public List<User> getUserList(String queryUserName, int queryUserRole, int currentPageNo, int pageSize)
        // public int getUserCount(String queryUserName, int queryUserRole)
        // public List<Role> getRoleList()

        /**
         * 初始化，从请求获取数据
         */
        String queryUserName = req.getParameter("queryname"); //搜索框用户名
        String temp = req.getParameter("queryUserRole"); //搜索框角色类型
        String pageIndex = req.getParameter("pageIndex"); //获取默认的pageIndex
        int queryUserRole = 0;

        List<User> userList = null; //接收返回的UserList
        List<Role> roleList =null; //接收返回的RoleList

        //第一次获取当前页面用户列表这个请求，一定是第一页，页面大小是固定的
        int pageSize = Constants.pageSize;//设置页面容量
        int currentPageNo = 1;//当前页码，默认为第一页

        /**
         * 测试代码
         * http://localhost:8090/SMBMS/userlist.do
         * ----queryUserName --NULL
         * http://localhost:8090/SMBMS/userlist.do?queryname=
         * --queryUserName ---""
         */
        System.out.println("queryUserName servlet--------" + queryUserName);
        System.out.println("queryUserRole servlet--------" + queryUserRole);
        System.out.println("query pageIndex--------- > " + pageIndex);
        /**
         * 对前端的数据进行非空校验
         */
        if (queryUserName == null) {
            queryUserName = "";
        }
        if (temp != null && !temp.equals("")) {
            queryUserRole = Integer.parseInt(temp);//给查询赋值，真实的值，0,1,2,3，
        }
        if (pageIndex!=null){

            try {
                currentPageNo = Integer.parseInt(pageIndex);
            } catch (NumberFormatException e) {
                try {
                    resp.sendRedirect("error.jsp");
                } catch (IOException ioException) {
                    ioException.printStackTrace();
                }
            }
        }
        /**
         *  下面操作为准备传递给查询用户列表的页面分页信息
         *
         */
        //记录总数量(表)  获取列表
        UserService userService = new UserServiceImpl();
        int totalCount = userService.getUserCount(queryUserName, queryUserRole);

        //总页数支持
        PageSupport pages = new PageSupport();
        pages.setCurrentPageNo(currentPageNo);
        pages.setPageSize(pageSize);
        pages.setTotalCount(totalCount);

        int totalPageCount = pages.getTotalPageCount();//当前一共有多少页

        //控制页首和页尾
        if (currentPageNo<1){//如果页面要小于1，就显示第一页的东西
            currentPageNo=1;
        }else if (currentPageNo>totalPageCount){//如果当前页面大于最后一页，就显示最后一页
            currentPageNo = totalPageCount;
        }
        //调用服务，获取用户列表userList
        userList = userService.getUserList(queryUserName, queryUserRole, currentPageNo, pageSize);
        //调用服务，获取角色Role List
        RoleService roleService = new RoleServiceImpl();
        roleList = roleService.getRoleList();


        //将数据返回给前端
        req.setAttribute("userList",userList);
        req.setAttribute("roleList", roleList);
        req.setAttribute("queryUserName", queryUserName);
        req.setAttribute("queryUserRole", queryUserRole);
        req.setAttribute("totalPageCount", totalPageCount);
        req.setAttribute("totalCount", totalCount);
        req.setAttribute("currentPageNo", currentPageNo);
        //重定向到userlist.jsp
        try {
            req.getRequestDispatcher("userlist.jsp").forward(req,resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```



#### 6、增删改的实现及总结

总结：

在用户的增删改查中，我们遇到的知识点

- 从前端向后端发起请求的方式有

  1) 超链接直接跳转到页面，例如/jsp/pwdmodify.jsp 这种不需要定义servlet

  2) 请求地址为 /jsp/user.do?method=query ，在servlet定义<url-pattern>/jsp/user.do</url-pattern>,同时可获取参数

  3)js发起请求方式

  4)通过表单提交 <form id="userForm" name="userForm" method="post" action="${pageContext.request.contextPath }/jsp/user.do">  参数可以放到表单的input中

- 一般servlet的基本作用是依次为：处理前端请求，调用业务函数，处理返回页面，处理前端请求需要接收前端传递参数，并进行处理，

业务函数为调用Service，进行底层数据操作和判断逻辑实现，返回页面为根据处理结果，决定跳转目录

- 返回页面有两种，需要将需要返回的页面进行处理，将对应的参数包装进去 ，返回参数 有 page,req,session，ServletContext 四种作用域

  ```
  resp.sendRedirect(req.getContextPath() + "/jsp/user.do?method=query");
  req.getRequestDispatcher("usermodify.jsp").forward(req, resp);
  ```

- 返回参数作用域，返回参数 有 page,req,session，ServletContext 四种作用域

  ​	page: 参数在当前页面

  ​	req: 参数在一次请求持续时间

  ​	session：参数在一次session

  ​	servletContext 参数在一次服务器启动期间

- 返回参数，如果需要Json，一般通过先包装进入到HashMap，通过执行writer写入json数据

  ```java
   if (StringUtils.isNullOrEmpty(userCode)) {
              resultMap.put("userCode", "empty");
          } else {
              UserService userService = new UserServiceImpl();
              User user = userService.selectUserCodeExist(userCode);
              if (user != null) {
                  resultMap.put("userCode", "exist");
              } else {
                  resultMap.put("userCode", "notexist");
              }
          }
   resp.setContentType("application/json");
  resp.getWriter().write(JSONArray.toJSONString(resultMap))
  ```

- 解决搜索和查询列表的方法，可以有基础查询语句，然后后面，根据是否有参数进行拼接，注意空格，例如用户列表查询，既有查询全部列表，也有可能查询用户名，也有可能查询用户角色

  ```java
  StringBuffer _sql = new StringBuffer();
  List<Object> list = new ArrayList<Object>();
  
  _sql.append("select u.*,r.roleName from smbms_user as u, smbms_role as r where u.userRole=r.id");
  
  if (!StringUtils.isNullOrEmpty(userName)) {
      _sql.append(" and u.userName LIKE ?");
      list.add("%" + userName + "%");
  }
  if (userRole > 0) {
      _sql.append(" and u.userRole = ?");
      list.add(userRole);
  }
  
  _sql.append(" order by creationDate desc limit ?,?");
  
  //组装params
  currentPageNo = (currentPageNo - 1) * pageSize;//mysql offset 例如currentPageNo=3，pageSize=5,则当前偏移量为从limit 10,5
  list.add(currentPageNo);
  list.add(pageSize);
  System.out.println("UserDaoImpl.getUserCount().sql-> " + _sql.toString());
  ```

- 组装params时，如果当前是一个更新或者新增，那么直接将对应Pojo对象值插入到params列表

```java
Object[] params = {user.getUserCode(), user.getUserName(), user.getUserPassword(), user.getGender(),
        user.getBirthday(), user.getPhone(), user.getAddress(), user.getUserRole(), user.getCreatedBy(),
        user.getCreationDate()};
```

- 以json格式返回一个List的方式

```java
List<Role> roleList = roleService.getRoleList();

//因为前端接收json,我们需要设置返回文本类型
try {
    resp.setContentType("application/json");
    PrintWriter writer = resp.getWriter();
    //使用阿里巴巴的JSONArray工具
    writer.write(JSONArray.toJSONString(roleList));
    writer.flush();
    writer.close();
} catch (Exception e) {
    e.printStackTrace();
}
```

- 实现vip等过滤的方式，比如所有进入/jsp/的目录都需要查询是否是登录用户

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
    System.out.println("SysFilter doFilter()===========");
    HttpServletRequest req= (HttpServletRequest)request;
    HttpServletResponse resp= (HttpServletResponse)response;
    //System.out.println("request.getContextPath():"+request.getContextPath());
    User userSession = (User)req.getSession().getAttribute(Constants.USER_SESSION);

    if (userSession==null){
        resp.sendRedirect(req.getContextPath()+"/error.jsp");
    }else {
        chain.doFilter(request,response);
    }
}
过滤器
    <!--权限过滤-->
    <filter>
    <filter-name>SysFilter</filter-name>
    <filter-class>top.aigoo.filter.SysFilter</filter-class>
        </filter>   
<filter-mapping>
    <filter-name>SysFilter</filter-name>
    <url-pattern>/jsp/*</url-pattern>
    </filter-mapping>
```





### 11.6 文件传输原理及实现

上传文件原理，网络传输工程十分浩大

![image-20201209124459173](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201209124459173.png)



1、首先让浏览器支持文件上传 <input type=file>

2、下载我们需要的依赖包

​	https://mvnrepository.com/artifact/commons-io/commons-io

​	https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload



3、导入jar的的两种方式

​	方式一、在工程目录下，创建一个lib文件夹，将jar包添加进入这个Lib文件夹，然后点击Lib文件夹--右键，选择mark as libary；

​	方式二、选择project structer，点击Libraries，点击添加，可以添加一个对应的目录，然后选择Artifacts，将包添加到打包文件中；

4、文件上传注意事项及调优方法：

​	1、为了保证服务器安全，上传文件应该放在外界无法直接访问的目录下，比如放在WEB-INF目录下

​	2、要防止文件覆盖的现象发生，要为上传文件产生一个唯一的名字

​	3、要限制上传文件的最大值

​	4、可以限制上传文件类型，在收到上传文件名时，判断后缀名是否合法

5、常用的类解释

​	ServletFileUpload， 负责处理上传的文件数据，并将表中每个输入项封装成一个FileItem对象，在使用ServletFileUpload对象解析请求时，需要DiskFileItemFactory对象，所以我们需要在进行解析工作签构造好DiskFileItemFactory对象，通过ServletFileUpload对象的构造方法或设置setFileItemFacoty方法设置ServletFileUpload对象的fileItemFactory属性

6、表单如果包含一个文件上传输入项的话，这个表单的enctype属性就必须设置为 enctype="multipart/form-data"

```jsp
<%--通过表单上传文件--%>
<form action="" method="post" enctype="multipart/form-data">
    上传用户:<input type="text" name="username"> <br/>
    <p><input type="file" name="file1"></p>
    <p><input type="file" name="file2"></p>
    <p>
        <input type="submit">
        <input type="reset">
    </p>
</form>
```

[常用方法介绍]

```java
//isFormField   用于判断FileItem类对象封装的数据是一个普通文本表单还是一个文件表单，如果是普通表单则返回true，否则返回false

boolean isFormField();


//getFieldName方法用于返回表单标签name属性的值

Str ing getFileldName();

//getString方法用于将FileItem对象中保存数据流内容以一个字符串返回
String getString();



//getName方法用于获得文件上传字段中的文件名字

String getName

//以流的形式返回上传文件的数据内容

InputStream getInputStream()


//delete方法用来清空FileItem类对象中存放的主题内容

如果主体内容被保存在临时文件中，delete方法将删除该临时文件

void delete()
```



**上传文件的常用步骤**

1、编写前端，需要使用表单提交，并且使用input type=“file”，表单需要有一个enctype="multipart/form-data" ,前端才能传文件

2、编写后台，注册Servlet，使用post接收数据，因为post接收数据比较大，get最大数据只有4k

2.1、先判断上传文件是表单文件还是文件,不是文件就直接跳出

```java
if (!ServletFileUpload.isMultipartContent(req)) {
    return;//如果是普通文件，我们就直接返回
}
```

2.2 创建上传文件的保存目录，记住需要放到用户无法直接访问的地区 WEB-INF

```java
String uploadPath = this.getServletContext().getRealPath("/WEB-INF/upload");
File uploadFile = new File(uploadPath);
if (!uploadFile.exists()) {
    uploadFile.mkdir();//如果这个文件不存在，就创建这个目录
}

//#3.加入一个缓存的概念,我们叫临时文件，加入文件超过我们预期的大小，我们就把他放到一个临时文件中，过几天自动删除或者提醒用户转存永久
String tempPath = this.getServletContext().getRealPath("/WEB-INF/temp");
File tempFile = new File(tempPath);
if (!tempFile.exists()) {
    tempFile.mkdir();//如果这个文件不存在，就创建这个临时目录
}
```

2.3 处理上传文件，我们通过使用ServletUploadFile来处理这个事情

2.3.1、创建一个DiskFileItemFactory，因为ServletUploadFile需要用到这个，并设置上传路径和文件大小

```java
DiskFileItemFactory factory = getDiskFileItemFactory(tempFile);

public static DiskFileItemFactory getDiskFileItemFactory(File file) {
    DiskFileItemFactory factory = new DiskFileItemFactory();
    //通过这个工厂设置一个缓冲区，当上传的文件大于这个缓冲区的时候，将他放到临时文件夹中
    factory.setSizeThreshold(1024 * 1024); //缓冲区大小为1M
    factory.setRepository(file); //临时目录的保存目录，需要一个file
    return factory;
}
```

2.3.2、创建ServletUpload，可以设置上传进度、乱码，单个文件最大，全部文件最大的配置

```java
ServletFileUpload upload = getServletFileUpload(factory);

public static ServletFileUpload getServletFileUpload(DiskFileItemFactory factory) {
    ServletFileUpload upload = new ServletFileUpload(factory);
    //监听文件上传进度
    upload.setProgressListener(new ProgressListener() {
        @Override
        //pBytesRead 已经读取到的文件的大小
        //pContentLength 文件的大小
        //pItems
        public void update(long pBytesRead, long pContentLength, int pItems) {
            System.out.println("总大小:" + pContentLength + " 已上传:" + pBytesRead);
        }
    });

    //处理乱码问题
    upload.setHeaderEncoding("utf-8");
    //设置单个文件的最大值
    upload.setFileSizeMax(1012 * 1024 * 10);
    //设置总共能够上传的文件的大小
    //1024 =1kb*1024 =1M*10 = 10M
    upload.setSizeMax(1024 * 1024 * 10);
    return upload;
}
```

2.3.3 处理请求，并使用ServletUploadFile的parseRequest(request)生成FileItem

```java
 String msg = uploadParsetRequest(upload, req, uploadPath);

public static String uploadParsetRequest(ServletFileUpload upload, HttpServletRequest req, String uploadPath)
    throws FileUploadException, IOException {
    String msg = "";
    //把前端请求解析，封装为一个FileItem对象
    List<FileItem> fileItems = upload.parseRequest(req);

    for (FileItem fileItem : fileItems) {
        if (fileItem.isFormField()) {//判断上传的文件是普通的表单还是带文件的表单
            //getFieldName指的是前端表单控件的name
            String name = fileItem.getFieldName();
            String value = fileItem.getString("utf-8");//处理乱码
            System.out.println(name + ":" + value);
        } else {//判断他是上传的文件
            //===========================处理文件========================//
            System.out.println("字段名:" + fileItem.getFieldName() + " 文件名:" + fileItem.getName() + " 文件大小:" + fileItem.getSize() + " 文件类型:" + fileItem.getContentType());
            //拿到文件名字
            String uploadFileName = fileItem.getName();
            System.out.println("上传的文件名:" + uploadFileName);
            //uploadFileName.trim（）是去掉首位空格
            if (uploadFileName.trim().equals("") || uploadFileName == null) {
                continue;
            }

            //获得上传的文件名 /images/girl/paojie.png
            String fileName = uploadFileName.substring(uploadFileName.lastIndexOf("/") + 1);
            //获取文件的后缀名
            String fileExtName = uploadFileName.substring(uploadFileName.lastIndexOf(".") + 1);

            /**
                 * 如果文件名后缀fileExtname不是我们需要的就直接return不处理，告诉用户类型不对
                 */
            System.out.println("文件信息[文件名:" + fileName + "----文件类型: " + fileExtName + "]");
            /**
                 * 可以使用UUID(唯一识别的通用码)保证文件名唯一；UUID.randomUUID()随机生成一个唯一识别的通用码；
                 */
            String uuidPath = UUID.randomUUID().toString();
            //============================处理文件完毕================================
            //存到那儿? uploadPath
            //文件真实存在的路径  realPath
            String realPath = uploadPath + "/" + uuidPath;
            //给每个文件创建一个对应的文件夹
            File realPathFile = new File(realPath);
            if (!realPathFile.exists()) {
                realPathFile.mkdir();
            }
            //=================存放地址完毕==========================
            //获得文件上传的文件流
            InputStream inputStream = fileItem.getInputStream();
            //创建一个文件输出流
            //realPath = 真实的文件夹
            //差了一个文件，加上输出文件的名字+“/”+uuidFileName
            FileOutputStream fos = new FileOutputStream(realPath + "/" + fileName);
            //创建一个缓冲区
            byte[] buffer = new byte[1024 * 1024];

            //判断是不是读取完毕
            int len = 0;
            //如果大于0说明还存在数据；
            while ((len = inputStream.read(buffer)) > 0) {
                fos.write(buffer, 0, len);
            }
            //关闭流
            fos.close();
            inputStream.close();

            msg = "文件上传成功";
            fileItem.delete();//上传成功，清除临时文件
            //====================文件传输完毕=====================
        }

    }
    return msg;
}
```



3. 处理返回

```java
req.setAttribute("msg", msg);
req.getRequestDispatcher("info.jsp").forward(req, resp);//转发
resp.sendRedirect(req.getContextPath() + "/jsp/info.jsp");//重定向
```







**作业：这几个概念要去学习查一下，温度下**

UUID

Serializer

native

thread

源码就需要多HashMap底层和多线程底层





###  11.7 邮件发送原理及实现



![image-20201210122842904](F:\MD格式学习笔记库\狂神JAVA-12-JavaWeb入门到实战.assets\image-20201210122842904.png)



发送邮件的基本流程

```
//1.创建定义整个应用程序所需要的环境信息的session对象
//2.通过session得到transport
//3.使用邮箱的用户名和授权码 连接服务器
//4.创建邮件
//5.发送邮件
```









