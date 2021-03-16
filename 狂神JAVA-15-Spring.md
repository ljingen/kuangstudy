# Spring Frame work5学习

## 1.11 简介

### 1.1 spring历史

Spring 春天---给软件行业带来了春天!

2002年，首次推出了Spring框架的雏形 interface21

Spring框架是以interface21框架为基础，经过重新设计，并不断丰富其内涵，与2004年3月24日，发布1.0

Rod Johnson  SpringFramework的创始人!

目的：解决企业开发的复杂性， 使现有的技术更加容易使用! 本身是一个大杂烩，整合了现有的技术框架!



SSH  Struct2+Spring+Hibernate

SSM  SpringMVC+Spring+ Mybatis

官网地址：https://spring.io/projects/spring-framework

官方下载地址：https://repo.spring.io/release/org/springframework/spring/

 GitHub地址：https://github.com/spring-projects/spring-framework

maven的下载地址

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>


<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>


```

### 1.2 优点

spring是一个开源的免费的框架(容器)，他是一个轻量级的非入侵式的框架!

控制反转(IOC)  什么是控制反转？ 面试必问

面向切面编程(AOP)  什么是面向切面编程? 面试必问

支持事务的处理，对框架整合的支持!



总结一句话：SPring是一个轻量级的控制反转(IOC)和面向切面编程(AOP）的框架!

### 1.3 组成 

![image-20201217094947259](F:\MD格式学习笔记库\狂神JAVA-14-Spring.assets\image-20201217094947259.png)

### 1.4 拓展

Spring Boot 构建一切

Spring Cloud  协调一切

SpringCloud Data Flow 连接一切





Spring Boot:  一个快速开发的脚手架  基于SpringBoot可以快速的开发单个微服务

Spring Cloud : 基于SpringBoot实现的, 对微服务的整合和协调

因为现在大多数公司，都在使用Spring Boot在进行快速开发! 学习SpringBoot的前提，需要完全掌握Spring及SpringMVC

Spring 和SpringMVC 起到承上启下作用!

弊端：发展了太久之后，违背了原来的理念! 配置十分的繁琐! 人称 "配置地狱!" 直到SpringBoot能得到部分解放! 



## 2.Ioc理论的推导

### 2.1 一个ioc原型例子

原来写一个业务

​	UserDao接口

```java

public interface UserDao {
    void getUser();
}
```



​	UserDaoImpl实现类

```java
//UserDaoImpl
public class UserDaoImpl implements UserDao {
    public void getUser() {
        System.out.println("userDaoImpl被调用了!");
    }
}

//MysqlUserDaoImpl
public class MysqlUserDaoImpl implements UserDao {
    public void getUser() {
        System.out.println("mysql userDaoImpl被调用了!");
    }
}
//OrcaleUserDaoImpl
public class OrcaleUserDaoImpl implements UserDao {
    public void getUser() {
        System.out.println("Orcale userDaoImpl被调用了!");
    }
}

```



UserService业务接口

```java
public interface UserService {
    void getUser();
}
```



UserServiceImpl业务实现类

```java 
public class UserServiceImpl implements UserService {

    private UserDao userDao = new UserDaoImpl();
    
    public void getUser() {
        userDao.getUser();
    }
}

```

Test测试方法:

```java
@Test
public void testDao(){
    UserServiceImpl userService = new UserServiceImpl();
    userService.getUser();
}
```

**由此产生一个问题, 在我们业务中，用户的需求可能会影响我们原来的代码，例如，上述例子，我们做完了UserDaoImpl，UserDao 又增加多个类实现了他的接口，但是接下来要求做 MysqlUserDaoImpl的拓展，就需要我们重新修改源码.**



我们需要根据用户的需求去修改源代码，如果程序代码量十分大，修改一次的代价非常大! 



所以，我们在UserService里面做一个setter注入，实现动态的接口注入，这样已经发生了革命性的变化!

```java
public class UserServiceImpl implements UserService{
    ...
    private UserDao userDao;
    //利用set进行动态实现值的注入!
    public void setUserDao(Userdao userDao){
        this.userDao = userDao;
    }
    ...
}
```

之前程序时主动创建对象，控制权在程序员手上! 所以程序每次改动都需要程序员改动代码!

使用了set注入后，程序不在具有主动性，而是变成了被动的接收对象!

我们去使用的时候，就可以在测试里面如下方式实现, 我们如果需要切换不同的UserDao接口实现，只需要在setUserDao()接口设置一下就可以了,如下面在测试类中:

```java
@Test
public void testDao(){
    UserServiceImpl userService = new UserServiceImpl();
    userService.setUserDao(new MysqlUserDaoImpl());
    userService.getUser();
}
```



**这样，当我们比如想扩展一个pgsql实现UserDao, 就只需要实现一个叫做PgsqlUserDaoImpl的类，其他在Service实现类就不需要做任何改变，在测试方法中，直接New PgsqlUserDaoImpl()即可**

这种思想，从本质上解决了问题，我们程序员不用再去管理对象的创建了! 系统的耦合性大大的降低了! 



让我们可以更加专注的在业务的实现上!

上述的例子是一个IOC的简单原型!

### 2.2 ioc的本质

控制反转IoC(Inversion of Control ) 是一种设计思想，DI(依赖注入) 是实现IoC的一种方法，也有人认为DI只是IoC的另一种叫法，没有Ioc的程序中，我们使用面向对象编程，对象的创建和对象间的依赖关系完全硬编码在程序中，程序的创建由程序员自己控制，控制反转后将对象的创建转移给第三方，

所谓控制反转，是指获取对象的方式反转了，由原来用户自己控制自己需要什么对象，控制对象的生成，程序主动生成对象 转换为 由第三方控制对象的生成，用户自己被动接收!

采用xml方式配置Bean的时候，Bean的定义信息和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的!

控制反转(IoC)是一种通过描述(XML或者注解) 并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方式是依赖注入(Dependency inJection DI)

## 3.Hello Spring



### 3.1 一个例子

准备：创建一个新的maven项目，并导入spring-webmvc的包，对应pom.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>spring-study</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>spring-01-ioc1</module>
        <module>spring-02-hello</module>
        <module>spring-03-ioc2</module>
        <module>spring-04-di</module>
        <module>spring-05-autowired</module>
        <module>spring-06-anno</module>
        <module>spring-07-appconfig</module>
    </modules>

    <dependencies>
        <!--SpringFramework5-->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```



首先，我们设计自己的pojo实体类, 记住一定要有setStr（）方法

```java
public class Hello {
    private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }
}
```

然后,我们在resources目录下，创建一个applicationContext.xml的spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="hello" class="com.kuang.pojo.Hello">
        <!-- collaborators and configuration for this bean go here -->
        <property name="str" value="我是使用了spring赋值的!"/>
    </bean>

</beans>
```

最后，让我们来测试一下这个创建方式

```java
public class MyTest {
    public static void main(String[] args) {
        //控制 由谁来控制对象的创建，传统的应用程序的程序创建由程序控制，而spring由spring控制对象创建 ，反转就是由程序创建对象变为被动接收对象
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.getStr());
    }
}
```



通过上面例子，请问 Hello 这个对象是谁创建的？Hello对象的属性是如何进行设置的？

> 通过学习我们找到，Hello已经被托管给Spring进行创建了，通过配置bean创建了我们的Hello类的对象，并通过 <property name="str" value="我是使用了spring赋值的!"/> 对Hello的属性进行赋值!



这个过程就叫做控制反转

控制：指的是由谁来控制对象的创建。传统应用程序的对象由程序本身控制创建的，使用spring后对象是由Spring来创建的

反转：指的是 程序本身不创建对象，而变成被动的接收对象

依赖注入： 就是利用set方法来进行注入的

Ioc是一种编程思想，由主动的编程变为被动的接收

可以搞过 new ClassPathXmlApplicationContext("bean.xml")去浏览一下底层的源代码。

OK 到了现在，我们就彻底不用在程序中改动了，要实现不同的操作哦，只需要在xml配置文件中进行修改，所谓的Ioc，一句话搞定：

对象由spring来创建、管理、装配!

`我自己的一个小结`

控制反转: 主要就是原来写程序是由程序控制对象的创建，而使用控制反转的思想，是对象的创建并不是由使用者创建，而是由第三方创建，而使用者变成被动接收对象即可。 

控制反转的实现方式 ：  方式1、DI 依赖注射



**没用控制反转**,比如我们当前有一个Dao层接口，实现连接数据库并读取用户，然后service层包含了dao层的接口，并调用dao层读取用户的方法，而使用层，直接创建service层实现类并调用service层的方法；如idea中spring-01-ioc1示例

private UserDao = new UserDaoImpl()

**但是**，随着系统的演进，Dao层需要进行平行拓展，除了支持原来默认的Dao实现，还有mysqlDao实现类，还有OracleDao类的实现，如果还是原来那样在service包含dao层接口，则每次都需要改动大量代码，代价高昂, 人们提出了在Service层动态进行设置具体Dao的实现类，从而实现节省成本，谁用谁来指定

UserServiceImpl

```java
public UserServiceImpl implements UserService{
    ..........
    prinvate UserDao userDao;
    publict void setUserDao(UserDao userDao){
        this.userDao = userDao
    }
    ......
    }
```

Client使用层

```java
public static void main(String [] args){
    UserService userSer = new UserServiceImpl();
    //*************下面这行是重点
    userSer.setUserDao(new OracleUserDaoImpl());
   	//********************
    userSer.getUser();  
}
这样如果换成其他的数据库，很容易更换，如换成Mysql
public static void main(String [] args){
    UserService userSer = new UserServiceImpl();
    //*************下面这行是重点
    userSer.setUserDao(new MysqlUserDaoImpl());
    //********************
    userSer.getUser();  
}
```

**而有了spring**，在上面的过程更加简化，只需要写完实现类，进行配置，然后调用即可

ApplicationContext.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDaoImpl" class="com.kuang.dao.UserDaoImpl">
        <!-- collaborators and configuration for this bean go here -->
    </bean>
    <bean id="mysqlDaoImpl" class="com.kuang.dao.MysqlUserDaoImpl">
        <!-- collaborators and configuration for this bean go here -->
    </bean>
    <bean id="oracleDaoImpl" class="com.kuang.dao.OrcaleUserDaoImpl">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="userServiceImpl" class="com.kuang.service.UserServiceImpl">
        <!-- collaborators and configuration for this bean go here -->
        <property name="userDao" ref="mysqlDaoImpl"/>
    </bean>

</beans>
```



client 使用类

```java
    public static void main(String[] args) {
        //用户实际调用的是业务层。dao层他们不需要接触
//        UserServiceImpl userService = new UserServiceImpl();
//        userService.setUserDao(new UserDaoImpl());
//        userService.getUser();
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        UserServiceImpl userService = (UserServiceImpl) context.getBean("userServiceImpl");
        userService.getUser();

    }
```

**现在，如果要进行更换对应的数据库，我们只需要在ApplicationContext.xml里面，把userServiceImpl的ref换一下，其他任何地方都不改动，就可以解决问题了!**

### 3.2 基于构造器的依赖注入

1、默认使用无参构造创建对象  默认!

2、加入我们使用有参构造对象，可以有多种方式，分别如下：

实例的pojo实体类

```java
public class User {

    private String name;

    public User(String name) {
        System.out.println("User的无参构造!");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("name="+name);
    }
}
```



方式1：按照下标赋值

```xml
<bean id="user" class="com.kuang.pojo.User">
        <!-- collaborators and configuration for this bean go here -->
        <constructor-arg index="1" value="秦僵"/>
        <property name="name" value="秦僵"/>
</bean>
```

方式2：按照参数类型匹配

```xml
    <bean id="user" class="com.kuang.pojo.User">
        <!-- collaborators and configuration for this bean go here -->
        <constructor-arg type="java.lang.String" value="秦僵"/>

        <property name="name" value="秦僵"/>
    </bean>

这个例子： 构造器是传递的String类型
如果两个参数都是String 就没法匹配了，不建议使用
```

方式3：直接通过引用构造器的参数名

```xml
    <bean id="user" class="com.kuang.pojo.User">
        <!-- collaborators and configuration for this bean go here -->
<!--        <constructor-arg ref="java.lang.String"/>-->
        <constructor-arg name="name" value="秦僵/"/>
        <property name="name" value="秦僵"/>
    </bean>
```



一个官网的例子，

首先我们的pojo类如下：

```java
package x.y;
public class Foo {

	public Foo(Bar bar, Baz baz) {
		// ...
	}
}
```

我们可以在spring.xml里面配置如下

```xml
<beans>
	<bean id="foo" class="x.y.Foo">
		<constructor-arg ref="bar"/>
		<constructor-arg ref="baz"/>
	</bean>
    
	<bean id="bar" class="x.y.Bar"/>
	<bean id="baz" class="x.y.Baz"/>
</beans>
```

另一个例子

```java
//对应的pojo对象
package examples;

public class ExampleBean {

	// Number of years to calculate the Ultimate Answer
	private int years;

	// The Answer to Life, the Universe, and Everything
	private String ultimateAnswer;

	public ExampleBean(int years, String ultimateAnswer) {
		this.years = years;
		this.ultimateAnswer = ultimateAnswer;
	}

}

#####//使用类型匹配模式
<bean id="exampleBean" class="examples.ExampleBean">
	<constructor-arg type="int" value="7500000"/>
	<constructor-arg type="java.lang.String" value="42"/>
</bean>
#####//还可以使用index属性进行注入
<bean id="exampleBean" class="examples.ExampleBean">
	<constructor-arg index="0" value="7500000"/>
	<constructor-arg index="1" value="42"/>
</bean>
#####//还可以使用构造器的参数名称
<bean id="exampleBean" class="examples.ExampleBean">
	<constructor-arg name="years" value="7500000"/>
	<constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了!

## 4 spring的配置文件

### 4.1 alias别名

```xml
    <bean id="user" class="com.kuang.pojo.User">
        <!-- collaborators and configuration for this bean go here -->
		<!--<constructor-arg ref="java.lang.String"/>-->
        <constructor-arg name="name" value="秦僵/"/>
        <property name="name" value="秦僵"/>
    </bean>   

<alias name="user" alias="user2123123"/>  将user配置为别名 user2123123
```

注意：别名区分大小写 一般用在类名特别长的地方

### 4.2 bean配置

id 代表要new出来的对象名字， bean的唯一标识符，也就是相当于我们学过的变量名 ，对象名

class bean对象所对应的全限定类名(包名+类名)

name: 也是别名，而且name更高级，可以同时取多个别名

### 4.3 import 

一般用于团队开发使用，可以将多个配置文件导入合并为一个

```xml
<import resource="bean.xml"/>
<import resource="bean1.xml"/>
```

## 5 依赖注入DI

依赖注入三种方式

### 5.1 构造器注入

https://docs.spring.io/spring-framework/docs/5.2.12.RELEASE/spring-framework-reference/core.html#beans-factory-collaborators

测试的构造器注入的类：

```java
package examples;

public class ExampleBean {

    // Number of years to calculate the Ultimate Answer
    private int years;

    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

方式一：采用参数类型匹配的方式

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

方式二：采用构造器参数索引的方式

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

方式三：采用构造器参数名字的方式

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```









### 5.2 set方式注入[重点]

https://docs.spring.io/spring-framework/docs/5.2.12.RELEASE/spring-framework-reference/core.html#beans-setter-injection

- 依赖注入
  - 依赖  bean对象的创建依赖于容器
  - 注入  bean对象中的所有属性，由容器来注入

测试环境搭建

1、复杂类型

```java
public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```



2、真实测试对象

```java
public class Student {
    private String name;
    private Address address;
    private String [] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private Properties info;
    private String wife; //是否有妻子
}
```

3、测试配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--pojo address-->
    <bean id="address" class="com.kuang.pojo.Address">
        <property name="address" value="北京海淀福缘门社区"/>
    </bean>
    <!--pojo student-->
    <bean id="student" class="com.kuang.pojo.Student">
        <!--第一种 普通值注入-->
        <property name="name" value="秦僵"/>
        <!--第二种 Bean注入-->
        <property name="address" ref="address"/>
        <!--第三种 数组注入-->
        <!-- results in a setSome Array call -->
        <property name="books">
            <array>
                <value>Django编程大法</value>
                <value>Python编程大法</value>
                <value>Java编程大法</value>
            </array>
        </property>
        <!--第四种 List注入-->
        <!-- results in a setSomeList(java.util.List) call -->
        <property name="hobbys">
            <list>
                <value>玩游戏</value>
                <value>跳舞</value>
                <value>看电影</value>
            </list>
        </property>
        <!--第五种 map注入-->
        <!-- results in a setSomeMap(java.util.Map) call -->
        <property name="card">
            <map>
                <entry key="key" value="12312312312321312312"/>
                <entry key ="address" value="安阿斯加德焚枯食淡金风科技撒附件"/>
            </map>
        </property>
        <!--第六种 set注入-->
        <property name="games">
            <set>
                <value>LoL</value>
                <value>ROB</value>
                <value>COF</value>
            </set>
        </property>
        <!--第七种 null注入-->
        <property name="wife">
            <null/>
        </property>
        <!--第八种 property注入-->
        <!-- the merge is specified on the child collection definition -->
        <property name="info">
            <props>
                <prop key="sales">sales@example.com</prop>
                <prop key="support">support@example.co.uk</prop>
            </props>
        </property>
    </bean>
</beans>
```



### 5.3 拓展方式注入

https://docs.spring.io/spring-framework/docs/5.2.12.RELEASE/spring-framework-reference/core.html#beans-dependency-resolution

我们可以用是P命令空间和C命令空间

官网: P命名空间的意思是，可以引入 下面的约束，则后续bean里面的名字就可以直接使用p:argument来代替!

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

使用

```java
public class Person {
    private String name;
    private Person spouse;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }



    public Person getSpouse() {
        return spouse;
    }

    public void setSpouse(Person spouse) {
        this.spouse = spouse;
    }
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", spouse=" + spouse +
                '}';
    }
}
```

测试

```xml
    <bean name="jane" class="com.kuang.pojo.Person">
        <property name="name" value="Jane Doe"/>
    </bean>

    <bean name="john-classic" class="com.kuang.pojo.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>

    <bean name="john-modern"
          class="com.kuang.pojo.Person"
          p:name="John Doe"
          p:spouse-ref="jane"/>
```





注意点：

1、P命名和C命令空间，不能直接使用，需要导入xml约束



### 5.4 注入的例子





### 5.5 官网的例子

https://docs.spring.io/spring-framework/docs/5.2.12.RELEASE/spring-framework-reference/core.html#beans-factory-properties-detailed

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne"> 
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```



对应的ExampleBean类

```java
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```





### 5.6 bean的作用域 Bean Scope



1.单例模式(singleton) SPring默认机制,  Singleton Scope   表示只要使用同一个bean，那么只有那么一个对象

```java
<bean name="john-classic" class="com.kuang.pojo.Person">
    <property name="name" value="John Doe"/>
    <property name="spouse" ref="jane"/>
</bean>

@Test
public void singletonScope(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
    Person bean = context.getBean("john-classic", Person.class);
    Person bean2 = context.getBean("john-classic", Person.class);

    System.out.println(bean.hashCode());
    System.out.println(bean2.hashCode());
    System.out.println("bean==bean2?:"+( bean==bean2));

}

结果:
1956710488
1956710488
bean==bean2?:true
        
```



2.原型模式（Prototype）:每次从容器中都会get的时候，都会产生一个新对象

```java
<bean name="john-classic" class="com.kuang.pojo.Person">
    <property name="name" value="John Doe"/>
    <property name="spouse" ref="jane"/>
    </bean>

 <bean name="john-modern"
    class="com.kuang.pojo.Person"
        p:name="John Doe"
            p:spouse-ref="jane"
                scope="prototype"/>


@Test
public void prototypeScope(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        Person bean = context.getBean("john-modern", Person.class);
        Person bean2 = context.getBean("john-modern", Person.class);

        System.out.println(bean.hashCode());
        System.out.println(bean2.hashCode());
        System.out.println("bean==bean2?:"+( bean==bean2));

}

retult:
1956710488
603856241
bean==bean2?:false
```

```txt
Bean中 id和name的区别
1)id与name 属性在作用上基本没有区别。推荐使用id。
 
2)id取值要求严格些，必须满足XML的命名规范。id是唯一的，配置文件中不允许出现两个id相同的<bean>。
 
3)name取值比较随意，甚至可以用数字开头。在配置文件中允许出现两个name相同的<bean>，在用getBean()返回实例时，后面一个Bean被返回。
 
4)如果没有id，name，则用类的全名作为name，如<bean class="test.Test">,可以使用getBean("test.Test")返回该实例。
```



3.其余的request、session、application 这些个作用域只能在web开发中使用到!





## 6. bean的自动装配

自动装配 是spring满足bean依赖的一种方式!  

spring会在上下文中自动寻找，并自动给bean装配属性!

他的优点：

自动装配可以大大减少指定属性或构造函数参数的需要。

自动装配可以随着对象的发展而更新配置。

自动装配有4中模式

No   默认是No,这时候bean的引用必须使用ref来定义，



在spring有三种装配的方式

​	1、在xml中显示的配置

​	2、在java中显示的配置

​	3、隐式的自动装配bean【重要掌握】



### 6.1 自动装配方式1---在xml中显示的配置Autowired

#### 1.测试环境

1、环境搭建： 一个人有两个宠物! 这种形式一般就是组合

```java
package com.kuang.pojo;

public class People {
  
    private Cat cat;
    private Dog dog;
    private String name;

    public Cat getCat() {return cat;}

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
        @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}
```



####  2.  autoWire= ByName

原理，会自动在容器的上下文查找，和自己对象 set 方法后面的值 对应的beanid! 

本例子中，people会自动装配一些值，装配的值根据 people里面的set方法确定， setCat(Cat cat) , setDog(Dog dog)

由于在手动配置xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率 降低。
采用自动装配将避免这些错误，并且使配置简单化。

没有使用自动装配的配置, 我们需要为People的每一个属性进行手动装配 如下面的例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd" >

    <bean id="dog" class="com.kuang.pojo.Dog"/>
    <bean id="cat" class="com.kuang.pojo.Cat"/>
    <bean id="people" class="com.kuang.pojo.People" >
        <property name="name" value="张三"/>
        <property name="dog" ref="dog"/>
        <property name="cat" ref="cat"/>
    </bean>
</beans>
```



使用@AutoWire 注解的，就可以自动进行装配，Spring配置文件像下面这个样子

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.kuang.pojo.Dog"/>
    <bean id="cat" class="com.kuang.pojo.Cat"/>
    <!--pojo people-->
    <bean id="people" class="com.kuang.pojo.People" autowire="byName">
        <property name="name" value="qinjiang"/>
    </bean>
</beans>
```

​	通过设置autowire=byName，在people中就不用设置dog和cat的<property name="name" ref="dog"/> , spring会自动在上下文里面找到和自己setDog后面同名的，找到就设置进去了。



测试：

```java
   
@Test
    public void testPeople(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        People people = context.getBean("people", People.class);
        people.getCat().shut();
        people.getDog().shut();
    }
```

结果：

```
喵喵
旺旺!
```

1. 再次测试，结果依旧成功输出！
2. 我们将 cat 的bean id修改为 cat22,cat33,
3. 再次测试， 执行时报空指针java.lang.NullPointerException。因为按byName规则找不对应set方 法，真正的setCat就没执行，对象就没有初始化，所以调用时就会报空指针错误。

**小结：**
当一个bean节点带有 autowire byName的属性时。

- 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。
- 去spring容器中寻找是否有此字符串名称id的对象。
- 如果有，就取出注入；如果没有，就报空指针异常



#### 3. autoWire= ByType



autoWire byType (按类型自动装配)
使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一 的异常。NoUniqueBeanDefinitionException

本例子中，people会自动装配一些值，装配的值根据 people里面的set方法确定， setCat(Cat cat) , setDog(Dog dog)

1、将user的bean配置修改一下 autowire="byType"

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd

        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd" >
    <!--开启注解支持-->
    <context:annotation-config/>
    <bean id="dog" class="com.kuang.pojo.Dog"/>
    <bean id="cat" class="com.kuang.pojo.Cat"/>
    <bean id="people" class="com.kuang.pojo.People" autowire="byType"/>
</beans>
```

byType,会自动的在容器上下文中查找，和自己对象属性类型相同的bean！

2、测试正常输出

```xml
喵喵
旺旺!
People{cat=com.kuang.pojo.Cat@1372ed45, dog=com.kuang.pojo.Dog@6a79c292, name='null'}
```



3、再注册一个dog的bean对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd

        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd" >
    <!--开启注解支持-->
    <context:annotation-config/>
    <bean id="dog" class="com.kuang.pojo.Dog"/>
    <bean id="dog2" class="com.kuang.pojo.Dog"/>
    <bean id="cat" class="com.kuang.pojo.Cat"/>
    <bean id="cat2" class="com.kuang.pojo.Cat"/>
    <bean id="people" class="com.kuang.pojo.People" autowire="byType"/>
</beans>
```

1. 测试，报错：NoUniqueBeanDeﬁnitionException
2. 删掉dog2，将dog的bean名称改掉！测试！因为是按类型装配，所以并不会报异常，也不影响后 的结果。甚至将id属性去掉，也不影响结果。
   这就是按照类型自动装配！

小结：

- 在byName的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致~
- 在byType的时候，需要保证所有Bean的calss唯一，并且这个bean需要和自动注入的属性的类型一致!



上述这些是通过xml配置，实现自动装配的方式，在spring中实现自动装配有两种方式，下面就是通过注解方式实现自动装配



### 6.2 自动装配方式2---使用注解



jdk1.5支持的注解，spring从2.5就支持了注解



要使用注解须知

1、导入约束

2、配置注解的支持

```xml
<context:annotation-config/>
```



一个使用注解进行自动装配的案例

首先，编辑我们对应的xml配置文件，在这里我们需要把注解装配开关打开 <context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd

        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd" >
    <context:annotation-config/>
    <bean id="dog" class="com.kuang.pojo.Dog"/>
    <bean id="cat" class="com.kuang.pojo.Cat"/>
    <bean id="people" class="com.kuang.pojo.People" />
</beans>
```

然后，到我们的People.java里使用@Autowired注解， 如下面的代码，就代表  People类的实例，将自动注解Cat cat和Dog dog这两个对象!

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;

    public Cat getCat() { return cat;}
    public void setCat(Cat cat) {this.cat = cat;}
    public Dog getDog() {return dog;}
    public void setDog(Dog dog) { this.dog = dog;}
    public String getName() {return name;}
    public void setName(String name) { this.name = name;}
    @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}
```

然后，我们输入测试：

```java
@Test
public void testProple(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
    People people = context.getBean("people", People.class);
    people.getCat().shut();
    people.getDog().shut();
    System.out.println(people.toString());
}
```

测试结果：

```
喵喵
旺旺!
People{cat=com.kuang.pojo.Cat@71a794e5, dog=com.kuang.pojo.Dog@76329302, name='null'}
```

小结:

1、通过使用@AutoWired 注解，比xml配置文件更省事，这样能让bean.xml里面对每个bean的定义更加简单

2、需要记住开启注解：<context:annotation-config/>

3、注解需要在对象上，比如private Cat cat; 不能用到private String name;

4、@AutoWired注解可以放在类的属性或者set方法上；

5、使用Autowired我们可以不用编写Set方法，前提是你这个自动装配的属性在IoC(Spring)容器中存在且符合名字byName

科普：

> @Nullable  字段标记了这个注解，说明这个字段可以是null;

```
@Autowired(required = false)  如果显示的改为false后，就说明这个对象可以为Null,否则不可以为空，false表示这个属性可以在配置文件中不设置
```

> @Qualifier(value="dog222")  这个意思是当前属性指定后面哪个bean。 
>
> 例如
>
> @Autowired
>
> @Qualifier(value="dog222")
>
> 而此时的xml配置文件为：
>
> ```
> <beans xmlns="http://www.springframework.org/schema/beans"
>        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
> 
>        xmlns:context="http://www.springframework.org/schema/context"
> 
>        xsi:schemaLocation="http://www.springframework.org/schema/beans
>         https://www.springframework.org/schema/beans/spring-beans.xsd
> 
>         http://www.springframework.org/schema/context
>         https://www.springframework.org/schema/context/spring-context.xsd" >
>     <context:annotation-config/>
>     <bean id="dog22" class="com.kuang.pojo.Dog"/>
>     <bean id="dog222" class="com.kuang.pojo.Dog"/>
>     <bean id="cat2" class="com.kuang.pojo.Cat"/>
>     <bean id="cat222" class="com.kuang.pojo.Cat"/>
>     <bean id="people" class="com.kuang.pojo.People" />
> </beans>
> ```
>
> @Autowired 这个注解就可以根据指定的dog222找到需要的了 
>
> 如果@Autowired自动装备的环境比较复杂，自动装备无法通过一个注解【@Autowired】完成的时候，我们可以用@Qualifier(value="xxx")去配合@Autowired的使用，指定一个唯一的bean对象注入!

**@Resource(name="cat2") 这样也可以实现，我们直接就找到了cat2**

```

pojo实体类代码
public class People {
    @Resource(name="cat2")
    private Cat cat;
    @Autowired
    @Qualifier("dog222")
    private Dog dog;
    private String name;
    
    xml配置
    <context:annotation-config/>
    <bean id="dog22" class="com.kuang.pojo.Dog"/>
    <bean id="dog222" class="com.kuang.pojo.Dog"/>
    <bean id="cat2" class="com.kuang.pojo.Cat"/>
    <bean id="cat222" class="com.kuang.pojo.Cat"/>
    <bean id="people" class="com.kuang.pojo.People" />
    
    测试代码
        @Test
    public void testProple(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        People people = context.getBean("people", People.class);
        people.getCat().shut();
        people.getDog().shut();
        System.out.println(people.toString());
    }
```



小结



@Resource和@Autowired的区别

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byname的方式实现，如果byName找不到，就按照byType实现，如果两个都找不到的情况下就报错。
- @Resource默认是通过byname方式实现，如果找不到名字，就通过byType实现，如果两个都找不到的情况下就报错。



### 6.3 自动装配方式3-隐式自动装配bean



<待学习>



## 7. 使用注解开发

在spring4之后，要使用注解开发，必须要保证AOP的包导入

使用注解需要导入context约束，增加注解支持!

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd

        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd" >
    <!--开启注解支持-->
    <context:annotation-config/>

</beans>
```

可以添加支持扫描注解

```xml
<!--指定要扫描的包，这个包下面的注解就会生效-->
<context:component-scan base-package="com.kuang.pojo"/>
```

1、bean

```java
@Component
public class User {
    //相当于  <property name="name" value="秦僵"/>
    @Value("狂神")
    public String name;

    @Value("狂神2")
    public void setName(String name) {
        this.name = name;
    }
}
```



2、属性如何注入

```java
@Component
public class User {
    //相当于  <property name="name" value="秦僵"/>
    @Value("狂神")
    public String name;

    @Value("狂神2")
    public void setName(String name) {
        this.name = name;
    }
}
```



3、衍生的注解

@Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层!

	- dao层----> @Repository  和Conponent一样，
	- service层 --->@Service   
	- controller层  --->@Controller  代表这个类被spring托管

这四个注解功能都是一样的，都是代表将某个类注册到Spring容器中，自动装配bean





4、自动装配

```
@Autowired 自动装配通过类型，名字，如果Autowired不能唯一装配上属性，就用@Qualifier("dog222")
@Nullable 字段标记了这个注解，说明这个字段可以为null
@Resource: 自动装配铜鼓类型。名字  使用方式  @Resource(name="cat2")
```

5、作用域

```java
@Component
@Scope("singleton")  //标记为单例模式
public class User {
    //相当于  <property name="name" value="秦僵"/>
    @Value("狂神")
    public String name;

    @Value("狂神2")
    public void setName(String name) {
        this.name = name;
    }
}
```

6、小结

xml与注解: 

- xml 更加万能，适用于任何场合，维护简单方便

- 注解 不是自己的类使用不了，维护相对复杂!

xml和注解的最佳实践：

	- xml用来管理bean;
	- 注解只负责完成属性的注入
	- 我们在使用的过程中，只需要注意一个问题：必须让注解生效，必须要在xml里面开启注解支持(扫描+开启)

## 8 使用java的方式配置Spring

我们现在要完全不使用Spring的xml配置了，全权交给java来做。 

JavaConfig 是Spring 的一个子项目，在Spring 4 之后，他成为了一个核心功能。

首先，编写pojo的User实体类, 注意，我们使用@Component  代表Spring组件 @Value("秦僵") 代表赋值

```java
//这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中
@Component
public class User {
    @Value("秦僵")
    private String name;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

然后编写一个KuangConfig.java, 编写配置文件  @Configuration 代表这是一个配置服务类， @Bean代表这是一个spring的bean 

```java
@Configuration  //这个也会被Spring容器托管，注册到Spring容器中，因为他本身就是一个@Component。
// @Configuration 代表这是一个配置类，就是和我们之前看的beans.xml一样的
public class KuangConfig {
    @Bean  //注册一个bean，就相当于我们之前写的一个bean标签，
    //这个方法的名字，就相当于bean标签中的id使用,这个方法的返回值就相当于bean标签中的class属性
    public User getUser(){
        return new User(); //就是要返回要注入到bean的对象!
    }
}

```

最后测试类使用, 这里使用的是KuangConfig.getUser作为bean的id

```java
    @Test
    public void testAnnotation(){
        ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.class);
        User getUser = context.getBean("getUser", User.class);
        System.out.println(getUser.getName());
    }
```



这种纯java的配置方式，在springboot中随处可见!



## 9. IoC配置spring小结

一共有3种方式，分别为：

### 方式一: [Java配置](http://docs.spring.io/spring/docs/5.0.0.M5/spring-framework-reference/html/beans.html#beans-java)  这种方式，是定一个AppConfig配置类，

```java
第一步 编写一个AppCOnfig的配置类
@Configuration
public class AppConfig {

	@Bean
	public User user() {
		return new User();
	}
}
bean 的id就是方法名是user,bean的class是 User
第二步：编写我们的pojo类User
@Component
public class User {
    @Value("秦僵") //属性注入
    private String name;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}        
    
第三步：Instantiating the Spring container using AnnotationConfigApplicationContext
public static void main(String[] args) {
	ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
	MyService myService = ctx.getBean(user.class);
	myService.doStuff();
}
```



### 方式二：[基于注解配置](http://docs.spring.io/spring/docs/5.0.0.M5/spring-framework-reference/html/beans.html#beans-annotation-config)

```java
第一步: 编写注解支持的xml配置文件，重点是加入 <context:annotation-config/> 支持注解
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd">

	<context:annotation-config/>

</beans>

第二步：在需要被Spring控住注入的pojo 添加注解
@Component
public class User {
    //相当于  <property name="name" value="秦僵"/>
    @Value("狂神")
    public String name;

    @Value("狂神2")
    public void setName(String name) {
        this.name = name;
    }
}

第三步: 在xml配置文件，加入bean
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd">

	<context:annotation-config/>

	<bean class="example.SimpleMovieCatalog">
		<!-- inject any dependencies required by this bean -->
	</bean>
</beans>

第四步：可以使用ClassPathXmlApplicationContext解析注解
    @Test
    public void testUser(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        System.out.println(user.name);
    } 
        
```

### 方式三： 基于XML配置 [xml配置](https://docs.spring.io/spring-framework/docs/5.0.0.M5/spring-framework-reference/html/beans.html#beans-java)

```java
第一步: 编写支持的xml文件  applicationContext.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.kunag.pojo.User">
        <!-- 在这里写 bean 的配置和相关引用 -->
        <property name="name" value="秦僵"/>
    </bean>
    </bean>

    <bean id="..." class="...">
        <!-- 在这里写 bean 的配置和相关引用 -->
    </bean>

    <!-- 在这里配置更多的bean -->

</beans>

第二步、编写我们的User类
public class User {
    private String name;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
} 
第三步: 用测试类使用测试
    @Test
    public void testUser(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        System.out.println(user.name);
    }  
```

Spring的使用属性

1.所有的类都需要装配到spring里面，

2.所有的bean都需要通过容器去取

3.容器里面取出来的bean，拿出来就是一个具体的对象



## 10 设计模式之代理模式

为什么要学代理模式？因为这就是spring aop 和spring mvc 的底层设计模式!



代理模式的分类-23中设计模式之一

URL关系模式 

聚合和组合的区别：

组合是直接在Proxy中new Host(), Proxy的对消亡，Host对象一起跟随消亡

聚合是传参，不会随着Proxy对象的消亡而消亡

![image-20201220104024457](F:\MD格式学习笔记库\狂神JAVA-15-Spring.assets\image-20201220104024457.png)

### 10.1 静态代理

角色分析：

- 抽象角色  一般会使用接口或者抽象类来解决
- 真实角色， 被代理的角色
- 代理角色  代理真实的角色，代理真实角色后，我们一般会做一些附属操作
- 客户 访问代理对象的人!

对应的代码：

```java
//步骤1、接口：租房接口，真实角色和代理角色要实现的
public interface RentHourse {
    public void rent();
}

//步骤2：真实角色  房东实现类，直接租房
public class Hourse implements RentHourse {
    public void rent() {
        System.out.println("房东:我出租房子!");
    }
}
//步骤三：代理角色  代理租房
public class Proxy implements RentHourse {
    private Hourse hourse;

    public Proxy() {
    }

    public Proxy(Hourse hourse) {
        this.hourse = hourse;
    }

    public void rent() {
        seeHouse();
        hourse.rent();
        hetong();
        fare();
    }

    //看房
    public void seeHouse() {
        System.out.println("代理:带你看房!");
    }

    //合同
    public void hetong() {
        System.out.println("代理:签订合同");
    }

    //收中介费
    public void fare() {
        System.out.println("代理:收中介费!");
    }
}
//## 步骤四 客户端访问代理角色 测试客户端
public class Client {
    public static void main(String[] args) {
        //房东要租房子
        Hourse hourse = new Hourse();
        //代理，中介帮房东租房子，但是呢，代理角一般会有一些附属操作!
        hourse.rent();

        Proxy proxy = new Proxy(hourse);
        proxy.rent();
    }
}
>>>结果：
房东:我出租房子!
代理:带你看房!
房东:我出租房子!
代理:签订合同
代理:收中介费!    
```

代理模式的好处：

	- 使得真实角色的操作更加纯粹，不用去关注一些公共的事情，业务
	- 公共业务交给代理角色!  实现了业务的分工!
	- 公共业务发生扩展的时候，方便集中管理!

代理模式的缺点：

​	一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率会变低~

### 10.2 加深理解

示例代码

```java
### 首先，第一步，设计一个UserService接口，定义 一个增删改查接口
package com.kuang.demo002;
//抽象接口，这个抽象接口
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}

### 然后，第二步，设计一个真实角色，这个真实角色实现了UserService
//真是角色UserServiceImpl，实现UserSercie
public class UserServiceImpl implements UserService {
    public void add() {
        System.out.println("UserServiceImpl.add()");
    }

    public void delete() {
        System.out.println("UserServiceImpl.delete()");
    }

    public void update() {
        System.out.println("UserServiceImpl.update()");
    }

    public void query() {
        System.out.println("UserServiceImpl.query()");
    }
}

### 然后第三步，实现我们一个代理类，一些基本的方法、公共方法可以放在这里
public class UserServiceProxy implements UserService {
    private UserService userService;

    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    public void add() {
        log("add()");
        userService.add();
    }

    public void delete() {
        log("delete()");
        userService.delete();
    }

    public void update() {
        log("update()");
        userService.update();
    }

    public void query() {
        log("query()");
        userService.query();
    }

    public void log(String msg){
        System.out.println("[debug] 当前执行方法为:"+msg);
    }
}

### 最后，第四步，我们使用这个代理类效果可以在client中
public class Client {
    public static void main(String[] args) {
        UserService userService = new UserServiceImpl();

        //代理模式
        UserServiceProxy userServiceProxy = new UserServiceProxy();
        userServiceProxy.setUserService(userService);
        userServiceProxy.add();


        UserService userService2 = new ProductServiceImpl();
        //代理模式
        ProductServiceProxy productServiceProxy = new ProductServiceProxy();
        productServiceProxy.setProductService(userService2);
        productServiceProxy.delete();

    }
} 
```



聊聊AOP，代码对应08-demo02

![image-20201220115719259](F:\MD格式学习笔记库\狂神JAVA-15-Spring.assets\image-20201220115719259.png)



[细说spring-AOP详解](https://blog.csdn.net/q982151756/article/details/80513340) 经典好文，解释这个概念很好

### 10.3 动态代理

为了解决静态代理，每个真实角色都需要一个代理的缺点，引入了动态代理的概念，也就是反射!

动态代理和静态代理的角色一样，分别为 抽象角色，真实角色，代理角色，使用类

动态代理的代理类是动态生成的，不是我们直接写好的!

动态代理按照事先方式分为两大类： a）基于接口的动态代理  b)基于类的动态代理

-基于接口-jdk动态搭理

-基于类:cglib

-javassist 字节码类库



需要了解两个类：Proxy 代理   InvocationHandler  调用处理程序

动态代理例子代码：

```java
### 接口
package com.kuang.demo05;

public interface UserService {
    public void select();
    public void update();
    public void add();
    public void delete();
}

### UserService的实现
package com.kuang.demo05;

public class UserServiceImpl implements UserService{
    @Override
    public void select() {
        System.out.println("UserServiceImpl.select()");
    }

    @Override
    public void update() {
        System.out.println("UserServiceImpl.update()");
    }

    @Override
    public void add() {
        System.out.println("UserServiceImpl.add()");
    }

    @Override
    public void delete() {
        System.out.println("UserServiceImpl.delete()");
    }
}
### 动态代理类
package com.kuang.demo05;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.util.Date;

public class UserServiceProxy implements InvocationHandler {

    Object target; //被代理的对象，实际的方法执行者

    public UserServiceProxy(Object target) {
        this.target = target;
    }


    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        before();
        Object result = method.invoke(target, args);//调用target的method方法 这是反射机制 这里记住，proxy代表的是动态生成的UserService的代理类，target才是实际类。
        after();

        return result;//返回方法的执行结果
    }

    //调用invoke方法之前执行
    private void before() {
        System.out.println(String.format("log start time [%s]", new Date()));
    }

    //调用invoke方法之后执行
    private void after() {
        System.out.println(String.format("log end time [%s]", new Date()));
    }


}
### 测试类 Client
public class Client {
    public static void main(String[] args) {
        // 设置变量可以保存动态代理类，默认名称以 $Proxy0 格式命名
        // System.getProperties().setProperty("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
        //1.创建被代理的对象，UserService 的接口实现类
        UserServiceImpl userServiceImpl = new UserServiceImpl();
        //2.获取需要传递给Proxy的ClassLoader, 获取UserServiceImpl对应的 ClassLoader
        ClassLoader classLoader = userServiceImpl.getClass().getClassLoader();
        //3.获取需要传递给Proxy的interfaces,这里的UserServiceImpl只实现了一个接口UserService，
        Class [] interfaces = userServiceImpl.getClass().getInterfaces();
        //4.生成需要传递给Proxy的IncovationHandler实现类, 须传入实际的执行对象 userServiceImpl
        UserServiceProxy userServiceProxy = new UserServiceProxy(userServiceImpl);
        //5 .至此，我们生成了我们需要的UserServiceImpl的代理类
        /*
        *根据上面提供的信息，创建代理对象 在这个过程中，
        *  a.JDK会通过根据传入的参数信息动态地在内存中创建和.class 文件等同的字节码
        *  b.然后根据相应的字节码转换成对应的class，
        *  c.然后调用newInstance()创建代理实例
        * */

        UserService proxy  = (UserService) Proxy.newProxyInstance(classLoader, interfaces, userServiceProxy);

        //如同使用静态代理一样代理类，调用方法
        proxy.add();
        proxy.select();
        proxy.update();
        // 保存JDK动态代理生成的代理类，类名保存为 UserServiceProxy
        // ProxyUtils.generateClassFile(userServiceImpl.getClass(), "UserServiceProxy");
    }
}
```







动态代理的好处：

	- 可以使真实角色的操作更加纯碎，不用去关注一些公共的业务
	- 公共也就交给代理角色，实现了业务的分工
	- 共欧诺个业务发生扩展的时候，方便几种管理
	- 一个动态代理类代理的是一个接口，一般就是对应的一类业务!
	- 一个动态代理类，可以代理多个类，只要是实现了同一个接口即可!





## 11 AOP

### 11.1 什么是AOP

AOP(Aspect oriented progranming) 意思是面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术，AOP是OOP的延续，是软件开发中的一个热点，也是spring框架中一个重要的内容，是函数式编程的一种衍生泛型。利用AOP可以对业务逻辑的各个部分进行有效的隔离，从而使得业务逻辑各部分之间的耦合度降低，提供程序的可重用性，同时提高了开发的效率。

![image-20201221091912194](F:\MD格式学习笔记库\狂神JAVA-15-Spring.assets\image-20201221091912194.png)



### 11.2 Aop在spring中的应用

提供声名式事务，允许用户自定义切面!

- 横切关注点： 跨越应用程序多个模块的方法或功能，即是与我们业务逻辑无关，但是我们需要关注的部分，就是横切关注点，如日志，安全，缓存，事务等等....
- `切面(Aspect)`  Aspect 声明类似于 Java 中的类声明，在 Aspect 中会包含着一些 Pointcut 以及相应的 Advice
- `增强(Advice)`  Advice 定义了在 `Pointcut` 里面定义的程序点具体要做的操作，它通过 before、after 和 around 来区别是在每个 joint point 之前、之后还是代替执行的代码
- `目标(Target)`  织入 `Advice` 的目标对象。 
- 代理(Proxy)  向目标对象应用通知之后创建的对象。 （生成的代理类)
- `切入点(PointCut)`  表示一组 joint point，这些 joint point 或是通过逻辑关系组合起来，或是通过通配、正则表达式等方式集中起来，它定义了相应的 Advice 将要发生的地方。
- `连接点(JointPoint)` ` 爪哇的小县城里的百姓: 因为根据定义, `Joint point` 是所有可能被织入 `Advice` 的候选的点, 在 Spring AOP中, 则可以认为所有方法执行点都是 `Joint point`. 而在我们上面的例子中, 命案发生在小县城中, 按理说在此县城中的所有人都有可能是嫌疑人.

>AOP基本概念
>
> **连接点（****Joinpoint****）：**
>
>  表示需要在程序中插入横切关注点的扩展点，连接点可能是类初始化、方法执行、方法调用、字段调用或处理异常等等，Spring只支持方法执行连接点，**在****AOP****中表示****为****“****在哪里做****”**；
>
>**切入点（****Pointcut****）：**
>
>  选择一组相关连接点的模式，即可以认为连接点的集合，Spring支持perl5正则表达式和AspectJ切入点模式，Spring默认使用AspectJ语法，**在****AOP****中表示为****“****在哪里做的****集合****”**；
>
>**增强（****Advice****）：**或称为增强
>
>  在连接点上执行的行为，增强提供了在AOP中需要在切入点所选择的连接点处进行扩展现有行为的手段；包括前置增强（before advice）、后置增强 (after advice)、环绕增强 （around advice），在Spring中通过代理模式实现AOP，并通过拦截器模式以环绕连接点的拦截器链织入增强 ；**在****AOP****中表示为****“****做什么****”****；**
>
>**方面****/****切面（****Aspect****）：**
>
>   横切关注点的模块化，比如上边提到的日志组件。可以认为是增强、引入和切入点的组合；在Spring中可以使用Schema和@AspectJ方式进行组织实现；**在****AOP****中表****示为****“****在哪里做和做什么集合****”****；**
>
>**目标对象（****Target Object****）：**
>
>  需要被织入横切关注点的对象，即该对象是切入点选择的对象，需要被增强的对象，从而也可称为“被增强对象”；由于Spring AOP 通过代理模式实现，从而这个对象永远是被代理对象，**在****AOP****中表示为****“****对谁做****”**；
>
>**AOP****代理（****AOP Proxy****）：**
>
>  AOP框架使用代理模式创建的对象，从而实现在连接点处插入增强（即应用切面），就是**通过代理来对目标对象应用切面**。在Spring中，AOP代理可以用JDK动态代理或CGLIB代理实现，而通过拦截器模型应用切面。
>
>**织入（****Weaving****）：**
>
>  织入是一个过程，是将切面应用到目标对象从而创建出AOP代理对象的过程，织入可以在编译期、类装载期、运行期进行。
>
>**引入（****inter-type declaration****）：**
>
>  也称为内部类型声明，为已有的类添加额外新的字段或方法，Spring允许引入新的接口（必须对应一个实现）到所有被代理对象（目标对象）, **在****AOP****中表示为****“****做什么****（新增什么）****”**；
>
> 
>
>AOP的Advice类型
>
>**前置增强（****Before advice****）：**
>
>  在某连接点之前执行的增强，但这个增强不能阻止连接点前的执行（除非它抛出一个异常）。
>
>**后置返回增强（****After returning advice****）：**
>
>  在某连接点正常完成后执行的增强：例如，一个方法没有抛出任何异常，正常返回。
>
>**后置异常增强（****After throwing advice****）：**
>
>  在方法抛出异常退出时执行的增强。
>
>**后置最终增强（****After (finally) advice****）：**
>
>  当某连接点退出的时候执行的增强（不论是正常返回还是异常退出）。
>
>**环绕增强****（****Around Advice****）：**
>
>  包围一个连接点的增强，如方法调用。这是最强大的一种增强类型。 环绕增强可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它们自己的返回值或抛出异常来结束执行。

![image-20201221095317463](F:\MD格式学习笔记库\狂神JAVA-15-Spring.assets\image-20201221095317463.png)

![image-20201221095518363](F:\MD格式学习笔记库\狂神JAVA-15-Spring.assets\image-20201221095518363.png)



### 11.3使用Spring实现Aop

重点，使用AOP植入，需要导入一个依赖包

```xml
<dependency>
    <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
       <version>1.9.4</version>
</dependency>
```



### 11.4方式1:使用spring 的api接口

首先，第一步，准备环境，把UserService和UserServiceImpl写好

```java
package com.kuang.service;

public interface UserService {
    public void select();
    public void update();
    public void add();
    public void delete();
}

package com.kuang.service;

public class UserServiceImpl implements UserService{
    public void select() {
        System.out.println("查询了一个用户!");
    }

    public void update() {
        System.out.println("更新了一个用户!");
    }

    public void add() {
        System.out.println("增加了一个用户!");
    }

    public void delete() {
        System.out.println("删除了一个用户!");
    }
}
```

然后，设计我们的切面类

```java
public class AfterLog implements AfterReturningAdvice {
    //returnValue  执行后返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("after: 执行了"+method.getName()+"方法, 返回的结果:"+returnValue);

    }
}

public class BeforeLog implements MethodBeforeAdvice {
    //method 要执行的目标对象的方法
    //args 参数
    //target 目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println("before:"+target.getClass().getName()+"的"+method.getName()+"被执行了!");
    }
}
```

接着，在applicationContext.xml里面配置aop

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd

        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd

        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--开启注解支持-->
    <context:annotation-config/>
    <!--指定要扫描的包，这个包下面的注解就会生效-->
    <context:component-scan base-package="com.kuang"/>
    <!--注册bean-->
    <bean id="userService" class="com.kuang.service.UserServiceImpl"/>
    <bean id="log" class="com.kuang.log.BeforeLog"/>
    <bean id="afterLog" class="com.kuang.log.AfterLog"/>
    <!--配置aop,需要导入aop的约束-->
    <aop:config>
        <!--首先需要一个切点  <aop:pointcut>
		execution(要执行的位置)： 定义切点的表达式部分
        第一个*： 表示返回值的类型任意
        com.kuang.service.UserServiceImpl： AOP所切的服务的类名，即，需要进行横切的业务类
        .*(..) ： 表示任何方法名，括号表示参数，两个点表示任何参数类型
        -->
        <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
        <!--执行环绕增强-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/> <!--表示将log这个类切入到pointcut这个切入点-->
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```

最后，就可以在我们的测试客户端使用了

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理的是接口
        UserService userService = context.getBean("userService", UserService.class);
        //执行add方法
        userService.add();
    }
}
```

### 11.5 方式2：自定义类实现AOP

首先，先定义一个自己的普通类

```java
public class DiyPointCut {
    public void before(){
        System.out.println("===============方法执行前=============");
    }

    public void after(){
        System.out.println("===============方法执行后=============");
    }
}
```

第二步，我们进入到applicationContext.xml，使用如下aop配置

```xml
    <!--方式二：自定义类-->
    <bean id="diy" class="com.kuang.dit.DiyPointCut"/>
    <aop:config>
        <!--自定义切面，ref要引入的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
            <!--增强 Advisor-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
```

然后，我们就可以去测试类进行使用测试了

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理的是接口
        UserService userService = context.getBean("userService", UserService.class);
        //执行add方法
        userService.add();
    }
}
```

运行的结果

```
===============方法执行前=============
增加了一个用户!
===============方法执行后=============
```

### 11.5 方式3 使用注解实现

注解我们可以直接在代码定义

```java
//方式3，使用注解方式实现AOP

@Aspect  //标注这个类是一个切面
public class AnnotationPointCut {
    //  @Before 表示 public void befor()是一个方法前增强，"execution(* com.kuang.service.UserServiceImpl.*(..))"是一个切点
    @Before("execution(* com.kuang.service.UserServiceImpl.*(..))") 
    public void before(){
        System.out.println("=====方法执行前=====");
    }
    //  @After 表示 public void after()是一个方法前增强，"execution(* com.kuang.service.UserServiceImpl.*(..))"是一个切点
    @After("execution(* com.kuang.service.UserServiceImpl.*(..))") 
    public void after(){
        System.out.println("=====方法执行前=====");
    }
    //在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点
    @Around("execution(* com.kuang.service.UserServiceImpl.*(..))") //这是原来配置的连接点
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("=====环绕前=====");
        //执行方法
        Object proceed = joinPoint.proceed();

        System.out.println("=====环绕后=====");

        // Signature signature = joinPoint.getSignature(); //获得当前执行方法的签名
        // System.out.println("signature" + signature);
    }
}
```

写完后，我们需要去进入到applicationContext.xml里面进行配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd

        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd

        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--开启注解支持-->
    <context:annotation-config/>
    <!--指定要扫描的包，这个包下面的注解就会生效-->
    <context:component-scan base-package="com.kuang"/>
    <!--注册bean-->
    <bean id="userService" class="com.kuang.service.UserServiceImpl"/>
    <bean id="log" class="com.kuang.log.BeforeLog"/>
    <bean id="afterLog" class="com.kuang.log.AfterLog"/>
    <!--&lt;!&ndash;方式一，使用apring api配置aop,需要导入aop的约束&ndash;&gt;-->
    <!--<aop:config>-->
    <!--    &lt;!&ndash;首先需要一个切入点  execution(要执行的位置)-->
    <!--    第一个*-->
    <!--    第二个 com.kuang.service.UserServiceImpl-->
    <!--    第三个: *(..) 下面的所有方法-->
    <!--    &ndash;&gt;-->
    <!--    <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>-->
    <!--    &lt;!&ndash;执行环绕增强&ndash;&gt;-->
    <!--    <aop:advisor advice-ref="log" pointcut-ref="pointcut"/> &lt;!&ndash;表示将log这个类切入到pointcut这个切入点&ndash;&gt;-->
    <!--    <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>-->
    <!--</aop:config>-->
    <!--方式二：自定义类-->
    <bean id="diy" class="com.kuang.dit.DiyPointCut"/>
    <!--<aop:config>-->
    <!--    &lt;!&ndash;自定义切面，ref要引入的类&ndash;&gt;-->
    <!--    <aop:aspect ref="diy">-->
    <!--        &lt;!&ndash;切入点&ndash;&gt;-->
    <!--        <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>-->
    <!--        &lt;!&ndash;通知 Advisor&ndash;&gt;-->
    <!--        <aop:before method="before" pointcut-ref="pointcut"/>-->
    <!--        <aop:after method="after" pointcut-ref="pointcut"/>-->
    <!--    </aop:aspect>-->
    <!--</aop:config>-->
    <!--方式三，注解方式实现AOP-->
    <bean id="annotationPointCut2" class="com.kuang.dit.AnnotationPointCut"/>
    <!--必须开启注解支持!-->
    <aop:aspectj-autoproxy/>
</beans>
```

最后，我们来进行测试

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理代理的是接口
        UserService userService = context.getBean("userService", UserService.class);
        //执行add方法
        userService.add();
    }
}
```

## 12 回顾MyBatis

步骤：

第1步 、导入相关jar包

junit

mybatis

mysql数据库的

spring相关的

aop织入器

mybatis-spring[new知识点]

### 12.1 回忆mybatis

1、编写实体类

2、编写核心配置文件

3、编写接口

4、编写mapper.xml, 要注册这个接口

5、编写测试程序

```xml
  <dependencies>
        <!--SpringFramework5-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <!--spring操作数据库的话，还需要spring-jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <!--AOP织入包-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
        <!--mybatis-spring-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
    </dependencies>
```



第2步、编写配置文件 resources/mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--引入外部配置文件-->
    <properties resource="db.properties" />
    <!--mybatis配置-->
    <settings>
        <setting name="cacheEnabled" value="true"/>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;characterEncoding=UTF-8&amp;useUnicode=True"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!--<mapper resource="com/kuang/mapper/UserMapper.xml"/>-->
        <mapper class="com.kuang.mapper.UserMapper"/>
    </mappers>

</configuration>
```

3、编写实体类 pojo/User.java

```java
import lombok.Data;

@Data
public class User {
    private int id;
    private String name;
    private String pwd;
}

```

4、编写实体类接口mapper/UserMapper.java

```java
public interface UserMapper {
    //查询用户列表
    public List<User> selectUser();
}
```



5、编写接口对应的配置文件  mapper/UserMapper.xml  要和UserMapper同一个名称和位置

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.UserMapper">
    <!--查询所有用户功能-->
    <select id="selectUser" resultType="user">
        select * from mybatis.user;
    </select>
</mapper>
```



6、进行测试

```java
public class MyTest {
    @Test
    public void selectUser() throws IOException {
        String resources = "mybatis-config.xml";
        InputStream in = Resources.getResourceAsStream(resources);
        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(in);
        SqlSession sqlSession = sessionFactory.openSession(true);//自动提交事务，加上true

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        List<User> users = mapper.selectUser();

        for (User user : users) {
            System.out.println(user);
        }


    }
}
```





### 12.2 整合spring方式一



先准备环境，新建实体类User.java，并创建UserMapper和UserMapper.xml

```java
//com.kuang.pojo.User
@Data
public class User {
    private int id;
    private String name;
    private String pwd;
}

//com.kuang.mapper.UserMapper
public interface UserMapper {
    public List<User> selectUser();
}

//com.kuang.mapper.UserMapper.xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
    <select id="selectUser" resultType="user">
    select * from mybatis.user;
    </select>
</mapper>
```



第一步：编写数据源的配置文件和mybatis的配置文件  spring-dao.xml  mybatis-config.xml。

```xml
### spring-dao.xml的配置
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置数据源-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <!-- collaborators and configuration for this bean go here -->
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;characterEncoding=UTF-8&amp;useUnicode=True"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>
</beans>

### mybatis  mybatis-config.xml 的配置

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>
</configuration>

```



第二步：在spring-dao.xml里面，配置加载sqlSessionFactiory

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--配置数据源-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <!-- collaborators and configuration for this bean go here -->
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;characterEncoding=UTF-8&amp;useUnicode=True"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>
    <!--配置sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!--绑定Mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/kuang/mapper/*.xml" />
    </bean>
    <!--使用sqlSessionFactory构建sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sqlSessionFactory" />
    </bean>
</beans>
```



第三步：在spring-dao.xml里面配置加载 SqlSessionTemplate

```xml
    <!--使用sqlSessionFactory构建sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sqlSessionFactory" />
    </bean>
```



第四步：给接口做实现类

```java
public class UserMapperImpl implements UserMapper{
    
    private SqlSession sqlSession;

    public void setSqlSession(SqlSession sqlSession) {
        this.sqlSession = sqlSession;
    }

    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.selectUser();
        return users;
    }
}
```

第五步：将自己写的实现类UserMapperImpl注入到spring配置文件 spring-dao.xml中

```xml
<bean id="selectUser" class="com.kuang.mapper.UserMapperImpl"/>
```



第六步：测试使用

```java
    @Test
    public void springSelectUser(){

        ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
        UserMapper userMapper = context.getBean("userMapper",UserMapper.class);
        List<User> users = userMapper.selectUser();
        for (User user : users) {
            System.out.println(user);
        }
    }
```





### 12.3 整合spring-mybatis方式二



第一步：编写实现类，并集成SqlSessionDaoSupport

```java
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper{
    //继承了SqlSessionDaoSupport就可以直接使用getSqlSession()获取sqlSession
    private SqlSession sqlSession;
    public List<User> selectUser() {
        sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.selectUser();
        return users;
    }
}
```

第二步：进入到spring的bean配置文件，注入

```xml
   <bean id="userMapper2" class="com.kuang.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
```

第三步，我们测试一下

```java
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper{
    //继承了SqlSessionDaoSupport就可以直接使用getSqlSession()获取sqlSession
    private SqlSession sqlSession;
    
    public List<User> selectUser() {
        sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.selectUser();
        return users;
    }
}
```

## 声明式事务

### 1. 回顾事务

要么都成功，要么都失败

事务在项目开发中，十分重要，涉及到数据的一致性问题，不能马虎

确保完整性和一致性!

事务的ACID原则

A：原子性

C 一致性

I 隔离性  多个业务可能操作同一个资源，是互相隔离的，防止数据损坏，

D 持久性 事务一旦完成，系统不管干嘛，发生什么问题，数据都是存在，结果不再被影响，被持久化写到存储器中



2、Spring中的事务管理

- 声明式事务 AOP

第一步，配置

```xml
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource" />
        <!--<property name="dataSource" ref="dataSource"/>-->
    </bean>
```

第二步：配置事务管理器, 结合AOP实现事务的织入

```xml
    <!--第二步:结合AOP实现事务的织入-->
    <!--配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--1.给那些方法配置事务-->
        <!--2.配置事务的传播特性 new propagation-->
        <tx:attributes>
            <tx:method name="add*" propagation="REQUIRED" />
            <tx:method name="delete*" propagation="REQUIRED" />
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*"/>   <!--配置所有事务-->
        </tx:attributes>
    </tx:advice>
```

第三步，配置切面

```xml
    <!--第三步 配置事务切入-->

    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.kuang.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>
```



- 编程式事务 需要在自己代码中写入事务管理



思考：为什么需要事务

如果不配置事务，可能存在数据提交不一致的情况下!

如果我们不在spring中去配置声明式事务，我们就需要在代码中手动配置事务!

事务在项目开发中十分重要，涉及到数据的一致性完整性

