Spring MVC 学习

## 1. 回顾servlet

JAVASE 认真学习，老师带，入门快

JavaWeb  认真学习，老师带，入门快

SSM框架，研究官方文档，锻炼自学能力，锻炼笔记能力，锻炼项目能力

Spring MVC+VUE+SpringBoot+SpringCloud+Linux

Srping IOC 和AOP

Spring MVC 执行流程特别重要! 需要重点听懂！

Sring mvc ssm框架整合，这个也是重点!

### 什么是MVC？

模型(Dao,service)   视图(jsp)     控制器(Servlet) 是一种软件设计的规范!

是将业务逻辑，数据显示分离的方法来组织代码

职责分析

Controller:控制器

1、取得表单数据

2、调用业务逻辑

3、转向指定的页面

Model： 模型

1、实现业务逻辑

2、保存数据的状态

View 视图

1、显示页面

### 回顾Servlet

如何新建一个servlet项目？

首先，创建一个空的maven父项目，创建后将src目录删除掉。同时加上依赖

```xml
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
```

第二步：点击项目，创建一个Module，也是不选择wepapp模板，创建一个springmvc-01-serlvet的子项目，添加成功后，在项目添加web Framework support

![`image-20201225121524456`](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201225121524456.png)

第三步: 编写一个Servlet类，用来处理用户的请求

```java
//实现servlet接口
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获得参数
        String method = req.getParameter("method");
        if (method.equals("add")&&method!=null){
            req.getSession().setAttribute("msg","执行了add方法");
        }
        if (method.equals("delete")&& method!=null){
            req.getSession().setAttribute("msg","执行了delete方法!");
        }
        //业务逻辑部分

        //视图跳转部分
        req.getRequestDispatcher("/WEB-INF/jsp/hello.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

第四步： 到web.xml里面配置servlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <session-config>
        <!--配置session30分钟超市-->
        <session-timeout>30</session-timeout>
    </session-config>
    <!--配置欢迎页-->
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```

第五步: 在WEB-INF文件夹下面，创建一个jsp文件夹，并添加hello.jsp文件

> 关于文件放到WEB-INF还是web的知识点：
>
> 如果项目文件让用户不可见，可以放到/web/WEB-INF/下面
>
> 如果共用的文件，可以放到/web/文件夹



> jsp页面取参数的形式  ${msg} 



MVC框架做了哪些事情？

1、将URL映射到JAVA类或者JAVA类的方法

2、封装用户提交的数据

3、处理请求-调用相关的业务处理--封装响应数据。

4、将响应的数据进行渲染 jsp/html等表示层数据

说明：

常见服务器端MVC框架有struts,SpringMVC ASP。net MVC  zend Framework,jsf,常见的mvc前端框架：vue angularjs,react,backbone,由MVC演化出来另外一些模式

比如MVP  MVVM等等....



## 2.初识springmvc及原理

Spring MVC是Spring Framework的一部分，是基于Java实现的MVC的轻量级Web框架!

为什么学习Spring MVC呢

1、轻量级，简单易学

2、高效，基于请求响应的MVC构架

3、与spring兼容性好，无缝结合

4、约定优于配置

5、功能强大RESTFul,数据验证，格式化，本地化，主题等

6、简洁灵活

### 1.springmvc 概述



Spring：大杂烩，我们可以将SpringMVC中所有需要的bean注册到Spring中

Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分发到不同的处理器，从spring2.5开始，使用java5或者以上版本的用户可以采用基于注释的controller声明方式

Spring mvc框架像许多其他mvc框架一样，以请求为驱动，围绕一个中型Servlet分配请求和提供其他功能，DispatcherServlet实际上也是一个Servlet（它集成自HttpServlet基类）

![image-20201225134130314](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201225134130314.png)



Spring MVC的原理如下图所示:

​	当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制其，控制器处理请求，创建数据类型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者!

![image-20201225134657571](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201225134657571.png)



### 2.第一个springmvc

首先，创建一个空的项目，项目是个maven子项目

然后，在项目中添加web支持

第3步, 在web.xml中注册DispatcherServlet

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册DispathcerServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件:[servlet-name]-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <!--/ 匹配所有的请求:不包括.jsp-->
    <!--/* 匹配所有的请求:包括.jsp-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

第4步，编写springmvc的配置文件，在src/main/resources 下新建 springmvc-servlet.xml:[servletname]-servlet.xml，说明，这里的名称要求是按照官方来的。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

第5步： 添加处理映射器

```xml
    <!--添加处理映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
```

第6步，添加处理适配器

```xml
   <!--添加处理适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```



第7步 添加视图解析器

```xml
    <!--添加视图解析器-->
    <!--视图解析器:DispatcherServlet给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```

第8步：编写我们操作业务的Controller，要么实现Controller接口，要么增加注解，需要返回一个ModelAndView,装数据，封视图

```java
package com.kuang.controller;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//注意 这里我们要先导入Controller接口
public class HelloController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        // ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();
        //封装对象，放在ModelAndView中，Model
        mv.addObject("msg","HelloSpringMVC!");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello"); //WEB-INF/jsp/hello.jsp
        return mv;
    }
}

```

第9步 将自己的类交给springioc容器，在springmvc-servlet.xml 注册bean, 注意这里的bean id必须以 /开头

```xml
    <!--Handler-->
    <bean id="/hello" class="com.kuang.controller.HelloController"/>
```

第10步 在/web/WEB-INF/jsp/hello.jsp   写要跳转的jsp页面，显示ModelAndView存放的数据，以及我们的正常页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>springmvc</title>
</head>
<body>
    ${msg}
</body>
</html>
```



可能到的问题:访问出现404，排查步骤

1、查看控制台输出，看一下是不是少了什么jar包

2、如果jar报存在 ，显示无法输出，就在IDEA的项目发布中，添加lib依赖

3、重启Tomcat即可解决

小结：看这个估计大部分同学就能理解其中原理了，但是我们实际开发不会这么写，不然就疯了，还学这个玩意干嘛，我们来看注解版实现，这个才是springmvc的精髓，到底多么简单，看这个图就知道了。



### 3. springmvc原理

Spring MVC的原理如下图所示:

​	当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制其，控制器处理请求，创建数据类型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者!



![image-20201225154309926](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201225154309926.png)

​	老师画的精细版SPRINGMVC执行原理，

![image-20201225151715507](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201225151715507.png)

这个图是spring mvc 的一个完整的流程图，只有虚线才是我们要做的，实现部分都是spring mvc已经帮我们做了的。





简要的分析一下执行的流程：

1、DispatcherServlet表示前置的控制器，是整个spring mvc的控制中心，用户发出请求，DispatcherServlet接受请求并拦截请求

假设url为 http://localhost:8080/springmvc/hello 这个url就可以拆分成3个部分

http://localhost:8080  这是服务器的域名

springmvc部署在服务器上的web站点

hello  表示控制器

通过分析，如上url表示为:请求位于服务器localhost:8080 上的springmvc站点的hello控制器

2、HandlerMapping为处理器映射，由DispathcerServlet自动调用

HandlerMapping根据请求url的控制器去查找在springmvc-config.xml里面的Handler.

3、HandlerExecution表示具体的Handler，其主要作用是根据url查找控制器，如上url被查找的控制器为 hello

4、HandlerExecution 将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5、HandlerApdapter表示处理器适配器，其按照特定的规则去执行Handler.

6、Handler让具体的Controller执行

7、Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView.

8、HandlerAdapter将试图逻辑名或者模型传递给DispatcherServlet

9、DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名

10、试图解析器将解析的逻辑视图名传递给DispatcherServlet.

11、DispatcherServlet根据试图解析器解析后的视图结构，调用具体的视图

12、最终视图呈现给用户，

在这里听一遍原理，不理解么有关系，我们马上来一写一个对应的代码实现大家就明白了，如果不明白，那就写10遍，没有笨人，只有懒人!

> spring mvc中  
>
> ```
> <servlet-mapping>
>     <servlet-name>springmvc</servlet-name>
>     <url-pattern>/</url-pattern>
> </servlet-mapping>
> ```
>
> / 和/* 有区别 
>
> / :只匹配所有的请求，不会去匹配jsp页面
>
> /*: 匹配所有的请求，包括Jsp页面

## 3. 使用注解开发SpringMVC



第1步：创建一个空的maven项目，并加入web支持，此处需要记住，web.xml必须使用4.0及以上版本，不然会出现代码无法运行

![image-20201226011001808](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201226011001808.png)

第2步: 配置项目的依赖，需要关注project structure，里面需要在项目里面加入lib包

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>springmvc-study</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>springmvc-01-servlet</module>
        <module>spring-03-hellomvc</module>
        <module>spring-02-practice1</module>
        <module>spring-02-practice2</module>
        <module>springmvc-04-annotation</module>
        <module>springmvc-04-practice1</module>
    </modules>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
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

![image-20201226011358741](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201226011358741.png)



第3步：配置web.xml，注册DispatcherServlet的servlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

第4步：在src/main/resources 下面建立 springmvc-servlet.xml，在这个配置文件需要干如下几件事

​	1、开启包自动扫描

​	2、开启注解驱动，自动加载控制器映射器HandlerMapping和控制器适配器HandlerAdapter

​	3、配置视图解析器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/cache"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/cache http://www.springframework.org/schema/cache/spring-cache.xsd">

    <context:component-scan base-package="com.kuang.controller"/>
    
    <mvc:annotation-driven/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```



第5步: 创建我们的jsp文件夹，在/WEB-INF/下

![image-20201226012221395](F:\MD格式学习笔记库\Spring MVC学习.assets\image-20201226012221395.png)



在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。



第6步：创建Controller,编写一个Java控制类，com.kuang.controller.HelloController, 注意编码规范

```java
@Controller
@RequestMapping("/HelloController")
public class HelloController {
    //真实访问地址: 项目名/HelloController/hello
    @RequestMapping("/hello")
    public String sayHello(Model model){
        //向模型中添加属性msg与值，可以在jsp页面中取出并渲染
        model.addAttribute("msg","hello,Spring MVC");
        //web-inf/jsp/hello.jsp
        return "hello";
    }

}
```



@Controller 是为了让Spring IoC容器初始化时自动扫描到；

@RequestMapping 是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是 /HelloController/hello

方法中声明Model类型的参数是为了把Action中的数据带到视图中

方法返回的结果是视图的名称hello 加上配置文件中的前后缀就变成了 WEB-INF/jsp/hello.jsp

第7步，创建视图层 hello.jsp

在WEB-INF/jsp目录创建hello.jsp，视图可以直接取出并展示出从Controller带回来的信息，可以通过EL表示取出来的Model中存放的值，或者对象!

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>springmvc-annotation</title>
</head>
<body>
    ${msg}
</body>
</html>
```

第8步 配置tomcat并运行

配置tomcat，开启服务器，访问对应的请求路径



小结：

实现步骤非常简单，分为如下

1、新建一个web项目，子项目

2、导入相关的jar包，配置pom.xml文件

3、编写web.xml,注册我们的DispatcherServlet

4、编写springmvc配置文件

5、接下来就是创建控制类，controller

6、最后完善前端视图和controller之间的对应关系

7、运行测试调试



使用springmvc必须配置三大件

处理映射器，处理器适配器，视图解析器

通常，我们只需要手动配置视图解析器，而处理映射器和处理器适配器只需要开启注解驱动即可<mvc:annotation-driven/>，这样可以省去大段的xml配置文件

和原来的对比

|                       | xml配置方式实现springmvc                                     | Annotation方式实现springmvc                                  |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建项目              | 创建一个空白的maven项目                                      | 创建一个空白的maven项目                                      |
| pom.xml               | 相同的类包，都需要添加lib                                    | 相同的类包，都需要添加lib                                    |
| web.xml               | 都需要配置，相同的配置 DispatcherServlet                     | 都需要配置，相同的配置 DispatcherServlet                     |
| springmvc-servlet.xml | 手动配置 处理器映射器 BeanNameUrlHandlerMapping<br />手动配置 处理器适配器 SimpleControllerHandlerAdapter<br /> 手动配置 视图解析器  InternalResourceViewResolver<br /> 手动配置 对应bean id="/hello" 控制器 | 开启自动扫包 <context:component-scan base-package="com.kuang.controller"/><br /> 开启注解驱动 ，自动生成映射器和适配器  <mvc:annotation-driven/> <br /> 开启视图解析器 ，InternalResourceViewResolver |
| Controller            | 实现Controller接口<br />                                     | @Controller<br /><br /> @RequestMapping                      |
|                       |                                                              |                                                              |



## 4.Controller控制器配置总结

控制器 Controller，复杂提供访问应用程序的行为，通常通过接口定义或者注解定义两种方式实现

控制器Controller ： 负责解析用户的请求并将其转换为一个模型

在spring mvc中一个控制器可以包含多个方法

在spring mvc中，对于controller的配置方式有很多种，下面讲述下具体的配置方式



### 1 方式一：实现Controller接口

第1步，新建一个maven子项目，并添加web能力支持，配置项目的lib包内容

![image-20201226130113329](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201226130113329.png)



第2步，编写web.xml，配置DispatcherServlet, 配置springmvc-servlet.xml,

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

第3步，编写web.xml，配置springmvc-servlet.xml, 配置springmvc-servlet.xml,

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <context:component-scan base-package="com.kuang.controller"/>

    <mvc:annotation-driven/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

第4步 实现Controller，并在springmvc-servlet.xml里面配置一下



```java
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ControllerTest1 implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "ControllerTest1");
        modelAndView.setViewName("test");
        return modelAndView;
    }
}
```

springmvc-xml.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <context:component-scan base-package="com.kuang.controller"/>
    <mvc:default-servlet-handler/>
    <mvc:annotation-driven/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <bean id="/t1" class="com.kuang.controller.ControllerTest1"/>

</beans>
```

第5步，配置tomcat，然后即可进行测试



小结：

1、实现接口controller定义控制器是一个较老的办法

2、缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller,定义的方式比较麻烦；

3、我们在springmvc-servlet.xml里面删掉控制器映射器，控制器适配器，一样可以实现，因为springmvc会根据默认的去查找

```xml
springmvc-servlet.xml
    <!--<context:component-scan base-package="com.kuang.controller"/>-->
    <!--<mvc:default-servlet-handler/>-->
    <!--<mvc:annotation-driven/>-->
```





### 2 使用注解方式实现controller

注解的说明：

@Component   代表是spring一个组件

@Service     代表service

@Controller    代表controller

@Repository



@Controller，注解的类，中的所有方法，如果返回值是String,并且有具体的页面可以跳转，那么就会被视图解析器解析！

注解方式必须开启如下三个配置

```xml
<context:component-scan base-package="com.kuang.controller"/>
<mvc:default-servlet-handler/>
<mvc:annotation-driven/>
```





### 3 RequestMapping

用于映射URL到控制器类或一个特定的处理程序方法。可以用于类或者方法上，用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径

我们可以测试下,只在方法上面加RequestMapping

```java
@Controller
public class ControllerTest3 {
    @RequestMapping("/test1")
    public String test(){
        return "test";
    }
}
```

访问路径: http://localhost:8080/项目名/test1

- 同时注解类和方法

```java
@Controller
@RequestMapping("/admin")
public class ControllerTest3 {
    @RequestMapping("/test1")
    public String test(){
        return "test";
    }
}
```

访问路径: http://localhost:8080/项目名/admin/test1

### 4 RestFul风格

RestFul就是一个资源定位和资源操作的风格，不是标准也不是协议，是一种风格，基于这个风格设计的软件可以更加简洁、更有层次、更易于实现缓存的机制

功能

资源：互联网所有的事务都可以抽象为资源

资源操作，使用POST,DELETE,PUT,GET, 使用不同方法对资源进行操作。

分别硬添加、删除、修改、查询

传统是操作资源：通过不同的参数来实现不同的效果!方法很单一，post和get

http://127.0.0.1/item/queryItem.action?id =1 查询，GET

http://127.0.0.1/item/saveItem.action 新增，POST

http://127.0.0.1/item/updateItem.action   更新，POST

http://127.0.0.1/item/deleteItem.action?id =1 删除，GET或者POST



更换为使用RESTFul操作资源，可以通过不同的请求方式来实现不同的效果，例如，请求地址一样，但是功能却不同

http://127.0.0.1/item/1   查询 GET

http://127.0.0.1/item/   新增POST

http://127.0.0.1/item/   更新 PUT

http://127.0.0.1/item/1   删除 DELETE



学习测试一下示例代码

第1步  新建一个类RestFulController

```java
@Controller
public class RestFulController {
    //原来的 http://localhost:8080/add?a=1&b=2
    //RestFul : http://localhost:8080/add/a/b/
    
    @RequestMapping("/add")
    public String test1(int a, int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}
```

第2步  在Srping mvc 中可以使用@PathVariable注解，让方法参数的值对应绑定到一个URI模板变量上

```java
@Controller
public class RestFulController {
    //原来的 http://localhost:8080/add?a=1&b=2
    //RestFul : http://localhost:8080/add/a/b/

    @RequestMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}
```

`注意`: 

1、{a}和{b} 需要使用@PathVariable注解的 形参名字

2、http://localhost:8080/add/3/4/ 可以正确调用

3、http://localhost:8080/add/3/demo/  这个就不能正确调用，因为格式要和形参的int格式匹配



第3步  增加对method的支持，

```java

@Controller
public class RestFulController {
    //原来的 http://localhost:8080/add?a=1&b=2
    //RestFul : http://localhost:8080/add/a/b/

    @RequestMapping(path ="/add/{a}/{b}",method= RequestMethod.GET )
    public String test1(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}

```

RequestMapping的参数 ，这里面重点介绍如下几个，

path   or value 就是我们之前的url路径， path ="/add/{a}/{b}"

method  :  这里的method代表前端需要发起的请求，这样就符合了RESTFul method= RequestMethod.GET 



小结：

Spring mvc的@RequestMapping注解能够处理HTTP请求的方法，比如GET PUT DELETE  POST以及PATCH

所有地址栏请求默认都会是 HTTP GET类型的

方法级别的注解变体有如下几个：组合注解

```
@GetMapping   @PostMapping  @PutMapping @DeleteMapping  @PatchMapping
```

@GetMapping就是一个组合注解

他相当于  @ReqeustMapping(mathod=RequestMethod.GET )的一个快捷方式，平时使用的会比较多

```java
@Controller
public class RestFulController {
    //原来的 http://localhost:8080/add?a=1&b=2
    //RestFul : http://localhost:8080/add/a/b/

    @RequestMapping(path ="/add/{a}/{b}",method= RequestMethod.GET )
    public String test1(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }

    @GetMapping(path ="/add/{a}/{b}")
    public String test2(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
    
    @PostMapping(path ="/add/{a}/{b}")
    public String test3(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}
```



思考： 使用路径变量的好处

1、 是路径变得更加简洁

2、获得参数更加方便，框架会自动进行类型转换

3、通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径/commit/1/a，则路径与方法不匹配，则不会是参数转换失败。

###  5.结果跳转ModelAndView

ModelAndView， 根据view的名称,和视图解析器调到指定的页面

页面= {视图解析器前缀}+viewName+{视图解析器后缀}

一个视图解析器

```xml

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

对应Controller类

```java
public class ControllerTest1 implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "ControllerTest1");
        modelAndView.setViewName("test");
        return modelAndView;
    }
}
```



ServletAPI实现重定向和跳转

通过设置原生的ServletAPI，不需要视图解析器, 我们也能够进行跳转，访问及重定向等操作

1、通过HttpServletResponse进行输出

1、通过HttpServletResponse实现重定向

1、通过HttpServletResponse进行转发

```java
@Controller
public class ForwardController {
    @RequestMapping("/result/t1")
    public void test1(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.getWriter().println("Hello,Spring by Servlet API");
    }
    @RequestMapping("/result/t2")
    public void test2(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.sendRedirect("/index.jsp");
    }
    @RequestMapping("/result/t3")
    public void test3(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
        //转发
        req.setAttribute("msg","/result/t3");
        req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,resp);
    }
}
```

springMVC实现重定向和跳转-无视图解析器



springMVC实现重定向和跳转, 无需视图解析器，测试前，需要将视图解析器注释掉

```java
@Controller
public class ResultSpringMVC {
    @RequestMapping("/rsm/t1")
    //转发
    public String test1() {
        return "/index.jsp";
    }
    @RequestMapping("/rsm/t2")
    //转发二
    public String test2() {
        return "forward:/index.jsp";
    }
    @RequestMapping("/rsm/t3")
    //重定向
    public String test3() {
        return "redirect:/index.jsp";
    }
}

```

spring mvc中，

 "/index.jsp"前面的 /就代表是转发的意思  

"forward:/index.jsp" 就是显示的告诉springmvc转发

这个时候使用的就是原生的HttpServletResponse





springMVC实现重定向和跳转-有视图解析器

重定向，不需要视图解析器，本质就是重新请求一个新地方，所以注意路径的问题

可以重定向到另外一个请求实现

```java
@Controller
public class ResultSpringMVC {
    @RequestMapping("/rsm/t1")
    //转发
    public String test1() {
        return "test";
    }

    @RequestMapping("/rsm/t3")
    //重定向
    public String test3() {
        //重定向
        return "redirect:/index.jsp";
        //return "redirect:hello.do"; //hello.do为另一个请求
    }
}
```

## 5. springmvc处理数据

### 5.1 处理前端请求数据

原来httpservlet获取前端请求数据的方式

request.getParamete("username")



springmvc 处理提交数据

1、提交的域中参数名城和处理的参数名称一致

提交数据为 http://localhost:8080/hello?username=kuang

处理方法:

```java
    @RequestMapping("/test1")
    public String test(String username){

        System.out.println(username);
        return "test";
    }
```

2、提交的域名城和树立的参数名称不一致, 使用@RequestParam

提交数据为 http://localhost:8080/hello?username=kuang

```java
    //@RequestParam("username") username是提交的域的名称。
    @RequestMapping("/test1")
    public String test(@RequestParam("username") String name){

        System.out.println(name);
        return "test";
    }
```

3、前端提交的是一个对象

要求提交的表单和对象的属性名一致，方法参数使用对象即可

1、实体类

```java
public class User {
    private int id;
    private int age;
    private String name;
```

2、提交数据为 http://localhost:8080/mvc03/user?name=kuangshen&id=1&age=20

3、处理方法

```java
    @RequestMapping("/user")
    public String user(User user Model model){
        System.out.println(user);
        //将返回结果传递给前端
        model.setAttribute("msg",name);
        return "hello";
    }
}
```

后台输出 User(id=1,name='kuangshen',age=15)_

1，接手前端用户传递的参数，判断参数名字，假设名字直接在方法上，则可以直接使用

2. 假设传递的是一个对象user，匹配User对象中的字段名，如果名字一致则ok，否则匹配不到

http://localhost:8080/mvc03/user?username=kuangshen&id=1&age=20  如这个请求就无法匹配到，因为username和name不匹配

说明：如果使用对象的话，前端传递的参数和对象名必须一致，否则就是null;



### 5.2 数据显示到前端

1、通过ModelAndView

我们前面一直都是如此，就不用过多解释

```java
public class ControllerTest1 implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "ControllerTest1");
        modelAndView.setViewName("test");
        return modelAndView;
    }
}
```

2.、通过Model

```java
    @RequestMapping(path ="/add/{a}/{b}",method= RequestMethod.GET )
    public String test1(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
```

3、通过ModelMap

```java
    @RequestMapping("/hello")
    public String hello(@RequestParam("username") String name, ModelMap modelMap){
        //封装要显示到视图中的数据
        //相当于req.setAttribute("name",name);
        System.out.println(name);
        return "hello";
    }
```

对于新手而言，简单的来说使用区别就是

Model 知识寥寥几个方法适用于储存数据，简化了新手对于Model对象的操作和理解

ModelMap 继承了LinkedMap, 除了实现了自身的一些方法，同样的继承LinkedMap的方法和特性

ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的的挑战

当然更多的以后开发考虑的跟那更多的是性能和优化，就不能单单仅限于此的了解

请使用80%的时间打好扎实的基础，剩下18%的时间研究框架，2%的时间去学点英文，框架的官方文档永远是最好的教程





>
>
>LinkedHashMap
>
>ModelMap  继承LinkedHashMap， 
>
>Model  精简版

### 5.3 乱码问题

首先我们尝试构造一个乱码

第1步  我们可以在首页编写一个提交的表单

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="${pageContext.request.contextPath}/e/t1" method="post">
    <input type="text" name="username">
    <input type="submit">
</form>
</body>
</html>
```

第2步 后台编写对应的处理类

```java
@Controller
public class EncodingController {
    @RequestMapping("/e/t1")
    public String test(Model model, @RequestParam("username")String name){
        System.out.println("===============/e/t1=========");
        model.addAttribute("msg", name); //获取表单的输入
        return "test"; //跳转到test页面显示输入的值
    }
}
```

第3步 输入中文测试，发现乱码

![image-20201226192120042](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201226192120042.png)



不得不说，乱码问题是在我们开发中十分常见的问题，也是让我们程序猿比较头大的问题!

以前乱码问题，我们通过过滤器解决，在SpringMVC给我们提供了一个过滤器，可以在web.xml中配置

修改xml文件需要重启服务器

```xml
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

有些极端情况下，这个过滤器对get的支持不好

修改Tomcat乱码处理方法:

1、修改Tomcat配置，设置编码!

```properties
    <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"/>
```



2、自定义过滤器

先编写自定义的过滤器

```java
public class EncodingFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("========执行字符过滤器");
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");
        servletResponse.setContentType("text/html; charset=utf-8");
        filterChain.doFilter(servletRequest,servletResponse);
    }

    public void destroy() {

    }
}
```

在编辑我们的web.xml注册

```xml
    <!--注册filter-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>com.kuang.filter.EncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



3、如果各种方式都解决不了，还可用下属的自定义办法

```java
package com.kuang.filter;


import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import java.util.Map;

/*
* 解决get和post的请求，全部乱码的过滤器
* */
public class GenericEncodingFilter implements Filter {
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
        //处理response的字符串
        System.out.println("===================doFilter============");
        HttpServletResponse myResp = (HttpServletResponse) resp;
        myResp.setContentType("text/html; charset=UTF-8");

        //转换为与协议相关对象
        HttpServletRequest myReq = (HttpServletRequest) req;
        //对request包装增强
        MyRequest myRequest = new MyRequest(myReq);

        chain.doFilter(myRequest,resp);
    }

    public void destroy() {

    }
}
//自定义request对象，HttpServletRequest的包装类


class MyRequest extends HttpServletRequestWrapper{
    private HttpServletRequest request;
    private boolean flag=true;


    public MyRequest(HttpServletRequest request) {
        super(request);
        this.request=request;
    }

    @Override
    public String getParameter(String name) {
        if(name==null || name.trim().length()==0){
            return null;
        }
        String[] values = getParameterValues(name);
        if(values==null || values.length==0){
            return null;
        }

        return values[0];
    }

    @Override
    /**
     * hobby=[eat,drink]
     */
    public String[] getParameterValues(String name) {
        if(name==null || name.trim().length()==0){
            return null;
        }
        Map<String, String[]> map = getParameterMap();
        if(map==null || map.size()==0){
            return null;
        }

        return map.get(name);
    }

    @Override
    /**
     * map{ username=[tom],password=[123],hobby=[eat,drink]}
     */
    public Map<String,String[]> getParameterMap() {

        /**
         * 首先判断请求方式
         * 若为post  request.setchar...(utf-8)
         * 若为get 将map中的值遍历编码就可以了
         */
        String method = request.getMethod();
        if("post".equalsIgnoreCase(method)){
            try {
                request.setCharacterEncoding("utf-8");
                return request.getParameterMap();
            } catch (UnsupportedEncodingException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }else if("get".equalsIgnoreCase(method)){
            Map<String,String[]> map = request.getParameterMap();
            //需要遍历map 修改value的每一个数据的编码
            if(flag){
                for (String key:map.keySet()) {
                    String[] arr = map.get(key);
                    //继续遍历数组
                    for(int i=0;i<arr.length;i++){
                        //编码
                        try {
                            arr[i]=new String(arr[i].getBytes("iso8859-1"),"utf-8");
                        } catch (UnsupportedEncodingException e) {
                            e.printStackTrace();
                        }
                    }
                }
                flag=false;
            }
            return map;
        }

        return super.getParameterMap();
    }

}
```



在做这个的时候，老师在filter的设置 使用了/ ，从而导致乱码无法解决，实际办法是/* 这样才能所有的含jsp被过滤

## 6.springmvc的Json处理

### 6.1 什么是Json

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

```java
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

```java
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```

**JSON 和 JavaScript 对象互转**

要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法：

```
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
```

要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法：

```java
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```

代码测试

1、新建一个module ，springmvc-05-json ， 添加web的支持

2、在web目录下新建一个 json-1.html ， 编写测试内容

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JSON_秦疆</title>
</head>
<body>

<script type="text/javascript">
    //编写一个js的对象
    var user = {
        name:"秦疆",
        age:3,
        sex:"男"
    };
    //将js对象转换成json字符串
    console.log("------------下面是js对象转换为json字符串---------")
    var str = JSON.stringify(user);
    console.log(str);
    console.log("------------下面是json字符串转换为js对象---------")

    //将json字符串转换为js对象
    var user2 = JSON.parse(str);
    console.log(user2.age,user2.name,user2.sex);

</script>

</body>
</html>
```

3、访问页面，查看控制台输出！

![image-20201227013345801](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201227013345801.png)

### 6.2 controller实现json



Jackson应该是目前比较好的json解析工具了

当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等,我们这里使用Jackson，使用它需要导入它的jar包；

第1步， 我们需要先导入他的依赖包  jackson

```properties
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
   <groupId>com.fasterxml.jackson.core</groupId>
   <artifactId>jackson-databind</artifactId>
   <version>2.9.8</version>
</dependency>
```

第2步，配置springmvc需要的配置，web.xml和springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
        version="4.0">

   <!--1.注册servlet-->
   <servlet>
       <servlet-name>SpringMVC</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:springmvc-servlet.xml</param-value>
       </init-param>
       <!-- 启动顺序，数字越小，启动越早 -->
       <load-on-startup>1</load-on-startup>
   </servlet>

   <!--所有请求都会被springmvc拦截 -->
   <servlet-mapping>
       <servlet-name>SpringMVC</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>

   <filter>
       <filter-name>encoding</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
           <param-name>encoding</param-name>
           <param-value>utf-8</param-value>
       </init-param>
   </filter>
   <filter-mapping>
       <filter-name>encoding</filter-name>
       <url-pattern>/</url-pattern>
   </filter-mapping>

</web-app>
```

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

   <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
   <context:component-scan base-package="com.kuang.controller"/>

   <!-- 视图解析器 -->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
         id="internalResourceViewResolver">
       <!-- 前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/" />
       <!-- 后缀 -->
       <property name="suffix" value=".jsp" />
   </bean>

</beans>
```

第3步 我们随便编写一个User的实体类，然后去编写我们的测试Controller

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private int age;
    private String sex;
}
```

第4步编写我们的controller，这里使用我们的jackson

这里我们需要两个新东西，一个是@ResponseBody，一个是ObjectMapper对象，我们看下具体的用法



```java
@Controller
public class UserController {
    @RequestMapping("/json1")
    @ResponseBody
    public String json1() throws JsonProcessingException {
        //创建一个jackson的对象映射器，用来解析程序
        ObjectMapper mapper = new ObjectMapper();
        //创建一个对象
        User user = new User("秦僵2号", 3, "男");
        //将我们的对象解析成为json格式
        String str = mapper.writeValueAsString(user);
        //由于@ResponseBody 注解，这里会将str换成json格式返回，十分方便
        return str;
    }
}
```

第5步 配置Tomcat ， 启动测试一下！

http://localhost:8080/json1

![image-20201227014912536](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201227014912536.png)



### 6.2 解决json乱码的问题



发现出现了乱码问题，我们需要设置一下他的编码格式为utf-8，以及它返回的类型；

解决办法1：通过@RequestMaping的produces属性来实现，修改下代码

```java
//produces:指定响应体返回类型和编码
@RequestMapping(value = "/json1",produces = "application/json;charset=utf-8")
或者
@RequestMapping(path = "/json1",produces = "application/json;charset=utf-8")

```

【注意：使用json记得处理乱码问题】



解决方法2：在springmvc的配置文件解决

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过Spring配置统一指定，这样就不用每次都去处理了！

我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！

```xml
<mvc:annotation-driven>
   <mvc:message-converters register-defaults="true">
       <bean class="org.springframework.http.converter.StringHttpMessageConverter">
           <constructor-arg value="UTF-8"/>
       </bean>
       <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
           <property name="objectMapper">
               <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                   <property name="failOnEmptyBeans" value="false"/>
               </bean>
           </property>
       </bean>
   </mvc:message-converters>
</mvc:annotation-driven>
```





在类上直接使用 **@RestController** ，这样子，里面所有的方法都只会返回 json 字符串了，不用再每一个都添加@ResponseBody ！我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

```java
@RestController
public class UserController {

   //produces:指定响应体返回类型和编码
   @RequestMapping(value = "/json1")
   public String json1() throws JsonProcessingException {
       //创建一个jackson的对象映射器，用来解析数据
       ObjectMapper mapper = new ObjectMapper();
       //创建一个对象
       User user = new User("秦疆1号", 3, "男");
       //将我们的对象解析成为json格式
       String str = mapper.writeValueAsString(user);
       //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
       return str;
  }

}
```

### 6.4 实现List等集合类型的json输出

在 上面测试基础上，增加一个新的方法

```java
@RequestMapping("/json2")
public String json2() throws JsonProcessingException {

   //创建一个jackson的对象映射器，用来解析数据
   ObjectMapper mapper = new ObjectMapper();
   //创建一个对象
   User user1 = new User("秦疆1号", 3, "男");
   User user2 = new User("秦疆2号", 3, "男");
   User user3 = new User("秦疆3号", 3, "男");
   User user4 = new User("秦疆4号", 3, "男");
   List<User> list = new ArrayList<User>();
   list.add(user1);
   list.add(user2);
   list.add(user3);
   list.add(user4);

   //将我们的对象解析成为json格式
   String str = mapper.writeValueAsString(list);
   return str;
}


```

测试的输出结果

![image-20201227015929096](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201227015929096.png)

### 6.5 实现日期的管理

```java
@RequestMapping("/json3")
public String json3() throws JsonProcessingException {

   ObjectMapper mapper = new ObjectMapper();

   //创建时间一个对象，java.util.Date
   Date date = new Date();
   //将我们的对象解析成为json格式
   String str = mapper.writeValueAsString(date);
   return str;
}
```

直接使用日期返回无法解决问题，运行的结果如下

![image-20201227020120289](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201227020120289.png)



解决办法：取消timestamps形式，使用自定义时间格式

```java
@RequestMapping("/json4")
public String json4() throws JsonProcessingException {

   ObjectMapper mapper = new ObjectMapper();

   //不使用时间戳的方式
   mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
   //自定义日期格式对象
   SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
   //指定日期格式
   mapper.setDateFormat(sdf);

   Date date = new Date();
   String str = mapper.writeValueAsString(date);

   return str;
}
```



### 6.6 抽取为工具类

**如果要经常使用的话，这样是比较麻烦的，我们可以将这些代码封装到一个工具类中；我们去编写下**

```java
package com.kuang.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;

public class JsonUtils {
   
   public static String getJson(Object object) {
       return getJson(object,"yyyy-MM-dd HH:mm:ss");
  }

   public static String getJson(Object object,String dateFormat) {
       ObjectMapper mapper = new ObjectMapper();
       //不使用时间差的方式
       mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
       //自定义日期格式对象
       SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
       //指定日期格式
       mapper.setDateFormat(sdf);
       try {
           return mapper.writeValueAsString(object);
      } catch (JsonProcessingException e) {
           e.printStackTrace();
      }
       return null;
  }
}
```

### 6.7 FastJson

fastjson.jar是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象与JavaBean对象的转换，实现JavaBean对象与json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法很多，最后的实现结果都是一样的。

fastjson 的 pom依赖！

```
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>fastjson</artifactId>
   <version>1.2.60</version>
</dependency>
```

fastjson 三个主要的类：

**JSONObject  代表 json 对象** 

- JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的。
- JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用诸如size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的。

**JSONArray  代表 json 对象数组**

- 内部是有List接口中的方法来完成操作的。

**JSON代表 JSONObject和JSONArray的转化**

- JSON类源码分析与使用
- 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化。



**代码测试，我们新建一个FastJsonDemo 类**

```
package com.kuang.controller;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.kuang.pojo.User;

import java.util.ArrayList;
import java.util.List;

public class FastJsonDemo {
   public static void main(String[] args) {
       //创建一个对象
       User user1 = new User("秦疆1号", 3, "男");
       User user2 = new User("秦疆2号", 3, "男");
       User user3 = new User("秦疆3号", 3, "男");
       User user4 = new User("秦疆4号", 3, "男");
       List<User> list = new ArrayList<User>();
       list.add(user1);
       list.add(user2);
       list.add(user3);
       list.add(user4);

       System.out.println("*******Java对象 转 JSON字符串*******");
       String str1 = JSON.toJSONString(list);
       System.out.println("JSON.toJSONString(list)==>"+str1);
       String str2 = JSON.toJSONString(user1);
       System.out.println("JSON.toJSONString(user1)==>"+str2);

       System.out.println("\n****** JSON字符串 转 Java对象*******");
       User jp_user1=JSON.parseObject(str2,User.class);
       System.out.println("JSON.parseObject(str2,User.class)==>"+jp_user1);

       System.out.println("\n****** Java对象 转 JSON对象 ******");
       JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
       System.out.println("(JSONObject) JSON.toJSON(user2)==>"+jsonObject1.getString("name"));

       System.out.println("\n****** JSON对象 转 Java对象 ******");
       User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
       System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>"+to_java_user);
  }
}
```

这种工具类，我们只需要掌握使用就好了，在使用的时候在根据具体的业务去找对应的实现。和以前的commons-io那种工具包一样，拿来用就好了！



Json在我们数据传输中十分重要，一定要学会使用！

## 7 整合SSM

整合SSM，首先对于环境我们需要具备如下的情况



>IDEA   + MySQL 5.7.19 + Tomcat 9 + Maven 3.6

要求： 需要熟练掌握MySQL数据库，spring,javaweb以及MyBatis知识，简单的前端知识!

###  章节1 准备数据库的环境和测试数据

> 第1步，准备数据库及测试数据

```mysql
create database `ssmbuild`;
use `ssmbuild`;
drop table if exists `books`;

create table `books`(
	`bookID` int(10) not null auto_increment comment '书id',
	`bookName` varchar(100) not null comment '书名',
	`bookCounts` int(11) not null comment '数量',
	`detail` varchar(200) not null comment '描述',
	key `bookID`(`bookID`)
)engine=innodb default charset=utf8

insert into `books`(`bookID`,`bookName`,`bookCounts`,`detail`) 
values (1,'Java',1,'从入门到放弃!'),
(2,'Mysql',10,'从删库到跑步!'),
(3,'Linux',5,'从进门到进牢!'),
(4,'Python',1,'从放弃到忘记!');
```

> 第2步 ： 创建一个普通的Maven项目，项目名称为ssmbuild,并导入一个ssm项目的最基础依赖

对应的pom.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>top.aigoo</groupId>
    <artifactId>ssmbuild</artifactId>
    <version>1.0-SNAPSHOT</version>

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
    -->
    <dependencies>
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
    </dependencies>

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

> 第3步 创建项目目录结构  controller,dao,pojo,service , 

至此，我们就准备好了一个空的SSM 项目，使用MAVEN构建，这个项目可以作为后续各个项目的模板

### 章节2 整合mybatis和数据库

> 第1步  在resources 添加 mybaits-config.xml  applicationContext.xml, db.properties，并完善各个配置文件的内容，分别如下

其中 ，mybatis-config.xml的内容

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

database.properties的内容为

```properties
jdbc.driver=com.mysql.jdbc.Driver
# 如果使用的是mysql8.0+,需要增加一个时区的配置
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=false&characterEncoding=utf8&useUnicode=True
jdbc.username=root
jdbc.password=123456
```

对于database.properties 注意，

1.我们使用的是jdbc.driver，而不是之前一直使用的名字，这里名字需要记住

2.如果使用mysql8.0以上版本，url里面还需要加上一个时区的配置



applicationContext.xml的干净内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```

> 第2步 ，配置我们的mybatis，这里我们仅使用setting 和 alias

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置数据源，交由spring去做,也就是前面学习的spring-mybatis整合知识-->
    <typeAliases >
        <package name="com.kuang.pojo"/>
    </typeAliases>
    <mappers>
        <mapper class="com.kuang.dao.BookMapper"/>
    </mappers>
</configuration>
```



> 第3步，准备mybatis的工作环境，我们需要分别针对数据库的books表，创建对应的实例，及对应的BookMapper和BookMapper.xml
>
> 去com.kuang.pojo创建实体类Books, 记住保证字段和数据库字段一模一样

a) 创建和数据库字段一一对应的实体类  Books

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Books {
    private int bookID;
    private String bookName;
    private int bookCounts;
    private String detail;
}
```

b）编写对pojo的接口操作  com.kuang.dao.BookMapper.java

```java
package com.kuang.dao;

import com.kuang.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookMapper {
    //增加一本书
    int addBook(Books books);
    //删除一本书
    int deleteBookById(@Param("bookId") int id);
    //更新一本书
    int updateBook(Books books);
    //根据id查询一本书
    Books queryBookById(@Param("bookId") int id);
    //查询全部的书
    List<Books> queryAllBook();
}
```

c) 编写 对BookMapper的mybatis实现， com.kuang.dao.BookMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.BookMapper">
    <!--增加一本书-->
    <insert id="addBook" parameterType="Books">
      insert into ssmbuild.books(bookName,bookCounts,detail)
      values (#{bookName},#{bookCounts},#{detail});
    </insert>
    <!--删除一本书-->
    <delete id="deleteBookById" parameterType="int">
        delete  from ssmbuild.books where bookID=#{bookId};
    </delete>
    <!--更新一本书-->
    <update id="updateBook" parameterType="Books">
       update ssmbuild.books
       set bookName=#{bookName},bookCounts=#{bookCounts},detail=#{detail}
       where bookID =#{bookID}
    </update>
    <!--根据id查询一本书-->
    <select id="queryBookById" resultType="Books">
        select * from ssmbuild.books where bookID=#{bookId};
    </select>
    <!--查询全部的书-->
    <select id="queryAllBook" resultType="Books">
        select * from ssmbuild.books;
    </select>
</mapper>
```

d) 将编写好的BooksMapper注册到mybatis-config.xml中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	......
    <mappers>
        <mapper class="com.kuang.dao.BookMapper"/>
    </mappers>
    ......
</configuration>
```

e) 接着，我们完善service层， 首先编写 UserService.java接口

```java
package com.kuang.service;

import com.kuang.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookService {
    //增加一本书
    int addBook(Books books);
    //删除一本书
    int deleteBookById(int id);
    //更新一本书
    int updateBook(Books books);
    //根据id查询一本书
    Books queryBookById(int id);
    //查询全部的书
    List<Books> queryAllBook();
}

```

f) 接下来，我们在实现我们的UserService ,com.kuang.service.BookServiceImpl

```java
public class BookServiceImpl implements BookService{
    //service 调用dao层
    private BookMapper bookMapper;

    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    public int deleteBookById(int id) {
        return bookMapper.deleteBookById(id);
    }

    public int updateBook(Books books) {
        return bookMapper.updateBook(books);
    }

    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}
```

上述工作都是为了完成mybatis层，至此，我们就将mybatis的整合全部完成了， 接下来 我们继续spring层的整合

### 章节3 spring-dao整合业务层接口及服务

第1步，我们先建立一个spring-dao.xml的配置文件，作为整合配置文件，这个配置文件至少要定义如下功能

​	a) 关联数据库配置+数据源

​	b)sqlSessionBeanFactory

​	c)整合sqlSession

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
    <!--1.关联数据库配置-->
    <context:property-placeholder location="classpath:database.properties"/>
    <!--2.datasource
        dbcp 半自动化操作，不能自动连接 c3p0 自动化操作，自动化加载配置文件，并且可以自动配置到对象中 druid hikari
    -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <!--c3p0连接池私有属性-->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <!--关闭连接后不自动commit-->
        <property name="autoCommitOnClose" value="false"/>
        <!--获取连接超时时间-->
        <property name="checkoutTimeout" value="10000"/>
        <!--获取连接失败的重试次数-->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>
    <!--3.sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>
    <!--4.新知识: 这里不用sqlSessionTemplate，采用另一种方法 配置DAO接口扫描包 动态实现UserMapper可以注入Spring-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--step 1.动态注入sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--step 2. 扫描dao包-->
        <property name="basePackage" value="com.kuang.dao"/>
    </bean>
</beans>
```



### 章节3.spring-service整合业务层接口及服务

在spring这一层，我们主要是扫描包，将业务注入到spring，声明事务，并执行aop切入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--step 1. 扫描service下的包-->
    <context:component-scan base-package="com.kuang.service"/>
    <!--step 2. 将我们的所有业务类注入到spring 可以通过配置或者注解实现 因为刚讲springmvc 这里用配置实现 -->
    <bean id="BookServiceImpl" class="com.kuang.service.BookServiceImpl">
        <property name="bookMapper" ref="bookMapper"/>
    </bean>
    <!--step 3.声明式事务配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--step 4 aop事务支持-->

</beans>
```



### 章节4.spring-mvc整合controller层接口及服务

step1， 在idea增加web项目的支持

在项目点击右键---Add  FrameWorkSupport ，选择web支持

step2 . 配置我们的web.xml，后续请求有DispatcherServlet接管，并配置过滤器和Session等信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--step 1 .DispathcerServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--整合spring-mvc配置-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <!--和服务器一起启动-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--Step 2 乱码过滤-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!--step 3 配置session过期时间-->
    <session-config>
        <session-timeout>60</session-timeout>
    </session-config>
</web-app>
```

step 3. 完成web.xml的配置，我们需要去调整spring-mvc.xml的配置了

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--1.注解驱动-->
    <mvc:annotation-driven/>
    <!--2.静态资源过滤-->
    <mvc:default-servlet-handler/>
    <!--3.扫描包 扫描controller-->
    <context:component-scan base-package="com.kuang.controller"/>
    <!--4.视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

同时，我们配置视图解析器时，就去web/WEB-INF/目录下面把jsp文件夹创建起来

至此，我们就将mybatis+spring+springMVC整合到了一起!  对应配置文件之间关系如下图

![image-20201228211018216](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201228211018216.png)



### 章节5.测试 实现前台页面 及优化

step1 . 编写一个Controller功能，实现查询图书列表

```java
@Controller
@RequestMapping("/book")
public class BooksController {
    /*
     service层持有Dao层
    *Controller层持有Service层
    * */
    @Autowired
    @Qualifier("BookServiceImpl")
    private BookService bookService;
    /*查询全部书籍并且返回到一个书籍列表页面*/
    @RequestMapping("/allbook")
    public String list(Model model){
        List<Books> books = bookService.queryAllBook();
        model.addAttribute("list", books);
        return "allBook";
    }

}
```

此处需要记住：

​	1 service层持有dao层

​	2、controller层持有service层

Step 2 .在web/WEB-INF/jsp/创建hello.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>allBook</title>
</head>
<body>
    <h1>展示全部书籍列表</h1>
</body>
</html>
```



step3 在测试可以跳转，我们引入bootstrap优化效果

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>allBook</title>
    <%--bootstrap美化界面--%>
    <!-- 新 Bootstrap 核心 CSS 文件 -->
    <link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
    <script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
    <!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
    <script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1><small class="center">书籍列表-显示所有书籍列表</small></h1>
            </div>
        </div>
    </div>

    <div class="row clearfix">
        <div class="col-md-12 column">
            <table class="table table-hover table-striped">
                <thead>
                    <tr>
                        <th>书籍编号</th>
                        <th>书籍名称</th>
                        <th>书籍数量</th>
                        <th>书籍详情</th>
                    </tr>
                </thead>
                <%--书籍从数据库中查询出来，从这个list中遍历出来 foreach--%>
                <tbody>
                    <c:forEach var="book" items="${list}">
                        <tr>
                            <td>${book.bookID}</td>
                            <td>${book.bookName}</td>
                            <td>${book.bookCounts}</td>
                            <td>${book.detail}</td>
                        </tr>
                    </c:forEach>
                </tbody>
            </table>
        </div>
    </div>
</div>
</body>
</html>
```



### 7.2 添加书籍功能



step 1 ，我们需要有一个新增图书的按钮,在allBook.jsp里面，增加一个新增图书按钮

```html
        <%--操作按钮--%>
        <div class="row">
            <div class="col-md-4 column">
                <%--toAddPaper--%>
                <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBookPage">新增书籍</a>
            </div>
        </div>
```

step 2. 编写跳转新增图书页面的功能

```java
public class BooksController {
 	.....
         /*跳转到增加书籍页面*/
    @RequestMapping("/toAddBookPage")
    public String toAddPaper(Model model) {
        return "addBook";
    }
    ....
}
```

step 3. 在/web-inf/jsp/下面新建一个addBook.jsp页面，可以添加图书

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>allBook</title>
    <%--bootstrap美化界面--%>
    <!-- 新 Bootstrap 核心 CSS 文件 -->
    <link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
    <script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
    <!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
    <script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1><small class="center">新增书籍</small></h1>
            </div>
        </div>
    </div>
    <%--书籍表单--%>
    <form class="form-horizontal" action="${pageContext.request.contextPath}/book/addBook" method="post">
        <div class="form-group">
            <label for="inputEmail3" class="col-sm-2 control-label">书籍名称</label>
            <div class="col-sm-10">
                <input type="text" class="form-control" id="inputEmail3" placeholder="书籍名称" name="bookName" required>
            </div>
        </div>

        <div class="form-group">
            <label for="bkcount" class="col-sm-2 control-label">书籍数量</label>
            <div class="col-sm-10">
                <input type="text" class="form-control" id="bkcount" placeholder="书籍数量" name="bookCounts" required>
            </div>
        </div>

        <div class="form-group">
            <label for="bkdetail" class="col-sm-2 control-label">书籍详情</label>
            <div class="col-sm-10">
                <input type="text" class="form-control" id="bkdetail" placeholder="书籍详情" name="detail" required>
            </div>
        </div>

        <div class="form-group">
            <input type="submit" value="添加" class="form-control btn btn-primary">
        </div>
    </form>

</div>
</body>
</html>
```

注意点

1、 name必须和实体类Books属性字段一一匹配，不然后端读取不到

### 7.3 修改删除书籍

step 1. 首先，在书籍列表，增加 修改删除的连接, 我们要实现先能跳进到修改页面

```java
<tbody>
    <c:forEach var="book" items="${list}">
        <tr>
        <td>${book.bookID}</td>
        <td>${book.bookName}</td>
        <td>${book.bookCounts}</td>
        <td>${book.detail}</td>
        <td>
        <a href="${pageContext.request.contextPath}/book/toUpdateBookPage?id=${book.bookID}">修改</a> &nbsp; | &nbsp;
		<a href="#">删除</a>
        </td>
    </tr>
    </c:forEach>
</tbody>
```

step2 . 进入/WEB-INF/jsp/，添加我们的jsp页面  updateBook.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>updateBook</title>
    <%--bootstrap美化界面--%>
    <!-- 新 Bootstrap 核心 CSS 文件 -->
    <link href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
    <script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
    <!-- 最新的 Bootstrap 核心 JavaScript 文件 -->
    <script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>

<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1><small class="center">修改书籍</small></h1>
            </div>
        </div>
    </div>
    <%--书籍表单--%>
    <form class="form-horizontal" action="${pageContext.request.contextPath}/book/updateBook" method="post">
        <%--出现问题：我们提交了修改sql的请求，但是修改失败，初次考虑，是事务问题，配置完毕事务，依旧失败--%>
        <%--看一下sql语句，能否执行成功:sql执行失败，修改未完成--%>
            <%--前端传递隐藏域--%>
        <input type="hidden" name="bookID", value="${books.bookID}">
        <div class="form-group">
            <label for="inputEmail3" class="col-sm-2 control-label">书籍名称</label>
            <div class="col-sm-10">
                <input type="text" class="form-control" id="inputEmail3" placeholder="书籍名称" name="bookName" required value="${books.bookName}">
            </div>
        </div>

        <div class="form-group">
            <label for="bkcount" class="col-sm-2 control-label">书籍数量</label>
            <div class="col-sm-10">
                <input type="text" class="form-control" id="bkcount" placeholder="书籍数量" name="bookCounts" required value="${books.bookCounts}">
            </div>
        </div>

        <div class="form-group">
            <label for="bkdetail" class="col-sm-2 control-label">书籍详情</label>
            <div class="col-sm-10">
                <input type="text" class="form-control" id="bkdetail" placeholder="书籍详情" name="detail" required value="${books.detail}">
            </div>
        </div>

        <div class="form-group">
            <input type="submit" value="修改" class="form-control btn btn-primary">
        </div>
    </form>

</div>

</body>
</html>
```



Step3.先在controller里面，添加一个跳转到修改页面的功能

```java
/*跳转到修改书籍页面*/
@RequestMapping("/toUpdateBookPage")
public String toUpdatePaper(Model model, int id) {
    Books books = bookService.queryBookById(id);
    model.addAttribute("books",books);
    return "updateBook";
}
```

step4 . 在controller，编写处理修改删除书籍的信息



```java
/*修改书籍*/
@RequestMapping("/updateBook")
public String updateBook(Model model, Books book) {
    System.err.println("updateBook=>"+book);

    bookService.updateBook(book);
    return "redirect:/book/allbook";
}
```

在做这个的时候碰到两个错误

1、BookMapper.xml里面配置错误

```xml
    <!--/*修改一本书*/-->
    <update id="updateBook" parameterType="books">
        update ssmbuild.books
        set bookName =#{bookName},bookCounts=#{bookCounts},detail=#{detail}
        where bookID=#{bookID};
    </update>
刚开始   where bookID=#{bookId}; 导致一直报错
```

2.添加事务处理器

```xml
    <!--step 3.声明式事务配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--结合AOP实现事务的织入，配置事务通知-->
    <tx:advice id="txAdvice"  transaction-manager="transactionManager" >
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!--step 4 aop事务支持-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.kuang.dao.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>
```

3. 我们修改页面，前台未传递BookID,导致无法更新

4. 配置mybatis-conf.xml的日志， 不能有空格，刚开始写的有空格，导致启动报错

   ```
   <settings>
       <setting name="logImpl" value="STDOUT_LOGGING"/>
   </settings>
   ```



> 删除图书，我们使用restFule风格	

step1 controller里面

```java
    /*删除数据*/
    /*复习一下restFul风格*/
    //@RequestMapping("/deleteBook")
    @RequestMapping("/deleteBook/{bookId}")
    public String deleteBook(@PathVariable("bookId") int id){
        bookService.deleteBookById(id);
        return "redirect:/book/allbook";
    }
```

step2 在前端调用里面

```jsp
 <a href="${pageContext.request.contextPath}/book/deleteBook/${book.bookID}">删除</a>
```

### 7.4 做一个搜索功能

step 1 , 需要做搜索，我们就先把搜索jsp页面整合进allBook.jsp

```jsp
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1><small class="center">书籍列表-显示所有书籍列表</small></h1>
            </div>
        </div>
        <%--操作按钮--%>
        <div class="row">
            <div class="col-md-4 column">
                <%--toAddPaper--%>
                <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBookPage">新增书籍</a>
            </div>
            <div class="col-md-4 column pull-right">
                <%--查询书籍--%>
                    <form action="${pageContext.request.contextPath}/book/queryBook" method="post" class="form-inline pull-right ">
                        <label class="sr-only" for="inlineFormInputName2">Name</label>
                        <input type="text" class="form-control mb-2 mr-sm-2" id="inlineFormInputName2" name="queryBookName" value="${queryBookName}" placeholder="请输入书籍名称">
                        <button type="submit" class="btn btn-primary mb-2">搜索</button>
                    </form>
            </div>
        </div>
    </div>
```



step 2 , 我们因为之前没有这个功能，所以我们需要从底层开始，先编写我们的BookMapper.java

```java
public interface BookMapper {
    .....
	/*通过书名查询图书*/
    List<Books> queryBookByName(@Param("bookName") String bookName);
    .....
}
```

step 3. 接着我们去设计我们的BookMapper.xml文件

```xml
<!--/*通过书名查询书籍
    复习回忆mybatis里面的 模糊查询，动态sql知识*/-->
<select id="queryBookByName" resultType="books">
    select * from ssmbuild.books
    <where>
        <if test="bookName!=null">
            bookName like "%"#{bookName}"%"
        </if>
    </where>
</select>
```

step 4. 开始完善我们的BookService.java

```java
.....
/*通过书名查询图书*/
List<Books> queryBookByName( String bookName);
.....
```

step 5. 实现我们的BookServiceImpl.java

```java
......
public List<Books> queryBookByName(String bookName) {
    return bookMapper.queryBookByName(bookName);
}
.....
```

step 6. 最后实现我们Controller事件

```java
/*通过名字查询书籍*/
@RequestMapping("/queryBook")
public String queryBook(String queryBookName,Model model){
    List<Books> booksList = bookService.queryBookByName(queryBookName);
    model.addAttribute("list", booksList);
    model.addAttribute("queryBookName",queryBookName);
    return "allBook";
}
```



这个案例，复习了mybatis.xml的 模糊查询和动态sql语句。

## 8 springmvc:Ajax技术

AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。

AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

AJAX 是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下。

Google Suggest

在 2005 年，Google 通过其 Google Suggest 使 AJAX 变得流行起来。

Google Suggest 使用 AJAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。



### 8.1 Ajax请求的5个步骤

1、建立XMLHttpRequest对象；

2、设置回调函数；

3、使用open方法与服务器建立链接；

4、向服务器发送数据；

5、在回调函数中针对不同的响应状态进行处理；

Ajax ：异步无刷新请求, 示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>iframe</title>
    <script>
        function a1(){
            //所有的值，变量，提前获取
            var url = document.getElementById("url").value;
            console.log(url)
            // document.getElementById("iframe1").src="http://www.baidu.com";
            document.getElementById("iframe1").src=url;
        }
    </script>
</head>
<body>
    <div>
        <p>请输入地址:</p>
        <p>
            <input type="text" id="url" value="http://www.baidu.com" >
            <input type="submit" value="提交" onclick="a1()">
        </p>

    </div>
    <div>
        <iframe id="iframe1" src="" frameborder="0" style="width: 100% ;height:400px"></iframe>
    </div>
</body>
</html>
```

利用ajax可以做到：

- 注册时，输入用户名自动检测用户是否已经存在。
- 登陆时，提示用户名密码错误
- 删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除。
- ....等等



> 配置环境要先确认环境可以跑起来，再往下写功能!



### 8.2 Jquery使用

JQuery是一个库，里面是一系列js函数

下载地址：https://code.jquery.com/jquery-3.5.1.js

![image-20201230135733424](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201230135733424.png)

通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。

jQuery 不是生产者，而是大自然搬运工。

jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

```
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```



我们来做个简单的例子测试，使用最原始的HttpServletResponse处理，。最简单，最通用

step1 配置web.xml和springmvc的配置文件，【记得静态资源过滤和注解驱动需要配置上】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

   <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
   <context:component-scan base-package="com.kuang.controller"/>
   <mvc:default-servlet-handler />
   <mvc:annotation-driven />

   <!-- 视图解析器 -->
   <beanclass="org.springframework.web.servlet.view.InternalResourceViewResolver"
         id="internalResourceViewResolver">
       <!-- 前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/" />
       <!-- 后缀 -->
       <property name="suffix" value=".jsp" />
   </bean>

</beans>
```

step2  编写index.jsp, 响应的js代码，要导入jquery，可以使用在线的，也可以下载导入

```jsp
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
<script src="${pageContext.request.contextPath}/statics/js/jquery-3.1.1.min.js"></script>
```





```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>$Title$</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
    <script>
        function test2() {
            $.post({
                url: "${pageContext.request.contextPath}/test2",
                data: {"name": $("#username").val()},
                success: function (data,status) {
                    console.log("data="+data);
                    console.log("status="+status);
                }
            });
        }

    </script>
</head>
<body>
<%--失去焦点的时候，发起一个请求(携带信息)到后台--%>
用户名 : <input type="text" id="username" onblur="test2()">
</body>
</html>
```

step3,后台接口Controller, 编写一个 AjaxTestController

```java
package com.kuang.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@RestController
public class AjaxTestController {
    @RequestMapping("/test1")
    public String test1() {
        return "Hello";
    }

    @RequestMapping("/test2")
    public void test2(String name, HttpServletResponse response) throws IOException {

        System.out.println("test2:param=>" + name);

        if ("kuangshen".equals(name)) {
            response.getWriter().print("true");
        } else {
            response.getWriter().print("false");
        }
    }
}
```

step4 .启动tomcat测试！打开浏览器的控制台，当我们鼠标离开输入框的时候，可以看到发出了一个ajax的请求！是后台返回给我们的结果！测试成功！



ajax原理

![image-20201230152226681](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201230152226681.png)

学习ajax需要 html+css(略懂)+js(精通，超级熟练)

js需要熟练掌握":" 

​	= 函数 闭包

​	= 面向对象

​	= 操作DOM元素(id name tag  create  remove)

​	=操作BOM元素(window时间，document事件)

​	ES6 import require 



### 8.3 模拟一个例子，展示json数据

step1 先完善一个pojo 实体类 User.java

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User implements Serializable {
    private String name;
    private int age;
    private String sex;
}
```

step2 . 完善controller

```java
@RequestMapping("/test3")
public List<User> test3(){

    System.out.println("test3:param=>");
    List<User> userList = new ArrayList<User>();
    //添加数据
    userList.add(new User("狂神说java",1,"男"));
    userList.add(new User("狂神说前团",1,"女"));
    userList.add(new User("狂神说运维",1,"男"));
    userList.add(new User("狂神说测试",1,"男"));
    userList.add(new User("狂神说产品",1,"男"));

    return userList;
}
```

step3 编写我们的前端页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>
    <script>
        $(document).ready(function (){
            /*点击按钮*/
            $("#btn").click(function () {
                console.log("test3.jsp 点击了!")
                /*
                * $.post(url,data(可以省略), function(){});
                * */
                $.post("${pageContext.request.contextPath}/test3",function (data){
                    console.log("data==>"+data);
                    /*动态增加节点*/
                    var html = "";
                    for (let i = 0; i < data.length; i++) {
                        html+="<tr>"+
                            "<td>"+data[i].name+"</td>" +
                            "<td>"+data[i].age+"</td>" +
                            "<td>"+data[i].sex+"</td>" +
                            "</tr>"
                    }
                    $("#content").html(html)
                });


            });
        })

    </script>
</head>
<body>
<input type="button" value="加载数据" id="btn">
<table>
    <thead>
    <tr>
        <td>姓名</td>
        <td>年龄</td>
        <td>性别</td>
    </tr>
    </thead>
    <tbody id="content">
        <%--数据，后台获取--%>

    </tbody>
</table>
</body>
</html>
```



### 8.4 模拟异步用户登录

我们模拟用户登录，当用户输入完信息，失去焦点，就会请求后台验证，如果符合用户名，则返回ok ，不符合，则返回"用户名错误"

step 1. 先设计我们的Controller

```java
    @RequestMapping("/test4")
    public String test4(String name, String pwd){
        String msg="";
        if (name!= null){
            //admin这些数据库应该在数据库中查询
            if ("admin".equals(name)){
                msg = "ok";
            }else {
                msg="用户名有误!";
            }
        }

        if (pwd!= null){
            //admin这些数据库应该在数据库中查询
            if ("123456".equals(pwd)){
                msg = "ok";
            }else {
                msg="密码有误!";
            }
        }
        return msg;
    }
```

step 2. 再去设计我们的jsp站点  login.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.5.1.js"></script>

    <script>
        function test4() {
            $.post({
                url: "${pageContext.request.contextPath}/test4",
                data: {"name": $("#name").val()},
                success: function (data) {
                    //console.log(data)
                    if (data.toString()==='ok'){
                        $("#userInfo").css("color","green");
                    }else {
                        $("#userInfo").css("color","red");
                    }
                    $("#userInfo").html(data);
                }
            })
        }

        function test5() {
            $.post({
                url: "${pageContext.request.contextPath}/test4",
                data: {"pwd": $("#pwd").val()},
                success: function (data) {
                    //console.log(data)
                    if (data.toString()==='ok'){
                        $("#pwdInfo").css("color","green");
                    }else {
                        $("#pwdInfo").css("color","red");
                    }
                    $("#pwdInfo").html(data);
                }
            })
        }
    </script>
</head>
<%--成功就在info里面追加成功，失败追加失败--%>
<body>
<p>
    用户名:<input type="text" id="name" onblur="test4()">
    <span id="userInfo"></span>
</p>
<p>
    密码:<input type="text" id="pwd" onblur="test5()">
    <span id="pwdInfo"></span>
</p>
</body>
</html>
```

这样就实现了我们的需求

知识点：

1、前端向后端传值，前端的传值参数名字，和后端处理事件的方法 形参名字要一一对应，如果名字不对应，会取不到值

2、通过msg 直接return, 如果用户名存在就返回Ok, 如果不存在就返回错误信息

3、在jsp，为了展示错误信息，用span元素去承载，

4、对两个input设计对应的js，用户焦点失去，就会触发js

5、在js中，通过对msg信息判断，来确认前端怎么处理

## 9.springmvc 拦截器

### 9.1 拦截器的一个概念

什么是拦截器**

springmvc的处理器拦截器类似Servlet开发中的过滤器Filter，用于对处理器金星预处理和后处理，开发者可以自己定义一些拦截器来进行特定的功能。

过滤器和拦截器有的区别：

拦截器是和AOP思想的具体应用。

过滤器 Filter：

servlet规范中的一部分，任何java web工程都可以使用

在url-pattern中配置了/* 之后，可以对所有要访问的资源进行过滤拦截



拦截器：

拦截器是springmvc框架自己的，只有使用了springmvc框架的工程才能使用

拦截器只会拦截访问的控制器方法，如果访问的是jsp/html/css/image/js 是不会进行拦截的



**如何使用拦截器**

想要使用自定义拦截器，必须实现HandlerIntercepter接口

1、新建一个Module,springmvc-07-interceptor,添加web支持，

2、配置web.xml和springmvc-servlet.xml

3、编写一个拦截器

```java
public class MyInterceptor implements HandlerInterceptor {
    //return true 会执行下一个拦截器，放行;
    //return false 拦截不放行
    /*在请求处理的方法之前执行*/
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("---------MyInterceptor 处理前----------");
        return true;
    }
    /*在请求处理方法执行之后执行*/
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("---------MyInterceptor 处理后----------");

    }
    /*在dispatcherServlet处理后执行，做清理工作*/
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("---------MyInterceptor 清理----------");
    }
}
```

我们如果实现拦截器，可以实现 HandlerInterceptor， 实现方法 preHandle就可以

4、在spring-mvc.xml配置文件中配置拦截器

```xml
    <!--关于拦截器配置-->
    <mvc:interceptors>
        <!--可以添加多个拦截器-->
        <mvc:interceptor>
            <!--"  /**   包括路径及其子路径
             /admin/*   拦截的是/admin/adsa等等这种， /admin/add/user不会被拦截 
             /admin/**  拦截的是/admin/下的所有-->
            <mvc:mapping path="/**"/>
    
            <!--bean配置的就是拦截器-->
            <bean class="com.kuang.config.TestInterceptor"/>
        </mvc:interceptor>
       <!-- 登录拦截器-->
        <!--测试拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/inter/*"/>
            <bean class="com.kuang.config.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

5. 编写一个controller，接收请求

```java
/*测试拦截器的控制器*/
@Controller
@RequestMapping("/inter")
public class InterceptorController {
    /*@ResponseBody的作用其实是将java对象转为json格式的数据*/
    @RequestMapping("/interceptor")
    @ResponseBody

    public String testFunction(){
        System.out.println("控制器中方法执行了!");
        return "hello";
    }
}
```

6. 前端index.jsp 测试 一下

```
<p><a href="${pageContext.request.contextPath}/inter/interceptor"> 拦截器测试 </a></p>
```

7 启动tomcat,测试一下

![image-20201231150205834](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201231150205834.png)



知识点：

​	1、1个拦截器interceptors 可以配置多个 interceptor 

​	2、在 mapping path /**  这个表示包括这个请求下面的所有请求

​	 如果配置admin下面请求 可以配置   <mvc:mapping path="/admin/**" />

​	3、对应的拦截器实现类，我们放在Bean里面

### 9.2 例子 验证用户是否登录(认证用户)



实现思路： 

1、有一个登陆页面，需要写一个controller访问页面

2、登陆页面有一提交表单的动作。需要在controller中处理。判断用户名密码是否正确。如果正确，向session中写入用户信息。*返回登陆成功。*

3、拦截用户请求，判断用户是否登陆。如果用户已经登陆。放行， 如果用户未登陆，跳转到登陆页面

首先，我们设计Controller，这个里面，实现 tologin()  --进入登录页，  login()---实现登录后进入到首页，main() ---去首页  logout()----释放session后到main页面

然后，我们设计拦截器，对含有session信息、login、toLogin进行放行，其他的跳转到登录页面



Step1 . 编写登录页面 login.jsp  /WEB-INF/jsp/login.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Login</title>
</head>
<body>
    <%--在web-inf下所有的页面或者资源，只能通过controller，或者servlet进行访问--%>
    <h1>登录页面</h1>

    <form action="${pageContext.request.contextPath}/user/login">
        用户名:<input type="text" name="username">
        密码:<input type="text" name="pwd">
        <input type="submit" name="提交">
    </form>

</body>
</html>
```

step2  编写 UserController 处理请求

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpSession;

@Controller
@RequestMapping("/user")
public class UserController {

   //跳转到登陆页面
   @RequestMapping("/jumplogin")
   public String jumpLogin() throws Exception {
       return "login";
  }

   //跳转到成功页面
   @RequestMapping("/jumpSuccess")
   public String jumpSuccess() throws Exception {
       return "success";
  }

   //登陆提交
   @RequestMapping("/login")
   public String login(HttpSession session, String username, String pwd) throws Exception {
       // 向session记录用户身份信息
       System.out.println("接收前端==="+username);
       session.setAttribute("user", username);
       return "success";
  }

   //退出登陆
   @RequestMapping("logout")
   public String logout(HttpSession session) throws Exception {
       // session 过期
       session.invalidate();
       return "login";
  }
}
```

step3   编写一个登录页面 success.jsp

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>success</title>
</head>
<body>
    <h1>登录成功页面</h1>
    <hr>
    <span>${user}</span>
    <p><a href="${pageContext.request.contextPath}/user2/logout">注销</a></p>
</body>
</html>
```

step 4. 编写我们的index.jsp页面，放入登录和成功页面的入口，启动tomcat测试下

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
 <head>
   <title>$Title$</title>
 </head>
 <body>
 <h1>首页</h1>
 <hr>
<%--登录--%>
 <a href="${pageContext.request.contextPath}/user/jumplogin">登录</a>
 <a href="${pageContext.request.contextPath}/user/jumpSuccess">成功页面</a>
 </body>
</html>
```



我们会发现，不管是登录页，还是成功页面，我们都可以进入，这是不符合正常业务逻辑，

按照业务逻辑，对于成功页面，需要用户登录才可访问，未登录用户，应该让用户去登录

step 5. 编写用户登录拦截器

```java
public class LoginInterceptor implements HandlerInterceptor {
    /*判断什么情况下没有登录，
    根据session判断
    登录就放行
    没有登录就不放行并跳转到指定页面
    */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        HttpSession session = request.getSession();
        ///*在登录页面也要放行*/
        //if (request.getRequestURI().contains("login")){
        //    System.out.println("---------LoginInterceptor 处理前:login----------");
        //    return true;
        //}
        System.out.println("username is null?==>"+StringUtils.isEmpty(request.getParameter("username")));
        if (!StringUtils.isEmpty(request.getParameter("username"))){
            return true;
        }

        /*session有用户的登录信息也放行*/
        if (session.getAttribute("user")!=null){
            System.out.println("---------LoginInterceptor 处理前: 有userLoginInfo----------");
            return true;//放行，继续执行下面的
        }
        /*如果没有登录或者不在登录页就转发*/
        System.out.println("---------LoginInterceptor 处理前:被拦截----------");
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
        return false;
    }
}
```





step 6. 在springmvc的配置文件中注册拦截器

```xml
<!--拦截器配置-->
<mvc:interceptors>
    <!--可以添加多个拦截器-->
    <mvc:interceptor>
        <!--"/**"/ 包括这个请求下面的所有请求 /admin/adsa/adsa-->
        <mvc:mapping path="/**"/>
        <bean class="com.kuang.config.MyInterceptor"/>
    </mvc:interceptor>
    <!--登录拦截器-->
    <mvc:interceptor>
        <!--可以只拦截user下面的请求-->
        <mvc:mapping path="/user/**"/>
        <bean class="com.kuang.config.LoginInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

7、再次重启Tomcat测试！登录拦截功能无误，未登录用户，无法进入登录成功页

### 9.3 例子 文件上传和下载

准备工作： 文件上传是项目开发中最常见的功能之一，springmvc可以很好的支持文件上传，但是springmvc上下文默认没有装配MultipartResolver,因此默认情况下，其不能处理文件上传工作，如果想要使用spring的文件上传功能，则需要在上下文中配置MultipartResolever.

前端表单要求：为了能上传文件，必须将表单的method设置为post，并将enctype设置为 multipart/form-data. 只有在这样的情况下，浏览器CIA会把用户选择的文件以二进制数据发送给服务器。

**对表单中的 enctype 属性做个详细的说明：**

- application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单会将表单域中的值处理成 URL 编码方式。
- multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码。
- text/plain：除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件。

```html
<form action="" enctype="multipart/form-data" method="post">
   <input type="file" name="file"/>
   <input type="submit">
</form>
```

一旦设置了enctype为multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对于文件上传的处理则涉及在服务器端解析原始的HTTP响应。在2003年，Apache Software Foundation发布了开源的Commons FileUpload组件，其很快成为Servlet/JSP程序员上传文件的最佳选择。

- Servlet3.0规范已经提供方法来处理文件上传，但这种上传需要在Servlet中完成。
- 而Spring MVC则提供了更简单的封装。
- Spring MVC为文件上传提供了直接的支持，这种支持是用即插即用的MultipartResolver实现的。
- Spring MVC使用Apache Commons FileUpload技术实现了一个MultipartResolver实现类：
- CommonsMultipartResolver。因此，SpringMVC的文件上传还需要依赖Apache Commons FileUpload的组件。

先来实现一个文件上传的功能

1、导入文件上传的jar包，commons-fileupload,maven会自动帮我们导入他的依赖包commons-io包

```xml
<!--文件上传-->
<dependency>
<groupId>commons-fileupload</groupId>
<artifactId>commons-fileupload</artifactId>
<version>1.3.3</version>
</dependency>
<!--servlet-api导入包-->
<dependency>
<groupId>javax.servlet</groupId>
<artifactId>javax.servlet-api</artifactId>
<version>4.0.1</version>
</dependency>
```

2、配置bean:  multipartResolver

[注意!!!这个bean的id必须为: multipartResolver,否则上传文件会报400的错误，在这里栽过坑，教训!]

```xml
<!--文件上传配置-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!--请求的编码格式，必须和jsp的pageEncoding属性一致，以便正确读取表单的内容，默认是ISO-885901-->
    <property name="defaultEncoding" value="utf-8"/>
    <!--文件上传的大小上限，单位为字节(10485760=10M)-->
    <property name="maxUploadSize" value="10485760"/>
    <property name="maxInMemorySize" value="40960"/>
</bean>
```

CommonsMultipartFile的常用方法：

	- String getOriginalFilename()   获取上传文件的原名
	- inputStream getInputStream()  : 获取文件流
	- void transTo(File dest) : 将上传文件保存到一个目录文件中

我们去实际测试一下

3、编写前端页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
    <%--上传文件表单--%>
    <form action="${pageContext.request.contextPath}/upload" enctype="multipart/form-data" method="post">
      <input type="file" name="file"/>
      <input type="submit" value="upload" />
    </form>
  </body>
</html>
```

4、编写Controller

```java
@RestController
public class FileController {
    /*@RequestParam("file")  将前端<input name = file>控件得到的文件封装成CommonsMultipartFile对象  */
    /*如果是批量上传文件，CommonsMultipartFile 为数组即可*/
    @RequestMapping("/upload")
    public String upload(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request, Model model) throws IOException {
        /*1. 获取文件名 */
        String uploadFileName = file.getOriginalFilename();
        /*2. 如果文件名是空，直接回到首页*/
        if ("".equals(uploadFileName)) {
            model.addAttribute("msg", "文件名为空!");
            return "redirect:/index.jsp";
        }
        System.out.println("上传文件名为:" + uploadFileName);
        /*3. 上传路径保存设置*/
        String uploadPath = request.getServletContext().getRealPath("/upload");
        //如果路径不存在就创建一个
        File realPath = new File(uploadPath);
        if (!realPath.exists()){
            realPath.mkdir();
        }
        System.out.println("上传文件保存地址: "+realPath);
        /*获取输入流*/
        InputStream is = file.getInputStream();  //文件输入流 ，从网络读入内存
        OutputStream os = new FileOutputStream(new File(realPath, uploadFileName));  //文件输出流，从内存写入服务器文件

        /*从网络读取并写出到文件*/
        int len =0;
        byte[] buffer = new byte[1024];
        while ((len=is.read(buffer))!=-1){
            os.write(buffer,0,len);
            os.flush();
        }
        os.close();
        is.close();

        model.addAttribute("msg", "文件上传成功!");
        return "redirect:/index.jsp";
    }
}
```

测试上传文件，ok！

此处有个bug，文件名相同无法第二次上传

>在SpringMvc后台进行获取数据，一般有三种：
>1.request.getParameter(“参数名”)
>2.用@RequestParam注解获取
>3.Springmvc默认支持的数据类型接收参数，可直接通过controller方法参数对应jsp中请求参数name直接获取

**采用file.Transto 来保存上传的文件**

1、编写Controller

```java
    public String fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request, Model model) throws IOException {
        /*上传路径保存设置*/
        String path = request.getServletContext().getRealPath("/upload");
        File realPath = new File(path);
        if (!realPath.exists()) {
            realPath.mkdir();
        }
        System.out.println("上传文件保存地址" + realPath);
        
        /*通过CommonsMultipartFile的方法直接写文件(注意这个时候)*/
        file.transferTo(new File(realPath+"/"+file.getOriginalFilename()));
        return "redirect:/index.jsp";
    }
}
```

2.前团表单提交地址修改

3.访问提交测试  ok！

### 9.4 文件下载实现

解决思路：

- 设置respose响应头
- 读取文件-inputStream
- 写出文件 OutputStream
- 执行操作
- 关闭流(先开后关)

```java
@RequestMapping("/download")
public String downloads(HttpServletRequest request, HttpServletResponse response) throws IOException {
    /*准备要下载的图片地址*/
    String path = request.getServletContext().getRealPath("/upload");
    String fileName = "1.png";

    /*1. 设置response响应头*/
    response.reset(); //设置页面不缓存，清空buffer
    response.setCharacterEncoding("utf-8");//字符编码
    response.setContentType("multipart/form-data"); //设置二进制传输数据
    response.setHeader("Content-Disposition","attachment;fileName="+ URLEncoder.encode(fileName,"UTF-8"));

    /*2. 操作文件*/
    File file = new File(path, fileName);
    InputStream fin = new FileInputStream(file);    //读取文件输入流
    OutputStream out = response.getOutputStream();  //从内存写入到网络响应

    byte[] buff = new byte[1024];
    int len =0;
    while ((len=fin.read(buff))!=-1){
        out.write(buff,0,len);
        out.flush();
    }

    out.close();
    fin.close();
    return null;
}
```

前端添加入口

```html
<hr>
<h1>测试下载</h1>
<a href="${pageContext.request.contextPath}/download">点击下载</a>
```

测试，文件下载OK，大家可以和我们之前学习的JavaWeb原生的方式对比一下，就可以知道这个便捷多了!



## 10 spring mvc 参数绑定过程

### 10.1 springmvc参数绑定过程

从客户端(网页)请求key/value数据，经过参数绑定，将 key/value数据绑定到controller方法的形参上!

springmvc中，接收页面提交的数据是通过方法的形参来接收的，而不是在controller类定义成员变更接收!



客户端请求 key/value ----->处理器适配器HandlerAdapter调用spring提供的参数绑定组件将 key/value数据转成 controller方法的  形参---

参数绑定组件：

在springmvc早期版本使用PropertyEditor (只能将字符串传成java对象)

后来使用converter(进行任意类型的转换)

springmvc提供了很多converter(转换器)

在特殊情况下需要自定义converter，对日期数据绑定需要自定义converter

### 10.2 springmvc默认支持的类型

- `HttpServletRequest` 
  通过`request`t对象获取请求信息
- `HttpServletResponse` 
  通过`response`处理响应信息
- `HttpSession` 
  通过`session`对象得到`session`中存放的对象
- `Model/ModelMap` 
  `model`是一个接口，`modelMap`是一个接口实现 。 
  作用：将`model`数据填充到`request`域。



### 10.3 简单类型的参数绑定

1. 可以直接传入绑定，但 request传入的参数名称 和 controller方法的形参名称 必须一致，才可绑定成功
2. 通过@RequestParam对简单类型的参数进行绑定，此时 request传入的参数名称 只用和 @RequestParam("value") 的value相同即可，

```java
@RequestMapping(value="/editItems",method={RequestMethod.POST,RequestMethod.GET})
//@RequestParam中的value属性指定request传入的参数名称
//required属性 : 指定这个参数是否必须要传入
//defaultValue属性: 可以设置默认值，如果id参数没有传入，就会将默认值和形参绑定
public String editItems(Model model,@RequestParam(value="id",required=true,defaultValue="1") Integer items_id) throws Exception {
    //调用sercice查询商品信息
    ItemsCustom itemsCustom = itemsService.findItemsById(items_id);
    //通过参数中的model将数据传到页面
    model.addAttribute("itemsCustom",itemsCustom);
    return "items/editItems";
}
```

### 10.4 pojo类的参数绑定

页面中`input`的`name`属性和·controller·的`pojo`形参中的属性名称一致，将页面中数据绑定到`pojo`

step 1. 在前端的jsp页面， 我们的表单name属性必须和controller的pojo形参属性一直

```jsp
<tr>
    <td>商品名称</td>
    <td><input type="text" name="name" value="${itemsCustom.name}"></td>
</tr>
<tr>
    <td>商品名称</td>
    <td><input type="text" name="price" value="${itemsCustom.price}"></td>
</tr>
<tr>
    <td>商品名称</td>
    <td><input type="text" name="pic" value="${itemsCustom.pic}"></td>
</tr>
```

step 2. 我们的Controller

```java
@Controller
public class ItemController {

    //商品信息修改提交
    public String updateItems(Integer id, ItemsCustom itemsCustom){
        //itemsService.updateItems(id,itemsCustom);
        return "success";
    }
}
```

![image-20201231232221613](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201231232221613.png)



step3 .我们的pojo实体类

![image-20201231232246487](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201231232246487.png)



step4.我们的前端请求效果

![image-20201231232306742](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201231232306742.png)、、



### 10.5 自定义参数绑定实现日期类型绑定

对于`controller`形参中`pojo`对象，如果属性中有日期类型，需要自定义参数绑定。 
将请求日期数据串传成日期类型，要转换的日期类型和`pojo`中日期属性的类型保持一致。 
比如我的`Items`类中的`createtime`属性的类型为`java.util.Date`类型： 

![image-20201231232459340](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20201231232459340.png)、、



需要向处理器适配器中注入自定义的参数绑定组件。 
**自定义日期类型绑件**

```java
package com.jiayifan.ssm.controller.converter;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

import org.springframework.core.convert.converter.Converter;

/**
 * 日期转换器
 * @author 贾一帆
 *
 */
public class CustomDateConverter implements Converter<String, Date> {

    @Override
    public Date convert(String source) {
        //实现将日期串转换为Date类型
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        try {
            //绑定成功，返回日期数据
            return simpleDateFormat.parse(source);
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        //如果参数绑定失败，返回空
        return null;
    }

}
```

**配制方法，在`springmvc.xml`中配置**

```xml
<!-- 自定义参数绑定 -->
<bean id="conversionService"
      class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <!-- 转换器 -->
    <property name="converters">
        <list>
            <!-- 日期类型的转换 -->
            <bean class="com.jiayifan.ssm.controller.converter.CustomDateConverter"/>
        </list>
    </property>
</bean>
```

## 11 springmvc工作中一些经验

对于`controller`形参中`pojo`对象，如果属性中有日期类型，需要自定义参数绑定。 
将请求日期数据串传成日期类型，要转换的日期类型和`pojo`中日期属性的类型保持一致。 
比如我的`Items`类中的`createtime`属性的类型为`java.util.Date`类型： 

### 1. 解决日期前后台问题的方法

> 方法1，直接在前台使用 <fmt: 格式化标签

[jstl中fmt标签详解](https://blog.csdn.net/lydong_/article/details/79812428)

这种方法直接在前台jsp页面，使用fmt标签即可

```jsp
<tbody>
    <c:forEach var="user" items="${userList}">
        <tr>
            <td>${user.id}</td>
            <td>${user.username}</td>
            <td><fmt:formatDate value="${user.birthday}" type="both" pattern="yyyy年MM月dd日" /> </td>
            <td>${user.sex}</td>
            <td>${user.address}</td>
        </tr>
    </c:forEach>
</tbody>
```

对应的pojo实体类

```java
public class Items {
    private int id;
    private String name;
    private float pice;
    private String pic;

    private Date createtime;

    private String detail;
}
```

结果页面：

![image-20210102124234070](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20210102124234070.png)



> 方法2. 使用日期转换类解决

step 1. 编写MyDateConverter

```java
/*从字符串转日期*/
public class MyDateConverter implements Converter<String, Date> {
    public Date convert(String s) {
        /*1999年6月6日， 创建相对应的日期格式化对象*/
        SimpleDateFormat simpleDateFormat = null;
        if (s.contains("年")) {
            simpleDateFormat = new SimpleDateFormat("yyyy年mm月dd日");
        } else if (s.contains(".")) {
            simpleDateFormat = new SimpleDateFormat("yyyy.mm.dd");
        } else if (s.contains("-")) {
            simpleDateFormat = new SimpleDateFormat("yyyy-mm-dd");
        } else if (s.contains("/")) {
            simpleDateFormat = new SimpleDateFormat("yyyy/mm/dd");
        }

        /*解析*/
        try {
            /*把字符串解析成日期对象*/
            Date date = simpleDateFormat.parse(s);
            /*返回结果*/
            return date;
        }catch (ParseException e){
            e.printStackTrace();
        }
        return null;
    }
}
```

step 2. 进入到springmvc-controller.xml里面配置FormattingConversionServiceFactoryBean

```xml
配置点1.
<mvc:annotation-driven conversion-service="conversionService"/>

配置点2、<!--5.配置日期转换-->
<bean id="formattingConversion" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <!--配置自己的转换器-->
            <bean class="com.kuang.utils.MyDateConverter"/>
        </list>
    </property>
</bean>
```



Step 3. 使用@DatetimeFormat和@NumberFormat注解对象属性

```java
public class User {
    private int id;
    private String username;
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date birthday;
    private String sex;
    private String address;
}
```

![image-20210102175454847](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20210102175454847.png)



![image-20210102175506611](F:\MD格式学习笔记库\狂神JAVA-16-SpringMVC学习.assets\image-20210102175506611.png)



