# MyBatis-2020-12.1系统学习

环境：JDK1.8+ MYSQL5.7+ MAVEN3.6.1

回顾：jdbc + mysql + java基础 + maven +junit

框架: 都有配置文件，最好的学习方式是看官网  SSM

## 1.mybatis简介

1.1  什么是Mybatis

​	MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。



1.2 如何获得mybatis

​	@ github   https://github.com/mybatis/mybatis-3

​	@ 中文文档地址  https://mybatis.org/mybatis-3/zh/index.html

​	@ maven  https://mvnrepository.com/artifact/org.mybatis/mybatis/3.5.2

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
</dependency>

```



1.3 持久层

持久化就是将程序的数据在持久状态和瞬时状态转化的过程。数据持久化。

因为内存断电即失

数据库是一种持久化(jdbc)，io文件持久化。

生活中 冷藏。罐头也是持久化

为什么需要持久化？

有一些对象，不能让他丢掉，内存太贵了



什么叫做持久层： 

Dao层，Service层，Controller层

完成持久化工作的代码块，层界限十分明显



1.4 为什么需要MyBatis

方便，传统的jdbc代码太复杂了，为了简化，提供了一个框架! 自动化，帮助程序员将数据存入到数据库中。不用MyBatis也可以，只是用了它更容易上手。

技术没有高低之分，只有使用这个的人有高低之分!

优点：

​	简单易学， 灵活，sql 和代码分离，提供映射标签，对公对象关系映射标签，提供xml标签，支持编写动态sql，最重要一点，用的人多!

Spring   SpringMVC  SprngBoot



## 2. 第一个mybatis程序

思路：搭建环境-->导入mybatis--->编写代码-->测试

第一步，写一个工具类 SqlSessionFactoryBuilder ()----SqlSessionFactory----SqlSession  （引申出需要去配置一个mybatis-config.xml）

> 每个基于 MyBatis 的应用都是以一个 **SqlSessionFactory** 的实例为核心的。**SqlSessionFactory** 的实例可以通过 **SqlSessionFactoryBuilder** 获得。而 **SqlSessionFactoryBuilder** 则可以从 XML 配置文件或一个预先配置的 Configuration 实例来构建出 SqlSessionFactory 实例。

### 2.1 搭建环境

step 1 搭建数据库, 相应的环境脚本

```sql
create database `mybatis`;
use `mybatis`

create table `user`(
	`id` int(20) not null,
	`name` VARCHAR(30) default null,
	`pwd` varchar(30) default null,
	primary key(`id`)
)ENGINE=INNODB default charset=utf8;

insert into `user`(`id`,`name`,`pwd`) 
VALUES(1,'狂神','123456'),
(2,'张三','123456'),
(3,'李四','123456'),
(4,'王五','123456')
```



step 2 新建 一个项目，普通maven项目

​	1、新建一个maven普通项目

​	2、删除src，变更项目为一个<!--父工程-->

​	3、导入对应的依赖 

​		<artifactId>mysql-connector-java</artifactId>，

​		<artifactId>mybatis</artifactId>，

​		<artifactId>junit</artifactId>

​	step 3. 创建一个普通的项目

​		接下来就是使用了

### 2.2 编写mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>  <!--事务管理-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/> <!--驱动-->
                <property name="url" value="jdbc:mysql://locahost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8"/><!--连接-->
                <property name="username" value="root"/><!--用户名-->
                <property name="password" value="123456"/><!--密码-->
            </dataSource>
        </environment>

    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```



工厂模式，建造模式

### 2.3 编写读取配置文件的工具类

```java
//工具类  sqlSessionFactory
public class MyBatisUtils {
    private  static  SqlSessionFactory sqlSessionFactory ;

    static {
        try {
            //使用mybatis第一步： 获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。SqlSession 提供了在数据库执行 SQL 命令所
    // 需的所有方法。你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句

    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
        //sqlSession 和 之前的prepareStatement一样
    }

}

```

### 2.4 编写业务代码

	- 实体类

```java
public class User implements Serializable {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```



	- Dao接口操作

```java
public interface UserDao {
    //获取用户列表
    List<User> getUserList();
}
```



	- 接口实现类

接口实现类由原来的UserDaoImpl转换为一个Mapper配置文件  resultType就要用完全路径

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace==绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
    <!--select 查询语句 id对应Dao/Mapper里面的方法名字-->
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```

### 2.5 测试(Junit)

注意点1 :ause: org.apache.ibatis.builder.BuilderException: Error parsing SQL Mapper Configuration. Cause: java.io.IOException: Could not find resource org/mybatis/example/BlogMapper.xml

注意点2. 在 mybatis-config.xml里面，mapper注册是用/ ,不是用.

```xml
    <!--每一个Mapper.xml都需要在mybatis核心配置文件注册-->
    <mappers>
        <!--<mapper resource="org/mybatis/example/UserMapper.xml"/>-->
        <mapper resource="com/kuang/dao/UserMapper.xml"/>
    </mappers>
```

注意点3.配置错了localhost，导致产生数据库连接错误

```
### Cause: com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure

```

注意点4. 没有注册mapper会报错

> org.apache.ibatis.binding.BingingException:Type interface com.kuang.dao.UserDao is not known to the MapperRegistry.
>
> MapperRegistry是什么？
>
> 没一个Mapper.xml都需要在Mybatis核心配置文件中注册!

测试代码

```java
public class UserDaoTest {
    @Test
    public void test() {
        //使用工具类,获取sqlSession对象
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        try {
            //方式一(推荐)执行sql
            UserDao userDao = sqlSession.getMapper(UserDao.class);
            List<User> userList = userDao.getUserList();
            //方式二
            List<User> objects = sqlSession.selectList("com.kuang.dao.UserDao.getUserList");

            for (User user : userList) {
                System.out.println("User:" + user.getName());
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }
}
```

可能会遇到的问题：

1、配置文件没有注册

2、绑定接口错误

3、方法名不对

4、返回类型不对

5、Maven导出资源问题



## 3 CRUD操作的MyBatis实现

### 1、namespace

 	namespace中的包名要和Dao/Mapper 接口中的包名完全一致! 全限定类名

### **2、select 选择、查询语句**

里面的属性有如下字段

	- id 就是对应的namespace中的方法名，
	- resultType sql语句执行的返回值类型 
	- parameterType 参数的类型，传递的参数类型

Step1 编写接口

```java
    //获取全部用户列表
    List<User> getUserList();
    //根据id查询用户
    User getUserById(int id);
```



Step2 编写对应的mapper中的sql语句

```xml
<!--namespace==绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
    <!--select 查询语句 id对应Dao/Mapper里面的方法名字-->
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select * from mybatis.user;
    </select>
    <!--根据id查找指定的一个用户-->
    <select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
        select * from mybatis.user where id = #{id};
    </select>
</mapper>
```



Step3 测试

```java
    @Test
    public void test() {
        //使用工具类,获取sqlSession对象
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        try {
            //方式一(推荐)执行sql
            /**
             * 1、一开始mybatis框架会解析映射配置文件UserMapper.xml文件，会将insert等sql标签封装为一个个的MappedStatement对象，
             * 再以全限定类名+方法名作为key，MappedStatement对象为value封装到MappedStatements这一个map集合中。
             * 2、当你调用getMapper方法时，mybatis会为Userdao使用动态代理创建一个代理对象，只有传入Userdao.class，框架才知道对哪一个
             * 接口创建代理对象，并且UserDao.class也包含了此接口的全限定类名，也就是能够作为key查询到MappedStatements集合中的value值
             * ，也就是对应的MappedStatement对象
             */
            UserDao userDao = sqlSession.getMapper(UserDao.class);
            List<User> userList = userDao.getUserList();
            //方式二
            List<User> objects = sqlSession.selectList("com.kuang.dao.UserDao.getUserList");

            for (User user : userList) {
                System.out.println("User:" + user.getName());
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }

    @Test
    public void getUserById() {
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        User userById = mapper.getUserById(1);
        System.out.println("userById:" + userById);
        sqlSession.close();
    }
```



### **3、insert**

Step1 编写接口

```java
    //insert一个用户
    int addUser(User user);
```



Step2 编写对应的mapper中的sql语句

```xml
    <!--添加一个用户，对象中的属性，可以直接取出来但是要和User属性一一对应否则拿不到-->
    <insert id="addUser" parameterType="com.kuang.pojo.User">
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
    </insert>
```



Step3 测试

```java
    //增删改需要提交事务
    @Test
    public void addUser() {
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        User user6 = new User(6, "钱六", "123456");
        int i = mapper.addUser(user6);
        if (i>0){
            System.out.println("添加成功");
        }
        //提交事务,必须提交事务，不然数据库没有
        //sqlSession.commit();
        System.out.println(mapper.getUserById(5));
        sqlSession.close();
    }
```



### **4、update**

Step1 编写接口

```java
    //update修改一个用户
    int updateUser(User user);
```



Step2 编写对应的mapper中的sql语句

```xml
    <!--修改，对象中的属性，可以直接取出来,但是要和User属性一一对应否则拿不到-->
    <update id="updateUser" parameterType="com.kuang.pojo.User">
        update mybatis.user set name =#{name},pwd=#{pwd}  where id=#{id};
    </update>
```



Step3 测试

```java
    @Test
    public void updateUser(){
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        int 呵呵 = mapper.updateUser(new User(4, "呵呵", "123123"));
        //一定要提交事务，不然没变更
        sqlSession.commit();
        if (呵呵>0){
            System.out.println("修改成功");
        }
        sqlSession.close();
    }
```



### **5、delete**

Step1 编写接口

```
    //删除一个用户
    int deleteUser(int id);
```



Step2 编写对应的mapper中的sql语句

```xml
    <!--删除，对象中的属性，可以直接取出来,但是要和User属性一一对应否则拿不到-->
    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id=#{id};
    </delete>
```



Step3 测试

```java
    @Test
    public void deleteUser(){
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        int 呵呵 = mapper.deleteUser(4);
        //一定要提交事务，不然没变更
        sqlSession.commit();
        if (呵呵>0){
            System.out.println("删除成功");
        }
        sqlSession.close();
    }
```







注意点&分析错误

增删改查需要提交事务!

标签不要配置错误

resource绑定mapper，需要使用路径，而不是.访问符

数据库连接，需要标记正确

**程序读错误日志，要从后往前读，这样比较容易排查错误**

### 6. 万能的mapper

假设我们的实体类或者数据库中字段或者参数过多，我们应当考虑使用Map！,这样我们就可以有如下两个好处

1、不用User的字段和select  sql字段一一对应，map传递参数，直接在sql中取出key即可

2、我们可以传递任意多个参数，而不用每次都必须传递一个User

实现方式

step1. 编写UserDao对应接口

```java
//insert一个用户
int addUser(User user);
//使用神奇的Map传递参数
int addUser2(Map<String,Object> map);

int addUser3(@Param("username")String username,@Param("passwordNew")String passwordNew)
//注意： 这里我们定义的是一个Map对象
```

step2.编写UserDaoMapper.xml, 在这里我们传递的parameterType是写的小写map

```xml
<!--添加一个用户，对象中的属性，可以直接取出来但是要和User属性一一对应否则拿不到-->
<insert id="addUser" parameterType="com.kuang.pojo.User">
    insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
</insert>
<!--对象中的属性，可以直接取出来,可以用map的key传递参数，可以随意进行定义命名，不需要一一对应-->
<insert id="addUser2" parameterType="map">
    insert into mybatis.user (id,name,pwd) value (#{userId},#{UserName},#{password})
</insert>
<insert id="addUser2" parameterType="map">
    insert into mybatis.user (name,pwd) value (#{username},#{password})
</insert>
```

step3.  编写测试累验证一下

```java
@Test
public void addUser2(){
    //1.获取我们的sqlSession
    SqlSession sqlSession = MyBatisUtils.getSqlSession();
    //2.拼接一个HashMap，把对应的key值和UserMapper.xml对应起来
    Map<String, Object> stringObjectHashMap = new HashMap<String, Object>();
    stringObjectHashMap.put("userId",7);
    stringObjectHashMap.put("UserName","helloworld");
    stringObjectHashMap.put("password","123123");
    //3.调用我们的mapper方法
    UserDao mapper = sqlSession.getMapper(UserDao.class);
    mapper.addUser2(stringObjectHashMap);
    //4.事务要提交
    sqlSession.commit();
	//5.关闭sqlSession
    sqlSession.close();
}
```



1.Map传递参数，直接在sql的 #{} 中使用key即可 例如  insert into mybatis.user (id,name,pwd) value (#{userId},#{UserName},#{password})

```xml
<insert id="addUser2" parameterType="map">
insert into mybatis.user (id,name,pwd) value (#{userId},#{UserName},#{password})
</insert>
```

2.对象传递参数，直接在sql的#{}中使用对象的属性即可 insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});

```xml
<insert id="addUser" parameterType="com.kuang.pojo.User">
    insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
</insert>
```



3.只有额基本类型参数的情况，可以直接在sql中#{}使用参数即可，例如  select * from mybatis.user where id = #{id};

```xml
<select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
    select * from mybatis.user where id = #{id};
</select>
```

### 7.模糊查询的使用

模糊查询怎么写？

​	1、java代码执行的时候，传递通配符%%



```java
//模糊查询接口
List<User> getUserLike(String value);

@Test
public void getUserLike(){
    SqlSession sqlSession = MyBatisUtils.getSqlSession();
    UserDao mapper = sqlSession.getMapper(UserDao.class);


    List<User> users = mapper.getUserLike("%张三%");
    for (User user : users) {
        System.out.println(user);
    }

    sqlSession.close();
}
```

​	2、在sql拼接中使用通配符!

```xml
<!--模糊查询-->
<select id="getUserLike" resultType="com.kuang.pojo.User">
    select * from mybatis.user where name like "%"#{value}"%"
</select>
```

## 4. 配置解析

### 4.1核心配置文件

![image-20201212141847626](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201212141847626.png)

 - mybatis-config.xml   这个名字可以随便起，官方建议叫mybatis-config.xml

### 4.2 环境配置(environments)

MyBatis 可以配置成适应多种环境 ,尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境

默认使用的环境 ID（比如：default="development"）

每个 environment 元素定义的环境 ID（比如：id="development"）

事务管理器的配置（比如：type="JDBC"）  一共两种 也就是 type="[JDBC|MANAGED]

- JDBC – 这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域
- MANAGED – 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期

数据源的配置（比如：type="POOLED"） 有三种内建的数据源类型（也就是 type="[UNPOOLED|POOLED|JNDI]"   数据源 有 dbcp c3p0

	- UNPOOLED 这个数据源的实现会每次请求时打开和关闭连接
	- POOLED 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间
	- JNDI 这个数据源实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的数据源引用

```xml
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
    <environment id="test">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;characterEncoding=UTF-8&amp;useUnicode=True"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </dataSource>
    </environment>
</environments>
```



Mybatis默认的事务管理器就是JDBC，连接池是POOLED



### 4.3 属性(Properties)

我们可以通过Properties属性来实现引用配置文件，这些属性都可以外部配置且可动态替换的，既可以在典型的jva配置文件中配置，也可以通过properties元素的子元素来传递

<Propertise></Properties>需要放在核心配置文件的第一个，不然就会报错，

![image-20201212163648531](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201212163648531.png)

方式一： 现在项目的resources目录下建立db.properties, 然后在核心配置文件引入 <properties resource="db.properties" />就可以使用了；

```java
	<properties resource="db.properties" />

    <environments default="development">
        <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </dataSource>
    </environment>
```



注意1：除了从配置文件获取参数，我们还可以在properties里面设置参数，如果在配置里面设置了username,在db.properties也设置了username，则db.properties的优先级大于配置里面的

```xml
<properties resource="db.properties" >
    <property name="username" value="testuser"/>
    <property name="password" value="123456"/>
</properties>

```

在上述例子，db.properties有username =root, 配置中有  <property name="username" value="testuser"/> 使用的username是db.properties里面的属性



如果一个属性在不只一个地方进行了配置，那么，MyBatis 将按照下面的顺序来加载：

	- 首先读取在 properties 元素体内指定的属性
	- 然后根据 properties 元素中的 resource 属性读取类路径下属性文件，或根据 url 属性指定的路径读取属性文件，并覆盖之前读取过的同名属性
	- 最后读取作为方法参数传递的属性，并覆盖之前读取过的同名属性



### 4.4 类型别名(typeAliases)



类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。意在减少类完全限定名的冗余

```xml
<typeAliases>
    <typeAlias alias="user" type="com.kuang.pojo.User"/>
</typeAliases>
```

对应的在Mapper里面就可以写

```xml
原来
<select id="getUserList" resultType="com.kuang.pojo.User">
select * from mybatis.user;
</select>
更改后
<select id="getUserList" resultType="user">
select * from mybatis.user;
</select>
```

方式二：可以直接扫描一个包

可以指定一个包名，MyBatis会在包名下面搜索需要的Java Bean,比如： 扫描实体类的包，他的默认别名就为这个类起别名，首字母小写!

```
<typeAliases>
    <typeAlias alias="user" type="com.kuang.pojo.User"/>
    <package name="com.kuang.pojo"/>
</typeAliases>
```

在实体类比较少的时候，可以使用第一种方式，

如果实体类较多，十分多，建议使用第二种方式

第一种可以DIY别名，第二种不行，第二种默认自动生成小写字母!



###  4.5 设置

| 设置名             | 描述                                                         | 有效值        | 默认值 |
| :----------------- | :----------------------------------------------------------- | :------------ | :----- |
| cacheEnabled       | 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。     | true \| false | true   |
| lazyLoadingEnabled | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false | false  |

| useGeneratedKeys         | 允许 JDBC 支持自动生成主键，需要数据库驱动支持。如果设置为 true，将强制使用自动生成主键。尽管一些数据库驱动不支持此特性，但仍可正常工作（如 Derby）。 | true \| false | False |
| ------------------------ | ------------------------------------------------------------ | ------------- | ----- |
| mapUnderscoreToCamelCase | 是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。 | true \| false | False |

| logImpl | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。 | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设 |
| ------- | ----------------------------------------------------- | ------------------------------------------------------------ | ---- |
|         |                                                       |                                                              |      |



### 4.6  其他设置







### 7. 映射器

MapperRegistry :注册绑定我们的Mapper文件

方式1:

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

方式2：使用class文件进行绑定

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

注意点：

UserMapper.java和UserMapper.xml尽可能放到一个文件夹，

如果把UserMapper.xml放入到其他文件夹，则使用方式二的时候系统会报找不到对应的Mapper的错误

同时UserMapper.java和UserMapper.xml文件名需要都是UserMapper，如果改成UserDao.java，那么也会出现找不到对应的mapper的错误



- 接口和他的Mapper配置文件必须同名
- 接口和他的Mapper配置文件必须同文件夹

方式三。使用扫描包注入进行绑定，注意事项和方式二一样



练习作业：

	- 将数据库配置文件外部引入
	- 实体类别名
	- 保证UserMapper和UserMapper.xml改为一致并且放在同一个包下面

### 8 声明周期和作用域

生命周期和作用域是至关重要的，，因为错误的使用会导致非常严重的并发问题

![image-20201212183142581](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201212183142581.png)

**SqlSessionFactoryBuilder**:

	- 一旦创建了SqlSessionFactory, 就不再需要SqlSessionFactoryBuild了，所以可以定于为局部变量

**SqlSessionFactory**

- 说白了就是可以想象为我们的数据库连接池
- 一旦创建了就应该在应用的运行期间一直存在， 没有任何理由丢弃它或者重新创建另一个实例，多次创建**SqlSessionFactoryBuilder**会导致程序崩溃
- 因此**SqlSessionFactoryBuilder**的最佳作用域是在应用作用域，程序开始就开始，程序结束再结束
- 最简单的是使用单例模式或者静态单例模式，全局只保存一个对象

**SqlSession**

- 连接到我们连接池的一个请求! 
- 请求完需要完毕，请求前需要开启，最佳的作用域是放到一个方法里面，用完之后就需要赶紧关闭掉，否则资源被占用! 这个关闭的操作是十分重要的!

![image-20201212183934691](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201212183934691.png)



这里面的每一个mapper，就代表一个具体的业务!



## 5. 解决属性名和字段名不一致的问题

### 5.1 问题模拟



数据库中的字段“

![image-20201212195513882](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201212195513882.png)

新建一个项目，拷贝之前的,测试实体类字段不一致的情况

```java
public class User {
    private int id;
    private String name;
    private String password;
}
```

测试后发现，因为sql数据库字段和User对象的属性名不同，从而导致出现password为空

```
User{id=4, name='张三丰', password='null'}
```

解决方法一：修改一下sql语句，把pwd as password

```xml
    <select id="getUserById" resultType="com.kuang.pojo.User">
        <!--select * from mybatis.user where id =#{id};-->
        select id,name,pwd as password from mybatis.user where id=#{id};
     </select>
```



### 5.2 resultMap

一个简单的结果集映射例子 ：



```
数据库字段   id   name   pwd
对象的属性   id   name   password
```



使用方法:

第一步，将Mapper中的resultType去掉，加上resultMap，如下图

```java
<select id="getUserById" resultMap="UserMap">
    select * from mybatis.user where id =#{id};
<!--select id,name,pwd as password from mybatis.user where id=#{id};-->
    </select>
```



resultMap = “UserMap”  这个名字自己随便起,但是要和第二步的结果集定义匹配上

第二步，在当前文件里，定义一个映射对

```xml
<resultMap id="UserMap" type="com.kuang.pojo.User">
    <!--property   实体类中的属性   column数据库中的字段-->
    <result column="id" property="id" />
    <result column="name" property="name" />
    <result column="pwd" property="password" />
</resultMap>
```

此处的id和 select里面的resultMap对应， type为映射的对象，property   实体类中的属性   column数据库中的字段

### 5.3 结果映射 resultMap

- `resultMap` 元素是 MyBatis 中最重要最强大的元素

- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。
- `ResultMap` 的优秀之处——你完全可以不用显式地配置它们, 什么不一样转什么, 上面的那个resultMap也可以写成下面的样子

```xml
    <!--结果集映射-->
    <resultMap id="UserMap" type="com.kuang.pojo.User">
        <!--property   实体类中的属性   column数据库中的字段-->
        <!--<result column="id" property="id" />-->
        <!--<result column="name" property="name" />-->
        <result column="pwd" property="password" />
    </resultMap>>
```

## 6. 日志设置

### 6.1 日志工厂

如果 数据库操作出现了异常，我们需要一个拍错，这时候日志就是最好的助手!

曾经:sout  debug

| logImpl | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。 | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING |
| ------- | ----------------------------------------------------- | :----------------------------------------------------------- |
|         |                                                       |                                                              |

如今:使用日志工厂

- slf4j
- log4j [掌握] 
- log4j2
- jdk-logging
- commons_logging
- STDOUT_LOGGING[掌握] 
- NO_LOGGING

在mybatis中具体使用那个日志，我们可以设置

**STDOUT_LOGGING 标准日志输出**

在mybatis核心配置文件中，配置我们的日志

```xml
<settings>
	<setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>settings>
```



![image-20201212215726401](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201212215726401.png)



### 6.2 log4j 日志实现

现在Mybatis的配置中设置，记住是大写的，并且格式一定要对，然后导入log4j的包

**什么是log4j：**

​	Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，

​	我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件，甚至是套接口服务器、[NT](https://baike.baidu.com/item/NT/3443842)的事件记录器、[UNIX](https://baike.baidu.com/item/UNIX) [Syslog](https://baike.baidu.com/item/Syslog)[守护进程](https://baike.baidu.com/item/守护进程/966835)等；

​	我们也可以控制每一条日志的输出格式；

​	通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。

​	这些可以通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码。

log4j的下载：

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```



log4j.properties

```properties
# priority  :debug<info<warn<error
#you cannot specify every priority with different file for log4j
log4j.rootLogger=DEBUG,console,file,info
#file
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File = ./log/kuang.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=[%p][%d{yy-mm-dd}][%c]%m%n
#console
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.Target=System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet = DEBUG
log4j.logger.java.sql.PrepareStatement=DEBUG


#info log
log4j.logger.info=info
log4j.appender.info=org.apache.log4j.DailyRollingFileAppender
log4j.appender.info.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.info.File=../log/info.log
log4j.appender.info.Append=true
log4j.appender.info.Threshold=INFO
log4j.appender.info.layout=org.apache.log4j.PatternLayout
log4j.appender.info.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
#debug log
log4j.logger.debug=debug
log4j.appender.debug=org.apache.log4j.DailyRollingFileAppender
log4j.appender.debug.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.debug.File=./src/com/hp/log/debug.log
log4j.appender.debug.Append=true
log4j.appender.debug.Threshold=DEBUG
log4j.appender.debug.layout=org.apache.log4j.PatternLayout 
log4j.appender.debug.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
#warn log
log4j.logger.warn=warn
log4j.appender.warn=org.apache.log4j.DailyRollingFileAppender 
log4j.appender.warn.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.warn.File=./src/com/hp/log/warn.log
log4j.appender.warn.Append=true
log4j.appender.warn.Threshold=WARN
log4j.appender.warn.layout=org.apache.log4j.PatternLayout 
log4j.appender.warn.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
#error
log4j.logger.error=error
log4j.appender.error=org.apache.log4j.DailyRollingFileAppender
log4j.appender.error.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.error.File=./src/com/hp/log/error.log 
log4j.appender.error.Append=true
log4j.appender.error.Threshold=ERROR 
log4j.appender.error.layout=org.apache.log4j.PatternLayout
log4j.appender.error.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
```



log4j的使用! 直接测试运行刚才的查询

![image-20201212223647843](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201212223647843.png)

### 6.3 简单使用



1.在要使用Log4j的类中，导入包import org.apache.log4j.Logger

2.日志对象，参数为当前类的class

```java
static Logger logger = Logger.getLogger(UserTest.class);
```

3. 设置日志级别

```java
@Test
public void testLog4j(){
logger.info("info:进入了testLog4j~~~我已经运行到这里了!");
logger.debug("debug:进入了testLog4j~~~我已经运行到这里了!");
logger.error("error:进入了testLog4j~~~我已经运行到这里了!");
}
```

### 6.4 log4j的日志规则

```
#如果使用pattern布局就要指定的打印信息的具体格式ConversionPattern，打印参数如下：

#%m 输出代码中指定的消息

#%p 输出优先级，即DEBUG，INFO，WARN，ERROR，FATAL

#%r 输出自应用启动到输出该log信息耗费的毫秒数

#%c 输出所属的类目，通常就是所在类的全名

#%t 输出产生该日志事件的线程名

#%n 输出一个回车换行符，Windows平台为“rn”，Unix平台为“n”

#%d 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyyy MMM dd HH:mm:ss,SSS}，输出类似：2002年10月18日 22：10：28，921

#%l 输出日志事件的发生位置，包括类目名、发生的线程，以及在代码中的行数。

#[QC]是log信息的开头，可以为任意字符，一般为项目简称。

#输出的信息

#[TS] DEBUG [main] AbstractBeanFactory.getBean(189) | Returning cached instance of singleton bean 'MyAutoProxy'
#
log4j.rootLogger=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy/MM/dd HH:mm:ss,S} %p [%c]  >>> %m %n
```

输出的结果：

![image-20210214121535889](F:\MD格式学习笔记库\狂神JAVA-13-MyBatis.assets\image-20210214121535889.png)



## 7、Mybatis的分页

思考：为什么要分页？ 减少数据的处理量

使用Limit分页

```
语法: select * from user limit startIndex,pageSize;
 select * from user limit 0,2;  每页显示2个，从第0个开始查，获取0,1
 select * from user limit 2,2;  每页显示2个，从第0个开始查，获取2,3
 select * from user limit 3；    #[0,n] 
```

### 7.1 使用Limit分页

使用Mybatis实现分页，核心就是sql, 下面是一个使用Limit分页的例子



step1、写接口  UserMapper

```java
public interface UserMapper {
    //根据ID查询用户
    User getUserById(int id);
    //分页查询,我们使用万能的map
    List<User> getUserByLimit(Map<String, Integer> map);
}
```



step2、写映射文件UserMapper.xml

```xml
<mapper namespace="com.kuang.dao.UserMapper">

    <!--结果集映射-->
    <resultMap id="UserMap" type="com.kuang.pojo.User">
        <!--property   实体类中的属性   column数据库中的字段-->
        <!--<result column="id" property="id" />-->
        <!--<result column="name" property="name" />-->
        <result column="pwd" property="password" />
    </resultMap>

    <select id="getUserById" resultMap="UserMap" resultType="com.kuang.pojo.User">
        select * from mybatis.user where id =#{id};
        <!--select id,name,pwd as password from mybatis.user where id=#{id};-->
    </select>
    
	<!--分页查询-->
    <select id="getUserByLimit" parameterType="map" resultMap="UserMap">
        select * from mybatis.user limit #{startIndex},#{pageSize};
    </select>

</mapper>
```



step3、测试代码 testGetUserByLimig

```java
    @Test
    public void testGetUserByLimig(){
        SqlSession sqlSession = Mybatis.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        Map<String, Integer> map = new HashMap<String, Integer>();
        map.put("startIndex",0);
        map.put("pageSize",2);
        List<User> userByLimit = mapper.getUserByLimit(map);
        for (User user : userByLimit) {
            System.out.println(user);
        }

        sqlSession.close();
    }
}
```



### 7.2 RowBounds分页

1、不再使用SQL实现分页，接口

```java
public interface UserMapper {
    //分页2，使用RowBounds
    List<User> getUserByRowBounds();
}
```



2、mapper.xml

```xml
<mapper namespace="com.kuang.dao.UserMapper">

    <!--结果集映射-->
    <resultMap id="UserMap" type="com.kuang.pojo.User">
        <!--property   实体类中的属性   column数据库中的字段-->
        <!--<result column="id" property="id" />-->
        <!--<result column="name" property="name" />-->
        <result column="pwd" property="password" />
    </resultMap>
    <!--分页2 查询 RowBounds-->
    <select id="getUserByRowBounds" resultMap="UserMap">
        select * from mybatis.user;
    </select>
</mapper>
```



3、测试

```java
@Test
public void getUserByRowBounds(){
    SqlSession sqlSession = Mybatis.getSqlSession();

    //RowBounds实现
    RowBounds rowBounds = new RowBounds(1,2);

    //通过Java代码层面实现分页
    List<User> objects = sqlSession.selectList("com.kuang.dao.UserMapper.getUserByRowBounds",null,rowBounds);

    for (User object : objects) {
        System.out.println(object);
    }
    sqlSession.close();
}
```

### 7.3 分页插件 mybatis pageHelper

![image-20201213152533578](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201213152533578.png)



了解即可，万一以后公司的架构师说，要使用，我们需要知道他是一个什么东西!

## 8、使用注解开发

### 8.1 面向接口编程

大家之前都学过面向对象编程，也学习过接口，但是真正的开发中，很多时候我们会选择面向接口编程，

根本原因，``解耦``，可拓展，提高复用，分层开发中，上层不用管具体的实现，大家都遵守共同的标准，使得开发变得容易，规范性更好

在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的，在这种情况下，各个对象内部是如何实现自己的，对系统设计人员来讲不那么重要了

而各个对象之间的协作关系则成为了系统设计的关键，小到不同类之间的通信，大到各个模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容，面向

接口编程就是指按照这种思想来编程。



关于接口的理解

接口从更深层的理解，应该是定义(规范，约束)与实现(实名分离的原则)的分离

接口的本身反映了设计人员对系统的抽象理解

接口应该有两类

	- 第一类对一个个体的抽象，他对应为一个抽象体(abstract class)
	- 第二类是一个个体某一方面的抽象，既形成一个抽象面(interface)

一个个体有可能有多个抽象面，抽象提与抽象面是有区别的。



三个面向的区别

面向对象是指，我们考虑问题时，以对象为单位，考虑他的属性及方法

面向过程是指 ，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现

接口设计与非接口设计是针对复用技术而言的， 与面向对象(过程)不是一个问题，更多的体现就是对系统整体的架构



### 8.2 使用注解开发

1、注解就是直接在接口上实现

2、需要在我们的mybatis核心配置文件中绑定接口

3、测试

本质，是我们反射机制实现

底层：动态代理模式

![image-20201213163511707](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201213163511707.png)

![image-20201213163635965](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201213163635965.png)

**Mybatis详细的执行流程!**



![image-20201213183347778](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201213183347778.png)

![image-20201213183359073](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201213183359073.png)



### 8.3 CRUD注解操作

我们可以在工具类创建的时候，实现自动提交事务, 这样就可以不用每次执行sqlSession.commit()的手动提交

```java
public static SqlSession getSqlSession(){
    //public SqlSession openSession(boolean autoCommit)
    return sqlSessionFactory.openSession(true);//这个地方的true就是autoCommit
}
```



一、查询注解方式

注意，这里面@Select("select * from user where id=#{id} and name=#{name}") 的id和name通 @param("id")是匹配的

```java
//1.通过id,name查询,方法存在多个参数，所有参数前面必须加上@Param注解
@Select("select * from user where id=#{id} and name=#{name}")
User getUserByIdName(@Param("id") int id, @Param("name") String name);
//2.通过id查询
@Select("select * from user where id=#{id}")
User getUserById(@Param("id") int id);
//3.查询用户
@Select("select * from user")
List<User> getUserList();
```

二、添加用户--注解方式

```java
//添加用户，对象类就直接用对象类，不用@param
@Insert("insert into user(id,name,pwd) values (#{id},#{name},#{password})")
int addUser(User user);
```

三、更改用户 -注解方式

```java
//修改用户，对象类直接用对象类，不用@param
@Update("update user set name = #{name}, pwd=#{password} where id =#{id}")
int updateUser(User user);
```



四、删除注解方式

```java
@Delete("delete from user where id =#{uid}")
int deleteUser(@Param("uid") int id);
```

`关于@Param()注解`

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上!
- 我们在SQL中引用的就是我们这里@Param("uid")中设定的属性名!



## 9.Lombok使用

使用步骤

1、在IDEA中安装Lambok

2、在Lombok中导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
    <scope>provided</scope>
</dependency>
```



3、可以使用的注解

```java
Features
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
experimental @var
@UtilityClass
Lombok config system
```

4、在实体类上面加注解

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
@EqualsAndHashCode
public class User {
    private int id;
    private String name;
    private String password;

}
```

## 10. 多对一的处理

![image-20201213212031483](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201213212031483.png)

- 多个学生，对应 一个老师
- 对于学生这边而言， **关联**...  多个学生关联一个老师  [多对一] association  is-a,张三是李老师的学生
- 对于老师这边而言，**集合**...一个老师有很多学生 [ 一对多]  collection   has a, 李老师有个学生叫张三

 

测试环境的Sql

```sql
use `mybatis`
create table `teacher`(
	`id` int(10) not null,
	`name` varchar(30) default null,
	primary key(`id`)
)engine=innodb default charset=utf8

insert into `teacher`(`id`,`name`)values(1,'秦老师');

create table `student`(
	`id` int(10) not null,
	`name` varchar(30) default null,
	`tid` int(10) default null,
	primary key(`id`),
	key `fktid`(`tid`),
	constraint `sktid` foreign key(`tid`) references `teacher`(`id`)
)engine=innodb default charset=utf8

insert into `student`(`id`,`name`,`tid`) values('1','小明','1');
insert into `student`(`id`,`name`,`tid`) values('2','小红','1');
insert into `student`(`id`,`name`,`tid`) values('3','小张','1');
insert into `student`(`id`,`name`,`tid`) values('4','小李','1');
insert into `student`(`id`,`name`,`tid`) values('5','小王','1');
```



**搭建测试环境步骤**

1、 导入lombok

2、新建实体类Teacher，Student

```java
@Data
@ToString
public class Teacher {
    private int id;
    private String name;
}
```



3、建立Mapper接口

```java
public interface TeacherMapper {
    //查询教师
    @Select("select * from teacher where id = #{tid}")
    Teacher getTeacherById(@Param("tid") int id);
}
```



4、建立Mapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.TeacherMapper">

</mapper>
```



5、在核心配置文件中绑定注册我们的Mapper接口或者文件![方式很多，随心选择]

```xml
<mappers>
    <mapper class="com.kuang.dao.TeacherMapper"/>
    <mapper resource="com/kuang/dao/StudentMapper.xml"/>
</mappers>
```

6、测试查询是否能够成功

```java
    @Test
    public void getTeacherById(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
        Teacher teacher = mapper.getTeacherById(1);
        System.out.println(teacher);


        sqlSession.close();
    }
```



### 10.1 按照查询嵌套处理

需求1:获取学生信息以及对应的老师信息

```xml
<mapper namespace="com.kuang.dao.StudentMapper">
    <!--查询所有学生信息及对应的老师信息
        问题:Teacher是一个对象，拿到的是空值
        解决思路:
            1.查询所有的学生信息
            2.根据查询出来的学生的tid，寻找对应的老师
    -->

    <!--<select id="getStudentList" resultType="com.kuang.pojo.Student">-->
    <!--    select s.id,s.name,t.name from student s,teacher t  where s.tid=t.id;-->
    <!--</select>-->
    <!--按照查询，嵌套处理：上面这个方法无法解决我们的问题，因为Teacher是一个对象，而不是一个字段，-->
    <select id="getStudentList" resultMap="StudentTeacher">
        select * from student;
    </select>

    <resultMap id="StudentTeacher" type="com.kuang.pojo.Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--对于Teacher是对象怎么写呢 : 复杂的属性，我们需要单独处理  对象:association 集合:collection-->
        <association property="teacher" column="tid" javaType="com.kuang.pojo.Teacher" select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" resultType="com.kuang.pojo.Teacher">
        select * from teacher where id =#{id};
    </select>
</mapper>

`注意`： select * from teacher where id =#{id}; 这里面的#{id}可以随便填写任意数据，比如#{affsdfasdfasd}都是可以的，因为
<association property="teacher" column="tid" javaType="com.kuang.pojo.Teacher" select="getTeacher"/> 这里只有一个property
```

### 10.2 按照结果嵌套处理

```xml
<!--方式一、按照查询，嵌套处理：上面这个方法无法解决我们的问题，因为Teacher是一个对象，而不是一个字段，-->
<select id="getStudentList" resultMap="StudentTeacher">
    select * from student;
</select>

<resultMap id="StudentTeacher" type="com.kuang.pojo.Student">
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <!--对于Teacher是对象怎么写呢 : 复杂的属性，我们需要单独处理  对象:association 集合:collection-->
    <association property="teacher" column="tid" javaType="com.kuang.pojo.Teacher" select="getTeacher"/>
</resultMap>

<select id="getTeacher" resultType="com.kuang.pojo.Teacher">
    select * from teacher where id =#{id};
</select>

<!--方式二、按照结果嵌套处理-->
<select id="getStudentList2" resultMap="StudentTeacher2">
    select s.id sid,s.name sname, t.name tname
    from student s, teacher t
    where s.tid = t.id
</select>

<resultMap id="StudentTeacher2" type="com.kuang.pojo.Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="com.kuang.pojo.Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```



回顾Mysql多对一查询方式；

一种叫做子查询

```sql
select id,name,tid from student
    where tid=(select tid from teacher)
```



一种叫做连表查询

```sql
select s.id sid,s.name sname, t.name tname
from student s, teacher t
where s.tid = t.id;
```

## 11 一对多查询

比如：一个老师拥有多个学生，对于老师而言，就是1对多的关系，集合

### 11.1 按照结果嵌套处理

1、环境搭建，和刚才一样

老师的实体类和学生的实体类改变一下

```java
//学生实体类
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

```java
//老师的实体类
public class Teacher {
    private int id;
    private String name;
    //一个老师拥有多个学生
    private List<Student> students;
}
```

**和多对一不同地方**

- Student实体类，在多对一，关联的时候， 学生的实体放入的是 private Teacher teacher; 现在变为了  private int tid;
- Teacher实体类，在多对一，关联的时候，老师的实体类放入的只有id和name，现在变成了新加一个学生集合：private List<Student> students;



需求：获取指定老师下的所有学生及老师的信息

接口信息：

```java
public interface TeacherMapper {
    //获取老师
    // List<Teacher> getTeacher();
    //获取指定老师下的所有学生及老师信息
    Teacher getTeacher(@Param("tid") int id);
    //方式2 子查询方式
    Teacher getTeacher2(@Param("tid") int id);
}
```

接口映射文件信息

```xml
<mapper namespace="com.kuang.dao.TeacherMapper">
    <!--方案一：按照结果嵌套查询  获得所有老师-->
    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid,s.name sname,t.name tname, t.id tid
        from student s,teacher t
        where s.tid=t.id and t.id=#{tid}
    </select>
    
    <resultMap id="TeacherStudent" type="com.kuang.pojo.Teacher">
        <result property="name" column="tname"/>
        <result property="id" column="tid"/>
        <!--复杂的属性，我们需要单独处理，对象association 集合 collection
            association  使用javaType 指定一个java类型的属性类型
            collection   使用list 集合中的泛型信息，我们使用 ofType获取
         -->
        <collection property="students" ofType="com.kuang.pojo.Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>
    </resultMap>
    
</mapper>
```

测试类

```
    @Test
    public void getTeacher(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
       Teacher teacher = mapper.getTeacher(1);

       System.out.println(teacher);
       
        sqlSession.close();
    }
```

测试结果:

```verilog
Teacher(id=1, name=秦老师, students=[Student(id=1, name=小明, tid=1), Student(id=2, name=小红, tid=1), Student(id=3, name=小张, tid=1), Student(id=4, name=小李, tid=1), Student(id=5, name=小王, tid=1)])
```



### 11.2 按照查询嵌套处理

方式二： 子查询方式使用

```xml
    <!--======================-->
    <!--方案二：子查询方式-->
    <select id="getTeacher2" resultMap="TeacherStudent2">
        select * from teacher where id = #{tid}
    </select>

    <resultMap id="TeacherStudent" type="com.kuang.pojo.Teacher">
        <!--记住集合有javaType-->
        <collection property="students" column="id" javaType="ArrayList" ofType="com.kuang.pojo.Student" select="getStudentTByTeacherID"/>
        <!--这里记住，这个里面的column是上面select查询结果的id，返回给下面select语句作为参数 tid-->
    </resultMap>

    <select id="getStudentTByTeacherID" resultType="com.kuang.pojo.Student">
        select * from student where tid = #{tid}
    </select>

`注意`： select * from teacher where id =#{id}; 这里面的#{id}可以随便填写任意数据，比如#{affsdfasdfasd}都是可以的，因为
<association property="teacher" column="tid" javaType="com.kuang.pojo.Teacher" select="getTeacher"/> 这里只有一个property
```





### 11.3 一对多小结

​	1、关联 association  多个学生关联一个老师，多对一  

​	2、集合 collection  一个老师包含多个学生  1对多

​	3、javaType & ofType

​			javaType用来指定实体类型中属性的类型

​	  	 ofType用来指定映射到List或者集合里面的pojo类型，类似泛型中的约束类型





注意点

	- 保证SQL的可读性，尽量保证通俗一度
	- 注意一对多和多对一种，属性名和字段的问题
	- 如果问题不好排查错误，可以使用日志，建议使用log4j



慢SQL  别人查询1s，你查询可能需要1000S，可能直接就被开除了，

面试必问的：

​	mysql innodb和其他引擎底层原理和区别， 

​	索引及索引的优化 存储的过程





## 12.动态SQL

什么是动态SQL    动态SQL就是指根据不同的条件生成不同的SQL语句

借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

- if
- choose (when, otherwise)
- trim (where, set)
- foreach



### 12.1 搭建开发环境

准备sql语句

```sql
use `mybatis`

create table `blog`(
	`id` varchar(50) not null comment '博客id',
	`title` varchar(100) not null comment '博客标题',
	`author` varchar(30) not null comment '博客作者',
	`create_time` datetime not null comment '创建时间',
	`views` int(30) not null comment '浏览量'
)engine=innodb default charset=utf8


```



创建一个基础工程

1、导包

2、编写核心配置文件

3、编写实体类

4、编写实体类对应的Mapper接口及和实体类丢应的Mapper.xml

### 12.2 if 动态sql

dao接口

```java
public interface BlogMapper {
    //插入数据
    int addBlog(Blog blog);
    //查询博客
    List<Blog> queryBlogIF(Map map);
}
```

xml配置文件

```xml
<mapper namespace="com.kuang.dao.BlogMapper">
    <insert id="addBlog" parameterType="Blog">
        insert into mybatis.blog (id,title,author,create_time,views)
        values (#{id},#{title},#{author},#{createTime},#{views});
    </insert>

    <select id="queryBlogIF" parameterType="map" resultType="Blog">
        select * from blog where 1=1
        <if test="title!=null">
            and title = #{title}
        </if>
        <if test="author!=null">
            and author =#{author}
        </if>
    </select>

</mapper>
```





测试代码

```java
public class TestBlogMapper {
    @Test
    public void test(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        Blog blog = new Blog();


        blog.setId(UUIDUtils.getUUID());
        blog.setTitle("Mybatis如此简单");
        blog.setAuthor("狂神说");
        blog.setCreateTime(new Date());
        blog.setViews(9999);
        //System.out.println("是否乱码"+blog);
        mapper.addBlog(blog);

        blog.setId(UUIDUtils.getUUID());
        blog.setTitle("JAVA如何简单");
        mapper.addBlog(blog);

        blog.setId(UUIDUtils.getUUID());
        blog.setTitle("SPring如何简单");
        mapper.addBlog(blog);

        blog.setId(UUIDUtils.getUUID());
        blog.setTitle("微服务如此简单");
        mapper.addBlog(blog);

        sqlSession.close();
    }
    @Test
    public void queryBlogIF(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        Map map = new HashMap();
        //map.put("title","JAVA如何简单");
        map.put("author","狂神说");

        List<Blog> blogs = mapper.queryBlogIF(map);
        for (Blog blog : blogs) {
            System.out.println(blog);
        }

        sqlSession.close();
    }
}
```



带有where的语句

*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。

```xml
<select id="findActiveBlogLike"  resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
         state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

### 12.3 choose(when,otherwise)

接口

```java
    //查询博客
    List<Blog> queryBlogChoose(Map map);
```





```xml
    <select id="queryBlogChoose" parameterType="map" resultType="blog">
        select * from blog
        <where>
            <choose>
                <when test="title!=null">
                    title =#{title}
                </when>
                <when test="author!=null">
                    and author=#{author}
                </when>
                <otherwise>
                    and views = #{views}
                </otherwise>
            </choose>
        </where>
    </select>
```

测试语句

```java
    @Test
    public void queryBlogChoose(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        Map map = new HashMap();
        //map.put("title","JAVA如何简单");
        //map.put("author","狂神说");
        //map.put("author","狂神说");
        map.put("views",9999);

        List<Blog> blogs = mapper.queryBlogChoose(map);

        for (Blog blog : blogs) {
            System.out.println(blog);
        }

        sqlSession.close();
    }
```

作用总结：



### 12.4 trim(where ,set)



解释：*set* 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的），*set* 元素可以用于动态包含需要更新的列，忽略其它不更新的列



例子： 我们更新一个博客

```java
//更新一个博客
int updateBlog(Map map);
```



配置

```xml
<update id="updateBlog" parameterType="map">
    <!--update blog set id=#{id},name=#{name} where id=#{id}-->
    update blog
    <set>
        <if test="title!=null">
            title =#{title},
        </if>
        <if test="author!=null">
            author =#{author},
        </if>
    </set>
    where id=#{id}
</update>
```

测试代码

```java
    @Test
    public void updateBlog(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        Map map = new HashMap();
        map.put("title","update JAVA如何简单");
        map.put("author","update狂神说");
        //map.put("views",9999);
        map.put("id","e08bf90edb6f4983b09c0cad995be57c");
        int i = mapper.updateBlog(map);
        System.out.println(i);
        sqlSession.close();
    }
```

set元素会动态前置set关键字，同时也会删除掉无关的逗号!

所谓的动态SQL，本质还是SQL语句，只是我么可以在sql外面，去执行一个逻辑 if  where chooe set when

### 12.5 Foreach语句及Sql片段

```sql
select * from user where 1=1 and(id=1 or id =2 or id =3)  //查询id为1，或者2，或者3的用户，这种我们就可以用foreach处理
```



**sql片段**

有的时候，我们可能将一些功能的公共部分抽取出来，方便通用

使用sql片段，需要先定义，后饮用

定义

```xml
<sql id="if-title-author">
    <if test="title!=null">
        title =#{title},
    </if>
    <if test="author!=null">
        author =#{author},
    </if>
</sql>
```

引用形式:

```xml
<sql id="if-title-author">
    <if test="title!=null">
        title =#{title},
    </if>
    <if test="author!=null">
        author =#{author},
    </if>
</sql>
<update id="updateBlog" parameterType="map">
    <!--update blog set id=#{id},name=#{name} where id=#{id}-->
    update blog
    <set>
        <include refid="if-title-author"></include>
    </set>
    where id=#{id}
</update>
```

注意事项：

​	最好基于单表来定义SQL片段，这样复用性更高

​	不要提where标签，因为where会自定优化我们的sql,去掉and等字符。所以不适合放入到sql片段中





**ForEach**

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM user u
  WHERE 1=1 and
  <foreach item="id" index="index" collection="ids"
      open="(" separator="or" close=")">
        #{id}
  </foreach>
</select>

(id=1 or id=2 or id=3)


```

对应接口方法

```java
    //查询第1-2-3号记录的博客
    List<Blog> queryBlogByForeach(Map map);
```



测试接口配置

```xml
    <!--select * from blog where 1=3 and (id=1 or id=2 or id=3)
        我们现在传递一个万能的map,这个map中的可以存在一个集合
    -->
    <select id="queryBlogByForeach" parameterType="map" resultType="blog">

        select * from blog
        <where>
            <foreach collection="ids" item="id" open="and(" close=")" separator="or">
                id = #{id}
            </foreach>
        </where>
    </select>
```

测试代码

```java
@Test
public void queryBlogByForeach(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
    Map map = new HashMap();
    List<Integer> ids = new ArrayList<Integer>();
    ids.add(1);
    ids.add(2);
    ids.add(3);

    map.put("ids",ids);

    List<Blog> blogs = mapper.queryBlogByForeach(map);
    sqlSession.close();
}
```

**动态sql就是在拼接sql语句，我们只要保证sql的正确性，按照sql的格式，去排列组合就可以了!**

建议

​	先在mysql中写出完整的sql,再对应的去修改成为我们的动态sql，实现通用即可；

### 13 mybatis缓存

### 13.1 简介

>  查询，连接数据库，耗费资源
>
> ​	字词查询的结构，给他暂存在一个可以直接取到的地方---内存；
>
> 我们再次查询相同数据的时候，直接走缓存，就不用走数据库了



1、什么是缓存[cache]

​	存在内存中的临时数据

​	将用户经常查询的数据放在缓存(内存)中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题

2、为什么使用缓存

​	减少和数据库的交互次数，减少系统的开销，提高系统的效率

3、什么样子的数据库能使用缓存

​	经常查询并且不经常改变的数据。



### 13.2 Mybatis缓存

mybatis包含一个非常强大的查询缓存特性，他可以非常方便的定制和配置缓存，缓存可以极大的提升查询效率

mybatis系统中默认定义了两级缓存，一级缓存和二级缓存

​	- 默认情况下，只有一级缓存开启，sqlsession级别的缓存，也成为了本地缓存

​	- 二级缓存需要手动开启和配置，他是基于namespace级别的缓存, 也就是在一个mapper里面，这个缓存都是有效的

​	- 为了提高扩展性，mybatis定义了缓存接口Cache，我们可以通过Cache来自定义二级缓存



### 13.3 一级缓存

一级缓存也叫本地缓存，生命周期就是一个sqlSession内

​	与数据库同一次会话期间查询到的数据会放在本地缓存中；

​	以后如果需要获取相同的数据，直接从缓存中拿，就没必须再去查询数据库；



可以测试一下，测试步骤：

开启日志

测试在一个Session中查询两次相同的记录

查看日志输出的情况

![image-20201215160555080](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201215160555080.png)

缓存失效的情况：

​	映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存

​	缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存

​	缓存不会定时进行刷新（也就是说，没有刷新间隔）

​	缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用

​	缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改



1、增删改操作，可能会改变原来的数据，所以必定会刷新缓存!

2、查询不同的东西

3、查询不同的Mapper.xml

4、手动删除缓存  sqlSession.clearCache(); //清理缓存，会导致重新查询 缓存失效



一级缓存默认开启，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间段!

一级缓存就是一个bug



### 13.4 二级缓存

二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存，

基于Namespace级别的缓存，一个名称空间，对应一个二级缓存

工作机制：

​	一个绘画查询一条数据，这个数据就会被放入到当前会话的一级缓存中；

​	如果当前会话关闭了，这个会话对应的一级缓存就没有了，但是我们想要的是，会话关闭了，一级缓存中数据被保存到了二级缓存中；

​	新的绘画查询信息，就可以从二级缓存中获取内容；

​	不同的mapper查出来的数据会放入自己对应的缓存(map)中

使用二级缓存的步骤：

1、先去核心配置中，设置开启核心缓存

```xml
<settings>
    <!--显示的开启全局缓存-二季缓存-->
    <setting name="cacheEnabled" value="true"/>
    <!--指定 MyBatis 所用日志的具体实现-->
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

2、到需要开启二级缓存的Mapper.xml二级缓存的配置文件中增加<cache/> 标签，即可使用

```xml
<cache/>
```



也可以自定义一些参数

```xml
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

![image-20201215162632872](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201215162632872.png)



测试：

```java
    @Test
    public void test2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        SqlSession sqlSession2 = MybatisUtils.getSqlSession();
        
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);

        User user = mapper.queryUserById(1);
        System.out.println(user);
        System.out.println("====================");

        User user2 = mapper2.queryUserById(1);
        System.out.println(user2);
        System.out.println("====================");

        sqlSession.close();
        sqlSession2.close();
    }
```

小结：

​	只要开启了二级缓存，在同一个mapper下就有效

​	所有的数据都会先放入一级缓存

​	只有当绘画提交或者关闭，这时候才会提交到耳机缓存

### 13.5 缓存原理

![image-20201215183826712](F:\MD格式学习笔记库\狂神JVA-13-MyBatis.assets\image-20201215183826712.png)



### 13.6 自定义缓存-ehcache



要在程序中使用ehcache，需要先导入包!

```pom
<dependency>
<groupId>org.mybatis.caches</groupId>
<artifactId>mybatis-ehcache</artifactId>
<version>1.1.0</version>
</dependency>
```

在mapper.xml中进行配置

```xml
<mapper namespace="com.kuang.dao.UserMapper">
    <!--在当前Mapper.xml中使用ehcache-->
    <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
    
    <select id="queryUserById" parameterType="_int" resultType="user">
        select * from user where id = #{id}
    </select>
</mapper>
```





ehcache的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
     <!--
 5         磁盘存储:将缓存中暂时不使用的对象,转移到硬盘,类似于Windows系统的虚拟内存
 6         path:指定在硬盘上存储对象的路径
 7      -->
     <diskStore path="./tmpdir/Tmp_EhCache" />


     <!--
         defaultCache:默认的缓存配置信息,如果不加特殊说明,则所有对象按照此配置项处理
         maxElementsInMemory:设置了缓存的上限,最多存储多少个记录对象
        eternal:代表对象是否永不过期
         timeToIdleSeconds:最大的发呆时间
        timeToLiveSeconds:最大的存活时间
         overflowToDisk:是否允许对象被写入到磁盘
     -->
     <defaultCache
             eternal="false"
             maxElementsInMemory="10000"
             overflowToDisk="false"
             diskPersistent="false"
             timeToIdleSeconds="1800"
             timeToLiveSeconds="259200"
             memoryStoreEvictionPolicy="LRU" />

     <!--
         cache:为指定名称的对象进行缓存的特殊配置
         name:指定对象的完整名
      -->
     <cache name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"
     />

 </ehcache>
```



## 13 mybatis作业

```
#1 //通过UserCode获取User
public User getLoginUser(@Param("userCode") String userCode) throws Excepti
```

碰到的错误:



### 练习3 

```java
public List<User> getUserList(@Param("userName") String userName,
                              @Param("useRole") Integer useRole,
                              @Param("from") Integer currentPageNo,
                              @Param("pageSize") Integer pageSize) throws Exception;
                              
    <select id="getUserList" resultType="user">
        <!--select * from smbms_user where userName=userName and useRole= useRole limit currentPageNo=currentPageNo and pageSize=pageSize-->
        select * from smbms_user
        <where>
            <if test="userName!=null">
                userName = #{userName}
            </if>
            <if test="useRole!=null">
                and userRole= #{useRole}
            </if>
        </where>
        limit
        <if test="from!=null">
           #{from}
         </if>
        <if test="pageSize!=null">
           , #{pageSize}
        </if>
    </select>                              
                              
# 碰到的错误
1、参数使用了@param，但是我在测试脚本使用map，导致提醒参数数量不一致，从而得到结论： @Param 需要直接传递给Mapper.xml，接口定义的不是Map，就不能用map
2、useRole这个参数，在数据库里面名字是userRole,但是参数定义为useRole， 需要在Mapper.xml里面  结论：@Param的名称和Mapper的要对应
3、传递的是4个参数，但是分页要用limit，并且分页的from和pageSize中间有个 逗号，记住得加上。
4、接口的形参名称，不影响测试方法和Mapper配置文件的名字，形参可以任意写，但是@Param的名称是传递到Mapper配置文件的
5、parameterType 这个是可加可不加，但遇到复杂类型，是需要加，不加就没法获取到对象或者map的属性
```

### 练习4

```
//通过条件查询-用户记录数
 public int getUserCount(@Param("userName") String userName,@Param("useRole") Integer useRole) throws Exception;
    
   <select id="getUserCount" resultType="int">

        select count(id) from smbms_user
        <where>
            <if test="userName!=null">
                userName =#{userName}
            </if>
            <if test="useRole!=null">
                and userRole =#{useRole}
            </if>
        </where>
    </select>

# 碰到的错误，
1、 我在上一个练习3，写了一个parameterType="" 空的值，在练习三的参数类型中，然后执行练习4，出现了错误日志： Could not resolve type alias ''.  Cause: java.lang.ClassNotFoundException: Cannot find class: 
   以后遇到类似这种无法解析 '' 可能就是参数填写空值了!
2、返回用户的年龄，因为birthday在数据库为空值，导致出现了错误，所以后续数据库数据，得校验一下是否是空
3、resultType="int" 这个在select语句是必填项，ExecutorException: A query was run and no Result Maps were found for the Mapped Statement 'com.kuang.dao.user.UserMapper.getUserCount'.  It's likely that neither a Result Type
```

### 练习5 

```java
//通过userId删除user
public int deleteUserById(@Param("id") Integer id) throws Exception;

<delete id="deleteUserById">
   delete from smbms_user where id =#{id}
</delete>
    
碰到错误：
    在select标签，写删除语句，会出现这个错误：Mapper method 'com.kuang.dao.user.UserMapper.deleteUserById attempted to return null from a method with a primitive return type (int).



Mapper method 'com.kuang.dao.user.UserMapper.deleteUserById attempted to return null from a method with a primitive return type (int).
```

### 练习6

```java
//通过userId获取user
public User getUserById(@Param("id") Integer id) throws Exception;


    <!--获取用户列表-->
    <select id="getUserById" resultMap="userMap">
        select u.*,r.roleName rname
        from smbms_user u,smbms_role r
        where  u.id = #{id} and u.userRole=r.id
    </select>
    <resultMap id="userMap" type="user">
        <result property="userRoleName" column="rname"/>
    </resultMap>
        
碰到的错误:
1、这里面 userRoleName这个字段，在User实体类，定义的是private String userRoleName, 这个值在Role实体中可以查询到，我以为应该使用关联，一开始我使用了
    <resultMap id="userMap" type="user">
        <association property="userRoleName" javaType="role">
            <result property="roleName" column="rname"/>
        </association>
    </resultMap>
这个是错误的，因为在User实体类，userRoleName并不是一个对象，只是一个基础数据类型，所以应该用下面形式：
     <resultMap id="userMap" type="user">
        <result property="userRoleName" column="rname"/>
    </resultMap>
```

### 练习7 

```java
//修改用户信息
public int modify(User user) throws Exception;

    <!--修改用户信息-->
    <update id="modify" parameterType="user">
        update smbms_user
        <set>
            <if test="userCode != null">userCode=#{userCode},</if>
            <if test="userName != null">userName=#{userName},</if>
            <if test="userPassword != null">userPassword=#{userPassword},</if>
            <if test="gender != null">gender=#{gender},</if>
            <if test="birthday != null">birthday=#{birthday},</if>
            <if test="phone != null">phone=#{phone},</if>
            <if test="address != null">address=#{address},</if>
            <if test="userRole!= null">userRole=#{userRole},</if>
            <if test="createdBy!= null">createdBy=#{createdBy},</if>
            <if test="creationDate != null">creationDate=#{creationDate},</if>
            <if test="modifyBy != modifyBy">modifyBy=#{modifyBy},</if>
            <if test="modifyDate != null">modifyDate=#{modifyDate}</if>
        </set>
        where id=#{id};
    </update>
        
碰到的错误:
1、 <if test="gender != null">gender=#{gender},</if>  少了一个逗号，导致系统插入失败，给出的报错信息
    You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'modifyDate='2020-12-16 14:09:28.322'
```

### 练习8

```java
//修改当前用户的密码
public int modifyPwd(@Param("id") Integer id, @Param("userPassword") String pwd) throws Exception;

<update id="modifyPwd">
        update smbms_user
        set userPassword=#{userPassword}
</update>
    
    
```

### 练习9

```java
//获取角色列表
 public List<Role> getRoleList() throws Exception;

 <select id="getRoleList" resultType="role">
    select * from smbms_role;
</select>
```

### 练习10

```
    //增加角色信息
    public int add(Role role) throws Exception
    
        <insert id="add" parameterType="role">
        insert into smbms_role
         (roleCode,roleName,createdBy,creationDate)
         values (#{roleCode},#{roleName},#{createdBy},#{creationDate});
    </insert>
```

### 练习11

````java
 //通过Id删除role
 public int deleteRoleById(@Param("Id") Integer delId) throws Exception;

    <!--通过Id删除role-->
    <delete id="deleteRoleById" parameterType="int">
        delete from smbms_role where id =${Id}
    </delete>
````

### 练习12 

```java
   //修改角色信息
    public int modify(Role role) throws Exception;


    <update id="modify" parameterType="role">
        update smbms_role
        <set>
            <if test="roleCode!=null">roleCode =#{roleCode},</if>
            <if test="roleName!=null">roleName =#{roleName},</if>
            <if test="createdBy!=null">createdBy =#{createdBy},</if>
            <if test="creationDate!=null">creationDate =#{creationDate},</if>
            <if test="modifyBy!=null">modifyBy =#{modifyBy},</if>
            <if test="modifyDate!=null">modifyDate =#{modifyDate}</if>
        </set>
        where id=#{id};
    </update>
        
碰到的错误:
1、在测试使用的时候，设置了role，但是role 的id未设置，导致修改失败，但是系统未报错
    
```

### 练习13

```java
//通过id获取role
 public Role getRoleById(@Param("id") Integer id) throws Exception;

    <!--通过id获取role-->
    <select id="getRoleById" parameterType="int" resultType="role">
        select * from smbms_role where id = #{id};
    </select>
```

### 练习14

```java
    //根据roleCode,进行角色编码的唯一性校验
    public int roleCodeIsExist(@Param("roleCode") String roleCode) throws Exception;

    
    <select id="roleCodeIsExist" parameterType="string" resultType="int">
        select id from smbms_role where roleCode=#{roleCode};
    </select>

碰到的错误:
1、resultType 填写int默认返回的是当前记录的id
2、Mapper method 'com.kuang.dao.role.RoleMapper.roleCodeIsExist attempted to return null from a method with a primitive return type (int).
    这是因为id可能为空，解决办法
       <select id="roleCodeIsExist" parameterType="string" resultType="int">
        select coalesce(max(id),0) as id from smbms_role where roleCode=#{roleCode};
    </select>
```

### 练习15 

```
    //增加用户信息
    public int add(Provider provider) throws Exception;
    
        <insert id="add" parameterType="provider">
        insert into smbms_provider
        (proCode,proName,proDesc,proContact,proPhone,proAddress,proFax,createdBy,creationDate)
        values (#{proCode},#{proName},#{proDesc},#{proContact},#{proPhone},#{proAddress},#{proFax},#{createdBy},#{creationDate});
    </insert>
```

### 练习16 

```java
//通过条件查询-providerList
public List<Provider> getProviderList(@Param("proName") String proName,@Param("proCode") String proCode,
                                          @Param("from") Integer currentPageNo,@Param("pageSize") Integer pageSize) throws Exception;

    <select id="getProviderList" resultType="provider">
        select * from smbms_provider
        <where>
            <if test="proCode!=null">proCode=#{proCode}</if>
            <if test="proName!=null">and proName like "%"#{proName}"%"</if>
        </where>
        limit
        <if test="from!=null">#{from}</if>
        <if test="pageSize!=null">,#{pageSize}</if>
    </select> 
```

### 练习17

```
   //获取供应商列表
    public List<Provider> getProList() throws Exception;
    
        <select id="getProList" resultType="provider">
        select * from smbms_provider;
    </select>
```



### 练习18

```java
    //通过条件查询-供应商表记录数
    public int getProviderCount(@Param("proName") String proName,@Param("proCode") String proCode) throws Exception
        
        
    <select id="getProviderCount" resultType="int">
        select count(id) from smbms_provider
        <where>
            <if test="proName!=null">
                proName =#{proName}
            </if>
            <if test="proCode!=null">
                and proCode =#{proCode}
            </if>
        </where>
    </select> 
```

### 练习19

```java
    //通过供应商id供应商信息
    public Provider getProviderById(@Param("id") Integer id) throws Exception;


    <!--通过供应商id供应商信息-->
    <select id="getProviderById" resultType="provider">
        select * from smbms_provider where id  =#{id}
    </select>
```

### 练习20

```java
   //修改供应商
    public int modify(Provider provider) throws Exception;


    <update id="modify" parameterType="provider" >
        update smbms_provider
        <set>
            <if test="proCode!=null"> proCode =#{proCode},</if>
            <if test="proName!=null">proName =#{proName},</if>
            <if test="proDesc!=null">proDesc =#{proDesc},</if>
            <if test="proContact!=null">proContact =#{proContact},</if>
            <if test="proPhone!=null">proPhone =#{proPhone},</if>
            <if test="proAddress!=null">proAddress =#{proAddress},</if>
            <if test="proFax!=null">proFax =#{proFax},</if>
            <if test="modifyBy!=null"> modifyBy=#{modifyBy},</if>
            <if test="modifyDate!=null">modifyDate =#{modifyDate}</if>
        </set>
        where id =#{id};
    </update>
```

### 练习21 根据供应商id查询订单量

```java
    //根据供应商id查询订单量
    public int getBillCountByProviderId(@Param("providerId") Integer providerId) throws Exception;

    <!--根据供应商id查询订单量-->
    <select id="getBillCountByProviderId" resultType="int">
        select count(id) from smbms_bill where providerId=#{providerId};
    </select>

```

### 练习22 增加订单

```java
    //增加订单
    public int add(Bill bill) throws Exception;

    <insert id="add" parameterType="bill">
        insert into smbms_bill
        (billCode,productName,productDesc,productUnit,productCount,totalPrice,isPayment,createdBy,creationDate,providerId)
         values (#{billCode},#{productName},#{productDesc},#{productUnit},#{productCount},#{totalPrice},#{isPayment},
         #{createdBy},#{creationDate},#{providerId});
    </insert>

```

### 练习23 通过查询条件后去供应商列表 getBillList

```java
    public List<Bill> getBillList(@Param("productName") String productName, @Param("providerId") Integer providerId,
                                  @Param("ispay") Integer ispay, @Param("from") Integer currentPageNo, @Param("pageSize") Integer pageSize) throws Exception;


    <!--通过查询条件后去供应商列表 getBillList-->
    <select id="getBillList" resultType="bill">
       select * from smbms_bill
       <where>
           <if test="productName!=null">productName=#{productName}</if>
           <if test="providerId!=null">and providerId=#{providerId}</if>
           <if test="ispay!=null">and isPayment=#{ispay}</if>
       </where>
           limit
            <if test="from!=null">#{from}</if>
            <if test="pageSize!=null">,#{pageSize}</if>
    </select>

```

### 练习24 通过条件查询-订单表记录数

```java
    //通过条件查询-订单表记录数
    public int getBillCount(@Param("productName") String productName,@Param("providerId") Integer providerId, @Param("ispay") Integer ispay)throws Exception;


```

### 练习25 通过delId删除Bill

```java
    //通过delId删除Bill
    public int deleteBillById(@Param("id") Integer delId) throws Exception;

    <!--通过delId删除Bill-->
    <delete id="deleteBillById">
        delete from smbms_bill where id =#{id};
    </delete>

```

### 练习26 通过billId获取Bill

```java
    //通过billId获取Bill
    public int getBillById(@Param("id") Integer delId) throws Exception;

    <!--    //通过billId获取Bill-->
    <select id="getBillById" parameterType="int" resultType="bill">
        select * from smbms_bill where id = #{id}
    </select>

```

### 练习27 修改订单信息

```java
    //修改订单信息
    public int modify(Bill bill) throws Exception;

    <!--修改订单信息-->
    <update id="modify" parameterType="bill" >
        update smbms_bill
        <set>
            <if test="billCode!=null"> billCode =#{billCode},</if>
            <if test="productName!=null">productName =#{productName},</if>
            <if test="productDesc!=null">productDesc =#{productDesc},</if>
            <if test="productUnit!=null">productUnit =#{productUnit},</if>
            <if test="productCount!=null">productCount =#{productCount},</if>
            <if test="totalPrice!=null">totalPrice =#{totalPrice},</if>
            <if test="isPayment!=null">isPayment =#{isPayment},</if>
            <if test="providerId!=null">providerId =#{providerId},</if>
            <if test="modifyBy!=null"> modifyBy=#{modifyBy},</if>
            <if test="modifyDate!=null">modifyDate =#{modifyDate}</if>
        </set>
        where id =#{id};
    </update>

```

### 练习28 根据供应商Id删除订单信息

```java
    //根据供应商Id删除订单信息
    public int deleteBillByProviderId(@Param("providerId") Integer providerId) throws Exception;
    
<delete id="deleteBillByProviderId">
        delete from smbms_bill where providerId=#{providerId};
    </delete>

```





