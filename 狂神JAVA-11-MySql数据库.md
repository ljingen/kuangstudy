# MySql数据库

##  1.初始MySQL

JAVAEE  企业级Java开发   web

前端  (页面：展示 数据)!

后台: 连接数据库JDBC， 连接前端Springmvc Serverlet  控制视图调整，和给前端传递数据

数据库  (存数据， txt,excel, word access)

只会写代码，基础码农，学好数据库，基本混饭吃， 

我们需要学习操作系统，需要学习存储事务， 学习数据结构与算法；当一个不错的程序员! 

当一个很优秀的程序员，离散数学，数字电路，体系结构，编译原理，+实战经验，算是一个高级程序员或者是一个优秀的程序员



### 1.1 为什么学习数据库

1.趋势，岗位需求

2.现在的世界，是一个大数据时代，数据是可以变现，得数据者得天下!

3.被迫需求  存数据

4.数据库， 是所有软件体系中最核心的存在

### 1.2 什么是数据库

数据库DB DataBase 

概念： 数据仓库

作用：存数据，管理数据，



### 1.3 数据库分类

关系型数据库(SQL): 有结构，有组织，典型代表  MySQL ,Oracle,SqlServer DB2,SQLite,通过表和表之间，行和列之间的关系进行数据存储，例如学员表、考勤表；

非关系型数据库(NoSQL  not only sql): {key:value}键值对	对象存储，通过对象的自身属性来进行决定。

==DBMS数据库管理系统==

- 数据库的一个管理软件，可以科学有效的管理我们的数据，维护和获取数据；
- MySQL

### 1.3 MySQL简介

MySQL是一个关系型数据库管理系统

是Oracle旗下产品

开放数据库软件~

体积小，速度快，总体拥有成本低，找人成本低，所有人必须会

我们一般安装的基本都是 5.7，最新版本是8.0，这两个版本有区别，JDBC都不同

### 1.4 安装mysql

去官网下载mysql

3.配置环境变量, 把mysql的bin目录放到系统环境变量里面

> C:\Program Files\MySQL\MySQL Server 5.7\bin

4.配置mysql.ini

```ini
[mysqld]
#目录一定换成自己的
basedir = d:\Environment\mysql-5.7.19\
datadir = d:\Environment\mysql-5.7.19\data\
port=3306
skip-grant-tables
```



5. 在管理员模式cmd输入所有的命令
6. 安装mysql服务
7. 初始化数据库文件
8. 启动mysql，进去修改密码
9.  进入mysql通过命令行 (-p 后面不要加空格)  修改命令(sql语句后面一定加;)
10. 注释掉ini中的跳过密码验证
11. 重启mysql，连接测试，如果连接成功，就可以了。

### 1.5 安装SQLyog

可以按照一个操作软件

无脑安装

新建一个数据库



### 1.6 命令行操作连接数据库



```mysql
mysql -uroot -p123456  ##连接数据库
update mysql.user set authentication_string=password('123456') where user='root' and Host='localhost'; ##更新root用户密码
flush privileges;##刷新权限
------------------------------------
show databases; ## 查看数据库，所有语句都需要用分号结束

mysql> use school; ##切换数据库 use 数据库

mysql> show tables; ##查看当前数据库的所有的表


mysql> describe student; ##显示数据库的所有表信息
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(10)      | NO   | PRI | NULL    | auto_increment |
| age   | int(10)      | YES  |     | NULL    |                |
| name  | varchar(255) | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
mysql> create database westos; ## 创建一个数据库
Query OK, 1 row affected (0.01 sec)
mysql> exit; 退出数据库
Bye

--单行注释；
## 多行注释；
/**/
```

数据库语言 :

DDL(Database xxxx Language) 定义  DML(Database xxx Language)操作 DQL(Database xxx Language)查询 DCL(Database  xxx Language) 控制

CRUD程序员，业务程序员	CV程序员，复制粘贴	API程序员，调用各种API



## 2操作.数据库

mysql数据库不区分大小写

### 2.1 操作数据库

> 创建数据库

create database 'wetos';

create database [if not exists] 'wetos'; 加了 中括号代表命令是可选的





> 删除数据库

DROP DATABASE \`westos\`；

drop database [if exists ] \`westos\`;

记住，如果表明或者特殊字段记住，要加上 \`\`, 



> 查看数据库

SHOW DATABASES;



> 使用数据库

use school;



学习思路：

​	对照sqlyog可视化历史记录查看sql

​	

### 2.2 操作数据库的表

数据库的列类型有哪些？

数值类型

- tinyint  十分小的数据，只占有1个字节
- smallint  较小的数据  占有2个字节
- mediumint  在较小和正常之间，占有3个字节
- **int   标准的整数，一般4个字节** 这个常用
- bigint   较大的数据，一般8个字节 
- float    小数 浮点数  4个字节
- double   浮点数      8个字节
- decimal   字符串形式浮点数，精度高，一般存金融数据





字符串

- char  字符串固定大小的  0~255
- **varchar 可变字符串  0~65536 对应java的String** 常用变量都可以用varchar
- tinytext   微型文本   承载2^8 -1 存一个博客就够了;
- text  文本串，承载2^16-1,一般保存一个大文本来用!



时间、日期

	- java.Util.Date	
	- date  YYYY-MM-DD 日期
	- time HH:mm:ss 时间 格式
	- **datetime   上面两个加起来，最常用的，对应java的Date   YYYY-MM-DD HH:mm:ss**
	- **timestamp    时间戳，从1970.1.1到现在的毫秒数， 全球统一，唯一的一个数据 也较为常用**
	- year   年份表示

null

- 没有值，未知
- 注意，不要使用NULL进行运算，使用它运算结果一定是NULL



Unsigned : 无符号整数， 声明该列不能声明为负数，勾上就不能是负数，不够上可以存储负数

zerofill  0填充，不足的位数，用0填充 int(3). 5---005

自增 ：

​	通常理解为自增，自动在上一条记录的基础上+1,(默认)

​	通常用来设计唯一的主键~index，必须是整数类型

​	可以自定义设计主键自增的起始只和步长

default: 非空 NULL not Null

​	     假设设置为not null,如果不给他赋值，就会报错!

​	      Null, 如果不填写值，默认就是null

​	    设置默认的值，例如sex，默认值为男，如果不指定改列的值，则会有默认值



**每个表，都必须存在以下五个字段**

主键

version

is_delete 伪删除

gmt_create 创建时间

cmt_delete 修改时间



### 2.3 操作数据库中表的字段

命令行创建一张表

```mysql
CREATE TABLE if not EXISTS `student1` (
	`id` INT(4) NOT NULL auto_increment COMMENT '学号',
	`name` VARCHAR(30) not NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(20) NOT NULL DEFAULT '女' COMMENT '性别',
	`birthday` DATE DEFAULT NULL COMMENT '出生日期',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;
```

格式：

> create table [if not exists] \`表名\` (
>
> ​	\`字段名\` 列类型 [属性] [索引] [注释],
>
> ) [表类型] [字符集设置] [注释],



**注意：**

​	- 字段名，表名等信息需要使用波浪符号 \`\`  但是一些字符串等描述 ，如default里面的信息，comment信息，使用'' 单引号或者双引号

​	- 所有的语句后面，都需要加英文的 “,” 最后一个字段可以不用加

​	- primary key 主键，一般一个表只有一个唯一的主键!

   - 设置表的字符集使用 charset=utf8;

     ```sql
     CREATE DATABASE `test` 
     CHARACTER SET 'utf8' 
     COLLATE 'utf8_general_ci';
     ```

使用工具几个重要命令

- 查看创建数据库的语句  **show CREATE DATABASE school;**

  ```mysql
  CREATE DATABASE `school` /*!40100 DEFAULT CHARACTER SET latin1 */
  CREATE DATABASE `smbms` character set 'utf8' collate 'utf8_general_ci';
  ```

  

- 查看创建数据表的定义语句 **show CREATE TABLE student;**

  ```mysql
  CREATE TABLE `student` (
    `id` int(10) NOT NULL AUTO_INCREMENT,
    `age` int(10) unsigned DEFAULT NULL COMMENT '123123',
    `unsigndnumber` int(11) DEFAULT NULL,
    `zerofiillnumber` int(10) unsigned zerofill DEFAULT NULL,
    `address` varchar(255) DEFAULT '',
    `phone` varchar(255) CHARACTER SET latin1 DEFAULT '13811230685',
    `birthday` date DEFAULT NULL COMMENT '出生日期',
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8 COMMENT='学生表'
  ```

- 查看表的结构 **desc student;**

  ```mysql
  - id	int(10)	NO	PRI		auto_increment
    age	int(10) unsigned	YES			
    unsigndnumber	int(11)	YES			
    zerofiillnumber	int(10) unsigned zerofill	YES			
    address	varchar(255)	YES			
    phone	varchar(255)	YES		13811230685	
    birthday	date	YES			
  ```

  

**MYISAM和INNODB的区别**

|            | MYISAM       | INNODB                   |
| ---------- | ------------ | ------------------------ |
| 事务支持   | 不支持       | 支持                     |
| 数据行锁定 | 不支持       | 支持                     |
| 外键约束   | 不支持       | 支持                     |
| 全文索引   | 支持全文索引 | 不支持                   |
| 表空间大小 | 较小         | 较大的，约等于MYISAM两倍 |

常规使用的操作好处：

​	MYISAM   好处就是节约空间，速度较快，

​	INNODB  安全性高，支持事务处理，支持多表多用户操作

在物理空间存在的位置：

​	所有的数据库文件都存在data目录下，一个文件夹对应一个数据库，本质还是文件的存储! MySQL引擎在物理文件上的区别

	- INNODB在数据库表中只有*.frm文件 ，以及上级目录下的Ibates1文件
	- MYISAM在数据库表中对应*.frm文件 ，以及\*.MYD数据文件(data) \*.MYI索引文件



**设置数据库表的字符**

>  DEFAULT CHARSET=utf8	不设置会有问题，mysql会不支持中文的!

不设置话，会是mysql默认的字符集编码~

MySQL默认编码是Latin1，不支持中文

在my.ini中可以以配置默认的编码

> character-set-server=utf8

### 2.4 修改删除表

--修改表名  alter table 旧表名 rename as 新表名

>  ALTER TABLE student1 RENAME as teacher;

--增加表的字段 alter table 表名 add 字段名 列属性

> alter table teacher ADD phone INT(18)

--修改表的字段(重命名)

> ALTER TABLE teacher MODIFY phone VARCHAR(11)  将teacher里面的phone字段由原来的int修改为varchar
>
> ALTER TABLE teacher CHANGE phone phone1 INT(18)	--字段重新命名

--删除表的字段

> alter table teacher drop phone1  --删除表字段  alter table 表明 drop 字段名

--删除表

> drop table if exists teacher  --删除teacher表

所有的创建和删除操作，尽量都加上判断，以免报错!

## 3.MySQL数据管理

### 3.1 外键

方式一，在创建表的时候，增加约束

```mysql
CREATE TABLE `grade` (
	`gradeid` INT(10) NOT NULL auto_increment COMMENT '年级id',
	`gradename` VARCHAR(30) not NULL COMMENT '年级名次',
	PRIMARY KEY(`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;


CREATE TABLE if not EXISTS `student2` (
	`id` INT(4) NOT NULL auto_increment COMMENT '学号',
	`name` VARCHAR(30) not NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(20) NOT NULL DEFAULT '女' COMMENT '性别',
	`birthday` DATE DEFAULT NULL COMMENT '出生日期',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	`gradeid` INT(10) NOT NULL COMMENT '学生年级',
	PRIMARY KEY(`id`),

	KEY `fk_gradeid` (`gradeid`),
	CONSTRAINT `fk_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

```

方式二，创建表没约束，之后添加

```mysql
ALTER TABLE `student2` 
ADD CONSTRAINT `fk_gradeid` FOREIGN KEY(`gradeid`) REFERENCES `grade`(`gradeid`);
```



### 3.2 添加  Insert

语法结构 insert into 表明(['字段名1','字段名2','字段名3','字段名4'.....]) values ('值1', '值2','值3','值4'.....)

```mysql
INSERT INTO `grade`(`gradename`) VALUES ('大四')
## 插入多个字段
INSERT INTO `grade`(`gradename`) VALUES ('大二'),('大一')
##插入多个字段
INSERT INTO `grade`(`name`,`pwd`,`sex`) VALUES ('大二','123aaa','男'),('大一','123aaa','女')


INSERT INTO `grade`(`gradename`) VALUES ('大四')

insert into `grade`(`gradename`) values('大三'),('大二'),('大一')

INSERT INTO `student2`(`name`,`pwd`,`sex`,`birthday`,`address`,`email`,`gradeid`) VALUES ('张三','aaa123','男','2020-01-01','北京','email@email.com','1')

INSERT INTO `student2`(`name`,`pwd`,`sex`,`gradeid`) VALUES ('张四','aaa123','女','1')
##插入多个字段多个值，可以省略字段，但是需要表字段所有的字段都赋值
INSERT INTO `student2` VALUES ('5','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1'),
('6','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1'),
('7','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1'),
('8','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1'),
('9','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1'),
('10','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1'),
('11','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1'),
('12','张四','aaa123','女','2020-01-01','北京','hel@admin.com','1')
```

### 3.3 修改 Update

语法结构： update 表名  set [ 具体字段=新值, 具体字段=新值, 具体字段=新值 ] where 条件

```mysql
UPDATE `student2` SET `name`='狂神' WHERE id =1  
UPDATE `student2` SET `name`='狂神',`sex`='女',`email`='wetos@aliyun.com' WHERE id =1
```



条件:where字句  运算符  id等于某个值，大于某个值，或者在某个区间进行修改....

操作符会返回布尔值



| 操作符             | 意义            | 范围                  | 结果  |
| ------------------ | --------------- | --------------------- | ----- |
| =                  | 等于            | 5=6                   | false |
| <>  或者  !=       | 不等于          | 5<>6                  | true  |
| >                  |                 |                       |       |
| <                  |                 |                       |       |
| >=                 |                 |                       |       |
| <=                 |                 |                       |       |
| BETWEEN ... AND... | [] 在某个范围内 | BETWEEN 2 AND 5 [2,5] |       |
| AND                | 我和你          | 5>1 and 1>2           | false |
| OR                 | 我或你 \|\|     | 通过多个条件定位数据  |       |

```mysql
update `student` set `name`='长江7号' where `name`='kuangshen44' and sex='女'
```

语法  update 表明 set colume_name=value,[colume_name=value,.....] where [条件]

注意：

	- column_name 是数据库的列，尽量带上``
	- 条件，筛选的条件，如果没有指定，则会修改所有的列
	- value 是一个具体的值，也可以是一个变量

### 3.4 删除 Delete

语法 `delete from 表名 [ where 条件]`

```mysql
delete from `student` ##删除所有数据

delete from `student ` where id =1; 
```

truncate命令

作用：完全清空一个数据库表，表的结构和索引约束不会变

>  truncate table \`student\`

两种删除方法的区别：

- 相同的 都能删除数据，都不会删除表结构
- 不同
  - TRUNCATE 重新设置，自增列，计数器会归零
  - TRUNCATE 不会影响事务



## 4. DQL查询数据(最重点)

### 4.1 DQL

Data Query Language 数据查询语言

- 所有查询操作都用他 select
- 简单查询，复杂查询他都能做
- 数据中最核心的语句，最重要的语句
- 使用频率最高的语句

### 4.2 指定查询字段

**在一张表，简单查询，查询全部的，未涉及两张表，未涉及排序，分页**

> select * from student

可以给表起别名，也可以给字段起别名

>  select StudentNo as 学号,StudentName as 学生名 from student
>
> select \`StudentNo\` as 学号,\`StudentName\` as 学生名 from \`student\` as 学生表

可以加函数，来进行统计Concat(a,b)

> select CONCAT('姓名:',StudentName) as 学生名 from `student` as 学生表



语法： `select 字段,.....from 表`

起别名：as    字段名 as 别名  表名 as 别名



去重复distinct  去除select查询出来的结果中重复的数据，重复的数据只显示一条

``` mysql
--查询一下哪些同学参加了考试 成绩
select * from result --查询全部的考试成绩
select `studentNo` from result -- 查询有哪些同学参加了考试
selct distinct `studentNo` from result --发现重复的数据，去重
```



数据库的列(表达式)

```mysql
select version --查询系统版本(函数)
select 100*3-1 as 计算结果  --用来计算
select @@auto_increment_increment --查询自增的步长

--学员考试成绩+1
select `studentNo`.`studentResult`+1 as '提分后' from result

```



### 4.3 where 条件字句

作用，检索数据中符合条件的值

| 运算符  | 语法            | 描述            |
| ------- | --------------- | --------------- |
| and &&  | a and b  a&&b   | 逻辑与          |
| or \|\| | a or b a \|\| b | 逻辑或          |
| Not !   | not a !a        | 逻辑非 非黑即白 |
|         |                 |                 |
|         |                 |                 |





```mysql
--查询成绩在95-100之间
select studentNo,studentrResult from result where studentResult >-95 and studentResult <=100

--模糊查询 (区间)
select studentNo,studentrResult from result where studentResult between 95 and 100

--除了学号1000学生之外的同学的成绩
select studentNo,studentrResult from result where studentNo!=10000;
select studentNo,studentrResult from result where not studentNo = 10000;
```



**模糊查询: 比较运算符**

| 运算符           | 语法                      | 描述                                 |
| ---------------- | ------------------------- | ------------------------------------ |
| is null          | studentNo is null         | 如果操作符为Null,结果为真            |
| is not null      | studentNo is not null     | 如果操作符不为Null,结果为真          |
| between and      | studentNo between a and b | 若studentNo在 a和b之间，则结果为真   |
| **Like**（重要） | **a like b**              | **Sql 匹配，如果a匹配b，则结果为真** |
| IN（重要）       | A IN (a1,a2,a3,a4....)    | 如果A在列表中，结果为真              |

```mysql
## Like 操作案例  模糊查询，查询姓刘的同学
like 结果 %(代表0到任意个字符 )  _(一个字符)

select `studentNo`,`studentName` from `student`
where studentName Like '刘%'

##查询姓刘的同学，名字后面只有一个字
select `studentNo`,`studentName` from `student`
where studentName Like '刘_'
##查询姓刘的同学，名字后面只有二个字
select `studentNo`,`studentName` from `student`
where studentName Like '刘__'
##查询名字中带有嘉的，
select `studentNo`,`studentName` from `student`
where studentName Like '%嘉%'


## IN 
##  模糊查询，查询1001 1002 1003号学员

select `studentNo`,`studentName` from `student`
where studentNo in(1001,1002,1003);

## null  not null
--查询地址为空的学生 null
select `studentNo`,`studentName` from `student`
where address ='' or address is null;


## not null  查询出生日期不为空的学生，查询有出生日期的同学
select `studentNo`,`studentName` from `student`
where `BornDate` is not null;


```

### 4.4 连表查询Join on

![img](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=4057843062,2771292381&fm=26&gp=0.jpg)



--------------链表查询 join

```mysql
##查询参加考试的同学(学号，姓名，科目编号，分数)

select * from student
select * from result

/*
思路:
1、分析需求，分析查询的字段来自那些表(连接查询)
2、确定使用哪种连接查询? 7中
确定交叉点(这两个表中哪个数据是相同的)，判断的条件：学生表中的studentNo= 成绩表 studentNo
*/
-->7中Join模型中的第二种 inner join
select s.studentNo,studentName,subjectNo,StudentResult
from student as s
inner join result as r
where s.studentNo=r.studentNo;

-->7中Join模型中的第三种 right join
select s.studentNo,studentName,subjectNo,StudentResult
from student s
right join result r          ##student是左表  result是右表
on s.studentNo = r.studentNo;

-->7中Join模型中的第一种 left join
select s.studentNo,studentName,subjectNo,StudentResult
from student s   ## result是左表， student是右表
left join result r
on s.studentNo = r.studentNo;

```



本质区别

| 操作       |                                                              |
| ---------- | ------------------------------------------------------------ |
| inner join | 如果表中至少有一个匹配，就返回行，如果都有，就明确指明一下从谁返回 |
| right join | 即使左表中没有匹配。会从右表中返回所有的值，                 |
| left join  | 即使右表中没有匹配。会从左表中返回所有的值，                 |
|            |                                                              |
|            |                                                              |

----我要查询那些数据select.......

----从那几个表中查from 表 xxx join 连接的表  on 交叉条件

----假设存在一张多表查询，慢慢来，先查询两张表然后再慢慢增加!

```mysql
思考题
查询了参加考试的同学信息：学号，学生姓名，科目名，分数
/*
思路：
1、分析需求，分析查询的字段来自哪些表，student,result,subject(连接查询)
2、确定使用哪种连接查询  7种
确定交叉点(这两个表中哪个数据是相同的)
判断条件：学生表中的studentNo =  成绩表studentNo
*/
select s.studentNo,studentName, SubjectName, StudentResult 
from student s
right join result r
on r.studentNo = s.studentNo
inner join `subject` sub
on r.SubjectNo = sub.SubjectNo

```

示例代码：

![image-20201118161739463](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201118161739463.png)

![image-20201118161801062](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201118161801062.png)



```mysql
select * from a INNER JOIN b on a.aid=b.bid
结果：
1	a1	1	b1
2	a2	2	b2

select * from a LEFT JOIN b on a.aid =b.bid
# 首先取出a表中所有数据,然后再加上与a,b匹配的的数据 
结果：
1	a1	1	b1
2	a2	2	b2
3	a3	null null

select * from a RIGHT JOIN b on a.aid =b.bid
# 指的是首先取出b表中所有数据,然后再加上与a,b匹配的的数据 

结果：
1	a1	 1	b1
2	a2	 2	b2
null null 4	b4

表A记录如下：
aID        aNum 
 1          a20050111
 2          a20050112
 3          a20050113
 4          a20050114
 5          a20050115 
 
 表B记录如下：
 bID        bName 
 1          2006032401
 2          2006032402
 3          2006032403
 4          2006032404
 8          2006032408
 
 >左接 sql语句如下: 
 
SELECT * FROM A
LEFT JOIN B 
ON A.aID = B.bID
 
总结：
left join是以A表的记录为基础的,A可以看成左表,B可以看成右表,left join是以左表为准的.左表(A)的记录将会全部表示出来,而右表(B)只会显示符合搜索条件的记录(例子中为: A.aID = B.bID).B表记录不足的地方均为NULL

结果如下: 
aID        aNum           bID      bName
 1         a20050111      1        2006032401
 2         a20050112      2        2006032402
 3         a20050113      3        2006032403
 4         a20050114      4        2006032404
 5         a20050115      NULL     NULL 
 
 >右连接  sql语句如下: 
 
SELECT * FROM A
RIGHT JOIN B 
ON A.aID = B.bID
 结果如下: 
aID        aNum           bID       bName
 1         a20050111      1         2006032401
 2         a20050112      2         2006032402
 3         a20050113      3         2006032403
 4         a20050114      4         2006032404
 NULL     NULL            8         2006032408 
 
总结：
right join仔细观察一下,就会发现,和left join的结果刚好相反,这次是以右表(B)为基础的,A表不足的地方用NULL填充
```

### 4.5 自连接

自己的表和自己的表连接，一张表拆为两张一样的表即可

首先在数据库的表结构为：

![image-20201119105558325](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201119105558325.png)



父类表： pid为1的相关记录

| categoryid | cagetoryName |      |
| ---------- | ------------ | ---- |
| 2          | 信息技术     |      |
| 3          | 软件设计     |      |
| 5          | 美术设计     |      |

子类表

| pid  | categoryid | categoryName |
| ---- | ---------- | ------------ |
| 3    | 4          | 数据库       |
| 2    | 8          | 办公信息     |
| 3    | 6          | web开发      |
| 5    | 7          | ps技术       |

操作：查询父类对应的子类关系

| 父类表   | 子类表   |
| -------- | -------- |
| 信息技术 | 办公信息 |
| 软件开发 | 数据库   |
|          | web开发  |
| 美术设计 | ps技术   |

--查询父子信息

```mysql
select a.`categorName` as '父栏目', b.`categoryName` as '子栏目'

from `category` as a , `category` as b

where a.`categoryid`=b.`pid`
```



练习

查询参加了 ·数据库结构-1·考试的同学信息：学号，学生姓名，科目名，分数

```mysql
SELECT s.`StudentNo`,`StudentName`,`SubjectName`,`SutdentResult`
from `student` as s
inner join `result` as r
on s.`StudentNo`=r.`StudentNo`
inner join `subject` as sub
on r.`SubjectNo` = sub.`SubjectNo`
where `subjectName`='数据库结构-1'
```



Select 完整的语法

```mysql
select [all |distinct]
{* |table.*| table.field1 [as alias1][,table.field2 [as alias1]][.......]}
from table_name [as table_alias]
	[left |right|inner join table_name2] ----联合查询
	[where ...] ---指定结果需要满足的条件
	[GROUP by .......] ----指定结果按照哪几个字段来分组
	[HAVing]  ----过滤分组的记录必须满足的次要条件
	[ORDER BY ...] ---指定查询记录按一个或者多个条件排序
	[LIMIT {[offset,] row_count| row_countOFFSET offset}];
	--指定查询的记录从哪条至哪条
    注意：[]括号代表可选，{}括号代表必选得 
```



### 4.6 分页和排序



---order by  

---limit 

```mysql
-----------降序  由高到低
SELECT s.`StudentNo`,`StudentName`,`SubjectName`,`SutdentResult`
from `student` as s
inner join `result` as r
on s.`StudentNo`=r.`StudentNo`
inner join `subject` as sub
on r.`SubjectNo` = sub.`SubjectNo`
where `subjectName`='数据库结构-1'
order by studentresult desc 
--------------升序 由低到高
SELECT s.`StudentNo`,`StudentName`,`SubjectName`,`SutdentResult`
from `student` as s
inner join `result` as r
on s.`StudentNo`=r.`StudentNo`
inner join `subject` as sub
on r.`SubjectNo` = sub.`SubjectNo`
where `subjectName`='数据库结构-1'
order by studentresult asc 
```

为什么要分页？ 环节数据库压力，给人的体验更好，现在很多瀑布流

```mysql
--------------分页语法：limit 当前页,页面大小
--------------limit 0,5 查出来1~5
SELECT s.`StudentNo`,`StudentName`,`SubjectName`,`SutdentResult`
from `student` as s
inner join `result` as r
on s.`StudentNo`=r.`StudentNo`
inner join `subject` as sub
on r.`SubjectNo` = sub.`SubjectNo`
where `subjectName`='数据库结构-1'
order by studentresult asc 
limit 0,5  ## 0，起始位置，5，取几条数据



--第一页 limit(0,5)  (1-1)*5
--第二页 limit(5,5)  (2-1)*5
--第三页 limit(10,5)  (3-1)*5
--第n页 limit(0,5)  (n-1)*5  pageSize
[pagesize:页面大小， (n-1)*5  n,当前页]
数据总数/5=总页数
n 当前页
(n-1)*pagesize :当前页起始值
```

limit语法： `Limit(查询起始下标,pagesize)`

练习题

查询`JAVA第一学年` 课程成绩排名前十的学生，并且分数要大于80的学生信息(学号，姓名，课程名称，分数)

```mysql
SELECT s.`StudentNo`,`StudentName`,`SubjectName`,`SutdentResult`
from student s
INNER JOIN result r
on s.`StudentNo`=r.`StudentNo`
INNER JOIN `subject` sub
on r.`SubjectNo`=sub.`SubjectNo`
WHERE `SubjectName`='JAVA第一学年' and `SutdentResult`>=80
ORDER BY SutdentResult DESC
LIMIT 0,10
```

### 4.6 子查询和嵌套查询

where （值是固定的，如果这个值不固定，是计算出来的，那就是子查询，嵌套查询）

本质：`在where语句里面嵌套一个子查询语句`

试题： 查询  数据库结构-1 的所有考试结果(学号，科目号，成绩)，降序排列

方式一 ：使用关联查询

```mysql
SELECT `StudentNo`,r.`SubjectNo`,`SutdentResult`
from result r
INNER JOIN `subject` s
ON r.`SubjectNo`= s.SubjectNo
where SubjectName='数据库结构-1'
ORDER BY SutdentResult DESC
```

方式二：使用嵌套查询，查找顺序是由里及外，

```mysql
SELECT `StudentNo`,r.`SubjectNo`,`SutdentResult`
from result
WHERE `SubjectNo`=(
	SELECT `SubjectNo` from `subject` where `SubjectName`='数据库结构-1'
)
ORDER BY SutdentResult DESC
```

方式三  查询高等数学分数不低于80分的 学号和姓名

```mysql
## 查询高等数学-2分数不低于80分的学号和姓名

SELECT `StudentNo`,`StudentName` FROM student WHERE StudentNo in(
	SELECT StudentNo FROM result WHERE SutdentResult>80 AND SubjectNo=(
		SELECT `SubjectNo` from `subject` WHERE SubjectName='高等数学-2'
	)
)
```

### 4.7 分组和过滤





## 5. Mysql函数

### 5.1 聚合函数



| 函数名称 | 描述   |
| -------- | ------ |
| COUNT()  | 计数   |
| SUM()    | 求和   |
| AVG（）  | 平均值 |
| MAX（）  | 最小值 |
| MIN（）  | 最大值 |
|          |        |
|          |        |

 例如 :

```mysql
SELECT COUNT(studentname)  FROM student ;	--Count(指定列) 会忽略所有的 null 值
SELECT COUNT(*)  FROM student   --不会忽略所有的 null 值
SELECT COUNT(1)  FROM student  ----不会忽略所有的 null 值


select SUM(`SutdentResult`) AS 总和 FROM result
select AVG(`SutdentResult`) AS 平均分 FROM result
select MAX(`SutdentResult`) AS 最高分 FROM result
select MIN(`SutdentResult`) AS 最低分 FROM result


例题：---查询不同课程的平均分，最高分，最低分
---核心：根据不同的课程分组
select `SubjectName`, AVG(SutdentResult),MAX(`SutdentResult`),MIN(`SutdentResult`)
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo` = sub.`SubjectNo`
GROUP BY r.SubjectNo   ## 通过什么字段来分组

例题：---查询不同课程的平均分，最高分，最低分，平均分大于80
---核心：根据不同的课程分组

select `SubjectName`, AVG(SutdentResult) as 平均分,MAX(`SutdentResult`) as 最高分,MIN(`SutdentResult`) as 最低分
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo` = sub.`SubjectNo`
GROUP BY r.SubjectNo   ## 通过什么字段来分组
having 平均分>80
```



### 5.2 数据库级别的MD5加密

什么是MD5？ 信息摘要算法第五代，主要增加算法复杂度和不可逆。具体的值是一样的

MD5破解网站的原理：背后有一个字典，存放没一个值对应的MD5值  MD5加密后的值  加密前的值



```mysql
##加密
update testmd5 set pwd=MD5(pwd) where id=1

---------插入时候加密
INSERT INTO testmd5 VALUES (4,'xiaoming',MD5('123456'))

----------如何校验，将用户传递进来的密码，进行md5加密，然后比对加密后的值
select * from testmd5 where `name`='xiaoming' AND pwd = MD5('123456')

```

## 6 事务Transaction

### 6.1 什么是事务？ 

一个事务中的sql语句  ，要么都执行，要么都失败!

1、SQL执行  A给B转账   A1000--->200  B200

1、SQL执行  B收到B转账   A800--->200  B400



将一组SQL放入一个批次中去执行~

innoDB 支持

事务有一个原则：ACID原则  原子性，一致性，隔离性，持久性  (脏读 、 幻读....)



执行事务，mysql默认是开启事务自动提交的，set autocommit  =0  就是关闭set autocommit  =1 开启(默认值)





```mysql
start TRANSACTION ## 标记一个事务的开始，从这之后的sql都在同一个事务

## 提交  持久化

COMMIT
## 回滚  如果失败了就回滚
ROLLBACK
```

第一步，关闭自动提交

> set autocommit =0  ##关闭自动提交

第二步：手动开启一个事务开始

> START TRANSACTION ## 标记一个事务的开始，从这之后的sql都在同一个事务



事务结束

> set autocommit =1 ##开启自动提交



设置事务保存点

> savepoint 保存点名   设置一个事务的保存点

手动处理事务，要设置事务开启

​	

设置事务结束



```mysql
/*创建shop数据库*/
CREATE DATABASE shop character set utf8 COLLATE utf8_general_ci  
/**/
/*在shop数据库创建account表*/
create TABLE `account`(
	`id` INT(3) NOT NULL auto_increment,
	`name` VARCHAR(30) NOT NULL,
	`money` DECIMAL(9,2) NOT NULL,
	PRIMARY KEY (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

/*在account表中插入一些数据*/
INSERT INTO `account`(`name`,`money`) VALUES ('A',2000.00),('B',20000.00)

/*模拟转账:事务*/

set autocommit=0;  ## 关闭自动提交

start TRANSACTION;  ##开启了一个事务(一组事务)
/*更新A的金钱-500，并将B的金钱+500*/
UPDATE `account` SET money = money -500 WHERE name='A';  ##A减少了500
UPDATE `account` SET money = money +500 WHERE name='B';		##B加了500

COMMIT;  ## 提交事务，就被持久化了
ROLLBACK;  ##回滚

SET autocommit=1;  ##恢复默认值

/*模拟事务操作结束*/
```

## 7.索引

### 7.1 索引的分类

在一个表中，主键索引只能有一个，而唯一索引可以有多个

索引的本质： 帮助mysql高效获取数据的数据结构

- 主键索引 primary key
  - 唯一标识，主键不可重复，只能列一个主键主键
- 唯一索引  unique key
  - 避免重复的列出现，唯一索引可以重复，多个列都可以标识为唯一索引
- 常规索引 key/index
  - 磨人的，index  key关键字来设置
- 全文索引  fulltext
  - 在特定的数据库引擎下才有MYISAM
  - 快速定位数据



```mysql
##索引的使用
##显示所有的索引信息
SHOW INDEX from student

添加全文索引
ALTER TABLE school.student add FULLTEXT INDEX `studentName`(`studentName`);

--EXPLAIN 分析sql执行的情况
EXPLAIN select * from student;

explain select * from student where match(studentName) against('刘');
```

### 7.2模拟测试

DELIMITER  &&  ## 写函数之前必须写，标志

```mysql
CREATE TABLE `app_user`(
	`id` BIGINT(20)	UNSIGNED NOT NULL auto_increment,
	`name` VARCHAR(50) DEFAULT '' COMMENT '用户昵称',
	`email` VARCHAR(50) NOT NULL COMMENT '用户邮箱',
	`phone`	VARCHAR(20) DEFAULT '' COMMENT '手机号',
	`gender` TINYINT(4) UNSIGNED DEFAULT '0' COMMENT '性别(0:男;1:女)',
	`password` VARCHAR(100) NOT NULL COMMENT '密码',
	`age`	TINYINT(4) DEFAULT '0' COMMENT '年龄',
	`create_time`	datetime DEFAULT CURRENT_TIMESTAMP,

	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表'

ALTER TABLE `app_user` ADD `update_time` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIME


```



创建索引方式三（常用）

```mysql
--id__表名__字段名
--create index id_app_user_name on app_user(`name`);
create index id_app_user_name on app_user(`name`);
SELECT * from app_user where `name`='用户99999'
EXPLAIN select * from app_user where `name`='用户99999'
```

没有添加索引时候，查询用户花费10秒钟，才从100万条数据查询到指定用户，增加了索引，查询时间缩短到了0.001s

### 7.3 索引原则



1、索引不是越多越好，500W以后在考虑

2、不要对进程变动数据加索引

3、小数据了的表不需要加索引

5、所以一般用在经常用来查询的字段上



**索引数据结构 ，查看一篇文章，去百度一下 《MySQL索引背后的数据结构及算法原理》**

Hash类型的索引，

Innodb默认的是Btree索引，

## 8 权限管理

### 8.1 用户管理

> navicat可视化管理



--创建用户

```mysql
CREATE USER kuangshen IDENTIFIED by '123456'
CREATE USER 用户名 IDENTIFIED by '密码'
```



--修改用户

```mysql
修改当前登录用户密码
SET PASSWORD=(`1111111`)
SET PASSWORD=(`新密码`)
修改制定用户密码
SET PASSWORD for root =PASSWORD(`123456789`)
SET PASSWORD for 用户名 =PASSWORD(`新密码`)
重新命名:
RENAME USER kuangshen to kuangshen2
RENAME USER 原来名字 to 新的名字
##用户授权 授予全部权限 ，库.表
GRANT ALL PRIVILEGES on *.* to kuangshen2 ##对所有的表授权限给kuangshen2 
## 撤销权限，revoke 那些权限，在哪个库，给谁撤销
revoke all privileges on *.* from kuangshen2
```



--删除用户

```mysql
##删除用户 drop user 用户名
drop user kuangshen2
```



--查找用户

```mysql
查看用户权限
show grants for root@localhost
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION
GRANT PROXY ON ''@'' TO 'root'@'localhost' WITH GRANT OPTION

show grants for kuangshen2
```



## 9 MySQL备份

为什么备份

- 保证重要的数据不会丢失
- 可以做数据的转移

mysql数据库备份的方式

	- 直接拷贝物理文件，在data里面
	- 使用sqlyog等可视化工具中手动导出
	- 还可以使用命令行导出  mysqldump,在命令行使用

```mysql
mysqldump -hlocalhost -uroot -p123456 school student >d:/a.sql
mysqldump -h主机 -u用户名 -p密码 数据库 表名[表名2 表名3 表名4 ...] > 物理磁盘位置/文件名

mysqldump -hlocalhost -uroot -p123456 school student student2 student3 student4 >d:/a.sql ##dump多个表

mysqldump -hlocalhost -uroot -p123456 school >d:/a.sql  # 一整个数据库

```

导入数据库

```mysql
# 登录数据库 先切换到指定的数据库，然后执行source 备份文件
mysql > source a.sql 
#未登录
mysql -uroot -p 123455 库名<备份文件
```



## 10. 数据库的规约和三大范式

### 10.1 数据库规范

**为什么需要数据库设计**

- 差的数据库设计
  - 数据冗余多，消耗内存
  - 使用过多的外键，不好插入数据，依赖太多
  - 程序性能差

 - 好的数据库设计
   	- 节省内存空间
    - 保证数据库的完整性
      	- 方便我们开发系统



**软件开发中，关于数据库设计**

	- 分析需求，分析业务和需要处理的数据库的需求
	- 概要设计，设计关系图E-R图



**设计数据库步骤：个人博客**

- step1. 收集信息分析需求
  - 用户表(用户登录注销，用户个人信息，写博客，创建分类)
  - 分类表(文章分类，谁创建的)
  - 文章表(文章信息)
  - 评论表
  - 友链表(友链信息)
  - 自定义表(系统信息，某个关键的字，或者一些主字段 例如 key:value)
- step.2 标识实体类(把需求落地到每个字段上)
  - user( id, username,	password,	phone,	email,	avatar(头像),	sex,	age,	borndate(生日),	openid(微信id) sign（签名）)
  - category(id, category_name, create_user_id )
  - article(id, title, author_id,  category_id,  content, create_time, update_time, like)
  - comment(id, article_id, user_id, content, create_time, update_time, user_id_parent(回复人id) )
  - links(id, links, href,sord(排序))
  - user_follow(id, user_id(被关注人id), follow_id(关注人id))
- step3 标识实体之间关系
  - user---->写blog 写博客
  - user--->category  创建分类
  - user---->user  关注
  - 友链 links
  - user--->user--->blog  用户写博客



`CRM系统，BBS系统，BLOG系统，前端开发`



可以研究的网站，作业  用第三方组件，ElementUI  layui, AntDesign pro

### 11.2 三大范式

为什么需要数据规范化？

	- 信息重复
	- 更新异常
 - 插入异常
   	- 插入时候无法正常显示信息
 - 删除异常
   	- 丢失有效的信息

第一范式（1NF）：

1、数据表中的每一列(字段)，必须是不可拆分的最小单元，也就是确保每一列的原子性。满足第一范式是关系模式规范化的最低要求，否则，将有很多基本操作在这样的关系模式中实现不了。

例如：

message(留言编号，留言人姓名，留言人座机，留言人手机，邮箱，内容，时间，标题，地址)

如果需求知道那个省那个市并按其分类，那么显然第一个表格是不容易满足需求的，也不符合第一范式。

message(留言编号，留言人姓名，留言人座机，留言人手机，邮箱，内容，时间，标题，省，市，详细地址)





第二范式（2NF）：

满足1NF后要求表中的所有列，每一行的数据只能与其中一列相关，即一行数据只做一件事。只要数据列中出现数据重复，就要把表拆分开来。

这样便实现啦一条数据做一件事，不掺杂复杂的关系逻辑。同时对表数据的更新维护也更易操作。

例如：

Order(订单编号，房间号，联系人，联系人电话，身份证号)  

**一个人同时订几个房间，就会出来一个订单号多条数据，这样子联系人都是重复的，就会造成数据冗余。我们应该把他拆开来。**

order(订单编号，房间号，联系人编号)   lianxiren(联系人编号，联系人，联系人电话，身份证号)

**这样便实现啦一条数据做一件事，不掺杂复杂的关系逻辑。同时对表数据的更新维护也更易操作。_**



第三范式（3NF）：

满足2NF后，要求：表中的每一列都要与主键直接相关，而不是间接相关（表中的每一列只能依赖于主键）。

数据不能存在传递关系，即没个属性都跟主键有直接关系而不是间接关系。像：a-->b-->c  属性之间含有这样的关系，是不符合第三范式的。

注意事项：

1.第二范式与第三范式的本质区别：在于有没有分出两张表。

第二范式是说一张表中包含了多种不同实体的属性，那么必须要分成多张表，第三范式是要求已经分好了多张表的话，一张表中只能有另一张标的ID，而不能有其他任何信息，（其他任何信息，一律用主键在另一张表中查询）。

2.必须先满足第一范式才能满足第二范式，必须同时满足第一第二范式才能满足第三范式。

三大范式只是一般设计数据库的基本理念，可以建立冗余较小、结构合理的数据库。如果有特殊情况，当然要特殊对待，数据库设计最重要的是看需求跟性能，需求>性能>表结构。所以不能一味的去追求范式建立数据库。



注意事项：

1.第二范式与第三范式的本质区别：在于有没有分出两张表。

第二范式是说一张表中包含了多种不同实体的属性，那么必须要分成多张表，第三范式是要求已经分好了多张表的话，一张表中只能有另一张标的ID，而不能有其他任何信息，（其他任何信息，一律用主键在另一张表中查询）。

2.必须先满足第一范式才能满足第二范式，必须同时满足第一第二范式才能满足第三范式。

三大范式只是一般设计数据库的基本理念，可以建立冗余较小、结构合理的数据库。如果有特殊情况，当然要特殊对待，数据库设计最重要的是看需求跟性能，需求>性能>表结构。所以不能一味的去追求范式建立数据库。





规范性和性能的问题

关联查询的表不要超过三张表

	- 考虑商业化的需求和模板(成本，用户体验!) 数据库的性能更加重要
	- 在规范性能的问题的时候回，需要适当考虑一下规范性!
	- 故意给某些表增加一些冗余的字段(从多表查询中变为单表查询)
	- 故意增加一些计算列，(从大数据量降低为小数据量的查询:索引)



## 11. JDBC

### 11.1 数据库驱动



![image-20201120101727886](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201120101727886.png)

我们的程序会通过数据库驱动，和数据库打交道



### 10.2 JDBC

SUN公司为了简化开发人员(对数据库的统一)的操作，提供了一个java操作数据库的规范，俗称JDBC，这些规范的实现，由具体的厂商去做! 对于开发人员，我们只需要学习JDBC的接口操作既可以!

java.sq

javax.sql

还需要导入一个数据库驱动包  mysql-connector-java-5.1.49.jar



### 11.3 第一个JDBC程序

准备工作：先在数据库创建一个初始化表供测试用

```mysql
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USE jdbcStudy;

CREATE TABLE users(
	id int PRIMARY KEY,
	NAME VARCHAR(40),
	PASSWORD VARCHAR(40),
	email VARCHAR(60),
	birthday DATE
);

INSERT INTO users(id,NAME,PASSWORD,email,birthday)
VALUE(1,'zhangsan','123456','zs@sina.com','1980-12-04'),
(2,'lisi','123456','lisi@sina.com','1981-12-04'),
(3,'wangwu','123456','wangwu@sina.com','1979-12-04');

```

准备工作2：打开IDEA，创建一个空的JAVA项目，一路继续创建项目

准备工作3：导入JAVA包

	- 首先在项目根目录，点击右键，新建一个lib目录，如下图

![image-20201120111112439](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201120111112439.png)

	- 然后将我们的JAR包拷贝到lib目录中

![image-20201120111206144](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201120111206144.png)

- 选择目录，点击右键，选择 [add as libray] 这个时候就导入包成功了

![image-20201120111314325](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201120111314325.png)



编写我们的测试代码

```java
//我的第一个jdbc程序
public class JdbcFirstDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
        /*三种反射方式  Class.forName  ;JdbcFirstDemo.calss ;jdbcFirstDemo.getClass()*/
        Class.forName("com.mysql.jdbc.Driver");  //固定写法，加载驱动
        //2.连接数据库信息  用户信息和url
        /*?useUnicode=true   支持中文编码，&characterEncoding=utf8 设定字符集为utf8  &useSSL=true 使用安全连接*/
        String url = "jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&characterEncoding=utf8&useSSL=false";
        String username ="root";
        String password = "123456";
        //3.连接成功了，返回数据库对象  connection代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);
        //4.执行sql的对象   Statement statement执行sql的对象
        Statement statement = connection.createStatement();
        //5.用执行sql的对象去执行sql语句，可能存在结果，查看返回结果
        String sql = "select * from users";
        ResultSet resultSet = statement.executeQuery(sql); //返回的结果集，结果集封装了我们查询出来的所有结果
        //resultSet是一个链表结构
        while (resultSet.next()){
            System.out.println("==========================================");
            System.out.println("id="+resultSet.getObject("id"));
            System.out.println("name="+resultSet.getObject("NAME"));
            System.out.println("pwd="+resultSet.getObject("PASSWORD"));
            System.out.println("email="+resultSet.getObject("email"));
            System.out.println("birthday="+resultSet.getObject("birthday"));
        }

        //6.释放连接
        resultSet.close();
        statement.close();
        connection.close();

    }
}
```



总结

​	1 加载驱动 Class.forName

​	2  连接数据库

​	3 获得执行的Sql的对象Statement

​	4 获得返回的结果集

​	5 释放连接

> DriverManager

```java
//DriverManager.registerDriver(new com.mysql.jdbc.Driver())
Class.forName("com.mysql.jdbc.Driver"); //固定写法，加载驱动
```

> ```java
> String url = "jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&characterEncoding=utf8&useSSL=false";
> ```
>
> //mysql-3306
>
> //jdbc:mysql://主机地址:端口号/数据库名?参数1&参数2&参数3.....
>
> //oracle---1521
>
> //jdbc:oracle:thin:@主机地址:端口号:1521:sid
>
> ```java
> Connection connection = DriverManager.getConnection(url, username, password);
> ```
>
> connection 代表数据库，
>
> //数据库设置自动提交
>
> //事务提交
>
> //事务回滚
>
> ​	connection.rollback();
>
> ​	connection.commit();
>
> ​	connection.setAutoCommit();
>
> 



> Statement 执行SQL事务的类  PrepareStatement 执行SQL的对象  createStatement
>
> 
>
> statement.executeQuery() //查询操作返回ResultSet
>
> statement.execute(); //执行任何SQL
>
> statement.executeUpdate(); //更新、插入、删除，都是用这个，返回一个受影响的行数；



> 查询的结果集对象 ResultSet 封装了所有查询结果
>
> Resultset resultSet = statement.exeQuery("select * from users")
>
> resultget.getObject()
>
> resultget.getInt()
>
> resultget.getFloat()
>
> resultget.getDate()
>
> ...........
>
> resultget.next()

> 遍历指针
>
> resultSet.beforeFirst(); //移动到最前面
>
> resultSet.afterLast(); //移动到最后面
>
> resultSet.next()  移动到下一个数据
>
> resultSet.previous()  移动到下一行
>
> resultSet.absolute(row) 移动到指定行
>
> 

> 释放资源
>
> ```java
> resultSet.close();
> statement.close();
> connection.close();
> ```

### 11.4 statement对象

1.编写提取工具类

 - step1. 首先，我们在项目的目录里面，添加我们的mysql配置文件 db.properties,记住这个目录一定要放到项目根目录src下，不然接下来的ClassLoader().getResourceAsStream()可能无法获取到，

![image-20201120192601302](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201120192601302.png)

 - 在配置文件写入对应的配置信息

   ```java
   driver=com.mysql.jdbc.Driver
   url=jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&characterEncoding=utf8&useSSL=false
   username=root
   password=123456
   ```

2.编写工具类，工具类的目的，首先需要加载我们的配置文件

```java
public class JDBCUtils{
    private static String driver =null;//获取数据库的驱动信息
    private static String url =null;//获取连接信息
    private static String username=null; //获取配置文件的用户名
    private static String password=null;//获取配置文件中的密码信息
    /*静态代码，获取配置文件*/
    static{
        try{
            InputStream in = JDBCUtils.class.getClassLoader().getResourceAsStream("db.preperties");//读取db.properties配置文件信息
            Properties properties = new Properties();
            properties.load(in);

            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");

            //驱动只会加载一次
            Class.forName(driver);
          }catch(IOException e){
            e.printStackTrace();
        }catch(Exception e){
            e.printStackTrace();
        }
    }
    
    //获取connection
    public static Connection getConnection() throws SQLException{
        return DriverManager.getConnection(url, username, password);
    }
    
    //释放资源
    public static void release(Connection conn, Statement st, ResultSet rs){
        if (rs!=null){
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (st!=null){
            try {
                st.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(conn!=null){
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}
```



3.编写增删改  executeUpdate()

增加一条数据

```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestInsert {
    public static void main(String[] args) {
        Connection conn =null;
        Statement st = null;
        ResultSet rs =null;
        try {
            conn = JDBCUtils.getConnection();
            st = conn.createStatement();
            String sql = "INSERT INTO users(id, `NAME`,`PASSWORD`,`email`,`birthday`)" +
                    "value(4,'kuangshen','1234560','123455@qq.com','2020-10-10')";
            int i = st.executeUpdate(sql);
            if (i>0){
                System.out.println("插入成功!");
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.release(conn,st,rs);
        }
    }
}
```

删除一条数据

```java
public class TestDelete {
    public static void main(String[] args) {
        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;
        try {
            conn = JDBCUtils.getConnection();
            st = conn.createStatement();
            String sql = "DELETE FROM `users` WHERE id=4";
            int i = st.executeUpdate(sql);
            if (i > 0) {
                System.out.println("删除成功!");
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, rs);
        }
    }
}
```

修改一条数据

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestUpdate {
    public static void main(String[] args) {
        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;
        try {
            conn = JDBCUtils.getConnection();
            st = conn.createStatement();
            String sql = "UPDATE `users` set NAME='修改的狂神' WHERE id=4;";
            int i = st.executeUpdate(sql);
            if (i > 0) {
                System.out.println("修改成功!");
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, rs);
        }
    }
}
```



4.查询操作executeQuery()



```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class TestQuery {
    public static void main(String[] args) {
        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;
        try {
            conn = JDBCUtils.getConnection();
            st = conn.createStatement();
            String sql = "select * from users";
            rs=st.executeQuery(sql);
            while (rs.next()){
                System.out.println("==========================================");
                System.out.println("id="+rs.getObject("id"));
                System.out.println("name="+rs.getObject("NAME"));
                System.out.println("pwd="+rs.getObject("PASSWORD"));
                System.out.println("email="+rs.getObject("email"));
                System.out.println("birthday="+rs.getObject("birthday"));
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, rs);
        }
    }
}
```



5.SQL注入的问题



SQL存在漏洞，导致攻击导致数据泄露，SQL会被拼接  or

示例代码

```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class SQL注入 {
    public static void main(String[] args) {
        /*当用户名使用 ' or 1=1 时候，拼接到查询语句："select * from users WHERE `NAME`='"+username+"' and
        `PASSWORD`='"+password+"'"就变成了
        * select * from users where `name` ='' or '1=1' and `password`='' or '1=1'
        * */
        login(" ' or '1=1","123456");
    }
    public static void login(String username,String password){
        Connection conn = null;
        Statement st = null;
        ResultSet rs = null;
        try {
            conn = JDBCUtils.getConnection();
            st = conn.createStatement();
            //只需要这样记住就行：单引号 括双引号 两个加号 中间变量   ( 0Q0   OMG)
            String sql = "select * from users WHERE `NAME`='"+username+"' and `PASSWORD`='"+password+"'";
            System.out.println("sql--->"+sql);
            rs=st.executeQuery(sql);
            while (rs.next()){
                System.out.println("==========================================");
                System.out.println("id="+rs.getObject("id"));
                System.out.println("name="+rs.getObject("NAME"));
                System.out.println("pwd="+rs.getObject("PASSWORD"));
                System.out.println("email="+rs.getObject("email"));
                System.out.println("birthday="+rs.getObject("birthday"));
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, rs);
        }
    }
}
```

### 10.5 PrepareStatement对象

可以防止sql注入并且效率更好

1、 添加

```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.util.Date;
import java.sql.PreparedStatement;
import java.sql.SQLException;


public class TestInsert {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;
        try {
            conn = JDBCUtils.getConnection();

            String sql = "insert into users(`id`,`name`,`password`,`email`,`birthday`) value (?,?,?,?,?)";

            st = conn.prepareStatement(sql);

            st.setInt(1,5);
            st.setString(2,"王二麻子");
            st.setString(3,"123456");
            st.setString(4,"Hellokity@sina.com");
            st.setDate(5,new java.sql.Date(new Date().getTime()));

            int i = st.executeUpdate();

            if (i > 0) {
                System.out.println("添加成功");
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, null);
        }
    }
}
```



2、 删除

```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestDelete {
    public static void main(String[] args) {
        Connection conn=null;
        PreparedStatement st =null;
        try {
            conn = JDBCUtils.getConnection();
            String sql = "delete from users where id=?";
            st = conn.prepareStatement(sql);
            st.setInt(1,5);
            int i = st.executeUpdate();
            if (i>0){
                System.out.println("删除成功!");
            }

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.release(conn, st, null);
        }
    }
}
```



3、 更新

```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestUpdate {
    public static void main(String[] args) {
        Connection conn =null;
        PreparedStatement st =null;
        try {
             conn = JDBCUtils.getConnection();
             String sql = "update users set name =? where id=?";
             st = conn.prepareStatement(sql);
			//手动给参数赋值
             st.setString(1,"测试框");
             st.setInt(2, 5);

            int i = st.executeUpdate();

            if (i>0){
                System.out.println("更新成功");
            }

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.release(conn, st, null);
        }
    }
}
```



4、查询

```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class TestQuery {
    public static void main(String[] args) {
        Connection conn =null;
        PreparedStatement st =null;
        ResultSet rs=null;


        try {
            conn = JDBCUtils.getConnection();
            String sql = "select * from users where name=?";
            st = conn.prepareStatement(sql);
            st.setString(1,"zhangsan");
            rs = st.executeQuery();

            while (rs.next()){
                System.out.println(rs.getString("name"));
            }

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JDBCUtils.release(conn,st,rs);
        }
    }
}
```



5、防止SQL注入

```java
import top.aigoo.lesson02.utils.JDBCUtils;

import java.sql.*;

public class SQL注入 {
    public static void main(String[] args) {
        /*当用户名使用 ' or 1=1 时候，拼接到查询语句："select * from users WHERE `NAME`='"+username+"' and
        `PASSWORD`='"+password+"'"就变成了
        * select * from users where `name` ='' or '1=1' and `password`='' or '1=1'
        * */
        login(" '' or 1=1","123456");
        //login("zhangsan","123456");
    }
    public static void login(String username,String password){
        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql = "select * from users WHERE `NAME`=? and `PASSWORD`=?";
            st = conn.prepareStatement(sql);
            st.setString(1,username);
            st.setString(2,password);

            rs=st.executeQuery();

            while (rs.next()){
                System.out.println("==========================================");
                System.out.println("id="+rs.getObject("id"));
                System.out.println("name="+rs.getObject("NAME"));
                System.out.println("pwd="+rs.getObject("PASSWORD"));
                System.out.println("email="+rs.getObject("email"));
                System.out.println("birthday="+rs.getObject("birthday"));
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, rs);
        }
    }
}
```

原理：PrepareStatement 会将传进来的参数都看作String,  也就是传进来的字符都会加上 ""，对于那些转义字符，直接忽略

### 11.6 使用IDEA连接数据库





### 11.7 JDBC操作事务

要么都成功，要么都失败

**ACID** 

原子性，要么都完成，要么都不完成

一致性，结果总数保持不变

**隔离性，多个进程互相不干扰** 隔离会导致问题，

- 脏读，事务读取了另一个没有提交的事务，
- 不可重复读，第一次读和第二次读不一致，
- 幻读或者虚读 在一个事务内，读取到了别人插入的数据，导致前后读出的结果不一致

持久性， 一旦提交了就不可回滚

事务成功的示例代码

```java
public class TestTransaction1 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;

        try {

            conn = JDBCUtils.getConnection();
            /*开启一个事务，类似于mysql语句中 :  set autocommit=0 ;start transaction*/
            conn.setAutoCommit(false);

            String sql1 = "update account set money=money-100 where name='A'";
            st = conn.prepareStatement(sql1);
            st.executeUpdate();

            String sql2 = "update account set money=money+100 where name='B'";
            st = conn.prepareStatement(sql2);
            int i = st.executeUpdate();
            if (i>0){
                System.out.println("更新成功!");
            }
            conn.commit();


        } catch (SQLException throwables) {
            try {
                conn.rollback();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, rs);
        }
    }
}
```

模拟事务失败的操作代码

```java
/*测试事务更新失败*/
public class TestTransaction2 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;
        ResultSet rs = null;

        try {

            conn = JDBCUtils.getConnection();
            /*开启一个事务，类似于mysql语句中 :  set autocommit=0 ;start transaction*/
            conn.setAutoCommit(false);

            String sql1 = "update account set money=money-100 where name='A'";
            st = conn.prepareStatement(sql1);
            st.executeUpdate();

            int x = 1/0;  //此处一定会导致下面业务代码无法执行，模拟事务失败的操作

            String sql2 = "update account set money=money+100 where name='B'";
            st = conn.prepareStatement(sql2);
            int i = st.executeUpdate();
            if (i > 0) {
                System.out.println("更新成功!");
            }
            conn.commit();


        } catch (SQLException throwables) {
            try {
                conn.rollback();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            throwables.printStackTrace();
        } finally {
            JDBCUtils.release(conn, st, rs);
        }
    }
}
```

### 11.8 数据库连接池

数据库连接--执行完毕---释放-

连接--释放十分浪费系统资源

**池化技术： 准备一些预先的资源，过来就连接预先准备好的**

最小连接数 10

最大连接数 15

等待超时 100mx



编写连接池，实现一个接口DataSource

世界上知名的连接池实现类  ，开源数据源实现，DBCP C3P0 Druid(阿里巴巴德鲁伊)，使用了这些数据库连接池之后，我们在项目开发中就不需要编写连接数据库的代码!

> DBCP

需要的JAR包

commons-pool2-2.9.0.jar   commons-dbcp2-2.8.0.jar

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Date;

public class TestInsert {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;
        try {
            conn = JdbcUtils_DBCP.getConnection();
            String sql = "insert into users(`id`,`name`,`password`,`email`,`birthday`) values (?,?,?,?,?)";
            st = conn.prepareStatement(sql);

            st.setInt(1,8);
            st.setString(2,"hewu");
            st.setString(3,"123456");
            st.setString(4,"hewu@qq.com");
            st.setDate(5, new java.sql.Date(new Date().getTime()));
            //执行
            int i = st.executeUpdate();
            if (i>0){
                System.out.println("插入成功!");
            }

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JdbcUtils_DBCP.release(conn,st,null);
        }
    }
}
```

```jdbcconfig.properties
#连接设置,这里面的名字，是DBCP数据源中定义好的
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&characterEncoding=utf8&useSSL=false
username=root
password=123456

#<!--初始化连接-->
initialSize =10

#最大连接数量
maxActive=50

#<!--最大空闲连接-->
maxIdle=20

#<!--最小空闲连接-->
minIdle=5

#<!--超时等待时间以毫秒为单位 600毫秒/1000 等于60s-->
maxWait=60000

#JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：[属性名=property;]
#注意:"user"与“password”两个属性会被明确地传递，因此这里不需要包含他们。
connectionProperties=useUnicode=true;characterEncoding=UTF8

#指定由连接池锁创建的连接的自动提交(auto-commit)状态。
defaultAutoCommit=true

#driver default 指定由连接池所创建的连接的只读(read-only)状态。
#如果没有设置该值，则"setReadOnly"方法将不被调用。(某些驱动并不支持只读模式，如Informix)
#defaultReadOnly=true

#driver default 指定由连接池所创建的连接的事务级别(TransactionIsolation).
#可用值为下列之一:(详情可见javadoc.)NONE,READ_UNCOMMITTED,READ_COMMITTED,REPEATABLE_READ,SERIALIZABLE
defaultTransactionIsolation=READ_UNCOMMITTED
```

工具java类

```java
import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JdbcUtils_DBCP {

    private static DataSource dataSource = null;

    static {
        try {
            InputStream in = JdbcUtils_DBCP.class.getClassLoader().getResourceAsStream("dbcpconfig.properties");//读取配置文件
            Properties properties = new Properties();
            properties.load(in);
            //进行创建数据源  工厂模式--创建对象
            dataSource = BasicDataSourceFactory.createDataSource(properties);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    //获取connection
    public  static Connection getConnection() throws SQLException {
        return dataSource.getConnection(); //从数据源中获取连接
    }
    //释放资源
    public static void release(Connection conn, Statement st, ResultSet rs){
        if (rs!=null){
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if (st!=null){
            try {
                st.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(conn!=null){
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}
```

c3p0 示例

需要的jar包:

c3p0-0.9.5.5.jar

mchange-commons-java-0.2.19.jar

```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
    <!-- C3P0默认配置，如果没有指定则使用这个配置
         如果在代码“ComboPooledDataSource ds = new ComboPooledDataSource();” 这样写就表示使用的是C3P0的缺省配置-->
    <default-config>


        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false</property>
        <property name="user">root</property>
        <property name="password">123456</property>

        <property name="acquireIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>

        <property name="checkoutTimeout">30000</property>
        <property name="idleConnectionTestPeriod">30</property>
        <property name="maxIdleTime">30</property>
        <property name="maxStatements">200</property>
    </default-config>

    <!-- C3P0的命名的配置,如果在代码中"ComboPooledDataSource ds = new ComboPooledDataSource(MySQL);" 这样写
    就表示使用的是name是MySQL的配置文件 -->
    <named-config name="MySQL">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false</property>
        <property name="user">root</property>
        <property name="password">123456</property>

        <!-- 如果池中数据连接不够时一次增长多少个 -->
        <property name="acquireIncrement">5</property>
        <!-- 初始化数据库连接池时连接的数量 -->
        <property name="initialPoolSize">20</property>
        <!-- 数据库连接池中的最小的数据库连接数 -->
        <property name="minPoolSize">5</property>
        <!-- 数据库连接池中的最大的数据库连接数 -->
        <property name="maxPoolSize">25</property>
    </named-config>
</c3p0-config>

```

```java
package top.aigoo.lesson05;

import top.aigoo.lesson05.utils.JdbcUtils_C3P0;
import top.aigoo.lesson05.utils.JdbcUtils_DBCP;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Date;

public class TestC3P0 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;
        try {
            conn = JdbcUtils_C3P0.getConnection();
            String sql = "insert into users(`id`,`name`,`password`,`email`,`birthday`) values (?,?,?,?,?)";
            st = conn.prepareStatement(sql);

            st.setInt(1,9);
            st.setString(2,"hewu");
            st.setString(3,"123456");
            st.setString(4,"hewu@qq.com");
            st.setDate(5, new java.sql.Date(new Date().getTime()));
            //执行
            int i = st.executeUpdate();
            if (i>0){
                System.out.println("插入成功!");
            }

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            JdbcUtils_DBCP.release(conn,st,null);
        }
    }
}

```

> 结论

无论使用什么数据源，本质是一样的，DataSource接口是不会变!

​		![image-20201121160518512](F:\MD格式学习笔记库\狂神JAVA-11-MySql数据库.assets\image-20201121160518512.png)

​		  

