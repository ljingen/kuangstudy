# javaweb实现购物车

在网上看了一个前辈写的购物车，我自己照着用javaweb做一遍，刚好学完javaweb，里面放入一部分bootstrap，用这个练练手。

这个是在网上看了一个前辈写的购物车，我自己照着用javaweb做一遍，刚好学完javaweb，里面放入一部分bootstrap，用这个练练手。

已经全部完成了，当然里面还有很多bug，我会后续持续做，最近开始学springboot了，等学完再继续。

如果你有任何疑惑或者问题，可以联系我Q :1642302522

## 1. 环境准备

使用IDEA编辑器+JDK 1.8+ servlet 4.0+jsp2.3+mysql

step 1. 创建一个空的meven项目，项目名称为 javaweb-02-servlet,创建完成后，删除掉无关的src文件夹

step 2. 点击new-model，选择maven，创建一个名称为shoppingcar的子项目

`创建maven父子工程是为了后续方便其他测试项目统一文件管理`

![image-20210106094259072](F:\MD格式学习笔记库\总结1-servlet+jsp 实现一个购物车.assets\image-20210106094259072.png)

下一步

![image-20210106094340913](F:\MD格式学习笔记库\总结1-servlet+jsp 实现一个购物车.assets\image-20210106094340913.png)

step 3. 待项目创建成功后，点击子项目shoppingcart,邮件，选择[Add framework support]，在进入的页面选择web框架支持

step 4. 在创建完的项目，整理项目结构，创建包文件夹，创建后的文件结构如下图

![image-20210106094924442](F:\MD格式学习笔记库\总结1-servlet+jsp 实现一个购物车.assets\image-20210106094924442.png)

step 5. 准备数据库，并插入数据, 数据库sql脚本如下

```sql
create database `javaweb1`;
use `javaweb1`;

CREATE TABLE `book` (
  `id` int(11) NOT NULL auto_increment,
  `name` varchar(45) NOT NULL,
  `autor` varchar(45) NOT NULL,
  `publicHouse` varchar(45) NOT NULL,
  `price` decimal(20,2) DEFAULT NULL COMMENT '图书价格',
  `nums` int(11) NOT NULL DEFAULT '1000',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


INSERT into `book`(`id`,`name`,`autor`,`publicHouse`,`price`,`nums`) VALUES
(2,'mysql入门'	,'张三'	,'新东方',	146.32,	1000),
(3,'python入门'	,'张三'	,'新东方',	146.32,	1000),
(4,'menven入门'	,'张三'	,'新东方',	146.32,	1000),
(5,'tomcat入门'	,'张三'	,'新东方',	146.32,	1000),
(6,'javaweb入门'	,'张三'	,'新东方',	146.32,	1000),
(7,'spring入门'	,'张三'	,'新东方',	146.32,	1000),
(8,'springmvc'	,'张三'	,'新东方',	146.32,	1000),
(9,'think in java'	,'张三'	,'新东方',	146.32,	1000);


CREATE TABLE `orders` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) NOT NULL,
  `totalPrice` decimal(20,2) NOT NULL DEFAULT '0',
  `orderDate` date NOT NULL,
  PRIMARY KEY (`id`),
  KEY `id_idx` (`user_id`),
  CONSTRAINT `id` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;

CREATE TABLE `ordersitem` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `orders_id` int(11) NOT NULL,
  `bookid` int(11) NOT NULL,
  `booknum` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `id_idx1` (`bookid`),
  KEY `id1_idx` (`orders_id`),
  CONSTRAINT `id1` FOREIGN KEY (`orders_id`) REFERENCES `orders` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION,
  CONSTRAINT `id2` FOREIGN KEY (`bookid`) REFERENCES `book` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=18 DEFAULT CHARSET=utf8;

CREATE TABLE `user` (
  `id` int(11) NOT NULL,
  `name` varchar(45) NOT NULL,
  `pwd` varchar(45) NOT NULL,
  `email` varchar(45) NOT NULL,
  `tel` varchar(45) NOT NULL,
  `grade` int(11) NOT NULL DEFAULT '1',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

step 6. 在工程的resource/db.properties里面，添加如下配置

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/javaweb1?useUnicode=true&characterEncoding=UTF-8&useSSL=false
username=root
password=123456
```

step 7 . 在utils文件夹下，创建数据库操作类 JDBCUtils

```java
package com.kuang.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

/*基础的数据库操作类，供Mapper层调用基础数据库操作
* 1.读取配置文件并加载驱动
* 2.获取connection
* 3.数据库执行增 删 改操作.
* 4.数据库执行查询操作，根据具数据库连接和条件进行查询
* 5.释放资源操作
* */
public class JDBCUtils {
    private static String driver = null;
    private static String url = null;
    private static String username = null;
    private static String password = null;

    Connection conn = null;

    /*1. 读取配置文件加载驱动*/
    static {
        try {
            InputStream resourceAsStream = JDBCUtils.class.getClassLoader().getResourceAsStream("db.properties");
            Properties prop = new Properties();
            prop.load(resourceAsStream);
            driver = prop.getProperty("driver");
            url = prop.getProperty("url");
            username = prop.getProperty("username");
            password = prop.getProperty("password");
            //加载驱动
            Class.forName(driver);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /*2. 获取connection连接*/
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url,username,password);
    }


    //3 数据库执行增 删 改操作
    public static int executeUpdate(Connection conn, PreparedStatement prep, String sql, Object[] params) throws SQLException {
        //String sql = "update smbms_user set userName='赵敏敏' where userCode='zhaomin'"
        prep = conn.prepareStatement(sql);
        //使用PreparedStatement.setObject()设置参数的位置
        for (int i = 0; i < params.length; i++) {
            prep.setObject(i + 1, params[i]);
        }
        return prep.executeUpdate();
    }

    //4 数据库执行查询操作，根据具数据库连接和条件进行查询，返回ResultSet
    public static ResultSet executeQuery(Connection conn, PreparedStatement prep, ResultSet rs, String sql, Object[] params) throws SQLException {
        //String sql = "select * from  smbms_user where userCode='zhaomin'"

        prep = conn.prepareStatement(sql);
        //使用PreparedStatement.setObject()设置参数的位置
        for (int i = 0; i < params.length; i++) {
            prep.setObject(i + 1, params[i]);
        }
        return prep.executeQuery();
    }

    /*5. 释放连接*/
    public static boolean closeResource(Connection conn, PreparedStatement prep, ResultSet rs){
        boolean flag = false;

        if(rs!=null){
            try {
                rs.close();
                rs=null;
                flag=true;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(prep!=null){
            try {
                prep.close();
                prep=null;
                flag=true;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(conn!=null){
            try {
                conn.close();
                conn=null;
                flag=true;
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        return flag;
    }

}
```

step 8, 添加父工程maven的配置pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>javaweb-02-servlet</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>shoppingcart</module>
    </modules>
    <!--添加父项目依赖，子项目继承父项目-->
    <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>javax.servlet.jsp-api</artifactId>
            <version>2.3.3</version>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.49</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
        </dependency>
    </dependencies>

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
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

检查一下子工程shoppingcart的pom.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>shoppingcart</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <parent>
    <artifactId>javaweb-02-servlet</artifactId>
    <groupId>org.example</groupId>
    <version>1.0-SNAPSHOT</version>
  </parent>
</project>

```

maven更新成功后的依赖包关系如下，确认maven添加的依赖包进入到项目依赖

![image-20210106104813698](F:\MD格式学习笔记库\总结1-servlet+jsp 实现一个购物车.assets\image-20210106104813698.png)



step 9. 点击File-ProjectStructure项目结构，确认项目打包成功，项目包的目录都在发布的target文件中

![image-20210106105013027](F:\MD格式学习笔记库\总结1-servlet+jsp 实现一个购物车.assets\image-20210106105013027.png)



step 10, 配置tomcat ,启动测试



![image-20210106102408407](F:\MD格式学习笔记库\总结1-servlet+jsp 实现一个购物车.assets\image-20210106102408407.png)



配置tomcat有几个注意点：

1、要点击+ 号增加tomcat服务器，

2、需要配置好tomcat的jre环境

3、需要配置好tomcat deployment部署的包，同时配置服务器访问路径Application context



结果：启动tomcat，测试一下，如果正常此时应该能进入index，如果不行，那就是环境没有配置好，需要检查环境配置，确认

1、java环境变量配置了

2、tomcat环境变量配置了

3、maven配置了

4、确认web.xml是 4.0版本  version="4.0">

## 2.完成一个例子

### 2.1 在com.kuang.pojo中编写User.java 实体类

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
/*用户实体类*/
public class User {
    private int id;
    private String name;
    private String pwd;
    private String email;
    private String tel;
    private int grade;
}
```

` 实体类的属性需要和数据库的字段一一对应,变量名和字段名要一样，如果不对应后续再SSM架构很麻烦，所以从javaweb养成习惯`

### 2.2 在com.kuang.mapper中编写Dao操作

1、编写 public interface UserMapper.java 接口

```java
public interface UserMapper {
    /*检查用户登录*/
     boolean checkUser(User user);
    /*获取所有用户*/
     List<User> getAllUser(Connection conn);
    /*通过id获取用户*/
     User getUserById(Connection conn,int id);
    /*通过密码获取用户*/
     User getUserByPwd(Connection conn,int id, String pwd);
}
```

`1、定义方法时，我们一般用对应的 get/delete/update/query/add 等能够指明意思的词语来做前缀，有条件则加上by`

`2. Connection 一般放到service层，因为事务处理在service层`

2、编写 UserMapper接口的实现类  public class UserMapperImpl

```java
public class UserMapperImpl implements UserMapper {
    @Override
    public boolean checkUser(User user) {
        String sql = "select * from user where id=? and pwd =?";

        return false;
    }

    @Override
    public List<User> getAllUser(Connection conn) {
        String sql = "select * from user";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {};
        List<User> userList = new ArrayList<>();
        if (conn != null) {
            try {
                rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
                while (rs.next()) {
                    User user = new User();
                    user.setId(rs.getInt("id"));
                    user.setName(rs.getString("name"));
                    user.setPwd(rs.getString("pwd"));
                    user.setEmail(rs.getString("email"));
                    user.setTel(rs.getString("tel"));
                    user.setGrade(rs.getInt("grade"));
                    userList.add(user);
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                JDBCUtils.closeResource(null, prep, rs);
            }
            JDBCUtils.closeResource(null, prep, rs);
        }
        return userList;
    }

    @Override
    public User getUserById(Connection conn, int id) {
        String sql = "select * from user where id =?";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {id};
        User user = new User();
        if (conn != null) {
            try {
                rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
                while (rs.next()) {

                    user.setId(rs.getInt("id"));
                    user.setName(rs.getString("name"));
                    user.setPwd(rs.getString("pwd"));
                    user.setEmail(rs.getString("email"));
                    user.setTel(rs.getString("tel"));
                    user.setGrade(rs.getInt("grade"));
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                JDBCUtils.closeResource(null, prep, rs);
            }
            JDBCUtils.closeResource(null, prep, rs);
        }

        return user;
    }

    @Override
    public User getUserByPwd(Connection conn, int id, String pwd) {
        String sql = "select * from user where id =? and pwd=?";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {id, pwd};
        User user = new User();
        if (conn != null) {
            try {
                rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
                while (rs.next()) {
                    user.setId(rs.getInt("id"));
                    user.setName(rs.getString("name"));
                    user.setPwd(rs.getString("pwd"));
                    user.setEmail(rs.getString("email"));
                    user.setTel(rs.getString("tel"));
                    user.setGrade(rs.getInt("grade"));
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                JDBCUtils.closeResource(conn,prep,rs);
            }
            JDBCUtils.closeResource(conn,prep,rs);
        }
        return user;
    }
}
```

`1.JDBCUtils  负责连接数据库并执行基础数据库操作`

`2. 对于查询User结果，需要在Dao层 拼装成一个对象或者列表，返回给调用层 拼装的关键语句    user.setId(rs.getInt("id")); `

`3. 不同的操作对应不同的sql语句`

`4. 使用完数据库，要关闭资源 JDBCUtils.closeResource(null,prep,rs)，这儿要记住，在这个方法创建了什么资源就关闭什么资源，按照后使用先关闭，例如conn是上下文传进来的，本层不要关闭，因为之前关闭了connect导致出现bug; `

### 2.3 在com.kuang.service中编写service操作

1.编写User接口 public interface UserService

```java
public interface UserService {
    //用户登录
    public User login(int id, String pwd);
}
```

2.编写UserService实现类

```java
public class UserServiceImpl implements UserService {
    private UserMapper userMapper;

    public void setUserMapper(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    @Override
    public User login(int id, String pwd) {
        Connection conn = null;
        User user = null;
        try {
            conn = JDBCUtils.getConnection();
            user=userMapper.getUserByPwd(conn,id,pwd);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.closeResource(conn,null,null);
        }
        /*判断密码*/
        if (user.getPwd()==null || (!user.getPwd().equals(pwd))){
            return null;
        }
        return user;
    }
}
```

`1. service层实现类包含 UserMapper， 到了spring阶段，就不用构造器初始化，直接注入`

`2 通过调用UserMapperImpl的对应方法进行查询数据库操作，业务层仅做业务相关的处理`

`3. 使用完数据库要关闭资源，本层仅申请了connection，所以只关闭了conn`

### 2.4 在com.kuang.controller编写controller

1.完成登录的controller编写 LoginController

```java
public class LoginController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("LoginServlet----start.....");
        //此处的字符处理已经通过Filter进行了处理所以这个地方屏蔽
        //resp.setContentType("text/html; charset = utf-8");
        //resp.setCharacterEncoding("utf-8");
        String type = req.getParameter("type");
        //根据type不同，进行对应处理，实现一个servlet处理多个url的需求
        if ("login".equals(type)){
            //和数据库中的密码进行对比，调用业务层；
            //获取用户名和密码
            int id = Integer.parseInt(req.getParameter("id"));
            String password = req.getParameter("password");

            UserServiceImpl userService = new UserServiceImpl();
            userService.setUserMapper(new UserMapperImpl());
            User user = userService.login(id, password);//到这里已经把登录的人给查出来了
            if (user!=null){//查有此人，可以登录
                //将用户的信息放到Session中
                req.getSession().setAttribute(Constants.USER_SESSION,user);
                // 将用户购物车信息放入到Session中
                MyCartServiceImpl mc = new MyCartServiceImpl();
                req.getSession().setAttribute(Constants.USER_CART, mc);
                //跳转到内部主页
                resp.sendRedirect(req.getContextPath()+"/hall");
            }else {//查无此人，无法登录
                //转发会登录页面，顺带提示它： 用户名或者密码错误
                req.setAttribute("error","用户名或者密码不正确!");
                req.getRequestDispatcher("jsp/login.jsp").forward(req, resp);
            }
        }else if ("exit".equals(type)){
            System.out.println("==>login.exit()");
            if (req.getSession().getAttribute(Constants.USER_SESSION)!=null){
                req.getSession().removeAttribute(Constants.USER_SESSION);
            }
            resp.sendRedirect(req.getContextPath()+"/jsp/login.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

`1. 获取前端的参数形式   req.getParameter("type")  其中type是前端参数值，传递形式为:/MyShopping/GoHallCI?type=login，在本例子中，当type是login，传递给登录处理方法体，对于type是exit，传递给退出登录处理方法体`

`2.req.getParameter("id") 获取前端提交的id，绑定了前端input的name属性，<input type="text" class="form-control" name="id" id="signinform1" placeholder="请输入您的用户id">`

`3. 可以通过在请求后带type参数形式，在controller进行判断，实现一个servlet处理多个请求 `

`4. 通过service层查询是否存在用户 User user = userService.login(id, password);`

`5. resp.sendRedirect()和req.getRequestDispatcher的区别和使用方式:`

>**getRequestDispatcher()与sendRedirect()的区别**
>
>1.request.getRequestDispatcher()是请求转发，前后页面共享一个request,返回的是一个RequestDispatcher对象, response.sendRedirect()是重新定向，前后页面不是一个request,
>
>2.RequestDispatcher.forward()是在服务器端运行,HttpServletResponse.sendRedirect()是通过向客户浏览器发送命令来完成,所以RequestDispatcher.forward()对于浏览器来说是“透明的”,而HttpServletResponse.sendRedirect()则不是
>
>3.ServletContext.getRequestDispatcher(String url)中的url只能使用绝对路径； 而ServletRequest.getRequestDispatcher(String url)中的url可以使用相对路径。因为ServletRequest具有相对路径的概念；而ServletContext对象无次概念。
>
>RequestDispatcher对象从客户端获取请求request，并把它们传递给服务器上的servlet,html或jsp
>
>假设转发代码包含于注册的servlet－url为/ggg/tt;jsp为/ggg/tt.jsp: 
>绝对路径：response.sendRedirect("[http://www.brainysoftware.com](http://www.brainysoftware.com/) 
>根路径：response.sendRedirect("/ooo")发送至http://localhost:8080/ooo 
>相对路径：response.sendRedirect("ooo")发送至http://localhost:8080/Test/ggg/ooo, 
>
>
>
>环境准备：
>
>1. login.jsp 所在目录: /webapp/  jsp/    login.jsp
>2. servelt  /GoHallCI
>3. 工程名字: /MyShopping
>
>实际测试：
>
>1、req.getRequestDispatcher("jsp/login.jsp").forward(req, resp);  测试结果: 可转发，实际访问地址：http://localhost:8080/MyShopping/GoHallCI?type=login
>
>1、req.getRequestDispatcher("/jsp/login.jsp").forward(req, resp);  测试结果：可转发 实际访问地址：http://localhost:8080/MyShopping/GoHallCI?type=login
>
>1、req.getRequestDispatcher("login.jsp").forward(req, resp);    测试结果：不可转发 实际访问地址，找不到
>
>1、req.getRequestDispatcher("/login.jsp").forward(req, resp);     测试结果：不可转发 实际访问地址 ，找不到 
>
>1、req.getRequestDispatcher(req.getContextPath()+"login.jsp").forward(req, resp);    实际访问地址，找不到  [/MyShoppinglogin.jsp] 未找到
>
>1、req.getRequestDispatcher(req.getContextPath()+"/login.jsp").forward(req, resp);    实际访问地址，找不到 [/MyShopping/login.jsp] 未找到
>
>1、req.getRequestDispatcher(req.getContextPath()+"jsp/login.jsp").forward(req, resp);	实际访问地址，找不到 [/MyShoppingjsp/login.jsp] 未找到
>
>1、req.getRequestDispatcher(req.getContextPath()+"/jsp/login.jsp").forward(req, resp);	 实际访问地址，找不到    [/MyShopping/jsp/login.jsp] 未找到
>
>1、req.getRequestDispatcher(req.getContextPath()+"/GoHallCI").forward(req, resp);      实际访问地址，[找不到 http://localhost:8080/MyShopping/GoHallCI?type=login]
>
>1、req.getRequestDispatcher("/MyShopping/jsp/login.jsp").forward(req, resp);             实际访问地址，找不到 文.件[/MyShopping/jsp/login.jsp] 未找到
>
>1、req.getRequestDispatcher("/GoHallCI").forward(req, resp);   实际访问地址 http://localhost:8080/MyShopping/GoHallCI?type=login
>
>`req.getRequestDispatcher()后面如果还有语句，那么在转发页面，后台仍然会执行对应语句，直到语句执行结束!`
>
>2、在jsp里面 \<a href="${pageContext.request.contextPath}/jsp/login.jsp">登录</a> , 实际访问地址：http://localhost:8080/MyShopping/jsp/login.jsp
>
>3、resp.sendRedirect(req.getContextPath()+"/hall");   测试结果 : 可重定向  实际重定向地址：http://localhost:8080/MyShopping/hall
>
>3、resp.sendRedirect("jsp/login.jsp")；  测试结果: 可重定向，实际访问地址：http://localhost:8080/MyShopping/jsp/login.jsp**
>
>3、resp.sendRedirect("/jsp/login.jsp");  测试结果：不可重定向   实际访问地址：http://localhost:8080/jsp/login.jsp
>
>3、resp.sendRedirect("/login.jsp");       测试结果：不可重定向    实际访问地址，http://localhost:8080/login.jsp 
>
>3、resp.sendRedirect("login.jsp");        测试结果：不可重定向        实际访问地址 ，http://localhost:8080/MyShopping/login.jsp
>
>3、resp.sendRedirect(req.getContextPath()+"login.jsp");    测试结果：不可重定向     实际访问地址，http://localhost:8080/MyShoppinglogin.jsp
>
>3、resp.sendRedirect(req.getContextPath()+"/login.jsp");    测试结果：不可重定向    实际访问地址，http://localhost:8080/MyShopping/login.jsp
>
>3、resp.sendRedirect(req.getContextPath()+"jsp/login.jsp");  测试结果：不可重定向 	   实际访问地址，http://localhost:8080/MyShoppingjsp/login.jsp
>
>3、resp.sendRedirect(req.getContextPath()+"/jsp/login.jsp");	 测试结果：可重定向        实际访问地址 http://localhost:8080/MyShopping/jsp/login.jsp
>
>3、resp.sendRedirect(req.getContextPath()+"/GoHallCI");      测试结果 ：可重定向    实际访问地址，http://localhost:8080/MyShopping/GoHallCI
>
>3、resp.sendRedirect("/MyShopping/jsp/login.jsp");            测试结果：可重定向            实际访问地址，http://localhost:8080/MyShopping/jsp/login.jsp
>
>3、resp.sendRedirect("/GoHallCI");   测试结果：不可重定向      实际访问地址 http://localhost:8080/GoHallCI
>
>小结：
>
>req.getRequestDispatcher() 的url 只能使用 jsp/login.jsp的相对路径或者/jsp/login.jps的绝对路径访问，支持填写servlet
>
>resp.sendRedirect()  除了支持 jsp/login.jsp的相对路径，还支持自己拼接全路径 req.getContextPath()+"/jsp/login.jsp，也可以自己直接写全，/MyShopping/jsp/login.jsp

`6. req.getRequestDispatcher()和resp.sendRedirect()并没有 return语句功能，执行完这个语句，代码仍然继续向下执行`

`7. UserServiceImpl userService = new UserServiceImpl(); controller层向service层进行请求操作`

`8. 验证登录成功,就将登录信息和购物车信息放入到session中`

### 2.5 在com.kuang.utils编写Constants

Constants.java 存放Session和一些常量,

```java
public class Constants {
    public final static String USER_SESSION ="userSession";
    public final static int pageSize = 5;
    public final static String USER_CART = "userCart";
}
```

`1.使用常量类管理常量，这样使用起来更方便简单，也更容易记忆，不会混乱!`

### 2.6 在com.kuang.service编写MyCartServiceImpl

`1、购物车数据使用Map形式保存  Map<Integer,Book> myCartMap = new HashMap<Integer,Book>(); 每个Map按照 编号和对应的pojo实例组织`

`2.购物车实现添加图书、删除图书、修改图书数量、计算购物车价格、清空购物车、以及列出来购物车的数据；`

`3.在MyCartServiceImpl持有一个 BookServiceImpl的实例，这样对于需要数据库查询商品信息，可以通过BookServiceImpl来进行操作`

```java
public class MyCartServiceImpl {
    
    private BookService bookService;
	Map<Integer,Book> myCartMap = new HashMap<Integer,Book>();
    
    public MyCartServiceImpl() {
        this.bookService = new BookServiceImpl(new BookMapperImpl());
    }
   
    /*1.向购物车添加图书*/
    public  void addBookToCar(int id){
        /*如果map里面已经有了这本书，就直接取出来，把购买数量+1*/
        if (myCartMap.containsKey(id)){
            Book cBook = myCartMap.get(id);
            int buynum = cBook.getBuynum();
            cBook.setBuynum(buynum+1);
        }else {
            /*如果map里面没有这本书，就取出来这本书，然后添加到map*/
            Book book = bookService.getBookById(id);
            myCartMap.put(id, book);
        }
    }
    /*2.计算购物车商品价格*/
    public BigDecimal getTotalPrice(){
        BigDecimal total = BigDecimal.valueOf(0);
        List al = new ArrayList();
        Iterator<Integer> iterator = myCartMap.keySet().iterator();
        while (iterator.hasNext()){
            Integer id = iterator.next();
            Book book = myCartMap.get(id);
            /*BigDecimal格式的 +- * / 运算 和标准运算不一样*/
            total =  total.add(book.getPrice().multiply(BigDecimal.valueOf(book.getBuynum())));

        }
        return total;
    }
    /*3.从购物车删除图书*/
    public void deleteBooKfromCart(int id){
        myCartMap.remove(id);
    }

    /*4.修改购物车图书数量*/
    public void updateBookfromCart(String id, String nums){
        Book book = myCartMap.get(Integer.valueOf(id));
        book.setBuynum(Integer.valueOf(nums));
    }
    /*5. 清空购物车*/
    public void clear(){
        myCartMap.clear();
    }

    /*6. 显示购物车商品信息*/
    public List listMyCart(){
        List cartList = new ArrayList();
        Iterator<Integer> iterator = myCartMap.keySet().iterator();
        while (iterator.hasNext()){
            Integer id = iterator.next();
            Book book = myCartMap.get(id);
            cartList.add(book);
        }
        return cartList;
    }

}
```



### 2.7 在con.kuang.controller编写controller

`1. 处理用户退出登录，采用从session中清除用户的sessionId方式`

```java
System.out.println("==>login.exit()");
if (req.getSession().getAttribute(Constants.USER_SESSION) != null) {
    req.getSession().removeAttribute(Constants.USER_SESSION);
}
resp.sendRedirect(req.getContextPath() + "/jsp/login.jsp"); //请求方式/MyShopping/jsp/login.jsp
```

### 2.8 编写前端页面

我们在/webapp/jsp/文件夹，新增login.jsp，

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html lang="en">
<head>
    <!-- meta data -->
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
    <!--font-family-->
    <link href="https://fonts.googleapis.com/css?family=Poppins:100,200,300,400,500,600,700,800,900&amp;subset=devanagari,latin-ext"
          rel="stylesheet">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/bootstrap.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/album.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/pricing.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/style.css">
    <title>sing in</title>
</head>
<body>
<div class="wraper">
    <section class="signin">
        <div class="container">
            <div class="sign-content">
                <h2>登录</h2>
                <div class="row">
                    <div class="col-sm-12">
                        <div class="sign-form">
                            <form id="loginForm" action="/MyShopping/GoHallCI?type=login" method="post">
                                <div class="form-group">
                                    <label for="signinform1">用户ID</label>
                                    <input type="text" class="form-control" name="id" id="signinform1"
                                           placeholder="请输入您的用户id">
                                </div>
                                <div class="form-group">
                                    <label for="signinform2">密码</label>
                                    <input type="password" class="form-control" name="password" id="signinform2"
                                           placeholder="Password">
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-sm-12">
                        <div class="signin-password">
                            <div class="awesome-checkbox-list">
                                <ul class="unstyled centered">
                                    <li>
                                        <input class="styled-checkbox" id="styled-checkbox-2" type="checkbox"
                                               value="value2">
                                        <label for="styled-checkbox-2">remember password</label>
                                    </li>
                                    <li><a href="#">Forgot email or password ?</a></li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-sm-12">
                        <div class="signin-footer">
                            <button id="btn1" type="submit" value="submit" class="btn signin_btn">登录</button>
                            <p>
                                还没有账户?
                                <a href="${pageContext.request.contextPath}/signup.jsp">注册</a>
                            </p>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </section>
    <!--footer -->
    <footer class="footer-copyright">
        <div id="scroll-Top">
            <i class="fa fa-angle-double-up return-to-top" id="scroll-top" data-toggle="tooltip" data-placement="top"
               title="" data-original-title="Back to Top" aria-hidden="true"></i>
        </div>
    </footer>
    <!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
    <script src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.js"></script>
    <!-- bootstrap.bundle.min.js 用于弹窗、提示、下拉菜单，包含了 popper.min.js -->
    <script src="https://cdn.staticfile.org/popper.js/1.15.0/umd/popper.min.js"></script>
    <!-- 最新的 Bootstrap4 核心 JavaScript 文件 -->
    <script src="https://cdn.staticfile.org/twitter-bootstrap/4.3.1/js/bootstrap.min.js"></script>
    <script src="${pageContext.request.contextPath}/static/js/login.js"></script>
</div>
</body>

</html>
```

`href="${pageContext.request.contextPath}/static/css/bootstrap.css"以这种方式，pageContext是jsp9大内置对象之一， 对象的作用是取得任何范围的参数，可以获取 JSP页面的out、request、reponse、session、application 等对象。pageContext对象的创建和初始化都是由容器来完成的，在JSP页面中可以直接使用 pageContext对象 `

`2. jsp9大内置对象    `

` final javax.servlet.jsp.PageContext pageContext; //页面上下文
 javax.servlet.http.HttpSession session = null;  //session
  final javax.servlet.ServletContext application;  //ServletContext 改名为applicationContext
  final javax.servlet.ServletConfig config; //ServletConfig 改为了config
  javax.servlet.jsp.JspWriter out = null;   //out的输出对象
  final java.lang.Object page = this;		//page代表了当前页
  javax.servlet.jsp.JspWriter _jspx_out = null; 
  javax.servlet.jsp.PageContext _jspx_page_context = null;
	final javax.servlet.http.HttpServletRequest request, // 请求
	final javax.servlet.http.HttpServletResponse response  //响应`

`3. 对css及js，我们使用bootstrap`

### 2.9 在web.xml 注册servlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>GoHallCI</servlet-name>
        <servlet-class>com.kuang.controller.LoginController</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>GoHallCI</servlet-name>
        <url-pattern>/GoHallCI</url-pattern>
    </servlet-mapping>
    
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```



至此，我们只对登录，从pojo,到mapper，到service，到controller，到购物车，到工具，到前端页面就做好了，可以启动我们的项目查看下。



## 3 完成主页

需求分析.

1、登录用户可以进入主页，非登录用户需要用户进行登录

2、能够将商品添加到购物车，同一个商品多次添加，会增加购物车中商品数量

3、查看购物车，进入到购物车页面，显示相关信息

4、含有分页功能

### 3.1 在com.kuang.pojo中实现Book.java实体类

```java
/*商品实体类*/
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Book {
    private int id;
    private String name;
    private String autor;
    private String publicHouse;
    private BigDecimal price;
    private int num;
    private int buynum = 1;
}
```

`1. 实体类的属性和数据字段没有一一对应，除了包含数据库相关属性，还有一个buynum属性`

`2. bynum用于表示购物车中一本书购买了多少本，因为这是一个随时会变化的所有这个没有存入数据库`

`3.对于价格等信息，我们使用BigDecimal属性，这个对应上数据库 decimal(20,2)字段`

### 3.2 在com.kuang.mapper实现BookMapper

需求分析: 因为需要对Book进行获取所有书籍、根据id获取书籍、获取总页数信息、根据分页进行显示

先设计接口 BookMapper.java

```java
public interface BookMapper {
    /*获取所有书籍*/
    List<Book> getAllBook(Connection conn);

    /*根据id获取书籍*/
    Book getBookById(Connection conn, int id);

    /*获取分页*/
    int getPageCount(Connection conn, int pageSize);

    /*根据分页显示*/
    List<Book> getBookByPage(Connection conn, int pageNow, int pageSize);
}
```

接着编写对应的实现BookMapperImpl.java

```java
public class BookMapperImpl implements BookMapper {
    @Override
    /*1. 获取所有书籍*/
    public List<Book> getAllBook(Connection conn) {
        String sql = "select * from book;";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {};
        List<Book> bookList = new ArrayList<>();
        if (conn != null) {
            try {
                rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
                while (rs.next()) {
                    Book book = new Book();
                    book.setId(rs.getInt("id"));
                    book.setName(rs.getString("name"));
                    book.setAutor(rs.getString("autor"));
                    book.setPublicHouse(rs.getString("publicHouse"));
                    book.setPrice(rs.getBigDecimal("price"));
                    book.setNum(rs.getInt("nums"));
                    bookList.add(book);
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                JDBCUtils.closeResource(null, prep, rs);
            }
            JDBCUtils.closeResource(null, prep, rs);
        }
        return bookList;
    }
    /*2.根据id获取书籍*/
    @Override
    public Book getBookById(Connection conn, int id) {
        //sql语句
        String sql = "select * from book where id = ?;";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {id};
        Book book = new Book();

        if (conn != null) {
            try {
                rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
                while (rs.next()) {
                    book.setId(rs.getInt("id"));
                    book.setName(rs.getString("name"));
                    book.setAutor(rs.getString("autor"));
                    book.setPublicHouse(rs.getString("publicHouse"));
                    book.setPrice(rs.getBigDecimal("price"));
                    book.setNum(rs.getInt("nums"));
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                JDBCUtils.closeResource(null, prep, rs);
            }
            JDBCUtils.closeResource(null, prep, rs);
        }
        return book;

    }
    /*3. 获取分页数量*/
    @Override
    public int getPageCount(Connection conn, int pageSize) {
        int rowCount = 0;
        String sql = "select count(1) as cou from book;";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {};
        if (conn != null) {
            try {
                /*1. 获取当前数据库一共有多少记录*/
                rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
                while (rs.next()) {
                    rowCount = rs.getInt("cou");
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                JDBCUtils.closeResource(null, prep, rs);
            }
            JDBCUtils.closeResource(null, prep, rs);
        }
        /*2. 例如数据库有 201条， pagesize为10，则 一共页数为 21页*/
        return (rowCount - 1) / pageSize + 1;
    }

    /*4.根据分页显示*/
    @Override
    public List<Book> getBookByPage(Connection conn, int pageNow, int pageSize) {
        //select * from book limit 2,5; 从第二个开始，取出来5个
        String sql = "select * from book limit " + (pageNow - 1) * pageSize + "," + pageSize;
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {};
        List<Book> bookList = new ArrayList<>();
        if (conn != null) {
            try {
                rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
                while (rs.next()) {
                    Book book = new Book();
                    book.setId(rs.getInt("id"));
                    book.setName(rs.getString("name"));
                    book.setAutor(rs.getString("autor"));
                    book.setPublicHouse(rs.getString("publicHouse"));
                    book.setPrice(rs.getBigDecimal("price"));
                    book.setNum(rs.getInt("nums"));
                    bookList.add(book);
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
                JDBCUtils.closeResource(null, prep, rs);
            }
            JDBCUtils.closeResource(null, prep, rs);
        }
        return bookList;
    }
}
```

`1. 因为之前定义的工具类JDBCUtils.executeQuery(conn, prep, rs, sql, params);需要这5个参数，所以方法前面都是准备这几个参数，然后调用JDBCUtils的对应方法，执行增删改查操作,用完关闭数据库连接，conn是由service传入的，所以mapper里面不用关闭`

`2. Object[] params = {};为传入的参数列表，如果是一个参数直接初始化，如果传入的参数是对象引用，则取出对应属性进行初始化`

`3. 统计数据使用的sql语句select count(1) as cou from book; `

`4.查询分页数 使用 (rowCount - 1) / pageSize + 1  其中rowcount为数据数，pagesize是每页大小 199条，200条，201条，每页显示10个，使用这个登录求出来的页数正好准确`

`5.分页显示的sql 语句 select * from book limit " + (pageNow - 1) * pageSize + "," + pageSize 其中pageNow为当前页,pagesize为大小  例如当前第3页，则 为 limit 20,10;即为第三页 数据,limit (3-1)*10,10`

### 3.3 在com.kuang.service完成BookService

同样，先编写接口BookService，再实现接口BookServiceImpl

```java
public interface BookService {
    /*获取所有书籍*/
    List<Book> getAllBook();

    /*根据id获取书籍*/
    Book getBookById(int id);

    /*获取分页*/
    int getPageCount(int pageSize);

    /*根据分页显示*/
    List<Book> getBookByPage(int pageNow, int pageSize);
}
```

接着编写对应的实现BookServiceImpl.java

```java
public class BookServiceImpl implements BookService{
    //持有dao层接口
    private BookMapper bookMapper;

    public BookServiceImpl(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    @Override
    /**
     * 获取所有书籍
     */
    public List<Book> getAllBook() {
        Connection conn = null;
        List<Book> bookList = new ArrayList<>();
        try {
            conn = JDBCUtils.getConnection();
            bookList= bookMapper.getAllBook(conn);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.closeResource(conn,null,null);
        }
        return bookList;
    }


    @Override
    /**
     * 根据id获取书籍
     */
    public Book getBookById( int id) {
        Connection conn = null;
        Book book = new Book();
        try {
            conn = JDBCUtils.getConnection();
            book= bookMapper.getBookById(conn,id);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.closeResource(conn,null,null);
        }
        return book;
    }

    @Override
    /**
     * 获取所有书籍页数
     */
    public int getPageCount(int pageSize) {
        Connection conn = null;
       int pageCount = 0;
        try {
            conn = JDBCUtils.getConnection();
            /*调用dao层进行处理*/
            pageCount= bookMapper.getPageCount(conn,pageSize);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.closeResource(conn,null,null);
        }
        return pageCount;
    }

    @Override
    /**
     * 获取指定页数数据
     */
    public List<Book> getBookByPage(int pageNow, int pageSize) {

        Connection conn = null;
        List<Book> bookList = new ArrayList<>();
        try {
            conn = JDBCUtils.getConnection();
            bookList= bookMapper.getBookByPage(conn,pageNow,pageSize);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.closeResource(conn,null,null);
        }
        return bookList;
    }
}
```

`1. 对数据库的操作通过组合private BookMapper bookMapper;实现`

### 3.4 在com.kuang.controller完成HallController

需求描述：

1、因为有分页，实现分页的逻辑为，获取到当前页，获取到数据库一共多少页，获取到每页设置多少条，从而实现分页获取



```java
public class HallController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(">>>HallController start....");
        int pageNow = 1;//当前页，默认是1
        int pageCount = 0;  //页数

        String spageNow = req.getParameter("pageNow");

        if (spageNow != null) {
            pageNow = Integer.parseInt(spageNow);
        }
        int pageSize = 3;//指定每页多少条记录
        BookService bookService = new BookServiceImpl(new BookMapperImpl());
        /*2.获取当前数据库书籍记录按照pagesize进行分页一共有多少页*/
        pageCount = bookService.getPageCount(pageSize);
        List<Book> bookList = bookService.getBookByPage(pageNow,pageSize);
        
        //3 将数据返回给前端
        req.setAttribute("bookList",bookList);
        req.setAttribute("pageNow",pageNow);
        req.setAttribute("pageCount",pageCount);

        /*4 返回页面*/
        try {
            req.getRequestDispatcher("jsp/shop.jsp").forward(req,resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



小结：

这块总的逻辑就是

1、首先，从前端接收参数，既当前页面spageNow，并初始化pageSize及操作Bookservice的引用

2、通过业务层，获取根据pageSize进行分页的总页数，获取当前当前页面的数据

3、当数据获取成功后，将数据通过req.setAttribute()返回给前端，BookList是商品列表，pageNow是当前页,pageCount是总页数

4、转发页面到首页

### 3.5 在webapp/jsp/编写shop.jsp

```jsp
<%@ page language="java" pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">

    <title>Album example · Bootstrap</title>
    <!-- 新 Bootstrap4 核心 CSS 文件 -->
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/bootstrap.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/bootstrap-grid.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/bootstrap-reboot.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/album.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/pricing.css">

    <style>
        .bd-placeholder-img {
            font-size: 1.125rem;
            text-anchor: middle;
            user-select: none;
        }

        @media (min-width: 768px) {
            .bd-placeholder-img-lg {
                font-size: 3.5rem;
            }
        }

        .item-header {
            height: 225px;
            background-color: rgb(85, 89, 92);
            background-repeat: no-repeat;
            background-size: 100% 100%;
            background-position: center center;
        }

        .item-img {
            width: 100%;
            height: 100%;
        }
    </style>

</head>
<body>
<div class="d-flex flex-column flex-md-row align-items-center p-3 px-md-4 mb-3 bg-white border-bottom shadow-sm">
    <h5 class="my-0 mr-md-auto font-weight-normal">中关村图书大厦</h5>
    <nav class="my-2 my-md-0 mr-md-3">
        <a class="p-2 text-dark" href="#">商品</a>
        <a class="p-2 text-dark" href="#">博客</a>
        <a class="p-2 text-dark" href="#">客服</a>
        <a class="p-2 text-dark" href="#">购物车</a>
    </nav>
    <a class="btn btn-outline-primary" href="${pageContext.request.contextPath}/GoHallCI?type=exit">退出</a>
</div>

<main role="main">
    <section class="jumbotron text-center">
        <div class="container">
            <h1>测试购物车</h1>
            <p class="lead text-muted">
                需求描述:
            <P class="lead text-muted">1、用户登录后进入到这个页面，非登录用户可以拦截，退出用户将退出到首页</P>
            <P class="lead text-muted">1、用户登录后进入到这个页面，非登录用户可以拦截，退出用户将退出到首页</P>

            </p>
            <p>
                <a href="#" class="btn btn-primary my-2">Main call to action</a>
                <a href="#" class="btn btn-secondary my-2">Secondary action</a>
            </p>
        </div>
    </section>
    <div class="album py-5 bg-light">
        <div class="container">
            <div class="row">
                <table class="table table-striped table-hover">
                    <thead>
                    <tr>
                        <th scope="col">编号</th>
                        <th scope="col">商品名称</th>
                        <th scope="col">商品价格</th>
                        <th scope="col">供应商信息</th>
                        <th scope="col">操作</th>
                    </tr>
                    </thead>
                    <tbody>
                    <c:forEach var="book" items="${bookList}">
                        <tr>
                            <th scope="row">${book.id}</th>
                            <td>${book.name}</td>
                            <td>${book.price}</td>
                            <td>${book.publicHouse}</td>
                            <td>
                                <a class="text-primary" href="/MyShopping/shoppingCart?method=add&id=${book.id}">购买</a>
                            </td>
                        </tr>
                    </c:forEach>
                    <tr>
                        <td><a href="/MyShopping/shoppingCart?method=see">查看购物车</a></td>
                    </tr>
                    </tbody>
                </table>
            </div>
            <div class="row">
                <%
                    int pageCount = (int) request.getAttribute("pageCount");
                %>
                <c:if test="${pageNow-1>=1}">
                    <%--<a href='/MyShopping/GoHallCI?type=logined&pageNow=<%=pageNow - 1 %>'>上一页</a>--%>
                    <a href="/MyShopping/hall?type=logined&pageNow=${pageNow-1}">上一页</a>
                </c:if>
                <%
                    for (int i = 0; i < pageCount; i++) {
                %>
                <a href="/MyShopping/hall?type=logined&pageNow=<%=i+1%>"><%=i + 1%>
                </a>
                <%
                    }
                %>
                <c:if test="${pageNow + 1 <= pageCount }">
                    <%--<a href='/MyShopping/GoHallCI?type=logined&pageNow=<%=pageNow + 1 %>'>下一页</a>--%>
                    <a href='/MyShopping/hall?type=logined&pageNow=${pageNow+1}'>下一页</a>
                </c:if>
            </div>
        </div>
    </div>
</main>


<footer class="text-muted">
    <div class="container"></div>
</footer>

<!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
<script src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.js"></script>
<!-- bootstrap.bundle.min.js 用于弹窗、提示、下拉菜单，包含了 popper.min.js -->
<script src="https://cdn.staticfile.org/popper.js/1.15.0/umd/popper.min.js"></script>
<!-- 最新的 Bootstrap4 核心 JavaScript 文件 -->
<script src="${pageContext.request.contextPath}/static/js/bootstrap.min.js"></script>
<script src="${pageContext.request.contextPath}/static/js/bootstrap.bundle.js"></script>

</body>
</html>
```



1、引入c标签，<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

2、对于数据，使用<c:forEach var="book" items="${bookList}">\</c:forEach> 其中items为后端传过来的变量，book为取出来的数据

3、可以在jsp中使用 <% %> 操作符，在里面写java语言，

4、<c:if test="${pageNow-1>=1}">  从pageNow取出值，减去1，然后和 1比较，如果>=1为真，否则为假 \${pageNow}-1>=1不成立，是非法格式；

5、下面这段语句，编译后实际生成的代码

```java
<% for (int i = 0; i < pageCount; i++) { %>
<a href="/MyShopping/hall?type=logined&pageNow=<%= i+1 %>"><%= i + 1 %></a>
<% } %>

编译后生成的java代码
    
for (int i = 0; i < pageCount; i++) {
    out.write("\r\n");
    out.write("<a href=\"/MyShopping/hall?type=logined&pageNow=");
    out.print(i + 1);
    out.write('"');
    out.write('>');
    out.print(i + 1);
    out.write("\r\n");
    out.write("</a>\r\n");
    out.write("");
}
```



至此，我们就完成了首页上关于展示商品和翻页相关能力，该页面有不少进入到购物车的入口，得下一个篇章完成

## 4. 实现购物车

需求分析：

1、从主页，点击购买，添加商品到购物车，点击查看购物车，能看到购物车商品

2、用户退出登录，购物车商品就消失

3、购物车商品信息含商品id, 商品名，价格，出版商，购买数量，及修改图书数量、删除添加到购物车的商品

4、根据购物车商品信息，计算商品价格

5、克返回到主页大厅或提交订单

实现分析:

1、购物车数据用HashMap<Integer, Book> 存储并在用户登录后放入到session中

2、service层，实现购物车数据的清空、展示、计算价格、添加及修改

3、controller层，根据method的不同，跳转到不同的处理逻辑

### 4.1 在com.kuang.service完成MyCartServiceImpl

先编写MyCartService.java

```java
public interface MyCartService {
    /*1. 添加商品到购物车*/
    void addBookToCar(int id);

    /*2.计算购物车商品价格*/
    BigDecimal getTotalPrice();

    /*3.从购物车删除图书*/
    void deleteBooKfromCart(int id);

    /*4.修改购物车图书数量*/
    void updateBookfromCart(String id, String nums);

    /*5. 清空购物车*/
    void clear();

    /*6. 显示购物车商品信息*/
    List listMyCart();
}
```

再编写MyCartService的实现类 MyCartServiceImpl.java

```java
public class MyCartServiceImpl implements MyCartService {
    private BookService bookService;

    public MyCartServiceImpl() {
        this.bookService = new BookServiceImpl(new BookMapperImpl());
    }
    /*使用map形式保存购物车数据*/
    Map<Integer,Book> myCartMap = new HashMap<Integer,Book>();
    /*1.向购物车添加图书*/
    public  void addBookToCar(int id){
        /*如果map里面已经有了这本书，就直接取出来，把购买数量+1*/
        if (myCartMap.containsKey(id)){
            Book cBook = myCartMap.get(id);
            int buynum = cBook.getBuynum();
            cBook.setBuynum(buynum+1);
        }else {
            /*如果map里面没有这本书，就取出来这本书，然后添加到map*/
            Book book = bookService.getBookById(id);
            myCartMap.put(id, book);
        }
    }
    /*2.计算购物车商品价格*/
    public BigDecimal getTotalPrice(){
        BigDecimal total = BigDecimal.valueOf(0);
        List al = new ArrayList();
        Iterator<Integer> iterator = myCartMap.keySet().iterator();
        while (iterator.hasNext()){
            Integer id = iterator.next();
            Book book = myCartMap.get(id);
            /*BigDecimal格式的 +- * / 运算 和标准运算不一样*/
            total =  total.add(book.getPrice().multiply(BigDecimal.valueOf(book.getBuynum())));

        }
        return total;
    }
    /*3.从购物车删除图书*/
    public void deleteBooKfromCart(int id){
        myCartMap.remove(id);
    }

    /*4.修改购物车图书数量*/
    public void updateBookfromCart(String id, String nums){
        Book book = myCartMap.get(Integer.valueOf(id));
        book.setBuynum(Integer.valueOf(nums));
    }
    /*5. 清空购物车*/
    public void clear(){
        myCartMap.clear();
    }

    /*6. 显示购物车商品信息*/
    public List listMyCart(){
        List cartList = new ArrayList();
        Iterator<Integer> iterator = myCartMap.keySet().iterator();
        while (iterator.hasNext()){
            Integer id = iterator.next();
            Book book = myCartMap.get(id);
            cartList.add(book);
        }
        return cartList;
    }
}
```

### 4.2 在com.kuang.controller完成MyCartController.java

```java
/*购物车*/
public class MyCartController extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("MyCartController==>");
        String method = req.getParameter("method");
        if (method != null && method.equals("see")) {
            /*1.进入购物车*/
            seeShoppingCart(req, resp);
        } else if (method != null && method.equals("del")) {
            /*2. 从购物车删除商品*/
            delFromShoppingCart(req, resp);
        } else if (method != null && method.equals("add")) {
            /*3.添加商品到购物车*/
            addShoppingCart(req, resp);
        } else if (method != null && method.equals("update")) {
            /*4. 更新购物车中商品数量*/
            updateShoppingCart(req, resp);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }

    /*1. 查看购物车*/
    public void seeShoppingCart(HttpServletRequest req, HttpServletResponse resp) {
        System.out.println("seeShoppingCart==>");
        /*1. 从session中获取购物车*/
        MyCartServiceImpl mc = (MyCartServiceImpl) req.getSession().getAttribute(Constants.USER_CART);

        //2. 将数据返回给前端
        req.setAttribute("booklist", mc.listMyCart());
        req.setAttribute("totalprice", mc.getTotalPrice());
        //3. 转发到购物车页面，使用相对路径
        try {
            req.getRequestDispatcher("jsp/showMyCar.jsp").forward(req, resp); //用的是相对路径，相对/webapp/
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /*2. 添加商品到购物车*/
    public void addShoppingCart(HttpServletRequest req, HttpServletResponse resp) {
        System.out.println("addShoppingCart==>");
        //1.接收前端传入商品id，从session取出购物车
        int id = Integer.parseInt(req.getParameter("id"));
        MyCartServiceImpl mc = (MyCartServiceImpl) req.getSession().getAttribute(Constants.USER_CART);
        //2.将商品id加入到购物车
        mc.addBookToCar(id);
        //3.数据返回给前端
        req.setAttribute("booklist", mc.listMyCart());
        req.setAttribute("totalprice", mc.getTotalPrice());
        //4.转发到购物车页面，使用绝对路径
        try {
            req.getRequestDispatcher("/jsp/showMyCar.jsp").forward(req, resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /*3. 从购物车删除商品*/
    public void delFromShoppingCart(HttpServletRequest req, HttpServletResponse resp) {
        System.out.println("delFromShoppingCart==>");
        //1. 从前端接收商品id
        int id = Integer.parseInt(req.getParameter("id"));
        MyCartServiceImpl mc = (MyCartServiceImpl) req.getSession().getAttribute(Constants.USER_CART);
        //2. 将商品id从购物车删除
        mc.deleteBooKfromCart(id);
        //3. 数据返回给前端
        req.setAttribute("booklist", mc.listMyCart());
        req.setAttribute("totalprice", mc.getTotalPrice());
        //4. 转发到购物车页面,使用绝对路径
        try {
            req.getRequestDispatcher("/jsp/showMyCar.jsp").forward(req, resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /*4. 更新购物车中商品的数量 */
    public void updateShoppingCart(HttpServletRequest req, HttpServletResponse resp) {
        System.out.println("delFromShoppingCart==>");
        /* 1.从前端接收参数，多个同名ids使用getParameterValues */
        //String ids = req.getParameter("ids");
        //String booknum = req.getParameter("booknum");
        String[] ids = req.getParameterValues("ids");
        String[] booknum = req.getParameterValues("booknum");
        MyCartServiceImpl mc = (MyCartServiceImpl) req.getSession().getAttribute(Constants.USER_CART);
        if (ids != null) {
            //前端的使用者,如果没打勾的话
            //request.getParameterValues("booknum")会接收到null值
            for (int i = 0; i < ids.length; i++) {
                /*2. 修改购物车商品数量 ，此处潜藏一个bug，可能ids和booknum不一致 导致修改的数量不对*/
                mc.updateBookfromCart(ids[i], booknum[i]);
            }

        }
        /*3. 数据返回给前端*/
        req.setAttribute("booklist", mc.listMyCart());
        req.setAttribute("totalprice", mc.getTotalPrice());
        /*4.转发到购物车页面,使用绝对路径 */
        try {
            req.getRequestDispatcher("/jsp/showMyCar.jsp").forward(req, resp);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

`1. 通过对method的判断，实现一个controller实现查看购物车、添加商品到购物车、更新购物车商品，从购物车删除商品的功能`

`2. MyCartServiceImpl mc = (MyCartServiceImpl) req.getSession().getAttribute(Constants.USER_CART); 从session中获取购物车  这个例子没有加空值判断`

`3.前端多个元素，具有相同的name名字，如每本书都有一个ids，商品列表有多个ids ,此时使用getParameterValues()获取一个ids列表 String[] ids = req.getParameterValues("ids");  `

### 4.3 在webapp/jsp/编写cart.shop

```jsp
<%@ page language="java" pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">

    <title>Album example · Bootstrap</title>
    <!-- 新 Bootstrap4 核心 CSS 文件 -->
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/bootstrap.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/bootstrap-grid.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/bootstrap-reboot.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/album.css">
    <link rel="stylesheet" href="${pageContext.request.contextPath}/static/css/pricing.css">

    <style>
        .bd-placeholder-img {
            font-size: 1.125rem;
            text-anchor: middle;
            user-select: none;
        }

        @media (min-width: 768px) {
            .bd-placeholder-img-lg {
                font-size: 3.5rem;
            }
        }

        .item-header {
            height: 225px;
            background-color: rgb(85, 89, 92);
            background-repeat: no-repeat;
            background-size: 100% 100%;
            background-position: center center;
        }

        .item-img {
            width: 100%;
            height: 100%;
        }
    </style>

</head>
<body>
<div class="d-flex flex-column flex-md-row align-items-center p-3 px-md-4 mb-3 bg-white border-bottom shadow-sm">
    <h5 class="my-0 mr-md-auto font-weight-normal">中关村图书大厦</h5>
    <nav class="my-2 my-md-0 mr-md-3">
        <a class="p-2 text-dark" href="#">商品</a>
        <a class="p-2 text-dark" href="#">博客</a>
        <a class="p-2 text-dark" href="#">客服</a>
        <a class="p-2 text-dark" href="#">购物车</a>
    </nav>
    <a class="btn btn-outline-primary" href="${pageContext.request.contextPath}/GoHallCI?type=exit">退出</a>
</div>

<main role="main">
    <section class="jumbotron text-center">
        <div class="container">
            <h1>测试购物车</h1>
            <p class="lead text-muted">
                需求描述:
            <P class="lead text-muted">1、用户登录后进入到这个页面，非登录用户可以拦截，退出用户将退出到首页</P>
            <P class="lead text-muted">1、用户登录后进入到这个页面，非登录用户可以拦截，退出用户将退出到首页</P>

            </p>
            <p>
                <a href="#" class="btn btn-primary my-2">Main call to action</a>
                <a href="#" class="btn btn-secondary my-2">Secondary action</a>
            </p>
        </div>
    </section>
    <div class="album py-5 bg-light">
        <div class="container">
            <div class="row">
                <table class="table table-striped table-hover">
                    <thead>
                    <tr>
                        <th scope="col">编号</th>
                        <th scope="col">商品名称</th>
                        <th scope="col">商品价格</th>
                        <th scope="col">供应商信息</th>
                        <th scope="col">操作</th>
                    </tr>
                    </thead>
                    <tbody>
                    <c:forEach var="book" items="${bookList}">
                        <tr>
                            <th scope="row">${book.id}</th>
                            <td>${book.name}</td>
                            <td>${book.price}</td>
                            <td>${book.publicHouse}</td>
                            <td>
                                <a class="text-primary" href="${pageContext.request.contextPath}/shoppingCart?method=add&id=${book.id}">购买</a>
                            </td>
                        </tr>
                    </c:forEach>
                    <tr>
                        <td><a href="${pageContext.request.contextPath}/shoppingCart?method=see">查看购物车</a></td>
                    </tr>
                    </tbody>
                </table>
            </div>
            <div class="row">
                <% int pageCount = (int) request.getAttribute("pageCount");%>
                <c:if test="${pageNow-1>=1}">
                    <%--<a href='/MyShopping/GoHallCI?type=logined&pageNow=<%=pageNow - 1 %>'>上一页</a>--%>
                    <a href="${pageContext.request.contextPath}/hall?type=logined&pageNow=${pageNow-1}">上一页</a>
                </c:if>
                <% for (int i = 0; i < pageCount; i++) { %>
                <a href="${pageContext.request.contextPath}/hall?type=logined&pageNow=<%=i+1%>"><%=i + 1%>
                </a>
                <% } %>
                <c:if test="${pageNow + 1 <= pageCount }">
                    <%--<a href='/MyShopping/GoHallCI?type=logined&pageNow=<%=pageNow + 1 %>'>下一页</a>--%>
                    <a href='${pageContext.request.contextPath}/hall?type=logined&pageNow=${pageNow+1}'>下一页</a>
                </c:if>
            </div>
        </div>
    </div>
</main>


<footer class="text-muted">
    <div class="container"></div>
</footer>

<!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
<script src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.js"></script>
<!-- bootstrap.bundle.min.js 用于弹窗、提示、下拉菜单，包含了 popper.min.js -->
<script src="https://cdn.staticfile.org/popper.js/1.15.0/umd/popper.min.js"></script>
<!-- 最新的 Bootstrap4 核心 JavaScript 文件 -->
<script src="${pageContext.request.contextPath}/static/js/bootstrap.min.js"></script>
<script src="${pageContext.request.contextPath}/static/js/bootstrap.bundle.js"></script>

</body>
</html>

```





## 5. 实现订单确认页



在购物车页面，点击提交订单进入到订单确认页，在订单确认页展示

1、用户订单信息

2、本次购买商品信息

3、价格信息

在订单确认页，确认提交，则生成订单，并返回到订单成功页

### 5.1 在com.kuang.pojo实现Orders.java

```java
/*订单实体类*/
public class Orders {
    private int id;
    private int user_id;
    private BigDecimal totalPrice;
    private Date orderDate;
}
```

id, 订单的id

user_id 当前订单的用户id

totaoprice， 当前订单总价

orderDate 订单创建时间

### 5.2 在com.kuang.mapper实现OrderMapper

完成添加订单和获取最近一条订单需求

```java
public interface OrderMapper {
    /*添加一条订单*/
    int addOrders(Connection conn, Orders order) throws SQLException;
    /*获取最近一条订单*/
    Orders getRecentlyOrder(Connection conn)throws SQLException;
}
```

OrderMapperImpl实现

```java
public class OrderMapperImpl implements OrderMapper {
    @Override
    public int addOrders(Connection conn, Orders order) throws SQLException {
        /*1. 环境准备*/
        String sql = "insert into orders(user_id,totalPrice,orderDate) values(?,?,?);";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {order.getUser_id(),order.getTotalPrice(),order.getOrderDate()};
        int updateRows = 0;
        /*2. 执行sql语句*/
        if (conn != null) {
            prep = conn.prepareStatement(sql);
            updateRows = JDBCUtils.executeUpdate(conn, prep, sql, params);
            JDBCUtils.closeResource(null,prep,rs);
        }
        /*3. 返回结果*/
        return updateRows;
    }

    @Override
    /*获取最近一条order*/
    public Orders getRecentlyOrder(Connection conn) throws SQLException {
        /*1. 环境准备*/
        String sql = "select * from orders where id =(select max(id) from orders);";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {};
        Orders orders =null;
        /*2. 执行语句并拼装order*/
        if (conn != null) {
            prep = conn.prepareStatement(sql);
            rs = JDBCUtils.executeQuery(conn, prep, rs, sql, params);
            while (rs.next()){
                orders=new Orders();
                orders.setId(rs.getInt("id"));
                orders.setUser_id(rs.getInt("user_id"));
                orders.setTotalPrice(rs.getBigDecimal("totalPrice"));
                orders.setOrderDate(rs.getDate("orderDate"));
            }
            JDBCUtils.closeResource(conn,prep,rs);
        }
        /*3. 返回结果*/
        return orders;
    }
}
```

`1. params里面，直接从order对象里面取出来对应字段 Object[] params = {order.getUser_id(),order.getTotalPrice(),order.getOrderDate()};  `

`2. 查询最近一条记录的sql 语句  select * from orders where id =(select max(id) from orders);`

### 5.3 在com.kuang.service实现OrderService

OderService和OderMapper实现需求类似

```java
public interface OrderService {
    /*添加一条订单*/
    boolean addOrders(Orders order);
    /*获取最近一条订单*/
    Orders getRecentlyOrder();
}
```

编写OrderServiceImpl实现

```java
public class OrderServiceImpl implements OrderService {
    private OrderMapper orderMapper;

    public OrderServiceImpl() {
        this.orderMapper = new OrderMapperImpl();
    }

    @Override
    public boolean addOrders(Orders order) {
        Connection conn = null;
        boolean flag = false;

        if (order != null) {
            try {
                conn = JDBCUtils.getConnection();
                conn.setAutoCommit(false); //设置事务
                int updateRows = orderMapper.addOrders(conn, order);
                conn.commit();

                if (updateRows > 0) {
                    flag = true;
                    System.out.println("add order success!");
                } else {
                    System.out.println("add order failed!");
                }
            } catch (Exception e) {
                e.printStackTrace();
                try {
                    System.out.println("rollback==================");
                    conn.rollback();
                } catch (SQLException e1) {
                    e1.printStackTrace();
                }
            } finally {
                JDBCUtils.closeResource(conn, null, null);
            }
        }
        return flag;
    }

    @Override
    /*获取最近一条Order*/
    public Orders getRecentlyOrder() {
        Connection conn = null;
        Orders orders =null;
        try {
            conn = JDBCUtils.getConnection();
            orders = orderMapper.getRecentlyOrder(conn);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.closeResource(conn, null, null);
        }
        return orders;
    }

```

`1. 对于增加、修改、删除，需要提交事务管理，事务的操作方式 先把事务自动提交关闭 conn.setAutoCommit(false); //设置事务，然后在完成操作后，执行 conn.commit(); 如果出现异常则执行回滚操作 conn.rollback();`

`Service层持有Dao层操作private OrderMapper orderMapper;`



### 5.4 在com.kuang.controller完成ConfirmOrderController

1. 当用户从购物车提交订到进入到订单确认页，先从session中出去购物车商品和商品总价，并将这些数据展示到订单确认页`



```java
/*进入到订单确认页面*/
public class ConfirmOrderController extends HttpServlet {

    @Override
    /*从Session中取出来购物车信息，并返回给订单准备页面*/
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("==>GoMyOrderController....");
        /*1. 从session取出当前用户的购物车信息，含商品列表，订单总价*/
        MyCartServiceImpl mc = (MyCartServiceImpl) req.getSession().getAttribute(Constants.USER_CART);
        List cartItemsList = mc.listMyCart();
        BigDecimal totalPrice = mc.getTotalPrice();
        /*2.将数据返回给前端*/
        req.setAttribute("bookList", cartItemsList);
        req.setAttribute("totalPrice", totalPrice);
        /*3.转发到页面*/
        req.getRequestDispatcher("jsp/order.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



### 5.5 在webapp/jsp编写order.jsp

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ page import="com.kuang.utils.Constants" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
</head>
<body>
<h1>我的订单</h1>
<table style="border-collapse: collapse; border :1px">
    <tr>
        <td colspan='2'>用户信息</td>
    </tr>
    <tr>
        <td>用户名</td>
        <td>${sessionScope.userSession.name}</td>
    </tr>
    <tr>
        <td>电子邮件</td>
        <td>${sessionScope.userSession.email}</td>
    </tr>
    <tr>
        <td>用户级别</td>
        <td>${sessionScope.userSession.grade}</td>
    </tr>
</table>
<table border="1">
    <tr>
        <td>bookid</td>
        <td>书名</td>
        <td>价格</td>
        <td>出版社</td>
        <td>数量</td>
    </tr>

    <c:forEach var="book" items="${bookList}">
        <tr>
            <td>${book.id}</td>
            <td>${book.name}</td>
            <td>${book.price}元</td>
            <td>${book.publicHouse}</td>
            <td>${book.buynum}本</td>
        </tr>
    </c:forEach>
    <tr>
        <td colspan="5">购物车总价格：${totalPrice}元</td>
    </tr>
</table>
<a href=${pageContext.request.contextPath}/submitOrder>确认提交</a>
</body>
</html>
```

`1. 对于用户的信息，从jsp页面的SessionScope变量中取出来`

`2. ${sessionScope.userSession.name}里面的userSession为我们登陆时向session里面放入的一个key`  

```
req.getSession().setAttribute("userSession", user)
```

`3.在订单确认页，此时未执行数据库操作，仅从缓存取出数据展示在订单确认页`

## 6.实现提交订单

在订单确认页点击提交订单，将orders和ordersItem写入数据库，同时前端提示用户已经提交订单成功，订单进行创建成功状态

### 6.1 在com.kuang.pojo实现OrderItem

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
/*订单详情实体类*/
public class OrderItem {
    private int id;
    private int orders_id;
    private int bookid;
    private int booknum;
}
```

在订单详情，每条数据库记录包括，订单详情id， 这条记录的订单id，商品id及对应购买的数量

### 6.2 在com.kuang.mapper实现OrderItemMapper

订单详情，目前只有一条添加订单详情的需求

先设计OrderItemMapper需求

```java
public interface OrderItemMapper {
    /*添加一条订单详情*/
    public int addOrderItem(Connection conn, OrderItem orderItem) throws SQLException;
}
```

编写OrderItemMapperImpl实现

```java
public class OrderItemMapperImpl implements OrderItemMapper {
    @Override
    /*添加一条订单详情*/
    public int addOrderItem(Connection conn, OrderItem orderItem) throws SQLException {
        String sql =  "insert into ordersItem(orders_id,bookid,booknum) values(?,?,?);";
        PreparedStatement prep = null;
        ResultSet rs = null;
        Object[] params = {orderItem.getOrders_id(),orderItem.getBookid(),orderItem.getBooknum()};
        int updateRows = 0;
        if (conn != null) {
            prep = conn.prepareStatement(sql);
            updateRows = JDBCUtils.executeUpdate(conn, prep, sql, params);
            JDBCUtils.closeResource(null,prep,rs);
        }
        return updateRows;

    }
}
```

### 6.3 在com.kuang.service实现 OrderItemService

```java
public interface OrderItemService {
    /*添加一条订单详情*/
    boolean addOrderItem(OrderItem orderItem) throws SQLException;
}
```

编写完成OrderItemServiceImpl

```java
public class OrderItemServiceImpl implements OrderItemService{
    private OrderItemMapper orderItemMapper;

    public OrderItemServiceImpl() {
        this.orderItemMapper = new OrderItemMapperImpl();
    }

    @Override
    public boolean addOrderItem(OrderItem orderItem) throws SQLException {
        System.out.println(">>>OrderItemServiceImpl......");
        Connection conn = null;
        boolean flag = false;

        if (orderItem != null) {
            try {
                conn= JDBCUtils.getConnection();
                //关闭事务自动提交
                conn.setAutoCommit(false);
                int updateRows = orderItemMapper.addOrderItem(conn, orderItem);
                //手动执行提交，将数据提交到数据库
                conn.commit();

                if (updateRows > 0) {
                    flag = true;
                    System.out.println(">>>add orderItem success!");
                } else {
                    System.out.println(">>>add orderItem failed!");
                }
            } catch (Exception e) {
                e.printStackTrace();
                try {
                    System.out.println(">>>rollback....");
                    conn.rollback();
                } catch (SQLException e1) {
                    e1.printStackTrace();
                }
            } finally {
                JDBCUtils.closeResource(conn, null, null);
            }
        }
        return flag;
    }
}

```

### 6.4 在com.kuang.controller实现SubmitOrderController

需求描述；

1、把订单写入数据库

2、把订单详情写入数据库

3、返回到订单成功页

```java
public class SubmitOrderController extends HttpServlet {
    private OrderService orderService = new OrderServiceImpl();
    private OrderItemService orderItemService = new OrderItemServiceImpl();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(">>>SubmitOrderController ......");
        /*1. 从session取出 用户和购物车*/
        User user = (User) req.getSession().getAttribute(Constants.USER_SESSION);
        MyCartServiceImpl myCart = (MyCartServiceImpl) req.getSession().getAttribute(Constants.USER_CART);
        if (user == null || myCart == null) {
            System.out.println(">>>缓存里没有User!");
            req.getRequestDispatcher("jsp/errorInfo.jsp").forward(req, resp);
            return;
        }

        /*2. 把order写入到数据库*/
        Orders order = new Orders();
        order.setUser_id(user.getId());
        order.setTotalPrice(myCart.getTotalPrice());
        order.setOrderDate(new Date());
        //向数据库添加一条order记录
        boolean b = orderService.addOrders(order);
        if (!b) {
            System.out.println(">>>order 添加失败了");
            req.getRequestDispatcher("jsp/errorInfo.jsp").forward(req, resp);
            return;
        }

        /*3. 取出刚添加的oder id，供下面添加orderItem 使用*/
        order = orderService.getRecentlyOrder();
        if (order==null){
            System.out.println(">>>order 是空的");
            req.getRequestDispatcher("jsp/errorInfo.jsp").forward(req, resp);
            return;
        }

        /*4 将缓存中的商品信息添加到订单详情*/
        List<Book> bookList = myCart.listMyCart();
        for (Book book : bookList) {
            OrderItem orderItem = new OrderItem();
            orderItem.setBookid(book.getId());
            orderItem.setBooknum(book.getBuynum());
            orderItem.setOrders_id(order.getId());
            try {
                boolean b1 = orderItemService.addOrderItem(orderItem);
                if(!b1){
                    System.out.println(">>>orderItem 添加失败了");
                    req.getRequestDispatcher("jsp/errorInfo.jsp").forward(req, resp);
                    return;
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        req.getRequestDispatcher("jsp/orderOk.jsp").forward(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



### 6.5 在web/jsp/下编写 orderOk.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>提交订单完毕</h1>
	<a href = '${pageContext.request.contextPath}/GoHallCI?type=exit'>安全退出</a>
</body>
</html>
```



### 6.6 到web.xml注册servlet

```xml
<servlet>
    <servlet-name>ConfirmOrderController</servlet-name>
    <servlet-class>com.kuang.controller.ConfirmOrderController</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ConfirmOrderController</servlet-name>
    <url-pattern>/confirmOrder</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>SubmitOrderController</servlet-name>
    <servlet-class>com.kuang.controller.SubmitOrderController</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>SubmitOrderController</servlet-name>
    <url-pattern>/submitOrder</url-pattern>
</servlet-mapping>
```



## 7. 使用过滤器解决乱码和登录验权

### 7.1 解决乱码问题

```
package com.kuang.filter;

import javax.servlet.*;
import java.io.IOException;

public class CharacterEncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void destroy() {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        /*需要调用chain.doFilter(request,response) 放行*/
        chain.doFilter(request,response);
    }
}

```

`1. chain.doFilter(request,response); 这句话一定要在方法最后调用，这样程序才会继续向下运行`

`2. 过滤器实现Filter`

编写完去web.xml注册

```
<!--filter-->
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>com.kuang.filter.CharacterEncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

`1. 这里/*代表所有的都被过滤，含jsp，如果用/则jsp将不会过滤，所以我们需要使用/*`



### 7.2 登录验权

```java
public class SysCheckPermissionFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void destroy() {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("==>SysCheckPermissionFilter...checked");
        HttpServletRequest req= (HttpServletRequest)request;
        HttpServletResponse resp= (HttpServletResponse)response;
        //System.out.println("request.getContextPath():"+request.getContextPath());
        User userSession = (User)req.getSession().getAttribute(Constants.USER_SESSION);

        if (userSession==null){
            resp.sendRedirect(req.getContextPath()+"/index.jsp");
        }else {
            chain.doFilter(request,response);
        }

    }
}
```

编写完，同样需要去web.xml注册

```
<filter>
    <filter-name>checkPermissionFilter</filter-name>
    <filter-class>com.kuang.filter.SysCheckPermissionFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>checkPermissionFilter</filter-name>
    <url-pattern>/jsp/frame/*</url-pattern>
</filter-mapping>
```

这个可以设计一下



至此，全部完成了，当然里面还有很多bug，我会后续持续做，最近开始学springboot了，等学完再继续。

如果你有任何疑惑或者问题，可以联系我Q :1642302522