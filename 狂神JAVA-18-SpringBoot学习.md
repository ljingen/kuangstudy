# Spring Boot学习笔记

## 1. 微服务阶段

### 1.1 简介

javase ： oop

mysql  ：持久化

html+css+js+jquery  +框架 ：视图层，框架运用不熟练，所以做得不如前端好，css也用的不好

javaweb :  独立开发MVC三层架构的网站了，这时候是原始的

SSM框架 ： 企业级框架，简化了我们的开发流程，配置也开始越来越复杂了；

------------------------

一直到这个时候，都是war包，在tomcat中运行



Spring 再简化, 这个时候出来了SpringBoot; 此时已经开始打jar包，内嵌了tomcat;  微服务架构!

服务越来越多，又引申出来了 springcloud;



### 1.2 springboot怎么学

springboot是什么(一共6天)

​	配置如何编写yaml

​	自动装配原理：重要谈资

​	继承web开发 业务的核心

​	集成数据库Druid

​	分布式开发 dubbo(RPC)+zookeeper

​	swagger 前后端接口文档

​	任务调度

​	SpringSecurity/shiro安全模块

springCloud（2天半）:

​	微服务

​	springcloud入门

​	restFul风格

​	Eureka 解决方案

​	负载均衡 Nginx

​	Ribbon/Feign

​	HyStrix容灾机制

​	Zuul路由网关

​	SpringCloudConfig  结合gitHub

Linux服务器:

JVM: 回顾JVM

## 2. springboot

### 2.1 什么是spring 

spring是一个开源框架，2003年兴起的时候是一个轻量级的JAVA开发框架，作者是Rod Johnson.

Spring是为了解决企业级应用开发的复杂性而创建的，进而简化开发。

Spring是如何去简化Java开发的

<span style="pading:10px; background:blue;color:#fff">为了降低Java开发的复杂性，Spring采用了一下4中关键策略：</span>

1、基于POJO的轻量级和最小侵入性编程；

2、通过IOC，依赖注入DI和面向接口实现松耦合；

3、基于切面AOP和惯例进行声明式编程

4、通过切面和模板减少样式代码

<span style="pading:10px; background:blue;color:#fff">什么是springBoot</span>

学过javaWeb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat，跑出一个HelloWorld程序，是要经历特别多的步骤；后来就用了框架Struts，再后来是SpringMVC, 到了现在的springBoot，再过一两年又会有其他的web框架出现。不知道你们有没有经历过框架不断的严谨，然后自己

### 2.2 第一个springboot程序

环境： jdk1.8 ； maven 3.6.1  ；springboot:最新版 ； IDEA

官方：提供了一个快速生成的网站  [Spring模板生成网站](https://start.spring.io/)



项目创建方式二：使用IDEA直接创建项目

step 1. 首先选择新建一个项目，在向导选择Srping Initializr，如下图

![image-20210131145025087](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131145025087.png)

step 2. 在下一步和在springboot官网配置一样，如下图

![image-20210131145221810](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131145221810.png)

Step 3.  在下一步选择加载web模板，使用springboot版本为 2.4.2 点击下一步就创建成功了一个项目

![image-20210131145325653](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131145325653.png)

step 4、 进入项目可以发现项目通springmvc项目类似

![image-20210131145601867](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131145601867.png)



<span style="background:blue; color:white">编写一个springboot启动banner</span>

step 1、 先去一个自动生成spring boot banner的在线网站[在线站点](https://www.bootschool.net/ascii-art/search) 生成一个图片的编码

step 2、在resources里面新建一个banner.txt，并把那个代码拷贝进去，重启springboot就可以了



<span style="background:blue; color:white">项目结构分析</span>

通过上面的步骤完成了基础项目的创建，会自动生成的文件，分别如下：

1、 程序主启动类   HellostudyApplication

2、一个application.properties配置文件  

3、一个测试类

4、一个pom.xml文件

<span style="background:blue; color:white">pom.xml的具体内容</span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--父依赖-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.kuang</groupId>
    <artifactId>hellostudy</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>hellostudy</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <!--web场景启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--srpingboot单元测试-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <!--打包插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

<span style="background:blue; color:white">编写一个http接口</span>

1、在主程序的同级目录下，新建一个controller包，一定要在同级目录下，否则识别不到

2、在包中新建一个DemoController类

```java
@RestController
public class DemoController {
    @GetMapping("/hello")
    public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {
        return String.format("Hello %s!", name);
    }
}
```

3、编写完毕后，从主程序启动项目，浏览器发起请求，看页面返回；控制台输出了Tomcat访问的端口号!

![image-20210131153755898](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131153755898.png)

简单几步，就完成了一个web接口的开发，springboot就是这么简单，所以我们常用它来建立我们的微服务项目!

<span style="background:blue; color:white">将项目打包成jar包，点击maven的package</span>

![image-20210131154308009](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131154308009.png)



如果打包成功，则在target目录下生成一个jar包

![image-20210131154347375](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131154347375.png)

打包成jar包后，就可以在任何地方运行了，ok

![image-20210131154435639](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131154435639.png)

<span style="background:blue; color:white">更改springboot的默认banner</span>

如何更改启动时显示的字符拼成的字母，SpringBoot呢？也就是 banner 图案；

只需一步：到项目下的 resources 目录下新建一个banner.txt 即可。

图案可以到：https://www.bootschool.net/ascii 这个网站生成，然后拷贝到文件中即可！



### 2.3 springboot原理初探

<span style="background:blue; color:white">运行原理探究</span>

hellostudy这个项目，到底是怎么运行的呢，Maven项目，我们一般先从pom.xml文件开始

| 父依赖

其中他主要依赖一个父项目，主要是管理项目的资源过滤及插件!

```xml
<!--父依赖-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.2</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

点进去，发现还有一个父依赖，这是核心依赖 spring-boot-dependencies

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>2.4.2</version>
</parent>
```

这里才是真正管理SpringBoot应用里面所有依赖版本的地方，SpringBoot的版本控制中心；

以后我们导入依赖默认是不需要写版本，但是如果导入的包没有再依赖中管理着就需要手动配置版本了;

<span style="background:blue; color:white">2. 启动器spring-boot-starter</span>

```xml
<!--web场景启动器-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

spring-boot-starter-xxxx: 就是spring-boot的场景启动器

spring-boot-starter-web : 帮我们导入了web模块正常运行锁依赖的组件;

springboot把所有的功能场景都抽象取出来，做成一个个starter(启动器)，只需要在项目引入这些startter即可，所有相关的依赖都会导入进来，我们要用什么功能就导入什么样自的场景启动器，我们未来也可以自己定义自己的starter;

在spring-boot-starter-web.pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- This module was also published with a richer model, Gradle metadata,  -->
  <!-- which should be used instead. Do not delete the following line which  -->
  <!-- is to indicate to Gradle or any Gradle module metadata file consumer  -->
  <!-- that they should prefer consuming it instead. -->
  <!-- do_not_remove: published-with-gradle-metadata -->
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <version>2.4.2</version>
  <name>spring-boot-starter-web</name>
  <description>Starter for building web, including RESTful, applications using Spring MVC. Uses Tomcat as the default embedded container</description>
  <url>https://spring.io/projects/spring-boot</url>
  <organization>
    <name>Pivotal Software, Inc.</name>
    <url>https://spring.io</url>
  </organization>
  <licenses>
    <license>
      <name>Apache License, Version 2.0</name>
      <url>https://www.apache.org/licenses/LICENSE-2.0</url>
    </license>
  </licenses>
  <developers>
    <developer>
      <name>Pivotal</name>
      <email>info@pivotal.io</email>
      <organization>Pivotal Software, Inc.</organization>
      <organizationUrl>https://www.spring.io</organizationUrl>
    </developer>
  </developers>
  <scm>
    <connection>scm:git:git://github.com/spring-projects/spring-boot.git</connection>
    <developerConnection>scm:git:ssh://git@github.com/spring-projects/spring-boot.git</developerConnection>
    <url>https://github.com/spring-projects/spring-boot</url>
  </scm>
  <issueManagement>
    <system>GitHub</system>
    <url>https://github.com/spring-projects/spring-boot/issues</url>
  </issueManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.4.2</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.4.2</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.4.2</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.3.3</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.3</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
</project>
```



<span style="background:blue; color:white">3. 主启动类HellostudyApplication</span>

分析完了pom.xml，我们来看看主启动类 HellostudyApplication

```java
//@SpringBootApplication 用来标注一个主程序类
//说明这是一个spring boot应用
@SpringBootApplication
public class HellostudyApplication {

    public static void main(String[] args) {
        //以为是启动一个方法，没想到是启动了一个服务
        SpringApplication.run(HellostudyApplication.class, args);
    }

}
```



这个启动类看似简单但是并不简单，我们先来分析一下这些注解都干了什么!

`1 . @SpringBootApplication`

作用: 标注在某个类上面，说明这个类是springboot的主配置类，springboot就应该运行这个类的main方法来启动springboot应用，进入这个注解，可以看到上面还有很多其他的注解!

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = { @Filter(
        type = FilterType.CUSTOM, 
        classes = TypeExcludeFilter.class),
		@Filter(
            type = FilterType.CUSTOM, 
            classes = AutoConfigurationExcludeFilter.class
        ) }
)
public @interface SpringBootApplication {
	//,.....
}
```

> @ComponentScan

这个注解在spring中很重要，它对应XML配置中的元素，

作用： 自动扫描并加载符合条件的组件或者BEAN，将这个bean定义加载到ioc容器中

> @SpringBootConfiguration

作用: SpringBoot的配置类，标注在某个类上，表示这是一个SrpingBoot的配置类

我们继续进入关注这个注解查看；

```java
    @SpringBootConfiguration
    @EnableAutoConfiguration
    public @interface SpringBootApplication {}
		-------------------------------------
        // 点进去得到下面的@Component
        @Configuration
        public @interface SpringBootConfiguration {}
			-----------------------------
            @Component
            public @interface Configuration {}

```

这里的 @Configuration，说明这是一个配置类 ，配置类就是对应Spring的xml 配置文件；

里面的 @Component 这就说明，启动类本身也是Spring中的一个组件而已，负责启动应用！

我们回到 SpringBootApplication 注解中继续看。

> @EnableAutoConfiguration

**@EnableAutoConfiguration ：开启自动配置功能**

以前我们需要自己配置的东西，而现在SpringBoot可以自动帮我们配置 ；@EnableAutoConfiguration告诉SpringBoot开启自动配置功能，这样自动配置才能生效；

点进注解接续查看：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

**@AutoConfigurationPackage ：自动配置包**

```java
@Import(AutoConfigurationPackages.Registrar.class)
public @interface AutoConfigurationPackage {}
```

**@import** ：Spring底层注解@import ， 给容器中导入一个组件

Registrar.class 作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器 ；

这个分析完了，退到上一步，继续看



@Import(AutoConfigurationImportSelector.class)  给容器导入组件

AutoConfigurationImportSelector：自动配置导入选择器，那么它会导入哪些组件的选择器呢？我们点击去这个类看源码：

1、在AutoConfigurationImportSelector类中有一个这样的方法

```java
// 获取候选的配置
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
    //getSpringFactoriesLoaderFactoryClass()方法返回的就是我们最开始看的启动自动导入配置文件的注解类，EnableAutoConfiguration
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
                                                                         getBeanClassLoader());
    Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
                    + "are using a custom packaging, make sure that file is correct.");
    return configurations;
}
```

2、这个方法又调用SpringFactoriesLoader的静态方法! 我们进入 SpringFactoriesLoader类loadFactoryNames（）方法

```java
    public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
        ClassLoader classLoaderToUse = classLoader;
        if (classLoader == null) {
            classLoaderToUse = SpringFactoriesLoader.class.getClassLoader();
        }
	
        String factoryTypeName = factoryType.getName();
        //这里它又调用了 loadSpringFactories 方法
        return (List)loadSpringFactories(classLoaderToUse).getOrDefault(factoryTypeName, Collections.emptyList());
    }
```



3、我们继续点击查看 loadSpringFactories 方法

 ```java
private static Map<String, List<String>> loadSpringFactories(ClassLoader classLoader) {
    	//获得classLoader ， 我们返回可以看到这里得到的就是EnableAutoConfiguration标注的类本身
        Map<String, List<String>> result = (Map)cache.get(classLoader);
        if (result != null) {
            return result;
        } else {
            HashMap result = new HashMap();

            try {
                  //去获取一个资源 "META-INF/spring.factories"
                Enumeration urls = classLoader.getResources("META-INF/spring.factories");
				  //将读取到的资源遍历，封装成为一个Properties
                while(urls.hasMoreElements()) {
                    URL url = (URL)urls.nextElement();
                    UrlResource resource = new UrlResource(url);
                    Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                    Iterator var6 = properties.entrySet().iterator();

                    while(var6.hasNext()) {
                        Entry<?, ?> entry = (Entry)var6.next();
                        String factoryTypeName = ((String)entry.getKey()).trim();
                        String[] factoryImplementationNames = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                        String[] var10 = factoryImplementationNames;
                        int var11 = factoryImplementationNames.length;

                        for(int var12 = 0; var12 < var11; ++var12) {
                            String factoryImplementationName = var10[var12];
                            ((List)result.computeIfAbsent(factoryTypeName, (key) -> {
                                return new ArrayList();
                            })).add(factoryImplementationName.trim());
                        }
                    }
                }

                result.replaceAll((factoryType, implementations) -> {
                    return (List)implementations.stream().distinct().collect(Collectors.collectingAndThen(Collectors.toList(), Collections::unmodifiableList));
                });
                cache.put(classLoader, result);
                return result;
            } catch (IOException var14) {
                throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var14);
            }
        }
    }
 ```



4、发现一个多次出现的文件：spring.factories，全局搜索它

我们根据源头打开spring.factories ， 看到了很多自动配置的文件；这就是自动配置根源所在！

![image-20210131181501570](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131181501570.png)

**WebMvcAutoConfiguration**

我们在上面的自动配置类随便找一个打开看看，比如 ：WebMvcAutoConfiguration

![image-20210131181609888](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210131181609888.png)

可以看到这些一个个的都是JavaConfig配置类，而且都注入了一些Bean，可以找一些自己认识的类，看着熟悉一下！

所以，自动配置真正实现是从classpath中搜寻所有的META-INF/spring.factories配置文件 ，并将其中对应的 org.springframework.boot.autoconfigure. 包下的配置项，通过反射实例化为对应标注了 @Configuration的JavaConfig形式的IOC容器配置类 ， 然后将这些都汇总成为一个实例并加载到IOC容器中。





结论:

springboot所有的自动配置都是在启动的时候扫描并加载!  spring.factories 所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的start，就有了对应的启动起了额，有了启动器，我们自动装配就会生效然后就配置成功了!

所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的start，就有了对应的启动起了额，有了启动器，我们自动装配就会生效然后就配置成功了!

**@import** ：Spring底层注解@import ， 给容器中导入一个组件

Registrar.class 作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器 ；

这个分析完了，退到上一步，继续看

步骤

step 1、 springboot在启动时，从类路径下/META-INF/ spring.factories 获取指定的值，这些值都是自动配置类

step 2、将这些自动配置类导入容器(WebMvcAutoConfiguration)，自动配置就会生效，帮我们进行自动配置

step 3、 以前我们需要配置的东西，现在springboot帮我们自动做了

step 4、 整合javaEE，解决方案和自动配置的东西都在 spring-boot-autoconfigure-2.4.2.jar这个包下面，它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器；配置文件里面都是全类名，

step 5、 容器中也会存在非常多的xxxxAutoConfiguration的文件(@BEAN)，就是这些类文件给容器中导入了这个场景需要的所有组件，并自动配置

@Configuration  ,是采用JavaConfig的配置方式！！！

Step 6 、 有了自动配置类，免去了我们的手动编写配置文件的工作。

step7、 新增加一个支持能力，整个路径一般为：

xxxxstater     springboot启动器

​	xxxxAutoConfiguration 自动装配类，给容器添加组件

​		xxxxProperties   封装配置文件的相关属性

​				application.properties



![image-20210202201914238](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210202201914238.png)





### 2.4 springboot run

<span style="background:blue; color:white">SpringApplication, 这是一个SrpingBoot自动生成的主启动程序</span>

```java
@SpringBootApplication
public class HellostudyApplication {

    public static void main(String[] args) {
        //以为是启动一个main方法，没想到是启动了一个服务
        // run 方法 该方法返回一个ConfigurableApplicationContext对象
        // 参数一 : 应用入口类， 参数类: 命令行参数
        SpringApplication.run(HellostudyApplication.class, args);
    }

}
```

这个方法开启了一个服务!

1、 SparingApplication 类，是SpringBoot的主启动类，主要做了以下的工作

	- 推断应用的类型是普通的项目还是Web的项目
	- 查找并加载所有可用的初始化器，设置initializers属性中
	- 找出所有的应用程序监听器，设置到listeners属性中
	- 推断并设置main方法的定义类，找到运行的主类

> 我们可以自定义一个类，这个类实现@SpringBootApplication并 执行SpringApplication，就可以作为Springboot的启动类



<li>根据程序路径创建一个适当的ApplicationContext实例</li>

<li>注册一个CommandLinePropertySource，将命令行参数公开为srping的properties属性</li>

<li>刷新应用程序上下文, 加载所有的beans单例</li>

<li>启动任意一个CommandLineRunner beans实例</li>

在大多数场景直接使用静态方法 run(Class,String[]) 方法来加载应用程序

```
@Configuration
@EnableAutoConfiguration
 public class MyApplication  {
 
   // ... Bean definitions
 
    public static void main(String[] args) {
      SpringApplication.run(MyApplication.class, args);
    }
 }
```

如果有一些高级配置，可以在run之前先构造一个SpringApplication实例

```
public static void main(String[] args) {
    SpringApplication application = new SpringApplication(MyApplication.class);
    // ... customize application settings here
    application.run(args)
}
```



我们查看一下springboot这个类的构造器

```java
	public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		// resourceload =null
        this.resourceLoader = resourceLoader;
        
		Assert.notNull(primarySources, "PrimarySources must not be null");
		
        this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
		// 推断应用服务类型REACTIVE、NONE、SERVLET
        this.webApplicationType = WebApplicationType.deduceFromClasspath();
		
        this.bootstrappers = new ArrayList<>(getSpringFactoriesInstances(Bootstrapper.class));
		//
        setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
		//
        setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		// 推断主启动类: com.chinda.ReadSpringApplication
        this.mainApplicationClass = deduceMainApplicationClass();
	}
```



run 方法流程分析：

![image-20210203123205842](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203123205842.png)



跟着源码和这幅图就可以一探究竟了！

### 2.5 yaml配置注入

关于YAML的官方配置地址 [YAML官方地址](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config-yaml)

springboot的application全部配置 [配置官方地址](https://docs.spring.io/spring-boot/docs/2.1.6.RELEASE/reference/htmlsingle/#common-application-properties)

#### 2.5.1 springboot配置文件

- springboot使用一个全局的配置文件，配置文件的名字是固定的叫做application，注意这个名字不能叫其他名，只能叫这个名

- 支持properties 和 yml格式

- 配置文件的作用: 修改springboot自动配置的默认值，因为springboot在底层都给我们自动默认配置好了；

  如果我们想要使用自己额外修改的配置，那么久可以修改配置文件来实现我么的需求

  例如，我们想修改tomcat的端口号，我们可以这么搞：

  ```properties
  server.port=8000
  ```

  这样我们的Tomcat就会在8000端口启动

#### 2.5.2 springboot配置的位置及优先级

springboot会自动的从下面几个位置，发现和加载`application.properties`和`application.yaml`

1、the classpath root  classpath根目录，如idea环境下，resource文件夹下配置

2、The classpath `/config` package ，classpath下的config目录，如idea环境下，resource文件夹下的config文件夹，编译之后就是classpath下的config文件夹下配置

3、The current directory 位于与jar包同级目录下(jar包当前目录)，如在idea环境下，在项目根目录

4、The `/config` subdirectory in the current directory   位于与jar包同级目录下的config文件夹，也就是当前目录下的/config子文件夹下配置

5、Immediate child directories of the `/config` subdirectory



在同一个项目，我们可以配置多个application.properties, 不同的位置优先级不一样, 优先级顺序如下

```shell
加载1： file:./config/ --优先级1。
加载2:  file:./        --优先级2。
加载3： classpath:/config/   ----优先级3.
加载4： classpath: /         ---优先级4
```

#### 2.5.3 yaml概述

YAML是 “YAML Ain't a Markup Language(YAML不是一种标记语言)的递归速写”，在开发这种语言的时候，YAML的意思是仍是一种标记语言

这种语言以数据为中心，而不是以标记语言为重点!

以前的配置文件，大多数都是使用XML来配置，比如一个简单的端口配置，我们来对比下yaml和xml，传统xml配置

```xml
<server>
	<port>8081</port>
</server>
```

传统yaml配置

```yaml
server:
 port: 8080
```

基础语法：

1、空格不能省略

2、以缩进来控制层级之间关系，主要是左边对齐一列数据都是同一层级的

3、属性和值的大小写都是十分敏感的。

**字面量： 普通的值[数字，布尔值，字符串]**

字面量直接写在后面就可以 ， 字符串默认不用加上双引号或者单引号；

注意：

- “ ” 双引号，不会转义字符串里面的特殊字符 ， 特殊字符会作为本身想表示的意思；

  比如 ：name: "kuang \n shen"  输出 ：kuang  换行  shen

- '' 单引号，会转义特殊字符 ， 特殊字符最终会变成和普通字符一样输出

  比如 ：name: ‘kuang \n shen’  输出 ：kuang  \n  shen

**对象、Map（键值对）**

```yml
# 对象、map格式
k2:
  v1: 100
  v2: 3.13

# 1.传统写法
student:
  name: qinjiang
  age: 3
# 2.行内写法
student2: {name: qinjiang2,age: 3}
```



**数组(List、Set)的Yaml表达**

用 - 值 表示数组中的一个元素，比如

```yaml
pets:
  - cat
  - dog
  - pig

# 行内的写法
pets2: [cat,dog,pig]
```



上述就是YAML的基本语法，使用起来非常方便，他的强大之处在于，他可以直接给我们的实体类直接注入匹配值!

#### 2.5.4 通过yaml注入配置文件

1、在springboot项目中的resources目录下新建一个文件 application.yml

2、编写一个实体类 Dog；

```java
@Component //注册bean到容器中
public class Dog {
    private String name;
    private Integer age;
    //有参无参构造、get,set方法,toString()方法
}
```

3、思考，我们原来是如何给bean注入属性值的! @Value, 给狗狗类测试一下

```java
@Component //注册bean到容器中
public class Dog {
    @Value("阿黄")
    private String name;
    @Value("18")
    private Integer age;
    //有参无参构造、get,set方法,toString()方法
}
```

4、在SpringBoot的测试类下注入狗狗输出一下

```java
@SpringBootTest
class HellostudyApplicationTests {
    @Autowired //将狗狗自动注入进来
    Dog dog;

    @Test
    void contextLoads() {
        System.out.println(dog); //打印看下狗狗对象
    }
}
```

结果成功输出，@Value注入成功，这是我们原来的办法对吧,



5、我们再编写一个复杂一点的实体类:Person类

```java
@Component //注册bean到容器中
public class Person {
    private String name;

    private Integer age;

    private Boolean happy;

    private Date birth;

    private Map<String, Object> maps;

    private List<Object> lists;

    private Dog dog;

    //有参无参构造，get\set方法，toString()方法
    
}
```

6、我们来使用yaml配置的方式进行注入，大家写的时候注意区别和优势，我们先编写一个yaml的配置文件! 在resources的application.yaml中

```yaml

person:
  name: qinjiang
  age: 3
  happy: false
  birth: 2000/01/01
  maps: {k1: v1,k2: v2}
  lists:
   - code
   - girl
   - music
  dog:
    name: 旺财
    age: 1
```

7、我们刚才已经把person这个对象的所有值都写好了，我们现在来注入到我们的类中！

 ```java

/*
@ConfigurationProperties作用：
将配置文件中配置的每一个属性的值，映射到这个组件中；
告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
*/
@Component //注册bean
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
 ```

8、IDEA 提示，springboot配置注解处理器没有找到，让我们看文档，我们可以查看文档，找到一个依赖！

![image-20210203172830194](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203172830194.png)

```xml

<!-- 导入配置文件处理器，配置文件进行绑定就会有提示，需要重启 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-configuration-processor</artifactId>
  <optional>true</optional>
</dependency>
```

9、确认以上配置都OK之后，我们去测试类中测试一下：

 ```java
@SpringBootTest
class HellostudyApplicationTests {
    @Autowired //将person自动注入进来
    Person person;

    @Test
    void contextLoads() {
        System.out.println(person); //打印看下person对象
    }
}
 ```

结果：所有值全部注入成功！通过这个例子，演示了一个yaml配置，进行动态注入的方式，

课堂测试：

1、将配置文件的key 值 和 属性的值设置为不一样，则结果输出为null，注入失败

2、在配置一个person2，然后将 @ConfigurationProperties(prefix = "person2") 指向我们的person2；



我们已经学会了三种给对象赋值的方式

1、new 赋值

2、在spring中，使用@Value,  @Autowired方式，进行自动注入

3、 我们今天学习了还可以通过使用yaml的配置，然后进行注入的方式@ConfigurationProperties(prefix = "person")



#### 2.5.5 加载指定的配置文件

**@PropertySource ：**加载指定的配置文件；

**@configurationProperties**：默认从全局配置文件中获取值；

1、我们去在resources目录下新建一个**person.properties**文件

```java
name=kuangshen
```

2、然后在我们的代码中指定加载person.properties文件

```java
@PropertySource(value = "classpath:person.properties")
@Component //注册bean
public class Person {

    @Value("${name}")
    private String name;

    ......  
}
```

3、再次输出测试一下，指定配置文件绑定成功

![image-20210203175000162](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203175000162.png)



#### 2.5.6 配置文件占位符

配置文件还可以编写占位符生成随机数

```java

person:
    name: qinjiang${random.uuid} # 随机uuid
    age: ${random.int}  # 随机int
    happy: false
    birth: 2000/01/01
    maps: {k1: v1,k2: v2}
    lists:
      - code
      - girl
      - music
    dog:
      name: ${person.hello:other}_旺财
      age: 1
```



执行测试后，结果如下

![image-20210203175742508](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203175742508.png)



#### 2.5.7 JSR303校验

使用方式，在自己的实体类，先加上@Validated， 然后在需要校验的字段加上对应的规则

使用的步骤：

step 1、 需要先引入依赖

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
```

step 2、在controller的入参处或者pojo实体类，加上@Validated 注解

```java
@Controller
@RequestMapping("/login")
public class LoginController {
    Logger logger = LoggerFactory.getLogger(LoginController.class);
    @Autowired
    private UserInfoSerivice userInfoSerivice;
    @RequestMapping(value = "/doLogin", method = RequestMethod.POST)
    @ResponseBody
    public Result<LoginUser> login(@Valid @RequestBody LoginUser loginUser){
        logger.info("loginUser:"+loginUser.toString());
        LoginUser result = userInfoSerivice.getByUsername(loginUser.getUsername());
        if (result==null){
            return Result.error(CodeMsg.MOBILE_NOT_EXIST);
        }
        if (MD5Util.formPassToDBPass(loginUser.getPassword(),loginUser.getSalt()).equals(result.getPassword())){
            return Result.success(result);
        }else {
            return Result.error(CodeMsg.PASSWORD_ERROR);
        }
    }
}
```

在person的实体类使用方式

```java
@Component //注册bean到容器中
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {
    @Email(message="需要输入邮箱格式!")
    private String lastName;
    @NotNull(message = "adultTax不能为空")
    private Integer age;

    private Boolean happy;
}
```



JSR303的基础校验规则

```
空检查 
@Null 验证对象是否为null 
@NotNull 验证对象是否不为null, 无法查检长度为0的字符串 
@NotBlank 检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格. 
@NotEmpty 检查约束元素是否为NULL或者是EMPTY.

Booelan检查 
@AssertTrue 验证 Boolean 对象是否为 true 
@AssertFalse 验证 Boolean 对象是否为 false

长度检查 
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内 
@Length(min=, max=) Validates that the annotated string is between min and max included.

日期检查 
@Past 验证 Date 和 Calendar 对象是否在当前时间之前，验证成立的话被注释的元素一定是一个过去的日期 
@Future 验证 Date 和 Calendar 对象是否在当前时间之后 ，验证成立的话被注释的元素一定是一个将来的日期 
@Pattern 验证 String 对象是否符合正则表达式的规则，被注释的元素符合制定的正则表达式，regexp:正则表达式 flags: 指定 Pattern.Flag 的数组，表示正则表达式的相关选项。

数值检查 
建议使用在Stirng,Integer类型，不建议使用在int类型上，因为表单值为“”时无法转换为int，但可以转换为Stirng为”“,Integer为null 
@Min 验证 Number 和 String 对象是否大等于指定的值 
@Max 验证 Number 和 String 对象是否小等于指定的值 
@DecimalMax 被标注的值必须不大于约束中指定的最大值. 这个约束的参数是一个通过BigDecimal定义的最大值的字符串表示.小数存在精度 
@DecimalMin 被标注的值必须不小于约束中指定的最小值. 这个约束的参数是一个通过BigDecimal定义的最小值的字符串表示.小数存在精度 
@Digits 验证 Number 和 String 的构成是否合法 
@Digits(integer=,fraction=) 验证字符串是否是符合指定格式的数字，interger指定整数精度，fraction指定小数精度。 
@Range(min=, max=) 被指定的元素必须在合适的范围内 
@Range(min=10000,max=50000,message=”range.bean.wage”) 
@Valid 递归的对关联对象进行校验, 如果关联对象是个集合或者数组,那么对其中的元素进行递归校验,如果是一个map,则对其中的值部分进行校验.(是否进行递归验证) 
@CreditCardNumber信用卡验证 
@Email 验证是否是邮件地址，如果为null,不进行验证，算通过验证。 
@ScriptAssert(lang= ,script=, alias=) 
@URL(protocol=,host=, port=,regexp=, flags=)
```









#### 2.5.7 回顾properties配置

我们上面采用的yaml方法都是最简单的方式，开发中最常用的；也是springboot所推荐的！那我们来唠唠其他的实现方式，道理都是相同的；写还是那样写；配置文件除了yml还有我们之前常用的properties ， 我们没有讲，我们来唠唠！

【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；

settings-->FileEncodings 中配置；

![image-20210203175947347](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203175947347.png)

测试步骤：

1、新建一个实体类Use

```java
@Component  //注册bean
public class User {
    
    private String name;
    private int age;
    private String sex;
}
```

2、编辑配置文件user.properties

```properties
user1.name=kuangshen
user1.age=18
user1.sex=男
```

3、在我们的User类上面使用@Value来进行注入!

```java
@Component  //注册bean
public class User {
    //直接使用@value
    @Value("${user.name}") //从配置文件中取值
    private String name;
    @Value("#{9*2}") //#{SPEL} spring表达式
    private int age;
    @Value("男")     //字面量
    private String sex;
}
```

结果 也可以正常输入：

![image-20210203180645224](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203180645224.png)

两种格式对比

@Value这个使用起来并不友好，我们需要为每个属性单独注解赋值，比较麻烦，我们看一下两个功能的对比图

![image-20210203181711195](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203181711195.png)

1、@ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加

2、松散绑定：这个什么意思呢? 比如我的yml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下

3、JSR303数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性

4、复杂类型封装，yml中可以封装对象 ， 使用value就不支持

**结论：**

配置yml和配置properties都可以获取到值 ， 强烈推荐 yml；

如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；

如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！



#### 2.5.8 springboot多环境配置

在项目resources下同时创建多个application-*.properties的配置文件，默认都会启动 application.properties，但可以修改配置让springboot启动其他配置

`spring.profiles.active=dev  springboot的多环境配置，可以选择激活哪一个配置文件`

注意： 文件名字必须以application开头

```properties
server.port=8080
#springboot多环境配置
spring.profiles.active=dev
```

对应的文件结构

![image-20210203183241808](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210203183241808.png)



方法二：使用yaml的多文档模块一样实现，实例代码如下

```yaml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8088
spring:
  config:
    activate:
      on-profile: dev
---
server:
  port: 8083
spring:
  config:
    activate:
      on-profile: test
```

说明：

1. --- 之间代表一个代码模块,可以对这个代码模块进行命名，
2. on-profile 为命名下面模块的配置名，这样在第一个模块就可以调用下面模块



####  2.5.9 再看自动配置原理

回顾一下springboot启动时候加载配置项的流程

第一步，先看所有注解

-@SpringBootApplication

 - @SpringBootConfiguration
   	- @Configuration 到此证明这个注解表示被注解的类是一个java配置类，类似spring的<beans></beans>配置文件
 - @EnableAutoConfiguration
    - @AutoConfigurationPackage
       - @Import(AutoConfigurationPackages.Registrar.class)
          -  static class Registrar implements ImportBeanDefinitionRegistrar, DeterminableImports{}
             -  private static final class PackageImports   导出所有的与主类同层的类
    - @Import(AutoConfigurationImportSelector.class)
       - 进入public String[] selectImports(AnnotationMetadata annotationMetadata)方法，获得候选的配置
          - 进入 protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata)方法，获得所有的实体
             - 进入 protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes)方法 获取候选的配置
                - 进入 public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader)方法
                   - 进入private static Map<String, List<String>> loadSpringFactories(ClassLoader classLoader)方法
                     	- 获取 Enumeration urls = classLoader.getResources("META-INF/spring.factories");配置 
	- @ComponentScan

上述的加载了配置，通过spring.factories，我们选择org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration来分析一下这个源码,



step1 、在springboot的spring-boot-autoconfigure的META-INF下，打开spring.factories

```java
//... 没一个xxxxAutoConfiguration就是容器中一个组件
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\ //容器中的一个组件
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration
//...
```

Step2、这些自动配置，和application.yml之间都是一一对应的联系，例如，我们在这些自动装配配置，点击一个比较简单比较熟悉的，例如HttpEncodingAutoConfiguration, 进入

```java
// 表示这是一个配置类
@Configuration(proxyBeanMethods = false)
//相当于把使用 @ConfigurationProperties 的类进行了一次注入,自动配置属性ServerProperties
@EnableConfigurationProperties(ServerProperties.class)
//Spring的底层注解，根据不同的条件，来判断当前配置或者类是否生效
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)
//是否含有CharacterEncodingFilter.class，存在则激活这个自动配置属性
@ConditionalOnClass(CharacterEncodingFilter.class)

@ConditionalOnProperty(prefix = "server.servlet.encoding", value = "enabled", matchIfMissing = true)

public class HttpEncodingAutoConfiguration {

	private final Encoding properties;

	public HttpEncodingAutoConfiguration(ServerProperties properties) {
		this.properties = properties.getServlet().getEncoding();
	}

	@Bean
	@ConditionalOnMissingBean
	public CharacterEncodingFilter characterEncodingFilter() {
		CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
		filter.setEncoding(this.properties.getCharset().name());
		filter.setForceRequestEncoding(this.properties.shouldForce(Encoding.Type.REQUEST));
		filter.setForceResponseEncoding(this.properties.shouldForce(Encoding.Type.RESPONSE));
		return filter;
	}

	@Bean
	public LocaleCharsetMappingsCustomizer localeCharsetMappingsCustomizer() {
		return new LocaleCharsetMappingsCustomizer(this.properties);
	}

	static class LocaleCharsetMappingsCustomizer
			implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>, Ordered {

		private final Encoding properties;

		LocaleCharsetMappingsCustomizer(Encoding properties) {
			this.properties = properties;
		}

		@Override
		public void customize(ConfigurableServletWebServerFactory factory) {
			if (this.properties.getMapping() != null) {
				factory.setLocaleCharsetMappings(this.properties.getMapping());
			}
		}

		@Override
		public int getOrder() {
			return 0;
		}

	}

}
```

说明:

1、@Configuration(proxyBeanMethods = false) 这个说明HttpEncodingAutoConfiguration.java是一个配置类，会被java装配

2、 @EnableConfigurationProperties 相当于把使用 @ConfigurationProperties 的类进行了一次注入

3、@ConditionalOnClass(CharacterEncodingFilter.class)    含有这个类才可以激活

4、@ConditionalOnProperty(prefix = "server.servlet.encoding", value = "enabled", matchIfMissing = true)  含有前缀prefix的配置

这个就是对应的HttpEncodingAutoConfiguration自动配置文件，我们选择点击@EnableConfigurationProperties(ServerProperties.class) 里面的ServerProperties，这是HttpEncodingAutoConfiguration的自动装配属性类

step3 、我们进入到ServerProperties类中

```java
@ConfigurationProperties(prefix = "server", ignoreUnknownFields = true)
public class ServerProperties {
    //////

	/**
	 * Server HTTP port.
	 */
	private Integer port;

	/**
	 * Network address to which the server should bind.
	 */
	private InetAddress address;

	@NestedConfigurationProperty
	private final ErrorProperties error = new ErrorProperties();

	/**
	 * Strategy for handling X-Forwarded-* headers.
	 */
	private ForwardHeadersStrategy forwardHeadersStrategy;

	/**
	 * Value to use for the Server response header (if empty, no header is sent).
	 */
	private String serverHeader;

	/**
	 * Maximum size of the HTTP message header.
	 */
	private DataSize maxHttpHeaderSize = DataSize.ofKilobytes(8);
	//.........
}
```

说明:

1、@ConfigurationProperties(prefix = "server", ignoreUnknownFields = true) 这个表示我们这个属性类的自动属性注入，是通过application.properties里面的以server开头的key来注入

2、这里面的属性，和application.properties里面的注入一一匹配即可

step4、我们进入到application.properties，这里我们可以编写如下的配置

```yaml
server:
  servlet:
    encoding:
      charset: utf-8
      force: on
    application-display-name: 胡汉三
  port: 8989
  ssl:
    ciphers: fjkjk
```

这样，我们就通过在application.propertis里面，完成对ServerProperties的值注入，从而HttpEncodingAutoConfiguration自动装配时候就会加载进入，在Starter启动时给激活；

![image-20210204105525546](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210204105525546.png)

一句话总结：根据当前不同的条件判断，库额定这个配置类是否生效!

一旦这个配置类生效，这个配置类就会给容器中添加各种组件，这些组件的属性是从对应的properties类中获取的，这些类里面的没一个属性又是和配置文件绑定的；

5、所有的配置文件中能配置的属性都是在XXXXProperties类中封装；配置文件能配置什么就可以参照这个功能对应的这个属性类。

#### 2.5.10 自定义一个starter

我们分析完毕了源码以及自动装配的过程，我们就可以尝试着自定义一个启动器来玩玩。

启动器模块是一个空jar文件，仅仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

明明规约：

​	官方命名一般前缀为：

​		spring-boot-starter-xxx

​		例如spring-boot-starter-web.....

自定义命名：

​	xxx-spring-boot-starter

​	比如  mybatis-srping-boot-starter

step1 、 编写启动器

1、我们在IDEA中新建一个空项目 spring-boot-starter-diy

2、新建一个普通的maven模块:  kuang-spring-boot-start

![image-20210205125852104](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205125852104.png)

​	3、新建一个springboot模块，kuang-spring-boot-starter-autoconfigure

![image-20210205130208402](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205130208402.png)

4、点击apply即可，基本结构如下

![image-20210205130836362](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205130836362.png)

5、选择starter的maven项目,进入pom.xml文件，在我们的starter里面导入刚编写的autoconfigure的依赖！

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.kuang</groupId>
    <artifactId>kuang-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--启动器-->
    <dependencies>
        <!--引入自动配置模块-->
        <dependency>
            <groupId>org.kuang</groupId>
            <artifactId>kuang-spring-boot-starter</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

</project>
```

6、将 autoconfigure 项目下多余的文件都删掉，Pom中只留下一个 starter，这是所有的启动器基本配置！

![image-20210205135218752](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205135218752.png)

7、我们编写一个自己的服务







## 3 springboot-web



使用springboot的步骤

1、创建应用，选择模块，

springboot到底帮我们配置了什么，？我们能不能修改？我们能修改哪些东西？我们能不能扩展？

xxxxAutoConfiguratino... 向容器中自动配置组件

xxxxxProperties  ... 自动配置类，装配配置文件中自定义的一些内容!

要解决的问题:

- 导入静态资源，我们应该如何导入呢？...
- 打开项目为404，首页怎么解决?
- jsp网页，那我们怎么写jsp---模板引擎ThyMeleaf?
- 装配扩展SpringMVC，这些还是我们写web的基础?
- 视图解析器时什么样子?
- 怎么实现增删改查?
- 怎么实现拦截器？
- 怎么实现国际化？

### 3.1 导入静态资源

我们先去读框架源代码 ，找源码的过程，

因为WebMVC的项目，所以资源配置可能在WebMvcAutoConfiguration

进入后发现，里面的字段没有关于Resource的描述，但在内部类EnableWebMvcConfiguration中含有addResourceHandlers()方法，从而定位到这个是管理静态资源的方法

```java
//   自动配置类: WebMvcAutoConfiguration>EnableWebMvcConfiguration
//   配置类:WebProperties>Resources
//   application.yaml里面前缀:"spring.web"
    
@Override
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
    super.addResourceHandlers(registry);
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
    }
    ServletContext servletContext = getServletContext();
    //实现方式1的功能
    addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
    // 实现方式2的功能  staticPathPattern = "/**"  staticLocations="classpath:/META-INF/resources/",
	//			"classpath:/resources/", "classpath:/static/", "classpath:/public/"
    
    addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
        registration.addResourceLocations(this.resourceProperties.getStaticLocations());
        if (servletContext != null) {
            registration.addResourceLocations(new ServletContextResource(servletContext, SERVLET_LOCATION));
        }
    });
}
```



<span style="pading:10px; background:red;color:#fff">方式一、webjars，系统实现获取静态资源的功能路径：</span>

- WebMvcAutoConfiguration
  - protected void addResourceHandlers(ResourceHandlerRegistry registry) {}
    - this.mvcProperties.getStaticPathPattern()
      - Class WebMvcProperties{return this.staticPathPattern;}
        - WebMvcProperties 属性：private String staticPathPattern = "/**";

至此，我们知道，如果想实现导入，

step1 .,在pom.xml里面，我们采用maven形式导入jquery

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.5.1</version>
</dependency>
```

step 2. 加载完成后，我们可以到jquery已经加载到对应的依赖包

![image-20210206222911520](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210206222911520.png)

step 3. 在执行上述操作，我们就可以直接访问到这个静态资源，根据源码，访问地址是
http://localhost:8080/webjars/jquery/3.5.1/jquery.js



这样我们会发现我们能够访问到了jquery.js了，也就是说，所有通过webjars导入的包，我们都是可以直接访问的。



<span style="pading:10px; background:red;color:#fff">方式二、支持的位置 /**  /META-INF/resources/  classpath:/resources/  classpath:/static/ classpath:/public/</span>

- WebMvcAutoConfiguration
  - public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer{}
    - WebProperties.class
      - public static class Resources {}
        - 在这个类里面，就有了一个CLASSPATH_RESOURCE_LOCATIONS，在application里面，使用spring.web前缀

```java


private static final String[] CLASSPATH_RESOURCE_LOCATIONS = 
{ "classpath:/META-INF/resources/",
      "classpath:/resources/", "classpath:/static/", "classpath:/public/" };
```

综上，我们可以在/resources/ 新建 public  resources  static,都能被访问

![image-20210204124205860](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210204124205860.png)

**总结：**

1、在springboot中，我们可以使用以下方式处理静态资源  

​		- webjars   `访问方式:localhost:8080/webjars/     比如webjar下的jquery 为 localhost:8080/webjars/jquery/3.4.1/jquery.js`

​		- /public /static /** /resource   `访问方式: localhost:8080 直接访问，比如public下有1.js  直接访问 localhost:8080/1.js`

2、优先级  resources > static（默认的） > public  一般我们习惯静态文件放到static里面，比如图片等等，public放一些公共资源，比如大家都会访问的js，resources一般会放一些上传的文件等等



<span style="pading:10px; background:red;color:#fff">方式三：进入到application.properties，编辑自己的路径</span>

```pro
spring.mvc.static-path-pattern=/hello/,classpath:/kuang/
```

这个时候，默认的路径就会失效，此时的访问路径就变更了，新的访问路径为:(自己研究)



### 3.2 首页如何定制

和刚才找静态资源一样，首页的定制，应该也是在WebMvcAutoConfiguration.java中，展开所有的方法，在内部类EnableWebMvcConfiguration我们能够找到如下的负责欢迎页的源码private Resource getWelcomePage()

```java
private Resource getWelcomePage() {
    for (String location : this.resourceProperties.getStaticLocations()) {
        Resource indexHtml = getIndexHtml(location);
        if (indexHtml != null) {
            return indexHtml;
        }
    }
    ServletContext servletContext = getServletContext();
    if (servletContext != null) {
        return getIndexHtml(new ServletContextResource(servletContext, SERVLET_LOCATION));
    }
    return null;
}

private Resource getIndexHtml(String location) {
    return getIndexHtml(this.resourceLoader.getResource(location));
}

private Resource getIndexHtml(Resource location) {
    try {
        Resource resource = location.createRelative("index.html");
        if (resource.exists() && (resource.getURL() != null)) {
            return resource;
        }
    }
    catch (Exception ex) {
    }
    return null;
}
```



<span style="pading:10px; background:red;color:#fff">知识点1 . index.html页面可以放到public、static、resources下，都可以直接通过localhost:8080访问到</span>

但是放到template目录下，无法被直接访问到，在template下面的所有页面，只能通过controller来跳转

对应的源码运行路径

- spring.factories
  - org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration
    - private Resource getWelcomePage() {}
      - this.resourceProperties.getStaticLocations()
        - public class WebProperties {}
          - private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
      - private Resource getIndexHtml(Resource location) {Resource resource = location.createRelative("index.html");}



在真实项目，不会直接访问index.html，都需要通过controller来进行访问

```java
// 在templates目录下的所有页面，只能通过controller来跳转!
//这个是需要模板引擎的支持
@Controller
public class IndexController {
    @RequestMapping("/index")
    public String index() {
        return "index";
    }
}
```

<span style="pading:10px; background:red;color:#fff">知识点2. 更改首页的图标</span>





### 3.3 Themeleaf模板引擎

前端交给我们的页面，是html页面，如果我们以前开发，我们需要把它们转换为jsp页面，jsp页面的好处就是当我们查出一些数据转发到jsp页面以后，我们可以用jsp轻松实现数据的显示，及交互等，jsp支持非常强大的功能，包括能写java代码，但是呢，我们现在的这种情况,springboot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的tomcat，所以呢，他现在默认是不支持jsp



那不支持jsp，如果我们直接用纯静态页面的方式，那给我们的开发会带来非常大的麻烦，那怎么办呢，springboot推荐我们使用模板引擎

那么这模板引擎，其实大家听到很多，jsp本身就是一个模板引擎，还有易用的比较多的freemarker，包括springboot给我们推荐的Thymeleaf,模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，什么样一个思想呢，我们可以看一下这个图

![image-20210204155358327](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210204155358327.png)



模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。其他的我就不介绍了，我主要来介绍一下SpringBoot给我们推荐的Thymeleaf模板引擎，这模板引擎呢，是一个高级语言的模板引擎，他的这个语法更简单。而且呢，功能更强大。

我们呢，就来看一下这个模板引擎，那既然要看这个模板引擎。首先，我们来看SpringBoot里边怎么用。

> 首先要引入Themeleaf

怎么引入呢，对于springboot来说，什么事情不都是一个start的事情嘛，我们去在项目中引入一下。给大家三个网址：

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：找到我们对应的版本

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter 

找到对应的pom依赖：可以适当点进源码看下本来的包！

```java
<!--thymeleaf-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```



Maven会自动下载jar包，我们可以去看下载的东西;

![image-20210205202218158](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205202218158.png)

> Themeleaf 分析



既然我们已经引入的Themeleaf，那我们要如何使用呢？

我们按照springboot的自动装配原理 (starter-->xxxautoconfiguration--->xxxproperties--->application.preperties)看一下这个Themeleaf的自动配置规则，在按照这个规则，我们进行使用

首先，我们先找到Themeleaf的自动配置类，ThemeleafProperties, 我们可以在IDEA里面双击shift，然后全局搜索ThemeLeafProperties

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;

	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";

	private boolean checkTemplate = true;

	private boolean checkTemplateLocation = true;

	private String prefix = DEFAULT_PREFIX;

	private String suffix = DEFAULT_SUFFIX;

	private String mode = "HTML";

	private Charset encoding = DEFAULT_ENCODING;
}
```

从自动配置类中，可以看到默认的前缀和后缀，根据代码，我们只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮助我们进行渲染了。

从而得出结论，使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可!

<span style="pading:10px; background:red;color:#fff">测试Themeleaf</span>

1、编写一个TestController，跳转到一个test.html页面

```java
@Controller
public class TestController {
    
    @RequestMapping("/t1")
    public String test1(){
        //classpath:/templates/test.html
        return "test";
    }
    
}
```

2、编写一个测试页面  test.html 放在 templates 目录下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>测试页面</h1>
</body>
</html>
```

3、启动项目请求测试，可以发现正确进入到test页面

<span style="pading:10px; background:red;color:#fff">thymeleaf的基础语法学习</span>

要学习语法，还是参考官网文档最为准确，我们找到对应的版本看一下，

Thymeleaf 官网：https://www.thymeleaf.org/ ， 简单看一下官网！我们去下载Thymeleaf的官方文档！

案例1 ， 数据传输

```java

@RequestMapping("/t1")
public String test1(Model model){
    //存入数据
    model.addAttribute("msg","Hello,Thymeleaf");
    //classpath:/templates/test.html
    return "test";
}
```

如果我们要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示，可以去官方文档的#3看一下命名空间拿过来

```xml
xmlns:th="http://www.thymeleaf.org"
```

这样，在前端页面可以用 th:text="{msg}" 获取值

```html

<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>狂神说</title>
</head>
<body>
<h1>测试页面</h1>

<!--th:text就是将div中的内容设置为它指定的值，和之前学习的Vue一样-->
<div th:text="${msg}"></div>
</body>
</html>
```

启动项目，访问http://localhost:8080/test ，就能发现值已经被正确显示在页面

我们可以使用任意的th:attr来替换html中原生属性的值!

![image-20210205204329871](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205204329871.png)

案例2. thymeleaf的循环使用th:each

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>test</title>
</head>
<body>

<div th:text="${msg}"></div>
<!--不转义-->
<div th:utext="${msg}"></div>

<!--遍历数据-->
<!--th:each 每次遍历都会生成当前这个标签:官方#9-->
<h3 th:each="user:${users}" th:text="${user}"></h3>
<h3>
    <!--行内写法: 官网#12-->
    <span th:each="user:${users}">[[${user}]]</span>
</h3>
</body>
</html>
```



我们看完语法，很多样式，我们即使现在学习了，也会忘记，所以我们在学习过程中，需要使用声明，根据官方文档来查询，这才是最重要的，要熟练的使用官方文档。



### 3.4 SpringMVC自动装配

在进行项目编写之前，我们还需要知道一些东西，就是springboot对springmvc还做了哪些配置，包括如果拓展，如何进行定制？

我们只有把这些都搞清楚了，我们在之后使用才会更加得心应手，我们一般研究一个框架，主要是两个途径

​	途径一： 源码分析

​	途径二： 官方文档 [文档](https://docs.spring.io/spring-boot/docs/2.4.2/reference/htmlsingle/#boot-features-developing-web-applications)

```java
Spring MVC Auto-configuration
//SpringBoot为springmvc提供的自动配置能够适用于大多数web应用程序
Spring Boot provides auto-configuration for Spring MVC that works well with most applications.
//自动配置在springboot的默认值之上添加下面的功能
The auto-configuration adds the following features on top of Spring’s defaults:
//包括视图解析器
- inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.
//支持静态资源文件夹路径，以及webjars
Support for serving static resources, including support for WebJars (covered later in this document)).
//自动注册了 转化器Converter和格式器 Formatter
// 转换器，这就是我们网页提交数据到后台自动封装成为对象的东西，比如把"1"字符串自动转换为int类型
// Formatter：【格式化器，比如页面给我们了一个2019-8-10，它会给我们自动格式化为Date对象】    
Automatic registration of Converter, GenericConverter, and Formatter beans.
// HttpMessageConverters
// SpringMVC用来转换Http请求和响应的的，比如我们要把一个User对象转换为JSON字符串，可以去看官网文档解释；    
Support for HttpMessageConverters (covered later in this document).
// 定义错误代码生成规则
Automatic registration of MessageCodesResolver (covered later in this document).
// 首页定制
Static index.html support.
// 初始化数据绑定器：帮我们把请求数据绑定到JavaBean中！
Automatic use of a ConfigurableWebBindingInitializer bean (covered later in this document).    
/*
如果您希望保留Spring Boot MVC功能，并且希望添加其他MVC配置（拦截器、格式化程序、视图控制器和其他功能），则可以添加自己
的@configuration类，类型为webmvcconfiguer，但不添加@EnableWebMvc。如果希望提供
RequestMappingHandlerMapping、RequestMappingHandlerAdapter或ExceptionHandlerExceptionResolver的自定义
实例，则可以声明WebMVCregistrationAdapter实例来提供此类组件。
*/
If you want to keep Spring Boot MVC features and you want to add additional MVC configuration 
(interceptors, formatters, view controllers, and other features), you can add your own 
@Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide 
custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or 
ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

// 如果您想完全控制Spring MVC，可以添加自己的@Configuration，并用@EnableWebMvc进行注释。
If you want to take complete control of Spring MVC, you can add your own @Configuration annotated with @EnableWebMvc.
// 或者添加你自己的@Configuration注释的DelegatingWebMvcConfiguration，，如@EnableWebMvc的Javadoc所述   
or alternatively add your own @Configuration-annotated DelegatingWebMvcConfiguration as described in the Javadoc of @EnableWebMvc.
```



<span style="pading:10px; background:red;color:#fff">ContentNegotiatingViewResolver   内容协商视图解析器</span>

自动配置了ViewResolver，就是我们之前学习的SpringMVC的视图解析器；

即根据方法的返回值取得视图对象（View），然后由视图对象决定如何渲染（转发，重定向）。

我们去看看这里的源码：我们找到 WebMvcAutoConfiguration ， 然后搜索ContentNegotiatingViewResolver。找到如下方法！

```java
@Bean
@ConditionalOnBean(ViewResolver.class)
@ConditionalOnMissingBean(name = "viewResolver", value = ContentNegotiatingViewResolver.class)
public ContentNegotiatingViewResolver viewResolver(BeanFactory beanFactory) {
    ContentNegotiatingViewResolver resolver = new ContentNegotiatingViewResolver();
    resolver.setContentNegotiationManager(beanFactory.getBean(ContentNegotiationManager.class));
    // ContentNegotiatingViewResolver uses all the other view resolvers to locate
    // a view so it should have a high precedence
    resolver.setOrder(Ordered.HIGHEST_PRECEDENCE);
    return resolver;
}
```

我们可以点进这个类看看，找到对应的解析视图的代码；

```java

@Override
@Nullable  //注解说明 @Nullable 既参数可为null
public View resolveViewName(String viewName, Locale locale) throws Exception {
    RequestAttributes attrs = RequestContextHolder.getRequestAttributes();
    Assert.state(attrs instanceof ServletRequestAttributes, "No current ServletRequestAttributes");
    List<MediaType> requestedMediaTypes = getMediaTypes(((ServletRequestAttributes) attrs).getRequest());
    if (requestedMediaTypes != null) {
        // 获取候选的视图对象
        List<View> candidateViews = getCandidateViews(viewName, locale, requestedMediaTypes);
        // 获取一个最适合的视图对象，然后把这个对象返回
        View bestView = getBestView(candidateViews, requestedMediaTypes, attrs);
        if (bestView != null) {
            return bestView;
        }
    }

    String mediaTypeInfo = logger.isDebugEnabled() && requestedMediaTypes != null ?
        " given " + requestedMediaTypes.toString() : "";

    if (this.useNotAcceptableStatusCode) {
        if (logger.isDebugEnabled()) {
            logger.debug("Using 406 NOT_ACCEPTABLE" + mediaTypeInfo);
        }
        return NOT_ACCEPTABLE_VIEW;
    }
    else {
        logger.debug("View remains unresolved" + mediaTypeInfo);
        return null;
    }
}
```

我们继续点进去看，他是怎么获得候选的视图的呢？

getCandidateViews中看到他是把所有的视图解析器拿来，进行while循环，挨个解析！



所以得出结论：**ContentNegotiatingViewResolver 这个视图解析器就是用来组合所有的视图解析器的** 

```JAVA

protected void initServletContext(ServletContext servletContext) {
    // 这里它是从beanFactory工具中获取容器中的所有视图解析器
    // ViewRescolver.class 把所有的视图解析器来组合的
    Collection<ViewResolver> matchingBeans = BeanFactoryUtils.beansOfTypeIncludingAncestors(this.obtainApplicationContext(), ViewResolver.class).values();
    ViewResolver viewResolver;
    if (this.viewResolvers == null) {
        this.viewResolvers = new ArrayList(matchingBeans.size());
    }
    // ...............
}
```

既然它是在容器中去找视图解析器，我们是否可以猜想，我们就可以去实现一个视图解析器了呢？

我们可以自己给容器中去添加一个视图解析器；这个类就会帮我们自动的将它组合进来；**我们去实现一下**

```java
//扩展SPRING MVC
// 如果你想diy一些定制化的功能，只需要写这个组件，然后将它交给springboot就不用管了
//springboot就会帮我们自动装配
@Configuration  //
public class MyMvcConfig implements WebMvcConfigurer {

    //ViewResolver 实现了视图解析器接口的类，我们就可以把它看做视图解析器
    @Bean
    public ViewResolver myViewResolver() {
        return new MyViewResolver();
    }

    //自定义了一个自己的视图解析器MyViewResolver
    public static class MyViewResolver implements ViewResolver {
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }
}
```

2、怎么看我们自己写的视图解析器有没有起作用呢？

我们给 DispatcherServlet 中的 doDispatch方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

![image-20210205230149851](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205230149851.png)

3、我们启动我们的项目，然后随便访问一个页面，看一下Debug信息；

找到this

![image-20210205230253589](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205230253589.png)

找到视图解析器，我们看到我们自己定义的就在这里了；

![image-20210205230328072](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205230328072.png)

所以，我们只需要自己写一个视图解析器，实现ViewResolver接口，然后把它注册到Bean，这个视图解析器就会被springboot自动装载!



<span style="pading:10px; background:red;color:#fff">转换器和格式化器</span>

在WebMvcAutoConfiguration找到格式化转换器 mvcConversionService()：

```java
@Bean
@Override
public FormattingConversionService mvcConversionService() {
    Format format = this.mvcProperties.getFormat();
    WebConversionService conversionService = new WebConversionService(new DateTimeFormatters()
                                                                      .dateFormat(format.getDate()).timeFormat(format.getTime()).dateTimeFormat(format.getDateTime()));
    addFormatters(conversionService);
    return conversionService;
}
```

我们点击getFormat()

```java

public String getDateFormat() {
    return this.dateFormat;
}

/**
* Date format to use. For instance, `dd/MM/yyyy`. 默认的
 */
private String dateFormat;
```

可以看到在我们的Properties文件中，我们可以进行自动配置它！

如果配置了自己的格式化方式，就会注册到Bean中生效，我们可以在配置文件中配置日期格式化的规则：

 ![image-20210205231352643](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205231352643.png)

其余的就不一一举例了，大家可以下去多研究探讨即可！

<span style="pading:10px; background:red;color:#fff">修改springboot的默认配置</span>

这么多的自动配置，原理都是一样的，通过这个WebMVC的自动配置原理分析，我们要学会一种学习方式，通过源码探究，得出结论；这个结论一定是属于自己的，而且一通百通。

SpringBoot的底层，大量用到了这些设计细节思想，所以，没事需要多阅读源码！得出结论；

SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；

如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！

们要做的就是编写一个@Configuration注解类，并且类型要为WebMvcConfigurer，还不能标注@EnableWebMvc注解；我们去自己写一个；我们新建一个包叫config，写一个类MyMvcConfig；

```java
//扩展SPRING MVC
// 如果你想diy一些定制化的功能，只需要写这个组件，然后将它交给springboot就不用管了
//springboot就会帮我们自动装配

//应为类型要求为WebMvcConfigurer，所以我们实现其接口
//可以使用自定义类扩展MVC的功能

@Configuration  //
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 浏览器发送/test ， 就会跳转到test页面；
        registry.addViewController("/test1").setViewName("test");
    }
}
```

我们去浏览器访问一下 

![image-20210205232509183](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210205232509183.png)

**确实也跳转过来了！所以说，我们要扩展SpringMVC，官方就推荐我们这么去使用，既保SpringBoot留所有的自动配置，也能用我们扩展的配置！**

1、WebMvcAutoConfiguration 是 SpringMVC的自动配置类，里面有一个类WebMvcAutoConfigurationAdapter

2、这个类上有一个注解，在做其他自动配置时会导入：@Import(EnableWebMvcConfiguration.class)

3、我们点进EnableWebMvcConfiguration这个类看一下，它继承了一个父类：DelegatingWebMvcConfiguration

这个父类中有这样一段代码：

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
    private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
    
  // 从容器中获取所有的webmvcConfigurer
    @Autowired(required = false)
    public void setConfigurers(List<WebMvcConfigurer> configurers) {
        if (!CollectionUtils.isEmpty(configurers)) {
            this.configurers.addWebMvcConfigurers(configurers);
        }
    }
}
```

4、我们可以在这个类中去寻找一个我们刚才设置的viewController当做参考，发现它调用了一个

```java
protected void addViewControllers(ViewControllerRegistry registry) {
    this.configurers.addViewControllers(registry);
}
```

5、我们点进去看一下

```java
	public void addViewControllers(ViewControllerRegistry registry) {
		for (WebMvcConfigurer delegate : this.delegates) {
             // 将所有的WebMvcConfigurer相关配置来一起调用！包括我们自己配置的和Spring给我们配置的
			delegate.addViewControllers(registry);
		}
	}
```

所以得出结论：所有的WebMvcConfiguration都会被作用，不止Spring自己的配置类，我们自己的配置类当然也会被调用；



<span style="pading:10px; background:red;color:#fff">全面接管springmvc</span>

官网文档

```
If you want to take complete control of Spring MVC
you can add your own @Configuration annotated with @EnableWebMvc.
```

全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

只需在我们的配置类中要加一个@EnableWebMvc。

我们看下如果我们全面接管了SpringMVC了，我们之前SpringBoot给我们配置的静态资源映射一定会无效，我们可以去测试一下；

我们发现所有的SpringMVC自动配置都失效了！回归到了最初的样子；

**当然，我们开发中，不推荐使用全面接管SpringMVC**

思考问题？为什么加了一个注解，自动配置就失效了！我们看下源码：

1、这里发现它是导入了一个类，我们可以继续进去看

```java
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
}
```

2、DelegatingWebMvcConfiguration它继承了一个父类 WebMvcConfigurationSupport

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport{
    //.....
}
```

3、我们来回顾一下Webmvc自动配置类

 ```java

@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
// 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
    ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
    
}
 ```

总结一句话：@EnableWebMvc将WebMvcConfigurationSupport组件导入进来了；

而导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能！

**在SpringBoot中会有非常多的扩展配置，只要看见了这个，我们就应该多留心注意~**



## 4 员工管理系统





### 4.1 首页及其他页面配置

当我们拿到一个模板页面是，我们将页面放到/resources/templates里面， 对应的css,js等文件放到/resources/static里面



对于首页，可以采用两种方式提供用户访问

方式一：将index.html放到templates文件夹，编写一个IndexController，

```java
@Controller
public class IndexController {
    @RequestMapping("/index")
    public String index(){
        return "index";
    }
}
```

这样我们就能实现访问首页的效果，但在实际项目开发中，我们一般不这么用，我们采用下面的方法

方式二：在config包中，定义如下的配置类，通过配置类实现访问首页

```java
@Configuration
public class MyMVCConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }
    //自定义的国际化组件就生效了
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocalResolver();
    }
}
```

通过addViewControllers（）实现用户访问/或者访问index.html，都能够访问templates/index.html



添加完页面，我们就需要对页面里面css及js等相关配置文件进行修改

1、引入thymeleaf命名空间   

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

2、对于页面中的css、js、img等相关的引入，我们需要修改为thymeleaf语法支持的格式

```html
原来：    <link href="asserts/css/bootstrap.min.css" rel="stylesheet">
th格式:  <link th:href="@{/css/bootstrap.min.css}" rel="stylesheet">

原来:  <img class="mb-4" src="assert/img/bootstrap-solid.svg" alt="" width="72" height="72">
th格式: <img class="mb-4" th:src="@{/img/bootstrap-solid.svg}" alt="" width="72" height="72">

国际化格式支持:
原来:<input name="username" type="text" class="form-control" placeholder="Username" required="" autofocus="">
th格式:<input name="username" type="text" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
或者格式:<input type="checkbox" value="remember-me"> [[#{login.remember}]]
```

th格式链接： @{} 

th格式国际化   #{}

th格式取值   ${}

### 4.2 页面国际化

step 1、进入idea的setting，搜索File Encoding, 首先确保代码为UTF8，如果这个不保证，接下来页面可能全是乱码

![image-20210207210852532](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210207210852532.png)



step 2、在resources里面，新建一个目录，命名为i18n

step 3、在目录下新建一个login.properties, 再建立一个login_zh_CN.properties，此时我们就形成了一个中文的国际化环境

![image-20210207211243750](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210207211243750.png)

step 4、我们可以点击Resource Bundle 'login' 然后点击右键，就可以添加一个新的语言包，如下图

![image-20210207211440154](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210207211440154.png)

step 5、 接下来我们可以点击login.properties， 点击添加，输入一个login.tip，此时右侧就会有三个输入框

![image-20210207212036838](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210207212036838.png)

step 6、 在MessageSourceAutoConfiguration，这个类是负责配置国家化的类，我们可以进入他的配置属性类MessageSourceProperties查看

step 7、 因此我们进入到application.properties中,添加我们的国际化basename

```properties
# 关闭模板的缓存
spring.thymeleaf.cache=false
# 配置国际化
spring.messages.basename=i18n.login
```

step 8、在thymeleaf中，表示国家化的表达式使用 #{}，我们进入到index.html，就可以使用国际化了，如下

```html
<form class="form-signin" action="dashboard.html">
    <img class="mb-4" th:src="@{/img/bootstrap-solid.svg}" alt="" width="72" height="72">
    <h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
    <label class="sr-only" th:text="#{login.username}">Username</label>
    <input type="text" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
    <label class="sr-only" th:text="#{login.password}">Password</label>
    <input type="password" class="form-control" th:placeholder="#{login.password}" required="">
    <div class="checkbox mb-3">
        <label>
            <input type="checkbox" value="remember-me"> [[#{login.remember}]]
        </label>
    </div>
    <button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
    <p class="mt-5 mb-3 text-muted">© 2017-2018</p>
    <a class="btn btn-sm">中文</a>
    <a class="btn btn-sm">English</a>
</form>
```

如上，th:text="#{login.tip}" 就会系统自动提示了

最终我们进入页面访问，可以看到如下效果

![image-20210207213446841](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210207213446841.png)



step 9、分析，我们知道AcceptHeaderLocaleResolver 是实现了LocaleResolver 从而实现国际化，所以我们可以自己编写一个自己的LocaleResolver，实例代码如下

```java
public class MyLocalResolver implements LocaleResolver {
    // 解析请求
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        //获取请求中的语言参数
        String language = request.getParameter("l");
        Locale locale = Locale.getDefault(); //获取默认的，如果没有就使用默认的

        //如果请求的连接携带了国家化参数
        if (!(language == null || "".equals(language))){
            //zh_CN
            String[] split = language.split("_");
            // 国家 地区
            locale = new Locale(split[0], split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {

    }
}
```

step 10、添加完成后，我们还需要注册一下我们的国际化，进入MyMVCConfig.java

```java
@Configuration
public class MyMVCConfig implements WebMvcConfigurer {
	//...
    //自定义的国际化组件
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocalResolver();
    }
    //...
}
```

step 11、进入到html页面，编辑我们的国际化入口，选择每次带入参数

```html
<form>
    <a class="btn btn-sm" th:href="@{/index.html(l=zh_CN)}">中文</a>
    <a class="btn btn-sm" th:href="@{/index.html(l=en_US)}">English</a>
</form>			
```

进入到页面, 我们点击切换就实现了国际化能力

注意：

1、我们需要配置i18n文件

2、我们如果需要在项目中进行按钮自动切换，我们需要自定义一个国际化组件 `LocalResolver`

3、记得将我们自己写的组件配置到spring容器 @Bean

4、在thymeleaf模板，使用的是  #{} 进行取值



 ### 4.3 用户登录

首先，我们需要在index.html页面，将from的action进行修改

```html
<form class="form-signin" th:action="@{/user/login}">
```

然后，编写登录控制器LoginController

```java
@Controller
public class LoginController {
    @RequestMapping("/user/login")
    public String login(@RequestParam("username") String username,
                        @RequestParam("password")String password,
                        Model model) {
        //具体的业务： 判断用户名密码正确
        if (username!=null && "123456".equals(password)){
            return "dashboard";
        }else {
            //告诉用户登录你失败了
            model.addAttribute("msg","用户名或者密码错误!");
            return "index";
        }
    }
}
```

最后，我们回到前端，让前端显示我们回传的值

```html
...
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg) }" ></p>
...
```

上面的代码意思，只有当msg值不为空，此时才会显示 p标签，其中

#strings.isEmpty(msg) 为thymeleaf里面的 strings函数用法，表示判断字符串是否为空

not #strings.isEmpty(msg)  的not为 ! 单目运算

th:if="${not #strings.isEmpty(msg) }" 为thymeleaf的if判断语句格式

<span style="pading:10px; background:red;color:#fff">屏蔽首页访问显示用户名密码的问题</span>

进入到MyMVCConfig, 在addViewControllers里面，添加对main.html的视图解析

```java
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController("/").setViewName("index");
    registry.addViewController("/index.html").setViewName("index");
    registry.addViewController("/main.html").setViewName("dashboard");
}
```

然后，进入到LoginController里面，我们让登录后用户重定向到main.html

```java
....
//具体的业务： 判断用户名密码正确
if (username!=null && "123456".equals(password)){
    return "redirect:/main.html";
}else {
    //告诉用户登录你失败了
    model.addAttribute("msg","用户名或者密码错误!");
    return "index";
}
....
```

这样我们就实现了用户登录的功能

### 4.4 拦截器



如果想拦截需求，那么就需要实现HandlerInterceptor，如果我们想要用户进入dashboard页面，拦截非登录用户，可以通过设计自定义的拦截器LoginHandlerInterceptor来实现

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //登录成功之后，应该有用户的session信息
        Object loginUser = request.getSession().getAttribute("loginUser");
        if (loginUser == null){
            //说明没有登录
            request.setAttribute("msg","没有权限，请先登录!");
            request.getRequestDispatcher("/index.html").forward(request, response);
            return false;
        }else {
            return true;
        }

    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}

```

改造LoginController，当用户登录成功，向Session中存入一条loginUser的数据

```java
@Controller
public class LoginController {
    @RequestMapping("/user/login")
    public String login(@RequestParam("username") String username
            , @RequestParam("password")String password, Model model
            , HttpSession session) {
        //具体的业务： 判断用户名密码正确
        if (username!=null && "123456".equals(password)){
            //登录后向session存入一条内容
            session.setAttribute("loginUser",username);
            return "redirect:/main.html";
        }else {
            //告诉用户登录你失败了
            model.addAttribute("msg","用户名或者密码错误!");
            return "index";
        }
    }
}
```

然后，进入到MyMVCConfig，注册我们的拦截器

```java
    //添加拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**")
                .excludePathPatterns("/index.html","/","/user/login");
    }
```

说明:  addInterceptor(new LoginHandlerInterceptor())   添加自定义的拦截器

.addPathPatterns("/**")   拦截哪些路径

.excludePathPatterns("/index.html","/","/user/login")  排除哪些路径？



我们访问页面，会发现页面的样式都丢失了，这是因为拦截器对静态文件并未放行，所以我们需要优化一下,把静态文件的路径添加到排除路径中

```java
    //添加拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**")
                .excludePathPatterns("/index.html","/","/user/login"
                        ,"/css/*","/js/**","/img/**");
    }
```

最后，我们再到前端页面，让我们登陆的用户名显示在前端页面, 进入dashboard.html，找到显示用户名的地方	

```html
<a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">[[${session.loginUser}]]</a>

```

采用thymeleaf的语法，我们使用[[${session.loginUser}]] 就可以取出来登陆的用户名了。

### 4.5 展示员工列表

<span style="pading:10px; background:red;color:#fff">1. 实现跳转到员工列表页面</span>

首先，编写EmployeeController控制器， 设计访问路径为/emp/all， 进入员工列表

```java
@Controller
public class EmployeeController {
    @Autowired
    EmployeeDao employeeDao;
    @RequestMapping("/emp/all")
    public String list(Model model){
        Collection<Employee> employees = employeeDao.getAll();
        model.addAttribute("employees", employees);
        return "employee/list";
    }
}
```

然后, 进入到dashboard.html页面，找到员工列表入口，修改连接到用户列表入口

```html
<li class="nav-item">
    <a class="nav-link" th:href="@{/emp/all}">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-users">
            <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
            <circle cx="9" cy="7" r="4"></circle>
            <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
            <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
        </svg>
        员工管理
    </a>
</li>
```

在 href使用 th:href ="@{/emp/all}" ，用户点击后跳入到员工列表，

然后，在templates里面，新建一个employee文件夹，将list.html文件放到employee文件夹，启动服务器就可以看到可以正确访问到员工列表





<span style="pading:10px; background:red;color:#fff"> 2. 提取公共页面</span>

在项目中，我们很少去每个页面的编辑，我们经常使用的是页面模板技术，此处我们可以使用Thymeleaf的模板功能，通过观察可知，我们可以将dashboard.html整体看作3个模板

1、`th:fragment="sidebar"`

2、`th:replace="~{commons/commons::topbar}"`

3、如果要传递参数，可以使用 （）传参，接收判断即可!

step 1. 我们在templates里面，新建一个commos文件夹，然后创建一个commons.html文件，里面只保留最简单的样式

```html
<!DOCTYPE html>
<html lang="en">

</html>
```

step 2. 从dashboard.html 里面我们使用th:fragment ，把这部分代码抽到commons.html中

```html
<!--导航-->
<nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="nav">
    <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">[[${session.loginUser}]]</a>
    <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
    <ul class="navbar-nav px-3">
        <li class="nav-item text-nowrap">
            <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">注销</a>
        </li>
    </ul>
</nav>
```

step 3、到dashboard.html里面，找到导航的部分，将导航的原来代码删除，并输入如下代码

```html
<!--导航-->
<div th:insert="~{commons/commons::nav}"></div>
```

这样就实现了模板导入，后续我们可以将这个导航栏同步到List.html及其他存在这个模板的页面

说明:

1、导入模板使用的thymeleaf的语法格式为: ~{}  

2、commons.html所在的地址为templates/commons/commons.html , 所以使用的结构为  ~{commons/commons::nav} ，其中第一个commons代表 commos文件夹，第二个commos代表commos.html文件，::nav 代表使用的是这个html模板中的 nav这个页面片段

​	

<span style="pading:10px; background:red;color:#fff">3. 公共页面传参</span>

当用户点击用户菜单，用户菜单并没有激活，此时仍然是首页为激活状态，这是不符合需求，为了解决这个问题，可以用户在点击用户或者首页菜单，能够有一个信息回传给组件，这样就可以激活对应的active

首先，到List.html里面，在使用页面片段的地方，传递active=list.html的参数，如下:

```html
<!--侧边栏-->
<div th:insert="~{commons/commons::sidebanner(active='list.html')}"></div>
```

然后，在页面片段中，找到sidebanner处，进行判断

```html
 <a th:class="${active == 'list.html'? 'nav-link active': 'nav-link'}" th:href="@{/emp/all}">
```

其他首页和相关菜单，也按照这样方式处理，

```
<div th:insert="~{commons/commons::sidebanner(active='main.html')}"></div>

// commons.html
<a th:class="${active=='main.html'?'nav-link active':'nav-link'}" th:href="@{/main.html}">
```

这样，就实现了传递参数的效果，从而实现用户点击不同菜单激活不同按钮功能

<span style="pading:10px; background:red;color:#fff">4.对列表的循环</span>

我们已经有了列表，那如何把数据显示在list里面? 我们可以使用 th：each

```html
<table class="table table-striped table-sm">
    <thead>
        <tr>
            <th>id</th>
            <th>lastName</th>
            <th>Email</th>
            <th>gender</th>
            <th>department</th>
            <th>birth</th>
        </tr>
    </thead>
    <tbody>
        <tr th:each="emp:${employees}">
            <td >[[ ${emp.getId()} ]]</td>
            <td th:text="${emp.getLastName()}"></td>
            <td th:text="${emp.getEmail()}"></td>
            <td th:text="${emp.getGender()}"></td>
            <td th:text="${emp.getDepartment().getDepartmentName()}"></td>
            <td th:text="${emp.getBirth()}"></td>
        </tr>
    </tbody>
</table>
```

但生成的表格，数据格式，如性别，及日期的格式显示错误，我们可以如下显示

对于gender，<td th:text="${emp.getGender()==0?'女':'男'}"></td>

对于date格式:    <td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>

### 4.6 添加员工

基本功能需求

1、列表页面点击 添加按钮

2、进入到添加页面

3、添加员工成功

4、返回到列表页面

需求1、在列表页面，需要有一个添加按钮，我们在前端页面修改下

```html
<h2><a class="btn btn-sm btn-success" th:href="@{/emp}">添加员工</a></h2>
```

当用户点击这个连接，就进入到/emp 控制器

```java
@GetMapping("/emp")
public String toAddpage(Model model){
    //查出所有部门的信息
    Collection<Department> departments = departmentDao.getDepartments();
    model.addAttribute("departments",departments);
    return "employee/add";
}
```

根据这个连接，我们增加一个add.html的页面

```html
<form action="">
    <div class="form-group">
        <label>LastName</label>
        <input type="text" name="lastName" class="form-control" placeholder="kuangshen">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input type="email" name="Email" class="form-control" placeholder="24736743@qq.com">
    </div>
    <div class="form-group">
        <label>Gender</label>
        <div class="form-check form-check-inline">
            <input type="radio" class="form-check-inline" name="gender" value="1">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input type="radio" class="form-check-inline" name="gender" value="0">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <select class="form-control" name="department">
            <option th:each="department:${departments}" th:text="${department.getDepartmentName()}"></option>
        </select>
    </div>
    <div class="form-group">
        <label>BirthDay</label>
        <input type="text" name="birth" class="form-control" placeholder="kuangshenstudy">
    </div>
    <button type="submit" class="btn btn-primary">添加</button>
</form>
```

说明:

​	1、对于部门这个select标签，我们使用 <option th:each="department:${departments}" th:text="${department.getDepartmentName()}"></option>

因为select提交的是部门id，所以还要增加一个value

```html
<select class="form-control" name="department">
    <option th:each="department:${departments}"
            th:text="${department.getDepartmentName()}"
            th:value="${department.getId()}"></option>
</select>
```

2、最后把form表单的action加上

```html
<form th:action="@{/emp}" method="post">
```



完成上述，我们进入到EmployeeController，增加一个/emp的postmapping

```java
@PostMapping("/emp")
public String addEmp(Employee employee){
    System.out.println("add==>"+employee);
    //添加的操作
    employeeDao.add(employee); //调用底层业务的方法，保存员工的信息
    System.out.println("==>我进来了");
    //重定向到列表页面  还有forward
    return "redirect:/emp/all";
}
```



在提交时，如果 2019/1/1可以支持，但2019-1-1不支持，  这里我们还需要设置一下日期格式

```properties
# 日期格式
spring.mvc.format.date=dd-MM-yyyy
```

另外，Department 是一个特殊属性，所以需要上传指定信息

```html
<select class="form-control" name="department.id">
    <option th:each="department:${departments}"
            th:text="${department.getDepartmentName()}"
            th:value="${department.getId()}"></option>
</select>
```

### 4.7 修改员工

stpe 1、在列表页面，修改员工的连接入口， 此处连接需要带入要修改的员工Id

```html
<td>
    <!--<button class="btn btn-sm btn-primary" th:href="@{/emp(id=${emp.getId()})}">编辑</button>-->
    <a class="btn btn-sm btn-primary" th:href="@{/emp/}+${emp.getId()}">编辑</a>
    <a class="btn btn-sm btn-danger">删除</a>
</td>
```

step 2、因为采用RestFul风格，在EmployeeController里面，我们使用@PathVariable

```java
//去员工的修改页面
@GetMapping("/emp/{id}")
public String toUpdateEmp(@PathVariable("id") Integer id, Model model){
    System.out.println("update==> toUpdateEmp");
    //查出原来的数据
    Employee employee = employeeDao.getEmployeeById(id);
    model.addAttribute("employee",employee);
    //查询部门信息
    Collection<Department> departments = departmentDao.getDepartments();
    model.addAttribute("departments",departments);

    return "employee/update";
}
```

step 3. 在templates/employee/update.html里面，根据回传数据，完善页面

```html
<form th:action="@{/updateEmp}" method="post">
    <input type="hidden" name="id" th:value="${employee.getId()}">
    <div class="form-group">
        <label>LastName</label>
        <input th:value="${employee.getLastName()}" type="text" name="lastName" class="form-control" placeholder="kuangshen">
    </div>
    <div class="form-group">
        <label>Email</label>
        <input th:value="${employee.getEmail()}" type="email" name="Email" class="form-control" placeholder="24736743@qq.com">
    </div>
    <div class="form-group">
        <label>Gender</label>
        <div class="form-check form-check-inline">
            <input th:checked="${employee.getGender()==1}" type="radio" class="form-check-inline" name="gender" value="1">
            <label class="form-check-label">男</label>
        </div>
        <div class="form-check form-check-inline">
            <input th:checked="${employee.getGender()==0}" type="radio" class="form-check-inline" name="gender" value="0">
            <label class="form-check-label">女</label>
        </div>
    </div>
    <div class="form-group">
        <label>department</label>
        <select class="form-control" name="department.id">
            <option th:selected="${department.getId()==employee.getDepartment().getId()}"
                    th:each="department:${departments}"
                    th:text="${department.getDepartmentName()}"
                    th:value="${department.getId()}"></option>
        </select>
    </div>
    <div class="form-group">
        <label>BirthDay</label>
        <input th:value="${#dates.format(employee.getBirth(),'yyyy-MM-dd HH:mm')}" type="text" name="birth" class="form-control" placeholder="kuangshenstudy">
    </div>
    <button type="submit" class="btn btn-primary">修改</button>
</form>
```

step 4、 完善修改的controller

```java
    @PostMapping("/updateEmp")
    public String updateEmp(Employee employee){
        System.out.println("==>updateEmp() ");
        employeeDao.add(employee);
        return "redirect:/emp/all";
    }
```



### 4.8 删除员工

先把url连接路由设置好

```html
<td>
    <!--<button class="btn btn-sm btn-primary" th:href="@{/emp(id=${emp.getId()})}">编辑</button>-->
    <a class="btn btn-sm btn-primary" th:href="@{/emp/} + ${emp.getId()}">编辑</a>
    <a class="btn btn-sm btn-danger" th:href="@{/delemp/} + ${emp.getId()}">删除</a>
</td>
```

再到后台，完善删除的接口

```java
//删除员工
@GetMapping("/delemp/{id}")
public String deleteEmp(@PathVariable("id") Integer id){
    employeeDao.delete(id);
    return "redirect:/emp/all";
}
```

这样我们就完成了删除的操作

> 处理404页面

直接在templates里面，新建一个error文件夹，然后新建404.html或者500.html 就可以了



> 退出登录

先到commons里面编辑页面

```html
<ul class="navbar-nav px-3">
    <li class="nav-item text-nowrap">
        <a class="nav-link" th:href="@{/user/logout}">注销</a>
    </li>
</ul>
```

完善我们的后台代码

```java
@RequestMapping("/user/logout")
public String logout(HttpSession session){
    session.invalidate();
    return "redirect:/main.html";
}
```



前端:

- 模板 别人写好的，我们哪来改成自己需要的
- 框架 组件 自己手动组合拼接，Bootstrap, Layui, semantic-ui
  - 栅格系统
  - 导航栏
  - 侧边栏
  - 表单



1、要有一套自己熟悉的后台模板  工作需要!  x-admin

2、前端界面 至少自己能够通过前端框架，组合出来一个网站页面

	- index
	- about
	- blog
	- post
	- user

3、让这个网站能够独立运行起来

一个月!

## 5. springboot jdbc

### 5.1 spring data 应用

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。

Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。

Sping Data 官网：https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

<span style="pading:10px; background:red;color:#fff">创建测试项目测试数据源</span>

1、新建一个时间springboot-data的项目， springboot-04-data, 引入相应的模块! 基础模块

![image-20210210155341987](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210210155341987.png)

2、项目建好之后，发现自动帮我们导入了如下的启动器 

```java
<dependencies>
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
    </dependency>

    <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
    </dependency>
    <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
    </dependency>
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    </dependency>
    </dependencies>
```

3、编写yaml配置文件链接数据库;

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/jdbcstudy?ussUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: 123456
```

4、配置完这一些东西后，我们就可以直接去使用了，因为SpringBoot已经默认帮我们进行了自动配置；去测试类测试一下

```java
@SpringBootTest
class Springboot04DataApplicationTests {
    //DI注入数据源
    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {
        //看一下默认数据源
        System.out.println(dataSource.getClass());
        //获得连接
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        //关闭连接
        connection.close();
    }

}
```

结果：我们可以看到他默认给我们配置的数据源为 : class com.zaxxer.hikari.HikariDataSource ， 我们并没有手动配置

我们来全局搜索一下，找到数据源的所有自动配置都在 ：DataSourceAutoConfiguration文件：

 ```java

@Import(
    {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}
)
protected static class PooledDataSourceConfiguration {
    protected PooledDataSourceConfiguration() {
    }
}
 ```



这里导入的类都在 DataSourceConfiguration 配置类下，可以看出 Spring Boot 2.2.5 默认使用HikariDataSource 数据源，而以前版本，如 Spring Boot 1.5 默认使用 org.apache.tomcat.jdbc.pool.DataSource 作为数据源；

**HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；**

**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。**

关于数据源我们并不做介绍，有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象 JdbcTemplate



<span style="pading:10px; background:red;color:#fff">JDBCTemplate</span>

1、有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；

2、即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。

3、数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。

4、Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用

5、JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。

测试一下JdbcTemplate

编写一个Controller,注入jdbcTemplate, 编写测试方法进行访问测试

```java
@RestController
@RequestMapping("/jdbc")
public class JdbcController {
    /**
     * springboot 默认提供了数据源，默认提供了org.springframework.jdbc.core.JdbcTemplate
     * JdbcTemplate 中会自己注入数据源，用于简化JDBC操作
     * 还能避免一些常见的错误，使用起来也不用再自己来管局数据库连接
     */
    @Autowired
    JdbcTemplate jdbcTemplate;
    //查询employee表中所有数据
    //List 中的1个 Map 对应数据库的 1行数据
    //Map 中的 key 对应数据库的字段名，value 对应数据库的字段值
    @GetMapping("/list")
    public List<Map<String, Object>> userList(){
        String sql = "select * from users";
        List<Map<String, Object>> listMap = jdbcTemplate.queryForList(sql);
        return listMap;
    }

    //新增一个用户
    @GetMapping("/add")
    public String addUser(){
        //插入语句，注意时间问题
        String sql = "insert into users(id,name,password,email,birthday) " +
                "values(10,'狂神说','123456','276314055@qq.com','"+new Date().toLocaleString()+"')";
        int update = jdbcTemplate.update(sql);
        //查询
        return "addOk";
    }

    //修改用户信息
    @GetMapping("/update/{id}")
    public String updateUser(@PathVariable("id") int id){
        //修改语句
        String sql = "update users set name=?,email=? where id="+id;
        //数据
        Object[] objects = new Object[2];
        objects[0] = "秦僵";
        objects[1] = "24736743@qq.com";
        jdbcTemplate.update(sql,objects);
        //查询
        return "updateOk";
    }

    //删除用户
    @GetMapping("/delete/{id}")
    public String delUser(@PathVariable("id") int id){
        //插入语句
        String sql = "delete from users where id=?";
        jdbcTemplate.update(sql,id);
        //查询
        return "deleteOk";
    }
}
```

测试请求，结果正常；

到此，CURD的基本操作，使用 JDBC 就搞定了。

<span style="pading:10px; background:red;color:#fff">SpringData的使用流程</span>

step 1、 创建项目，选择springboot 的项目，引入 spring web, Thymeleaf,  jdbc api , mysql driver, 这些是使用spring jdbc的需要依赖

step 2、 配置application.yaml配置文件，加入jdbc的相关配置

```properties
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/jdbcstudy?ussUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: 123456
```

step 3、使用springboot test 测试一下，验证环境连接正常

```java
@SpringBootTest
class Springboot04DataApplicationTests {
    //DI注入数据源
    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {
        //看一下默认数据源
        System.out.println(dataSource.getClass());
        //获得连接
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        //关闭连接
        connection.close();
    }

}
```

说明: 

1、 DataSource 是springboot创建的，我们可以直接引用并注入

2、有了数据源我们就可以直接获取连接，执行相关数据库操作；

3、在这里，我们可以打印一下，看看连接是否建立，如果已建立，就可以开始正常使用了

step 4、新建controller文件夹，并增加一个JdbcController文件

```java
@RestController
@RequestMapping("/jdbc")
public class JdbcController {
    /**
     * springboot 默认提供了数据源，默认提供了org.springframework.jdbc.core.JdbcTemplate
     * JdbcTemplate 中会自己注入数据源，用于简化JDBC操作
     * 还能避免一些常见的错误，使用起来也不用再自己来管局数据库连接
     */
    @Autowired
    JdbcTemplate jdbcTemplate;
    //查询employee表中所有数据
    //List 中的1个 Map 对应数据库的 1行数据
    //Map 中的 key 对应数据库的字段名，value 对应数据库的字段值
    @GetMapping("/list")
    public List<Map<String, Object>> userList(){
        String sql = "select * from users";
        List<Map<String, Object>> listMap = jdbcTemplate.queryForList(sql);
        return listMap;
    }

    //新增一个用户
    @GetMapping("/add")
    public String addUser(){
        //插入语句，注意时间问题
        String sql = "insert into users(id,name,password,email,birthday) " +
                "values(10,'狂神说','123456','276314055@qq.com','"+new Date().toLocaleString()+"')";
        int update = jdbcTemplate.update(sql);
        //查询
        return "addOk";
    }

    //修改用户信息
    @GetMapping("/update/{id}")
    public String updateUser(@PathVariable("id") int id){
        //修改语句
        String sql = "update users set name=?,email=? where id="+id;
        //数据
        Object[] objects = new Object[2];
        objects[0] = "秦僵";
        objects[1] = "24736743@qq.com";
        jdbcTemplate.update(sql,objects);
        //查询
        return "updateOk";
    }

    //删除用户
    @GetMapping("/delete/{id}")
    public String delUser(@PathVariable("id") int id){
        //插入语句
        String sql = "delete from users where id=?";
        jdbcTemplate.update(sql,id);
        //查询
        return "deleteOk";
    }
}
```

说明:

使用jdbc提供的JdbcTemplate, 一样可以执行CRUD

### 5.2 整合Druid

<span style="pading:10px; background:red;color:#fff">1、什么是Druid</span>

Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。

Druid是阿里巴巴开源平台上一个数据库连接池实现，结合了C3P0，BPCP，PROXOOL等DB池的优点，同时加入日志监控

Druid可以很好的监控DB吃连接和SQL的执行情况，天生就是针对监控而生的DB连接池。

srpingboot 2.0以上默认使用Hikari数据源，可以说Hikari与Druid都是当前JavaWeb上最优秀的数据源，我们来重点介绍SpringBoot如何继承Druid数据源，如何实现数据库监控。

Github地址：https://github.com/alibaba/druid/

**om.alibaba.druid.pool.DruidDataSource 基本配置参数如下：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LYLicOHVGnwu7ibGvbwXibYeupdhDcaDPRLHgnULFbaJB5kPtC8n5QVLaUbbTRfa4ZyqficzZYrd2llA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LYLicOHVGnwu7ibGvbwXibYeubiciawTdz0tg1EKDjZ1xaIgjRW9CZ4Apr4hvNz3iaQVQIKS3sXy629Lgg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LYLicOHVGnwu7ibGvbwXibYeuaVD6mK3LJrtZ4B6fRKCLDgYicAVGzTUTzWdCNB5lF4tLpbcCT0uq1EA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<span style="pading:10px; background:red;color:#fff">2、Druid有什么好处？</span>

1、速度非常快

2、自带监控，能够监控DB池的连接以及SQL的执行情况，

<span style="pading:10px; background:red;color:#fff">3、如何在项目使用Druid</span>

step 1、 加入druid的 数据源依赖

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.4</version>
</dependency>
```

step 2、 切换数据源，；之前已经说过 Spring Boot 2.0 以上默认使用 com.zaxxer.hikari.HikariDataSource 数据源，但可以 通过 spring.datasource.type 指定数据源。

```properties
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/jdbcstudy?ussUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
```

step 3、数据源切换之后，在测试类中注入 DataSource，然后获取到它，输出一看便知是否成功切换；

![image-20210213121643686](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210213121643686.png)

step 4、切换成功！既然切换成功，就可以设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

```properties
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

step 5、 导入Log4j的依赖

```java

<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

step 6、现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

```java
@Configuration
public class DruidConfiguration {
    /**
     * 将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
     * 绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
     * @ConfigurationProperties(prefix = "spring.datasource.druid")：作用就是将 全局配置文件中
     * 前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中=
     *    */
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource() {
        return new DruidDataSource();
    }
}
```

step 7、 这样我们就可以正常使用Druid数据源了，我们可以在springboot测试一下，看看是否 成功

```java
@Test
public void testConnection() throws SQLException {
    //看一下默认数据源
    System.out.println(dataSource.getClass());
    //获得连接
    Connection connection =   dataSource.getConnection();
    System.out.println(connection);

    DruidDataSource druidDataSource = (DruidDataSource) dataSource;
    System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
    System.out.println("druidDataSource 数据源初始化连接数：" + druidDataSource.getInitialSize());

    //关闭连接
    connection.close();
}
```

![image-20210213124606541](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210213124606541.png)

做完这些，我们就可有像使用其他数据源一样，使用Druid数据源了，我们还可以配置一下，从而使用Druid的数据监控能力

<span style="pading:10px; background:red;color:#fff">4、使用Druid特有的监控能力</span>

Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器 时，人家也提供了一个默认的 web 页面。

step1、设置 Druid 的后台管理页面，比如 登录账号、密码 等；配置后台管理；我们在DruidConfiguration.java中增加如下代码

```java

//配置 Druid 监控管理后台的Servlet；
//内置 Servlet 容器时没有web.xml文件，所以使用 Spring Boot 的注册 Servlet 方式
@Bean
public ServletRegistrationBean statViewServlet() {
    ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");

    // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet 
    // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
    Map<String, String> initParams = new HashMap<>();
    initParams.put("loginUsername", "admin"); //后台管理界面的登录账号
    initParams.put("loginPassword", "123456"); //后台管理界面的登录密码

    //后台允许谁可以访问
    //initParams.put("allow", "localhost")：表示只有本机可以访问
    //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
    initParams.put("allow", "");
    //deny：Druid 后台拒绝谁访问
    //initParams.put("kuangshen", "192.168.1.20");表示禁止此ip访问

    //设置初始化参数
    bean.setInitParameters(initParams);
    return bean;
}
```

配置完毕后，我们可以选择访问 ：http://localhost:8080/druid/login.html

![image-20210213124929439](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210213124929439.png)

step 2、配置 Druid web 监控 filter 过滤器，这样我们就可以实现监控指定的功能或者接口，或者语句

```java
/配置 Druid 监控 之  web 监控的 filter
//WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
@Bean
public FilterRegistrationBean webStatFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    bean.setFilter(new WebStatFilter());

    //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
    Map<String, String> initParams = new HashMap<>();
    initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
    bean.setInitParameters(initParams);

    //"/*" 表示过滤所有请求
    bean.setUrlPatterns(Arrays.asList("/*"));
    return bean;
}
```



### 5.3 整合mybatis

<span style="pading:10px; background:red;color:#fff">1、如何整合mybatis</span>

对于在springboot中添加mybatis,和我们之前在springMVC整合mybatis一样，需要添加特有的依赖。 在这个项目中，添加mybatis的依赖，依赖的地址为：

```properties
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

官方文档：http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

Maven仓库地址：https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1



<span style="pading:10px; background:red;color:#fff">2、使用mybatis的完整demo</span>

step 1 、在项目中导入Mybatis所需要的的依赖

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

step 2、数据库的连接信息配置不需要改变，如下：

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/jdbcstudy?ussUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源

    # Spring Boot 默认是不注入这些属性值的，需要自己绑定
    # druid 数据源专有配置

    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

step 3、 执行springboot的测试，看一下，我们的连接是否成功

![image-20210213154023073](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210213154023073.png)

step 4、*创建实体类，导入 Lombok！*

```java
@Data
@NoArgsConstructor
@AllArgsConstructor

public class Department {
    private Integer id;
    private String departmentName;
}
```

step 5、创建mapper目录以及对应的Mapper接口

```java
//Mapper : 表示本类是一个MyBatis的Mapper
@Mapper
@Repository
public interface DepartmentMapper {
    // 获取所有部门信息
    List<Department> getDepartments();

    // 通过id获得部门
    Department getDepartment(Integer id);
}
```

step 6、创建Mapper对应的映射文件，我们可以选择直接放在和DepartMapper相同的目录，也可以在resource下面新建mybatis/mapper，然后在里面新建DepartmentMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.kuang.mapper.DepartmentMapper">

    <select id="getDepartments" resultType="Department">
       select * from department;
    </select>

    <select id="getDepartment" resultType="Department" parameterType="int">
       select * from department where id = #{id};
    </select>
</mapper>
```

step 7、maven配置资源过滤问题，不然xml不会被包含进去

```xml

<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
    </resource>
</resources>
```

step 8、 编写部门的DepartmentController进行测试

```java
@RestController
public class DepartmentController {
    @Autowired
    DepartmentMapper departmentMapper;

    // 查询全部部门
    @GetMapping("/all")
    public List<Department> getDepartments(){
        return departmentMapper.getDepartments();
    }

    //获取部门
    @GetMapping("/dep/{id}")
    public Department getDepartment(@PathVariable("id") int id){
        return departmentMapper.getDepartment(id);
    }
}
```

step 9、修改application.yaml, 增加mybatis的mapper文件配置

```yaml
mybatis:
  typeAliasesPackage: com.kuang.pojo  # 指明实体类命名别名所在目录
  mapper-locations: classpath*:mybatis/mapper/*.xml # 指定XXXXMapper.xml配置文件所在目录
  # config-location: classpath:mybatis-config.xml config-location是指定Mybatis的配置文件
  configuration:
    map-underscore-to-camel-case: true  # 配置驼峰命名规则
```

启动项目访问进行测试即可!



<span style="pading:10px; background:red;color:#fff">3、使用mybatis的CURD</span>

step 1、新建一个pojo类，Employee

```java

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Employee {

    private Integer id;
    private String lastName;
    private String email;
    //1 male, 0 female
    private Integer gender;
    private Integer department;
    private Date birth;

    private Department eDepartment; // 冗余设计

}
```

step 2、新建一个EmployeeMapper接口

```java
/@Mapper : 表示本类是一个 MyBatis 的 Mapper
@Mapper
@Repository
public interface EmployeeMapper {

    // 获取所有员工信息
    List<Employee> getEmployees();

    // 新增一个员工
    int save(Employee employee);

    // 通过id获得员工信息
    Employee get(Integer id);

    // 通过id删除员工
    int delete(Integer id);

}
```

step 3、 编写EmployeeMapper.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.kuang.mapper.EmployeeMapper">

    <resultMap id="EmployeeMap" type="Employee">
        <id property="id" column="eid"/>
        <result property="lastName" column="last_name"/>
        <result property="email" column="email"/>
        <result property="gender" column="gender"/>
        <result property="birth" column="birth"/>
        <association property="eDepartment"  javaType="Department">
            <id property="id" column="did"/>
            <result property="departmentName" column="dname"/>
        </association>
    </resultMap>

    <select id="getEmployees" resultMap="EmployeeMap">
        select e.id as eid,last_name,email,gender,birth,d.id as did,d.department_name as dname
        from department d,employee e
        where d.id = e.department
    </select>

    <insert id="save" parameterType="Employee">
        insert into employee (last_name,email,gender,department,birth)
        values (#{lastName},#{email},#{gender},#{department},#{birth});
    </insert>

    <select id="get" resultType="Employee">
        select * from employee where id = #{id}
    </select>

    <delete id="delete" parameterType="int">
        delete from employee where id = #{id}
    </delete>

</mapper>
```

step 4、编写EmployeeController类进行测试

```java
@RestController
public class EmployeeController {

    @Autowired
    EmployeeMapper employeeMapper;

    // 获取所有员工信息
    @GetMapping("/getEmployees")
    public List<Employee> getEmployees(){
        return employeeMapper.getEmployees();
    }

    @GetMapping("/save")
    public int save(){
        Employee employee = new Employee();
        employee.setLastName("kuangshen");
        employee.setEmail("qinjiang@qq.com");
        employee.setGender(1);
        employee.setDepartment(101);
        employee.setBirth(new Date());
        return employeeMapper.save(employee);
    }

    // 通过id获得员工信息
    @GetMapping("/get/{id}")
    public Employee get(@PathVariable("id") Integer id){
        return employeeMapper.get(id);
    }

    // 通过id删除员工
    @GetMapping("/delete/{id}")
    public int delete(@PathVariable("id") Integer id){
        return employeeMapper.delete(id);
    }

}
```

## 6 spring security

在 Web 开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。因此，从应用开发的第一天就应该把安全相关的因素考虑进来，并在整个应用的开发过程中。

市面上存在比较有名的：Shiro，Spring Security ！

这里需要阐述一下的是，每一个框架的出现都是为了解决某一问题而产生了，那么Spring Security框架的出现是为了解决什么问题呢？

首先我们看下它的官网介绍：Spring Security官网地址

Spring Security is a powerful and highly customizable authentication and access-control framework. It is the de-facto standard for securing Spring-based applications.

Spring Security is a framework that focuses on providing both authentication and authorization to Java applications. Like all Spring projects, the real power of Spring Security is found in how easily it can be extended to meet custom requirements

Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架。它实际上是保护基于spring的应用程序的标准。

Spring Security是一个框架，侧重于为Java应用程序提供身份验证和授权。与所有Spring项目一样，Spring安全性的真正强大之处在于它可以轻松地扩展以满足定制需求

从官网的介绍中可以知道这是一个权限框架。想我们之前做项目是没有使用框架是怎么控制权限的？对于权限 一般会细分为功能权限，访问权限，和菜单权限。代码会写的非常的繁琐，冗余。

怎么解决之前写权限代码繁琐，冗余的问题，一些主流框架就应运而生而Spring Scecurity就是其中的一种。

Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套 Web 应用安全性的完整解决方案。一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。用户认证指的是验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

对于上面提到的两种应用情景，Spring Security 框架都有很好的支持。在用户认证方面，Spring Security 框架支持主流的认证方式，包括 HTTP 基本认证、HTTP 表单验证、HTTP 摘要认证、OpenID 和 LDAP 等。在用户授权方面，Spring Security 提供了基于角色的访问控制和访问控制列表（Access Control List，ACL），可以对应用中的领域对象进行细粒度的控制。

### 6.1、认识SpringSecurity

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入 spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式  

Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。

**“认证”（Authentication）**

身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

 **“授权” （Authorization）**

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。



### 6.2 搭建学习环境

shiro springsecurity 很像，除了类不一样，名字不一样

认证 authentication 和授权 authorization（vip1 vip2 vip3）

一般去做功能权限、访问权限、菜单权限

采用AOP-横切的思想



step 1、 新建一个项目，名 springboot-06-security, 导入springweb和thymeleaf这两个支持

step 2、导入静态资源文件，

step 3、编写controller跳转

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class RouterController {

   @RequestMapping({"/","/index"})
   public String index(){
       return "index";
  }

   @RequestMapping("/toLogin")
   public String toLogin(){
       return "views/login";
  }

   @RequestMapping("/level1/{id}")
   public String level1(@PathVariable("id") int id){
       return "views/level1/"+id;
  }

   @RequestMapping("/level2/{id}")
   public String level2(@PathVariable("id") int id){
       return "views/level2/"+id;
  }

   @RequestMapping("/level3/{id}")
   public String level3(@PathVariable("id") int id){
       return "views/level3/"+id;
  }

}
```

可以测试一下，这个时候，我们的环境都是可以进入。

### 6.3 添加spring security支持

[官网](https://docs.spring.io/spring-security/site/docs/5.4.2/reference/html5/#jc)

step 1、 导入spring-security模块的支持

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

step 2、编写Spring Security的配置类

查看我们自己项目中的版本，找到对应的帮助文档：

https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5  #servlet-applications 8.16.4

```java
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

/**
 * Created by Josh luo on 2021/2/13
 */
@EnableWebSecurity //开启WebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
    }
}
```

step 3、 定制请求的授权(authorization)规则

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    super.configure(http);
    // 定制请求的授权规则
    // 首页所有人可以访问，但是里面的功能页只有对应有权限的人才能访问
    http.authorizeRequests().antMatchers("/").permitAll()
        .antMatchers("/level1/**").hasRole("vip1")
        .antMatchers("/level2/**").hasRole("vip2")
        .antMatchers("/level3/**").hasRole("vip3");
}
```

step 4、测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！

step 5、在configure()方法中加入以下配置，开启自动配置的登录功能！

```java
// 开启自动配置的登录功能
// /login 请求来到登录页
// /login?error 重定向到这里表示登录失败
http.formLogin();
```

step 6、测试一下：发现，没有权限的时候，会跳转到登录的页面，而不是所有页面都可以跳进去了！

![image-20210213201432541](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210213201432541.png)

step 7、查看刚才登录页的注释信息；

我们可以定义认证Authentication规则，重写configure(AuthenticationManagerBuilder auth)方法

```java
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    //super.configure(auth);
    //创造一个从内存获取用户的接口 如果从jdbc就是auth.jdbcAuthentication() 这些数据正常应该从数据中心读
    auth.inMemoryAuthentication()
        .withUser("kuangshen").password("123456").roles("vip2","vip3")
        .and()
        .withUser("root").password("123456").roles("vip1","vip2","vip3")
        .and()
        .withUser("guest").password("123456").roles("vip1","vip2");
}
```

step 8、测试，我们可以使用这些账号登录进行测试！发现会报错！

There is no PasswordEncoder mapped for the id “null”

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   //在内存中定义，也可以在jdbc中去拿....
   //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
   //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
   //spring security 官方推荐的是使用bcrypt加密方式。
   
   auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
          .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
          .and()
          .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
}
```

这个是因为没有密码的加密，在5.0+以后需要进行编码才能进行校验，我们改了编码后，就可以正常访问了，。

使用JDBCAuthentication的官网地址 [JDBCAuthentication](https://docs.spring.io/spring-security/site/docs/5.4.2/reference/html5/#servlet-authentication-unpwd)

![image-20210213205817807](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210213205817807.png)



### 6.4 权限控制和注销

springboot中使用thymeleaf-extras-springsecurity实现权限管理

step 0、环境

springboot：2.4.2
thymeleaf-extras-springsecurity：5.x
jdk：1.8
maven：3.6.3

step 1、 导入依赖， 在pom.xml中导入thymeleaf和security还有thymeleaf-security整合的相关依赖

```xml
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity5 -->
        <!--支持thymeleaf-springsecurity-->
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity5</artifactId>
            <version>3.0.4.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

step 2、security的配置

继承WebSecurityConfigurerAdapter ，编写Security配置，在config包下新建一个自命名的配置文件，比如SecurityConfig类，继承WebSecurityConfigurerAdapter，重写父类方法，可以配置http拦截请求、登录拦截，权限的拦截等等自定义功能，我这里简单写个demo，如下

```java
@EnableWebSecurity //开启WebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //super.configure(http);
        // 定制请求的授权规则
        // 首页所有人可以访问
        http.authorizeRequests().antMatchers("/").permitAll() //首页页面不拦截
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3")
                .antMatchers("/css/**").permitAll()
                .antMatchers("/js/**").permitAll()
                .antMatchers("/img/**").permitAll();
        // 开启自动配置的登录功能
        // /login 请求来到登录页
        // /login?error 重定向到这里表示登录失败
        http.formLogin(form->form
                .loginPage("/tologin")  	/*自定义登录页面地址*/
                .loginProcessingUrl("/login")
                .usernameParameter("username")
                .passwordParameter("password")
                .permitAll()
        );

        //防止网站攻击工具  get post
        //http.csrf().disable(); //关闭csrf功能，登录失败肯存在的原因
        //注销  开启注销功能，重定向到首页
        http.logout()
                .logoutUrl("/logout") /*自定义登出链接*/
                .logoutSuccessUrl("/") /*如果成功登出，需要跳转的页面*/
                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
                .permitAll();
        // 开启记住我功能 cookie 默认保存两周，自定义接手前端的参数
        //http.rememberMe().rememberMeParameter("remember");

    }

    // 定义认证的规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //super.configure(auth);
        //创造一个从内存获取用户的接口 如果从jdbc就是auth.jdbcAuthentication()
        //在spring security 5.0+ 新增了很多的加密方式
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
                .and()
                .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
    }
}
```

更多的配置信息可以点进父类的源码，其中的注释非常详细！

step 3、thymeleaf中使用security



一些常用的属性：

```java
// 展示登录名
<div th:text="${#authentication.name}"></div>

// 使用属性获取登录名
<div sec:authentication="name">
  The value of the "name" property of the authentication object should appear here.
</div>

// 条件判断，判断是否有ADMIN这个角色
<div th:if="${#authorization.expression('hasRole(''ROLE_ADMIN'')')}">
  This will only be displayed if authenticated user has role ROLE_ADMIN.
</div>

// 使用属性判断是否有相应的角色（权限）
<div sec:authorize="${hasRole(#vars.expectedRole)}">
  This will only be displayed if authenticated user has a role computed by the controller.
</div>

// 获取登录用户的相应权限（角色）
<div th:text="${#authentication.getAuthorities()}"></div>
    
// 判断是否登录
<div sec:authorize="${isAuthenticated()}"></div>
```

更多的可以看官网或者源码获取。

step 4、 登录注销小案例

```html
<!--登录注销-->
<div class="right menu">
    <!--如果未登录-->
    <div sec:authorize="${!isAuthenticated()}">
        <a class="item" th:href="@{/tologin}">
            <i class="address card icon"></i> 登录
        </a>
    </div>
    <!--如果已登录-->
    <div sec:authorize="${isAuthenticated()}">
        <a th:href="@{/usr/toUserCenter}">
            用户名: <span th:text="${#authentication.name}"></span>
            角色: <span th:text="${#authentication.getAuthorities()}"></span>
        </a>
    </div>
    <div sec:authorize="${isAuthenticated()}">
        <a class="item" th:href="@{/logout}">
            <i class="sign-out icon"></i> 注销
        </a>
    </div>
</div>
```

踩过的坑：

1、hymeleaf-security如果是使用`4.x`版本的，那么html文件中的命名空间是 `xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4`，如果是用的`5.x`的版本，html头文件申明的命名空间应该是`xmlns:sec="http://www.thymeleaf.org/extras/spring-security"`

2、spring security默认开始csrf，所以登录和退出都需要post提交，form里面包含

```html
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>
```

3、如果Logout不使用表单提交，想仍然使用post，需要加入 

```java
 .logoutRequestMatcher(new AntPathRequestMatcher("/logout"));
```

### 6.5 角色功能模块认证

```html
    <div>
        <br>
        <div class="ui three column stackable grid">
            <div class="column" sec:authorize="hasRole('vip1')">
                <div class="ui raised segment">
                    <div class="ui">
                        <div class="content">
                            <h5 class="content">Level 1</h5>
                            <hr>
                            <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
                            <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
                            <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="column" sec:authorize="hasRole('vip2')">
                <div class="ui raised segment">
                    <div class="ui">
                        <div class="content">
                            <h5 class="content">Level 2</h5>
                            <hr>
                            <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
                            <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
                            <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="column" sec:authorize="hasRole('vip3')">
                <div class="ui raised segment">
                    <div class="ui">
                        <div class="content">
                            <h5 class="content">Level 3</h5>
                            <hr>
                            <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
                            <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
                            <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
                        </div>
                    </div>
                </div>
            </div>

        </div>
```

这样，不同角色进入首页，就能看到不同的功能了

```
<div class="column" sec:authorize="hasRole('vip1')">
```

### 6.6 记住我

现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？很简单

1、开启记住我功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
//。。。。。。。。。。。
   //记住我
   http.rememberMe();
}
```

2、我们再次启动项目测试一下，发现登录页多了一个记住我功能，我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！

思考：如何实现的呢？其实非常简单

我们可以查看浏览器的cookie

![image-20210214002526071](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210214002526071.png)



3、我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

![image-20210214002538990](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210214002538990.png)

4、结论：登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，具体的原理我们在JavaWeb阶段都讲过了，这里就不在多说了！

## 7、Shiro

### 7.1 shiro简介

Apache shiro是一个java的安全(权限)框架

Shiro可以非常容易的开发出足够好的应用，不仅可以用在JavaSE环境，也可以用在JavaEE环境

Shiro可以完成 认证、授权、加密、会话管理、web继承、缓存等

下载地址: http://shiro.apache.org

`有哪些功能`

![image-20210214124139597](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210214124139597.png)

- Authentication  身份认证、登录、验证用户是不是拥有相应的身份
- Authorization  授权，权限验证，验证某个已认证的用户是否拥有某个权限，既判断用户是否可以进行什么操作，例如某个用户是否拥有某个角色，或者细粒度的验证某个用户对某个资源是否具有某个权限!
- Session manager 会话管理，既用户登录后就是第一次绘画，在没有退出之前，它的所有信息都在会话中，会话可以是普通的JavaSE环境，也可以是Web环境
- Cryptography 加密，保护数据的安全性，如密码加密存储到数据库中，而不是明文存储
- web support web支持，可以非常容易的集成到web环境
- caching 缓存，比如用户登录后，用户信息，拥有的权限、角色不必每次都去查询，这样可以提高效率
- Concurrency  shiro支持多线程应用的并发验证，既，如在一个线程中开启另一个线程，能把权限自动的传播过去
- Testing 提供测试支持
- Runas 允许一个用户假装成另一个用户(如果他们允许)的身份进行访问
- Remember me  记住我，这个是非常常见的功能，既一次登录后，下次再来的话不用在登陆

`shiro的框架(外部的)`

从外部看shiro，即从应用程序的角度来观察如何使用shiro来完成工作

![image-20210214124820923](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210214124820923.png)

- subject 应用代码直接交互的对象是Subject，也就是说Shiro的对外api核心就是subject,subject代表了当前的用户，这个用户不一定是一个具体的人，与当前应用交互的任何东西都可以是subject，例如网络爬虫，机器人等，与subject的所有交互都委托给SecurityManager，subject其实是一个门面，SecurityManager才是实际的执行者
- SecurityManager  安全管理器，既所有与安全有关的操作都会与SecurityManager交互，并且它管理着所有的subject,可以看出他是Shiro的核心，他负责与shiro的其他组件进行交互，他相当于SpringMVC的DispatcherServlet角色
- Realm  shiro从Realm获取安全数据(如用户、角色、权限)，也就是说SecurityManager要验证用户身份，那么它需要从Realm获取相应的用户进行比较，来确定用户的身份是否合法，也需要从Realm得到用户响应的角色、权限，进行验证用户的操作是否能够进行，可以吧Realm看作是DataSource

### 7.2 HelloWorld 快速实践

查看官网文档：http://shiro.apache.org/get-started.html

官方的quickstack: http://shiro.apache.org/10-minute-tutorial.html

1、创建一个maven父工程，用于学习shiro，删掉不必要的东西，含父工程的src/目录下的内容

![image-20210214175025512](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210214175025512.png)

2、创建一个普通的Maven子工程，shiro-01-helloshiro

3、根据官方文档，我们来导入shiro的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-guice</artifactId>
        <version>1.7.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>1.7.1</version>
    </dependency>
    <!-- configure logging -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>2.0.0-alpha1</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>2.0.0-alpha1</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

4、增加log4j和shiro.ini，拷贝官网的QuickStart

```java
public class Quickstart {

    private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);


    public static void main(String[] args) {

        // The easiest way to create a Shiro SecurityManager with configured
        // realms, users, roles and permissions is to use the simple INI config.
        // We'll do that by using a factory that can ingest a .ini file and
        // return a SecurityManager instance:

        // Use the shiro.ini file at the root of the classpath
        // (file: and url: prefixes load from files and urls respectively):
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        SecurityManager securityManager = factory.getInstance();

        // for this simple example quickstart, make the SecurityManager
        // accessible as a JVM singleton.  Most applications wouldn't do this
        // and instead rely on their container configuration or web.xml for
        // webapps.  That is outside the scope of this simple quickstart, so
        // we'll just do the bare minimum so you can continue to get a feel
        // for things.
        SecurityUtils.setSecurityManager(securityManager);

        // Now that a simple Shiro environment is set up, let's see what you can do:

        // get the currently executing user:
        Subject currentUser = SecurityUtils.getSubject();

        // Do some stuff with a Session (no need for a web or EJB container!!!)
        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            log.info("Retrieved the correct value! [" + value + "]");
        }

        // let's login the current user so we can check against roles and permissions:
        if (!currentUser.isAuthenticated()) {
            UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
            token.setRememberMe(true);
            try {
                currentUser.login(token);
            } catch (UnknownAccountException uae) {
                log.info("There is no user with username of " + token.getPrincipal());
            } catch (IncorrectCredentialsException ice) {
                log.info("Password for account " + token.getPrincipal() + " was incorrect!");
            } catch (LockedAccountException lae) {
                log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                        "Please contact your administrator to unlock it.");
            }
            // ... catch more exceptions here (maybe custom ones specific to your application?
            catch (AuthenticationException ae) {
                //unexpected condition?  error?
            }
        }

        //say who they are:
        //print their identifying principal (in this case, a username):
        log.info("User [" + currentUser.getPrincipal() + "]: logged in successfully.");

        //test a role:
        if (currentUser.hasRole("schwartz")) {
            log.info("May the Schwartz be with you!");
        } else {
            log.info("Hello, mere mortal.");
        }

        //test a typed permission (not instance-level)
        if (currentUser.isPermitted("lightsaber:wield")) {
            log.info("You may use a lightsaber ring.  Use it wisely.");
        } else {
            log.info("Sorry, lightsaber rings are for schwartz masters only.");
        }

        //a (very powerful) Instance Level permission:
        if (currentUser.isPermitted("winnebago:drive:eagle5")) {
            log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                    "Here are the keys - have fun!");
        } else {
            log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
        }

        //all done - log out!
        currentUser.logout();

        System.exit(0);
    }
}
```

总结：SpringSecurity都有

1、获取Subject   Subject currentUser = SecurityUtils.getSubject();

2、session  Session session = currentUser.getSession();

3、是否被验证 !currentUser.isAuthenticated()

4、获取角色信息  currentUser.hasRole("schwartz")

5、currentUser.isPermitted("lightsaber:wield")

6、 currentUser.logout();

7、currentUser.getPrincipal()

### 7.3 springboot集成环境准备

环境包括以下几个方面

1、导入的依赖   shiro-spring、spring-boot-starter-thymeleaf、spring-boot-starter-web

2、需要有一个实现AuthorizingRealm的自定义UserRealm

3、需要有一个自定义的JavaConfiguration ,  ShiroConfig, 在这个java配置类，向springboot注入UserRealm、DefaultWebSecurityManager、ShiroFilterFactoryBean

具体执行步骤如下:

step 1、导入依赖

```java
	<dependencies>
		<!--
			subject   用户
			SecurityManager 管理所有用户
			Realm 连接数据
		-->
		<!--导入shiro 整合spring的包-->
		<!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-spring -->
		<dependency>
			<groupId>org.apache.shiro</groupId>
			<artifactId>shiro-spring</artifactId>
			<version>1.7.0</version>
		</dependency>


		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```

step 2、 新建ShiroConfig,编写核心配置

```java
//自定义UserRealm
public class UserRealm extends AuthorizingRealm {
    // 授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        System.out.println("执行了=>授权doGetAuthorizationInfo");
        return null;
    }
    // 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("执行了=>doGetAuthenticationInfo");
        return null;
    }
}


@Configuration
public class ShiroConfig {
    /*先创建对象，再接管对象*/
    //1、 提供ShiroFilterFactoryBean step 3 需要
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(securityManager);
        return bean;
    }
    //2、 DefaultWebSecurityManager  step 2
    @Bean(name ="securityManager" )
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){

        //@Qualifier("userRealm") 通过这个，将getDefaultWebSecurityManager和下面的bean绑定了起来
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        // 关联realm
        securityManager.setRealm(userRealm);
        return securityManager;
    }
    //3、创建Realm对象, 需要自定义类 step1
    @Bean(name = "userRealm")
    public UserRealm userRealm(){
        return new UserRealm();
    }
}
```



step 3 、完善一下测试环境的html

index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="https://www.thymeleaf.org/">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    <h1>首页</h1>
    <p th:text="${msg}"></p>
    <hr>
    <a th:href="@{/user/add}">add</a> | <a th:href="@{/user/update}">update</a>
</body>
</html>
```

/user/add.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>add</title>
</head>
<body>
    <h1>add</h1>
</body>
</html>
```

/user/update.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>update</title>
</head>
<body>
    <h1>update</h1>
</body>
</html>
```

### 7.4 shiro实现登录拦截

环境准备

1、页面增加add.html,update.html

2、依赖增加  shiro-spring、spring-boot-starter-thymeleaf、spring-boot-starter-web

3、编写一个实现AuthorizingRealm的UserRealm，里面实现doGetAuthorizationInfo()和doGetAuthenticationInfo()方法

4、实现对add.html和Update.html访问的Controller

5、index 增加 add.html和update.html的连接

功能需求： 有的用户可以点击add.html，有的用户无法点击add.html```

知识点1： 实现拦截，shiro提供了一组默认的过滤器，分别为

```
/**
* anon  无需认证就可以访问
* authc 必须认证了才能访问
* user  必须拥有记住我的功能才能访问
* perms： 拥有对某个资源的权限才能访问
* role  拥有某个角色的权限才能访问
*/
```

使用的方式:

```java
Map<String, String> filterMap =  new LinkedMap();
filterMap.put("/user/add","anon");
filterMap.put("/user/update","authc");

bean.setFilterChainDefinitionMap(filterMap);
```

上述实现后，用户未登录就无法进入到update了，权限拦截后，就会跳到登录页，为了实现登录页，我们需要自己写一个login.html页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>login</title>
</head>
<body>
    <h1>登录</h1>
    <hr>
    <form action="">
        <p>用户名:<input type="text" name="username" ></p>
        <p>密 码:<input type="password" name="password" ></p>
        <p><input type="submit"></p>
    </form>
</body>
</html>
```

然后，我们需要在Controller里面添加一个跳转

```
@RequestMapping("/toLogin")
public String toLogin(){
    return "login";
}
```

最后，再回到ShiroConfig的getShiroFilterFactoryBean（）方法中，添加验权失败跳转

```
//如果没权限就跳入到登录页面
bean.setLoginUrl("/toLogin");
```

这样，用户没有权限或者未登录用户就会跳转到登录页，这样就对未认证用户的拦截成功



拦截器也可以使用通配符实现：

```
filterMap.put("/user/*","authc");
```

完整的代码

```java
@Configuration
public class ShiroConfig {
    /*先创建对象，再接管对象*/
    //1、 提供ShiroFilterFactoryBean step 3 需要
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(securityManager);
        // shiro功能1 : 通过添加shiro的内置过滤器实现登录的拦截
        /**
         * anon  无需认证就可以访问
         * authc 必须认证了才能访问
         * user  必须拥有记住我的功能才能访问
         * perms： 拥有对某个资源的权限才能访问
         * role  拥有某个角色的权限才能访问
         */
        Map<String, String> filterMap =  new LinkedMap();
        filterMap.put("/user/add","anon");
        filterMap.put("/user/update","authc");
        filterMap.put("/user/*","authc");
        bean.setFilterChainDefinitionMap(filterMap);
        //如果没权限就跳入到登录页面
        bean.setLoginUrl("/toLogin");
        return bean;
    }
    //2、 DefaultWebSecurityManager  step 2
    @Bean(name ="securityManager" )
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){

        //@Qualifier("userRealm") 通过这个，将getDefaultWebSecurityManager和下面的bean绑定了起来
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        // securityManager需要关联realm
        securityManager.setRealm(userRealm);
        return securityManager;
    }
    //3、创建Realm对象,交给spring托管 需要自定义类 step1
    @Bean(name = "userRealm")
    public UserRealm userRealm(){
        return new UserRealm();
    }
} 
```





### 7.5 shiro实现登录认证

shiro的认证需求是在Realm里面实现的，这个需要注意，也就是我们自定义的UserRealm类里面添加认证的代码

功能需求： 提交用户名密码，可以进行认证，并通过认证，

思路： 通过模拟一个提交登录的请求，可以发现请求都会通过doGetAuthenticationInfo（）方法

<span style="pading:10px; background:red;color:#fff">1、 不连接数据库，模拟数据实现认证</span>

step 1、 编写登录页面

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>login</title>
</head>
<body>
    <h1>登录</h1>
    <span th:text="${msg}" style="color: red"></span>
    <hr>
    <form th:action="@{/login}" method="post">
        <p>用户名:<input type="text" name="username" ></p>
        <p>密 码:<input type="password" name="password" ></p>
        <p><input type="submit"></p>
    </form>
</body>
</html>
```

step 2、 编写登录的Controller

```java
//登录
@RequestMapping("/login")
public String login(String username, String password, Model model){
    //1 先获取Subject
    Subject subject = SecurityUtils.getSubject();
    //2 .封装用户的登录数据, 获取令牌
    UsernamePasswordToken userToken = new UsernamePasswordToken(username, password);
    //3 . 获取登录
    try {
        //执行登录的方法，如果么有异常就说明登录成功了
        subject.login(userToken);
        return "index";
    } catch (UnknownAccountException uae) { //用户名不存在
        System.out.println(">>>用户名不存在!");
        model.addAttribute("msg","用户名不存在!");
        return "login";
    } catch (IncorrectCredentialsException ice) { //密码不对
        model.addAttribute("msg","密码错误!");
        return "login";
    } catch (LockedAccountException lae) { //用户账户被锁定
        model.addAttribute("msg","用户账户被锁定");
        return "login";
    }catch (AuthenticationException e) {
        model.addAttribute("msg","登录异常错误，请检查!");
        return "login";
    }

}
```

step 3、 进入到UserRealm的doGetAuthenticationInfo（）方法，完成认证的操作

```java
    // 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("执行了=>doGetAuthenticationInfo");
        //用户名，密码，数据库中取

        String name = "root";
        String password ="123456";
        //强制类型转换成认识的Token
        UsernamePasswordToken userToken = (UsernamePasswordToken)token;
        if (!userToken.getUsername().equals(name)){
            return null; //返回return null,前面的subject.login(userToken)就会抛出一个UnknownAccountException

        }
        //密码的认证，shiro自己做
        //需要传递三个参数
        return new SimpleAuthenticationInfo("",password,"");
    }
}
```

这样我们就实现了对数据的校验，当然上面这个例子是模拟的校验，下面我们来真实通过Mybatis连接数据库，实现用户登录的校验

<span style="pading:10px; background:red;color:#fff">2、通过mybatis连接数据库认证登录</span>

我们先从搭建一个完整的项目赖做这个真实的连接数据库项目

step1 、添加对应的依赖

```xml
<dependencies>
    <!--整合mysql驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.49</version>
    </dependency>
    <!--整合druid资源-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.4</version>
    </dependency>
    <!--整合log4j-->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <!--集成mybatis 这是mybatis官方提供的适配springboot的，不是springboot自己的-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.1.4</version>
    </dependency>
    <!--
   subject   用户
   SecurityManager 管理所有用户
   Realm 连接数据
  -->
    <!--导入shiro 整合spring的包-->
    <!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-spring -->
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-spring</artifactId>
        <version>1.7.0</version>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

step 2、编写application.yaml配置文件，增加druid，mybatis等相关配置

```yaml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC

    # spring boot 默认是不注入这些属性，需要自己绑定
    # druid 数据源的专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 60000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    ## 配置监控统计拦截器 filters,stat:监控统计、log4j,日志统计，wall， 防御sql注入
    # 如果允许时报错 java.lang.ClassNotFoundException org.apache.log4j.Priority
    # 则导入log4j， 依赖即可，Maven地址 https://mvcrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.kuang.pojo
```

step 3、加入mybatis的相关信息，分别为pojo实体类，MapperDao接口，XXXMapper.xml

pojo实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

Mapper接口

```java
@Repository
@Mapper
public interface UserMapper {

    public User getUserByName(String name);
}
```

mapper.xml配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.UserMapper">
    <!--获取用户通过名字-->
    <select id="getUserByName" parameterType="String" resultType="User">
        select * from user where name = #{name}
    </select>
</mapper>
```

service实现

```java
@Service
public class UserServiceImpl implements UserService {
    @Autowired
    UserMapper userMapper;
    @Override
    public User getUserByName(String name) {
        return userMapper.getUserByName(name);
    }
}
```

在这里 我们可以测试一下，看看底层的相关配置是否都是好的。如果是好的，我们就可以开始接着继续往下，进行shiro的登录认证数据库实现了

step 4、 进入到UserRealm的doGetAuthenticationInfo（），方法，我们进行优化

```java
// 认证
//获取数据库的操作接口
@Autowired
UserServiceImpl userService;
@Override
protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
    System.out.println("执行了=>doGetAuthenticationInfo");
    //案例 2 连接真实的数据库
    UsernamePasswordToken userToken = (UsernamePasswordToken)token;
    User user = userService.getUserByName(userToken.getUsername());
    if (user== null){//没有这个用户
        return null; //返回return null,前面的subject.login(userToken)就会抛出一个UnknownAccountException
    }
    //密码的认证，shiro自己做
    //可以加密，例如Ejfakdjf100001kjkfajsf0
    // MD5燕颜值加密   替换掉原来的方法
    return new SimpleAuthenticationInfo("",user.getPwd(),"");
}
```

这样我们就实现了用户的登录认证，在登录认证这里，有需要MD5DE ,可以去研究过研究

### 7.6 shiro实现授权

1 > 实现对自定义权限的拦截 ，对于授权的拦截形式, 我们在ShiroConfig的getShiroFilterFactoryBean（）方法中进行校验拦截

```java
@Bean
public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
    ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
    bean.setSecurityManager(securityManager);
    Map<String, String> filterMap =  new LinkedMap();
    filterMap.put("/user/*","authc");
    //授权的操作
    filterMap.put("/user/add", "perms[user:add]"); //这代表只有用户带有[user:add]权限才能访问
    bean.setFilterChainDefinitionMap(filterMap);
    //如果没权限就跳入到登录页面
    bean.setLoginUrl("/toLogin");
    return bean;
}
```

2 > 授予用户自定义权限

step 1、 首先，在数据库中，需要用户具有权限的记录，例如

![image-20210215171704288](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210215171704288.png)

step 2、然后，我们到自定义的UserRealm中，在doGetAuthorizationInfo（）方法中，对登录的用户进行授权

```java
@Override
protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
    System.out.println("执行了=>授权doGetAuthorizationInfo()");
    // 授权的类
    SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
    // info.addStringPermission("user:add"); //测试代码，给所有登录用户授予 [user:add]权限， 如果多个权限就是 [user:add|user:update]
    // step 1 拿到当前登录的对象
    Subject subject = SecurityUtils.getSubject();
    //因为Authentication里面放了Principal
    User currentUser = (User)subject.getPrincipal();
    //step 2 设置当前用户的权限，从数据库中读取当前用户的权限
    info.addStringPermission(currentUser.getPerms());
    // return AuthorizationInfo
    return info;
}
```

step 3、在UserRealm，在doGetAuthenticationInfo()方法中，我们将登录的用户传入到Principal中

```java
@Autowired
UserServiceImpl userService;
@Override
protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
    System.out.println("执行了=>认证doGetAuthenticationInfo()");
    //案例 2 连接真实的数据库
    UsernamePasswordToken userToken = (UsernamePasswordToken)token;
    User user = userService.getUserByName(userToken.getUsername());
    if (user== null){//没有这个用户
        return null; //返回return null,前面的subject.login(userToken)就会抛出一个UnknownAccountException
    }
    //密码的认证，shiro自己做
    //可以加密，例如Ejfakdjf100001kjkfajsf0
    // MD5燕颜值加密   替换掉原来的方法
    //参数1 @principal 当前要放入到principal的资源
    //参数2 @credentials 加密的密码
    //参数3 @ealmName
    return new SimpleAuthenticationInfo(user,user.getPwd(),"");
}
```

step 4、然后我们再进入到ShiroConfig的getShiroFilterFactoryBean（）方法中，添加对自定义授权的拦截

```java
@Bean
public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
    ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
    //设置安全管理器
    bean.setSecurityManager(securityManager);
    // shiro功能1 : 通过添加shiro的内置过滤器实现登录的拦截
    /**
     * anon  无需认证就可以访问
     * authc 必须认证了才能访问
     * user  必须拥有记住我的功能才能访问
     * perms： 拥有对某个资源的权限才能访问
     * role  拥有某个角色的权限才能访问
     */
    Map<String, String> filterMap =  new LinkedMap();
    //filterMap.put("/user/add","anon"); //无需认真就可以访问
    //filterMap.put("/user/update","authc"); //必须认证了才能访问
    
    //授权的操作，正常情况下，未授权的请求会跳转到未授权页面
    filterMap.put("/user/add", "perms[user:add]"); //这代表只有用户带有[user:add]权限才能访问
    filterMap.put("/user/update", "perms[user:update]");

    filterMap.put("/user/*","authc");
    bean.setFilterChainDefinitionMap(filterMap);
    //如果没权限就跳入到登录页面
    bean.setLoginUrl("/toLogin");
    // 如果没有授权请求，跳入这个页面
    bean.setUnauthorizedUrl("/noAuthor");
    return bean;
}
```

说明:

bean.setUnauthorizedUrl("/noAuthor");  这句话表示权限验证未通过的用户都会跳到这个地方



### 7.7 shiro和thymeleaf整合

step 1 、整合依赖包

```xml
<!--shiro和thymeleaf整合-->
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.0.0</version>
</dependency>
```

step 2、 到ShiroConfig里面，配置shrioThymeleaf的整合

```java
    //整合shiroDialect 用来整合shiro和thymeleaf
    @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
```

step 3、这样我们在前端就可以使用了

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="https://www.thymeleaf.org/"
      xmlns:shiro="https://www.pollix.at/thymeleaf/shiro">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    <h1>首页</h1>
    <p th:text="${msg}"></p>
    
    <p><a th:href="@{/toLogin}">登录</a></p>
    <hr>
    <!--假设用户有 user:add权限就显示-->
    <div shiro:hasPermission="user:add">
        <a th:href="@{/user/add}">add</a> |
    </div>
    <!--假设用户有 user:update权限就显示-->
    <div shiro:hasPermission="user:update">
        <a th:href="@{/user/update}">update</a>
    </div>

    <hr>
    <a th:href="@{/logout}">退出登录</a>
</body>
</html>
```



step 4、 我们对于登录按钮，需要登录用户不显示登录，未登录用户显示登录，这个实现方式如下

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="https://www.thymeleaf.org/"
      xmlns:shiro="http://www.pollix.at/thymeleaf/shiro">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    <h1>首页</h1>
    <p th:text="${msg}"></p>
    <!--需求1 ： 对于未登录的用户，显示登录按钮 shiro:notAuthenticated   shiro:Authenticated -->
    <div shiro:notAuthenticated>
        <p><a th:href="@{/toLogin}">登录</a></p>
    </div>


    <hr>
    <!--需求2 假设用户有 user:add权限就显示-->
    <div shiro:hasPermission="user:add">
        <a th:href="@{/user/add}">add</a> |
    </div>
    <!--需求3 假设用户有 user:update权限就显示-->
    <div shiro:hasPermission="user:update">
        <a th:href="@{/user/update}">update</a>
    </div>

    <hr>
    <!--需求4 : 登录的用户，显示退出登录按钮 如果当前session有loginUser就显示退出登录-->
    <div th:if="${session.loginUser}!=null">
        <a th:href="@{/logout}">退出登录</a>
    </div>

</body>
</html>
```



step 5、 为了支持这个功能，我们需要在用户登录成功后，把user放到session里面，进入到MyController的login方法，修改如下

```java
 @RequestMapping("/login")
    public String login(String username, String password, Model model){
        //1 先获取Subject
        Subject subject = SecurityUtils.getSubject();
        //2 .封装用户的登录数据, 获取令牌
        UsernamePasswordToken userToken = new UsernamePasswordToken(username, password);
        //3 . 获取登录
        try {
            //执行登录的方法，如果么有异常就说明登录成功了
            subject.login(userToken);
            // ++++++++++++++++从principal里面获取user并存入到session
            User user = (User) subject.getPrincipal();
            Session session = subject.getSession();
            session.setAttribute("loginUser",user);

            return "index";
        } catch (UnknownAccountException uae) { //用户名不存在
            System.out.println(">>>用户名不存在!");
            model.addAttribute("msg","用户名不存在!");
            return "login";
        } catch (IncorrectCredentialsException ice) { //密码不对
            model.addAttribute("msg","密码错误!");
            return "login";
        } catch (LockedAccountException lae) { //用户账户被锁定
            model.addAttribute("msg","用户账户被锁定");
            return "login";
        }catch (AuthenticationException e) {
            model.addAttribute("msg","登录异常错误，请检查!");
            return "login";
        }

    }
```



## 8. Swarger 使用和配置



学习目标

- 了解swarger的作用
- 了解前后端分离
- 在springboot中集成Swagger

Swarger简介

前后端分离，

Vue + SpringBoot 前后端分离技术栈



后端时间： 前端只用管理静态页面，写html， css,	js， 后端通过模板引擎，例如jsp, 后端是主力, 早期前端很弱!

前后端分离式时代：

	- 先把后端分离  后端控制层，后端服务处，后端数据访问层  【后端团队】
 - 前端也分离了   前端控制层(双向绑定) 视图层(html)   [前端团队]
   	- 伪造后端数据， json，在前端写的时候，已经存在了，不需要后端，前端工程依旧能够跑起来
	- 前后端如何交互 ===>API
	- 前后端相互独立，松耦合，甚至可以部署在不同的服务器上



这样容易产生一个问题

- 前后端集成联调，前端人员和后端人员无法做到及时协商，所以就争取尽早解决，导致问题集中爆发，工程延期；

解决方案:

	- 首先指定schema[计划的提纲]， 实时更新最新的API，降低集成的风险，
	- 早些年，编写word文档，定义接口标准，按照word版本进行管理
 - 前后端分离，
   	- 前端测试后端接口，使用postman或者idea的httpClient
      	- 后端提供接口 需要实时更新最新的消息及改动



Swagger

- 号称世界上最流行的api框架
- RestFul Api文档在线自动生成工具=》Api文档与API的定义同步更新
- 可以直接运行，可以在线测试API接口，就是我们的Controller
- 支持多种语言，(Java,Php.....)

官网：

https://swagger.io



### 8.1 springboot集成swagger

在spring中也可以集成swagger, 在项目使用swagger需要springbox；

- swagger2
- ui

step1 、新建一个springboot-web项目, 

step2、导入相关依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>

```

step 3 编写一个HelloWorld工程

```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hell(){
        return "Hello World!";
    }
}
```

step 4、 application.yaml配置swagger

```yaml
# ===== 自定义swagger配置 ===== #
swagger:
  enable: true
  application-version: 1.0
  application-description: springfox swagger 3.0整合Demo
  try-host: http://localhost:${server.port}
```



Step5 、在idea的kuang/包下，新建一个config包，在这个包里面创建一个SwaggerConfiguration的java配置类，用来配置swagger

```java
@Configuration	// 表示这是一个Java配置类
@EnableOpenApi	
public class SwaggerConfiguration {

}
这样就可以使用默认值开始使用了
```

测试运行，访问http：//localhost:8080/swagger-ui/index.html

![image-20210216005112825](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210216005112825.png)

`备注`:  一个很好的博客地址  [Spring Boot 整合 springfox-swagger 3.0.0](https://blog.csdn.net/wangzhihao1994/article/details/108408420)

### 8.2 配置swagger 的ApiInfo

Swagger的Bean实例 Docket，我们对swagger的相关功能，都是通过这个Docket实现

```java
@Configuration
@EnableOpenApi
public class SwaggerConfiguration {

    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo());
    }
    //配置Swagger信息  apiinfo
    private ApiInfo apiInfo(){
        //作者信息
        Contact contact = new Contact("秦僵", "http://www.aigoo.top", "1642302522@qq.com");
        return new ApiInfo(
            "狂神的SwaggerApi文档", //对应的title
            "即使再小的帆也能远航!",	//对应签名
            "v 1.0.0",	//对应版本
            "http://www.aigoo.top",	//对应Terms of service
            contact,	// 联系人相关信息
            "Apache 2.0",
            "http://www.apache.org/licenses/LICENSE-2.0",
            new ArrayList());
    }

}

}
```

运行后截图

![image-20210216010702645](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210216010702645.png)

### 8.3 Swagger 配置功能使用

`1、实现多个组，可以在SwaggerConfiguration里，创建多个bean, 使用不同groupName()`

```java
@Bean
public Docket docketSanfeng(){
    return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo()).groupName("罗三疯");
}
@Bean
public Docket docketSifeng(){
    return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo()).groupName("罗四疯");
}
@Bean
public Docket docketWufeng(){
    return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo()).groupName("罗五疯");
}
@Bean
public Docket docketLiufeng(){
    return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo()).groupName("罗六疯");
}
```



`4、paths() 起到过滤的作用，可以过滤掉不希望被扫描到的包`  

paths(PathSelectors.ant("/kuang/**"))//过滤框下面的接口

paths(PathSelectors.any()) //所有的都过滤

paths(PathSelectors.none())//所有的都不过滤

paths(PathSelectors.regex()) //正则表达式过滤



```java
@Bean
public Docket docketSanfeng(){
    return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo()).groupName("罗三疯")
            .select()
            //RequestHandler 配置要扫描接口的方式
            //basePackage  扫描指定包
            //any() 扫描任何包
            // none() 不扫描
            //withClassAnnotation(RestController.class))
            .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
            .apis(RequestHandlerSelectors.withClassAnnotation(RestController.class)) //扫描带有RestController注解的
            .apis(RequestHandlerSelectors.withMethodAnnotation(GetMapping.class)) //扫描带有GetMapping注解的
            /*
              paths()  过滤功能
              paths(PathSelectors.ant("/kuang/**"))//过滤框下面的接口
              paths(PathSelectors.any()) //所有的都过滤
              paths(PathSelectors.none())//所有的都不过滤
              paths(PathSelectors.regex()) //正则表达式过滤
             */
            .paths(PathSelectors.ant("/kuang/**"))//过滤框下面的接口
            .paths(PathSelectors.any()) //所有的都过滤
            .paths(PathSelectors.none())//所有的都不过滤
            .build();
}
```

`5、扫描接口 select().api().build() 这样就可以添加扫描指定的文件包 `

支持的有  

basePackage()  扫描指定包  

any() 扫描任何包   

none() 不扫描

withClassAnnotation(RestController.class) //扫描带有RestController注解的

withMethodAnnotation(GetMapping.class)) //扫描带有GetMapping注解的

```java
// 1. 配置扫描com.kuang.swagger.controller
    @Bean
    public Docket docketSanfeng(){
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo()).groupName("罗三疯")
                .select()
                //RequestHandler 配置要扫描接口的方式
                //basePackage  扫描指定包
                //any() 扫描任何包
                // none() 不扫描
                .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
                .apis(RequestHandlerSelectors.withClassAnnotation(RestController.class)) //扫描带有RestController注解的
                .apis(RequestHandlerSelectors.withMethodAnnotation(GetMapping.class)) //扫描带有GetMapping注解的
                .build();
    }
```

### 8.4 根据环境开/关swagger

enable(boolean b)  通过这个可以关闭当前swagger

 需求分析：

1、可以在docketSanfeng()方法放入Environment environment就能实现获取环境变量，

2、Profiles profiles = Profiles.of("dev","test");可以获取到当前环境配置中是否为dev 或者test的实体

完整的示例代码如下

```java
 @Bean
    public Docket docketSanfeng(Environment environment){
        // 返回一个环境变量的实例, 设置要显示的swagger环境
        Profiles profiles = Profiles.of("dev","test");
        //通过Environment.acceptsProfiles(profiles)判断是否处在自己设定的环境当中
        boolean flag = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
                .groupName("罗三疯")
                .enable(flag) //enable 是否启动swagger, false 不启动
                .select()
                //RequestHandler 配置要扫描接口的方式
                //basePackage  扫描指定包
                //any() 扫描任何包
                // none() 不扫描
                //withClassAnnotation(RestController.class))
                .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
                .apis(RequestHandlerSelectors.withClassAnnotation(RestController.class)) //扫描带有RestController注解的
                .apis(RequestHandlerSelectors.withMethodAnnotation(GetMapping.class)) //扫描带有GetMapping注解的
                /*
                  paths()  过滤功能
                  paths(PathSelectors.ant("/kuang/**"))//过滤框下面的接口
                  paths(PathSelectors.any()) //所有的都过滤
                  paths(PathSelectors.none())//所有的都不过滤
                  paths(PathSelectors.regex()) //正则表达式过滤
                 */
                .paths(PathSelectors.ant("/kuang/**"))//过滤框下面的接口
                .paths(PathSelectors.any()) //所有的都过滤
                .paths(PathSelectors.none())//所有的都不过滤
                .build();
    }
```



### 8.5 实体类配置和扫描

step 1、准备环境: 在F:\code\srpingboot-study\springboot-07-swagger项目建立实体类  User

step 2、 进入到被扫描的包 com.kuang.swagger.controller，在里面新增加一个接口, 这个接口的返回值存在User实体类，那么这个User就会被Swagger扫描到

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello(){
        return "Hello World!";
    }

    // 只要接口中，返回值中存在实体类，就会被swagger扫描到
    @PostMapping("/user")
    public User user(){
        return new User();
    }
}
```



### 8.6 加注释的Annotation

```
@Api("用户类")
@ApiModel("用户实体类")  //给实体类User加中文注释

 @ApiModelProperty("用户名")  // 给字段加上注释
    public String username;
    
@ApiOperation("hello控制类")	//

public String hello(@ApiParam("用户名") String username){   // 给参数加上注释


```



### 8.7 swagger执行测试





## 9 分布式

### .0 优秀博客内容

[1-RPC理论及入门](https://blog.csdn.net/KingCat666/article/details/78577079?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

[2-RPC框架与Dubbo使用](https://blog.csdn.net/u010297957/article/details/51702076)





### 9.1 同步异步任务

- 异步任务
- 定时任务
- 邮件发送  

上面三个任务时必须要会的



什么叫做异步任务？

一般按钮一点击，立刻就有响应，就是异步任务! 此时前端显示是已经完成，但后端邮件还在继续发!

使用多线程模拟一种异步任务状态？ 下面我们来搭建一个环境

step1 、在一个springboot的web项目中，新建一个com.kuang.service的包，并增加一个服务

```java
@Service
public class AsyncService {
    public void hello(){
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("数据正在处理，3s....");
    }
}
```

step 2、 我们新建一个com.kuang.controller，并增加一个Controller

```java
@RestController
public class AsyncController {
    @Autowired
    AsyncService asyncService;
    @RequestMapping("/hello")
    public String hello(){
        asyncService.hello(); //调用服务的休息3s
        return "3s已经到了 ，ok!";
    }
}
```



经过这个环境，我们可以发现，当前端访问的时候，页面会有一个3s转圈的过程，然后才能进行显示，这在用户使用上很影响使用，因此，提出了异步任务的的需求!



如何在springboot项目开启异步功能？

step1 、在springboot主启动类，添加注解@EnableAsync

```java
@EnableAsync
@SpringBootApplication
public class Springboot08TaskApplication {
```

step 2、在需要进行异步操作的接口上，使用@Async 注解，告诉Spring这是一个异步方法

```java
//告诉Spring这是一个异步的方法
@Async
public void hello(){
    try {
        Thread.sleep(3000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println("数据正在处理，3s....");
}
```

这样我们再去进行访问，就不会出现这种情况了

### 9.2 邮件任务



step 1、 在项目依赖，增加javamail的启动器

```xml
<!--javax.mail-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```



step 2、在springboot中，mail的自动配置为MailSenderAutoConfiguration， 在里面，可以发现@EnableConfigurationProperties(MailProperties.class)， 我们可以点进MailProperties，在这里我们可以发现，加配置为@ConfigurationProperties(prefix = "spring.mail")，所以我们可以进入application.properties中增加相关的配置

```properties
spring.mail.host=smtp.aliyun.com
spring.mail.username=luojingen@aliyun.com
spring.mail.password=luo1381026
```

这样我们就配置好了我们发送配置

step 3、在MailSenderAutoConfiguration，还导入一个类MailSenderJndiConfiguration，这个类就是发送类，里面JavaMailSenderImpl mailSender(Session session) 是已经注册好的bean，所以我们可以直接注入使用, 接下来编写发送类，先编写一个发送简单邮件的示例代码

```java
@SpringBootTest
class Springboot08TaskApplicationTests {
    @Autowired
    JavaMailSenderImpl mailSender;
    @Test
    void contextLoads() {
        SimpleMailMessage simpleMailMessage = new SimpleMailMessage();
        simpleMailMessage.setTo("1642302522@qq.com");   //设置发送给谁
        simpleMailMessage.setSubject("小狂神最近在讲新的课程");    //设置邮寄标题
        simpleMailMessage.setText("谢谢你的回顾，谢谢你的狂神说JAVA系列课程");    //设置邮寄内容
        simpleMailMessage.setFrom("luojingen@aliyun.com");  //设置发件人
        mailSender.send(simpleMailMessage);
    }

}
```

这样就实现了发送简单邮件功能

step 4 我们还可以发送富文本邮件，使用方式略有不同，

```java
@Test
void sendMineMessage() throws MessagingException {
    // 创建复杂邮件 方法1
    MimeMessage mimeMessage = mailSender.createMimeMessage();
    // 组装富文本邮件, 开启true表示是带有附件的邮件
    MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage,true);
    mimeMessageHelper.setTo("1642302522@qq.com");
    mimeMessageHelper.setSubject("小狂神的课程!");
    mimeMessageHelper.setFrom("woodme0000@163.com");
    //带有true表示这是一个富文本邮件
    mimeMessageHelper.setText("<p style='color:red'><a href='http://www.sina.com'> 我是小狂神JAVA课程</a></b>", true);
    //添加附件
    mimeMessageHelper.addAttachment("1.jpg",new File("F:\\code\\srpingboot-study\\springboot-08-task\\src\\main\\resources\\static\\1.jpg"));
    mimeMessageHelper.addAttachment("2.jpg",new File("F:\\code\\srpingboot-study\\springboot-08-task\\src\\main\\resources\\static\\1.jpg"));

    mailSender.send(mimeMessage);
}
```

这样我们就实现了发邮件的任务了，之前我们在javaweb，利用原生的，写一封发邮件，搞了接近半天，

为了以后使用，我们还可以抽成工具，如下

```java
//封装成方法

/**
     * @param html  是否是html文档
     * @param subject  邮件的主题
     * @param  text  邮件的正文
     * @throws MessagingException
     */
public void sendMail(Boolean html,String subject, String text) throws MessagingException {
    // 创建复杂邮件 方法1
    MimeMessage mimeMessage = mailSender.createMimeMessage();
    // 组装富文本邮件, 开启true表示是带有附件的邮件
    MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage,html);
    mimeMessageHelper.setTo("1642302522@qq.com");
    mimeMessageHelper.setSubject(subject);
    mimeMessageHelper.setFrom("woodme0000@163.com");
    //带有true表示这是一个富文本邮件
    mimeMessageHelper.setText(text, true);
    //添加附件
    mimeMessageHelper.addAttachment("1.jpg",new File("F:\\code\\srpingboot-study\\springboot-08-task\\src\\main\\resources\\static\\1.jpg"));
    mimeMessageHelper.addAttachment("2.jpg",new File("F:\\code\\srpingboot-study\\springboot-08-task\\src\\main\\resources\\static\\1.jpg"));

    mailSender.send(mimeMessage);
}
```

### 9.3 定时任务

两个重要接口

1、TaskExecutor（org.springframework.core.task.TaskExecutor） 任务执行，

2、TaskScheduler  (org.springframework.scheduling.TaskScheduler) 任务调度

两个重要的注解

@EnableScheduling   //开启定时功能的注解

@Scheduled  //表示什么时候执行这个注解



掌握一个知识点 :cron表达式

示例功能： 我们想要一个特定时间，开启一个打印功能服务

step 1、到主启动类，添加@EnableScheduling 开启定时功能注解

```java
@EnableAsync    //开启异步功能的注解
@EnableScheduling   //开启定时功能的注解
@SpringBootApplication
public class Springboot08TaskApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot08TaskApplication.class, args);
    }

}
```

step 2、到定时任务中，我们添加@Scheduled

```java
@Service

public class ScheduledService {
    // 我们想让系统在一个特定的时间执行这个方法

    /**
     * cron 表达式
     * 秒 分 时 日 月 周几
     * cron = "30 34 18 * * 0-7  周一到周日的每一天的 18:34:30秒 执行一次
     * cron = "0 * * * * 0-7  周一到周日的每一天的 18:34:30秒  执行一次
     * cron = "30 44 18 * * ?   ?代表周一都周七  每天的18:44:30 执行一次
     * cron = "30 0/5 10,18 * * ?"  每天的10点，18点，每隔5分钟 执行一次
     * cron = "30 15 10 ？ * 1-6"   每个月周一到周六，10:15:30秒 执行一次
     */
    @Scheduled(cron = "0 43 18 * * 0-7")
    public void hello(){
        System.out.println(">>>你被执行了...");
    }
}
```



cron表达式的知识点，表达式很多，可以自己去百度搜索查查：

```
/**
 * cron 表达式
 * 秒 分 时 日 月 周几
 * cron = "30 34 18 * * 0-7  周一到周日的每一天的 18:34:30秒 执行一次
 * cron = "0 * * * * 0-7  周一到周日的每一天的 18:34:30秒  执行一次
 * cron = "30 44 18 * * ?   ?代表周一都周七  每天的18:44:30 执行一次
 * cron = "30 0/5 10,18 * * ?"  每天的10点，18点，每隔5分钟 执行一次
 * cron = "30 15 10 ？ * 1-6"   每个月周一到周六，10:15:30秒 执行一次
 */
```



### 9.4 分布式理论

分布式Dubbo+zookeeper

什么是分布式？

在《分布式系统原理与泛型》一书中有如下定义"分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统";

分布式系统是由一组通过网络进行通信，为了完成共同的任务而协调工作的计算机节点组成的系统，分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务，其目的是利用更多的机器，处理更多的数据。

分布式系统(Distrubuted system) 是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升(如加内存，加磁盘，使用更好的cpu)高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为分布式系统要解决的问题本身就和单机系统一样的，但由于分布式系统的多借点、通过网络通信的拓扑结构，会引入单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。

Dubbo文档

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用框架已经无法应对，分布式服务加购以及流动计算架构势在必行，急需一个治理系统确保架构有条不紊的演进。

在Dubbo的官网文档有这样一张图

![image](https://dubbo.apache.org/imgs/user/dubbo-architecture-roadmap.jpg)

 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，提升效率的方法之一是将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的分布式服务框架(RPC)是关键。

流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于提高机器利用率的资源调度和治理中心(SOA)是关键。

### 9.5 RPC理论

什么是RPC？

RPC[Remote Procedure Call0] 是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范，他允许程序调用另一个地址空间(通常是共享网络的另一台机器)的过程或者函数，而不用程序员显示编码这个过程调用的细节，既程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同!

也就是说两台服务器A,B, 一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据，为什么要用PRC呢，就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不通过的系统间的通讯，需要不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用，RPC就是要向调用本地的函数一样去调用远程的函数

[推荐阅读文章] (https://www.jianshu.com/p.2accc2840a1b)

[RPC基本原理] (https://blog.csdn.net/zkp_java/article/details/81879577)

[RPC入门总结(一)](https://blog.csdn.net/KingCat666/article/details/78577079?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)



![img](https://img-blog.csdnimg.cn/20190524101100997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3c4OTk4MDM2,size_16,color_FFFFFF,t_70)

PRC两个核心的模块  通信，序列化

序列化： 数据传输需要进行转换， 

可以去学习一下Netty ，大约需要30天，是Dubbo的底层

**Dubbo了解**

Dubbo实现了RPC的思想和原理，~ 18年重启，Dubbo 3.0 RPC



### 9.6 Dubbo和Zookeeper安装使用

什么是Dubbo?

Apache Dubbo 是一款高性能、轻量级的开源javaRPC框架，他提供三大核心能力，面向接口的远程方法调用，智能容错和负载均衡，以及服务的自动注册和发现

[dubbo官网](https://dubbo.apache.org/zh/)

1、了解dubbo的特性

2、查看官方文档



dubbo的架构和基本概念

![image-20210217214917336](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217214917336.png)

| **`Provider**` | **暴露服务的服务提供方**               |
| -------------- | -------------------------------------- |
| `Consumer`     | 调用远程服务的服务消费方               |
| `Registry`     | 服务注册与发现的注册中心               |
| `Monitor`      | 统计服务的调用次数和调用时间的监控中心 |
| Container`     | 服务运行容器                           |

invoke 调用 execute 执行

**1. 安装zookeeper**



1、下载zookeeper ：http://mirror.bit.edu.cn/apache/zookeeper/  我们下载的是apache-zookeeper-3.5.9

2、运行 /bin/zkServer.cmd, 初次运行会报错，可能闪退

​	解决办法：编辑zkServer.cmd,在文件末尾，添加pause , 这样运行出错就不会退出，会提示错误信息，方便找到原因。

![image-20210217220658006](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217220658006.png)

![image-20210217220726799](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217220726799.png)



3、我们到zookeeper/conf/里面拷贝一份zoo.cfg，这样就可以了，zookeeper默认的端口是2181，修改zoo.cfg配置文件

即可，注意几个重要位置

dataDir=./  临时数据存储的目录  (可以写相对路径)

clientPort = 2181   zookeeper的端口号

修改完再次启动zookeeper即可

![image-20210217222336980](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217222336980.png)

测试，我们可以在/bin/下面，启动zkCli.cmd ，就可以启动测试了，要保证服务是开启的

![image-20210217222658978](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217222658978.png)

zookeeper本质就是RPC中的 注册服务中心



**2. windows下安装dubbo-admin**

dubbo本身并不是一个服务软件，他其实就是一个jar包，能够帮助你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。

但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。

我们这里来安装一下

1、下载dubbo-admin

地址: https://github.com/apache/dubbo-admin/tree/master

2、解压进入目录

修改dubbo-admin\src\mail\resources  \application.properties 指定zookeeper的地址

```properties
server.port=7001
spring.velocity.cache=false
spring.velocity.charset=UTF-8
spring.velocity.layout-url=/templates/default.vm
spring.messages.fallback-to-system-locale=false
spring.messages.basename=i18n/message
spring.root.password=root
spring.guest.password=guest
# 注册中心的地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

3、因为这是一个maven项目，我们在项目目录下，打包dubbo-admin

```
mvn clean package -Dmaven.test.skip=true
```

-clean 先把里面旧的清除掉

-package 执行打包

-Dmaven.test.skip =true 打包不执行测试

第一次打包有点慢，需要耐心等待，直到成功!

![image-20210217224933176](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217224933176.png)

4、执行 F:\Environment\dubbo-admin-master\dubbo-admin\target>java -jar dubbo-admin-0.0.1-SNAPSHOT.jar

![image-20210217225240888](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217225240888.png)

注意，zookeeper的服务一定要启动着，不然dubbo-admin会启动失败的

执行完毕，我们去访问一下http://localhost:7001，使用root/root 就可以登录

登录成功后，查看界面

![image-20210217225530486](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217225530486.png)



dubbo-admin就是一个监控管理后台，可以查看我们注册了哪些服务，哪些服务被消费了

zookeeper 是一个注册中心

dubbo 是一个jar包，

**3、整合dubbo**

step1 、先准备环境，我们需要创建一个maven父项目，在maven父项目中，创建两个springboot子项目，分别为

springboot-09-dubbo-providerserver -扮演RPC中的Provider 服务提供者角色

springboot-09-dubbo-consumerserver -扮演RPC中的Consumer 消费者角色

紧接着，将服务提供者项目的端口改为8001， 将服务消费者项目的端口改为8002



在springboot-09-dubbo-providerserver 项目中，我们创建一个简单的买票服务接口和实现，如下：

```java
package com.kuang.service;

/**
 * Created by Josh luo on 2021/2/17
 */
public interface TicketService {
    public String getTicket();
}

public class TicketServiceImpl implements TicketService {
    @Override
    public String getTicket() {
        return "《狂神说Java》";
    }
}
```

在springboot-09-dubbo-consumerserver，我们创建一个简单的服务消费者接口和实现，如下

```java
package com.kuang.service; /**
 * Created by Josh luo on 2021/2/17
 */

/**
 * Created by jin
 */
public interface UserService {
    //想拿到provider提供的票

}
```



step2 、在springboot-09-dubbo-providerserver，导入dubbo的启动器依赖

```xml
<!--导入dubbo依赖+zookeeper-->
<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.8</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
<!--引入zookeeper-->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.14</version>
    <!--排除这个slf4j-logj12-->
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

[新版本的坑]导入zookeeper及其依赖包，解决日志冲入，还需要提出日志依赖，需要排除slf4j-log4j12这个包

step 3、 接下来我们要开始配置在springboot-09-dubbo-providerserver配置，并将服务注册到zookeeper，

在application.properties中

```properties
server.port=8001
# 注册
# 服务应用名字,代表当前服务名字
dubbo.application.name=ticket-provider-server
#服务中心地址, 我们使用的是zookeeper注册中心
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 哪些服务要被注册,就是一个扫描包，扫描哪些被注册
# 代表 service包下面的所有包都被注册到服务中心
dubbo.scan.base-packages=com.kuang.service
```

然后到com.kuang.service. TicketServiceImpl,增加@DubboService注解

```java
/**
 * Created by Josh luo on 2021/2/17
 */
//zookeeper  服务注册与发现
@Service
@DubboService   //可以被dubbo扫描到，在项目一启动就会自动注册到注册中心
public class TicketServiceImpl implements TicketService {
    @Override
    public String getTicket() {
        return "《狂神说Java》";
    }
}
```

这样，我们只要一起动springboot-09-dubbo-providerserver项目，就可以在dubbo-admin这个监控页面，看到应用的注册信息

![image-20210217234038896](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210217234038896.png)



step 4、接下来就是让我们的消费者可以进行到服务中心进行消费的实现

我们也是先导入相关的依赖，和springboot-09-dubbo-providerserver一样

```xml
<!--导入dubbo依赖+zookeeper-->
<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.8</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
<!--引入zookeeper-->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>2.12.0</version>
</dependency>
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.14</version>
    <!--排除这个slf4j-logj12-->
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

首先，我们要在application.properties配置，我们从哪个服务中心获取服务

```properties
server.port=8002
# 消费者要去哪里拿服务？还需要知道是谁拿，所以得暴露自己名字
# 服务应用名字,代表当前服务名字
dubbo.application.name=ticket-comsumer-server

#服务中心地址, 我们使用的是zookeeper注册中心
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

然后，我们要开始使用，首先要去注册中心拿到我们需要的服务

step 2、为了拿到提供者提供的TicketService，我们需要在消费者项目里面也建立一个和服务者项目一样的TicketService接口

```
public interface TicketService {
    public String getTicket();
}
```

step3、在springboot-09-dubbo-consumerserver消费者项目中，我们完善UserService的类

```java
package com.kuang.service; /**
 * Created by Josh luo on 2021/2/17
 */

import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.stereotype.Service;

/**
 * Created by jin
 */
@Service //这里是放入到spring容器中
public class UserService {
    //想拿到provider提供的票
    //想拿到provider-server提供的票，我们就要去注册中心拿到服务
    @DubboReference //引用
    /**
     * 因为 TicketService在当前消费者项目没有
     * 方法1、正常开发我们引入pom坐标
     * 方法2、可以定义在路径相同的接口名
     */
    TicketService ticketService;
    //定义一个买票接口
    public void buyTicket(){
        String ticket = ticketService.getTicket();
        System.out.println("在注册中心拿到票===>"+ticket);
    }

}
```

step 4、 这样，我们可以去测试一下了

```java
@SpringBootTest
class Springboot09DubboConsumerserverApplicationTests {
    @Autowired
    UserService userService;
    @Test
    void contextLoads() {
        userService.buyTicket();
    }

}
```

测试结果：

![image-20210218001655008](F:\MD格式学习笔记库\狂神JAVA-18-SpringBoot学习.assets\image-20210218001655008.png)



总结步骤：

准备工作： zookeeper服务要一直开启着!



1、提供者提供服务

​	1、导入依赖

​	2、配置注册中心的地址，以及服务发现名和要扫描的包~

​	3、在想要被注册的服务上，增加@DubboService

​	4、启动项目

2、消费者如何消费

​	1、导入依赖

​	2、配置注册中心地址以及配置自己的服务名

​	3、从远程注入服务@DubboReference

​	4、一定要注意，提供者的com.kuang.service.TicketService一定要和消费者包路径一样



## 10.springboot整合radis





## 11. 聊聊现在和未来





