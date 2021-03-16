# 狂神说JAVA\---1.JAVA基础

## 1.预备工作

###  什么是计算机

- 由硬件和软件组成

- 常见计算机有台式计算机、笔记本计算机、大型计算机、超级计算机等
- 广泛应用在科学计算、数据处理、自动控制、计算机辅助设计、人工智能、网络等领域





###  cmd的启动方式



1. 开始+系统+命令提示符；
2. Win键+R键，输入cmd，进入控制台(推荐使用)；
3. 在任意文件夹下面，按住shift+鼠标右键，点击，选择此处打开命令行窗口；
4. 资源管理器的地址栏，前面加上cmd路径



常用的Dos命令

```shell
# 切换目录 cd  change director
# 清除屏幕  cls (clear screen)
# 查看电脑ip ipconfig
# 打开画图 mspaint
# 打开记事本 notepad
# 打开计算器  calc
# ping命令，一般测试网络是否正常，查看网络的Ip

D:\src\aigoo_blog>ping www.baidu.com

正在 Ping www.a.shifen.com [220.181.38.149] 具有 32 字节的数据:
来自 220.181.38.149 的回复: 字节=32 时间=13ms TTL=54
来自 220.181.38.149 的回复: 字节=32 时间=14ms TTL=54

220.181.38.149 的 Ping 统计信息:
    数据包: 已发送 = 2，已接收 = 2，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 13ms，最长 = 14ms，平均 = 13ms
    
 # 删除文件# del a.txt
 
 # 创建目录/文件夹# md test
 
 # 移除目录 # rd test
 
 # 新建一个文件# cd> a.txt
  
```

###  JAVA入门

> JAVA版本的区分

JAVA  SE  基础班

JAVA  ME  手机版

JAVA  EE  企业版

2006年 Hadoop



> JAVA 的工具包专业名词

JDK  java development kit   JAVA开发者工具 包括JRE和JVM，除了共有的，还有一些开发者指令，如 java ,javac,javadoc等

JRE   JAVA Runtime Environment  包含JVM  ,安装了JRE就可以运动java程序，但是开发的话，就需要安装jdk

JVM  JAVA Vitrual Machine  JAVA的虚拟机，在宿主机模拟cpu去处理，解释性、编译形两种；屏蔽了底层操作系统差别，一次编译，到处运行



![image-20201017225120029](F:\MD格式学习笔记库\狂神JAVA-什么是计算机.assets\image-20201017225120029.png)





> JDK 开发环境的搭建

#### 卸载jdk

1. 删除java的安装目录
2. 删除JAVA_HOME
3. 删除path下关于JAVA的目录
4. 执行java-version 查看是否还有java

这样就卸载成功了

#### 安装jdk

1. 下载jdk  , 我们使用jdk8  [下载地址](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
2. 下载电脑对应的版本  windows-64
3. jdk-8u181
4. 双击安装jdk即可
5. 记住安装的路径: F:\Environment
6. 配置环境变量
   1. 我的电脑-->右键-->属性
   2. 环境变量--系统环境变量-->JAVA_HOME  变量名用大写JAVA_HOME,变量的值为jdk安装路径，
   3. 配置path变量， 找到PATH ,点击新建，%JAVA_HOME%\bin,   %JAVA_HOME%\jre\bin
7. 在cmd进行检查 java version

![image-20201017233244164](F:\MD格式学习笔记库\狂神JAVA-什么是计算机.assets\image-20201017233244164.png)

这样 就下载成功了

### 第一个HelloWorld文件

1. 新建一个文件夹，存放我们的代码，例如 f:\code
2. 新建一个java文件
   - 文件名的后缀为.java
   - 文件名字起名字脚HelloWorld.java
3. 编辑我们的java文件

```java
public class HelloWorld{
	public static void main(String[] args){
		System.out.print("Hello World!");
	}
}

# 注意:
  1、文件名称和我们在里面的类名都要是Helloworld；
  2、语句结束要按照 ; 
  3、java完全区分大小写，需要大小写记住
```

	4. 编译 javac java文件   : 进入对应的目录，执行 javac HelloWorld.java进行编译，编译后就会有HelloWorld.class
 	5. 运行class文件， java class文件， 执行java HelloWorld, 控制台就会输出 Hello ,World！

![image-20201018101634238](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018101634238.png)



可能遇到的情况：

	1. 每个单词的大小写不能出现问题 **Java是大小写敏感的**；
 	2. 尽量使用英文;
 	3. 文件名和类名必须保持一致，并且首字母大写；
 	4. 冒号、大括号、小括号、中括号是否用中文输入法，导致输入中文符号;



### 解释一下编译型和解释型

编译的**时机**不同，从而区分出来编译型和解释型，比如一本书，一次性编译出来，这种叫做编译型，但如果逐句进行编译，就是解释型；

>  编译型知识点

![image-20201018102739977](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018102739977.png)



> 解释型知识点



随着硬件性能的提升，解释型和编译型的区分越来越弱，Java既有解释型特点也有编译型特点。

### 安装 Intellj IDEA



> 两个快捷键

  psvm  直接生成 public static vode main(String[] args)

  sout   直接生成 System.out.print()

> 安装教程的地址

1. [[IDEA2020.2.3激活，IntelliJ IDEA2020.2.3注册码，IntelliJ全家桶激活码](https://tech.souyunku.com/?p=16235)](https://tech.souyunku.com/?p=16235)

2. 下载IDEA的地址: [下载地址](https://www.jetbrains.com/idea/download/#section=windows)

![image-20201018114813879](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018114813879.png)

	3.  激活补丁和插件所在本地目录:    F:\开发工具包
 	4.  jaca项目的代码所在位置：  F:\code
 	5.  java 的jdk安装地址:  F:\Environment



## 2.JAVA基础语法



> 为了后续我们的基础语法代码，都放到一个环境，不至于到处都是，我们采用空项目模板，来配置IDEA环境

1. 打开IDEA，选择新建项目，在项目类型，选择[Empty Project],创建出来一个JAVASE的空项目,用这一个文件工程就够我们学习JAVA基础语法了

![image-20201018122812216](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018122812216.png)

2. 点击IDEA的File-->Module,创建一个Module(模块)，Module名字就叫基础语法

<img src="F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018123052833.png" alt="image-20201018123052833" style="zoom:80%;" />

创建Module叫基础语法

<img src="F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018123144232.png" alt="image-20201018123144232" style="zoom:80%;" />

3. 点击IDEA的File--->project_structure,配置这个空模板的运行环境，

![image-20201018123259171](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018123259171.png)

4. 配置Project SDK和 Project language level，这两个值要匹配，比如jdk是1.8, 选择的语言级别就是8-Lambdas

<img src="F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018123447357.png" alt="image-20201018123447357" style="zoom:80%;" />

5. 下面就可以和正常项目一样使用了，我们可以创建对应的java文件。进行编译和执行

<img src="F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018123536103.png" alt="image-20201018123536103" style="zoom:80%;" />



### 注释、标识符、关键字

- 注释(Comments),在语言中并不会执行，给编写者和开发者阅读的文字，帮助编写者和开发者理解功能的意义，java有三种类型的注释,<font color='red'>书写注释是一个非常好的习惯!</font>
  - 单行注释(Line comment)： // 在java用两个斜线记做单行注释     `//这是一个java代码的单行注释，使用两条反斜杠`  
  - 块注释/多行注释(Block comment)： /* */ 在java中用反斜杠加 *的方式来表示块注释  `  /**/    ` 可以注释多行文本
  - Doc注释(JavaDoc):  /**  */来表示JavaDoc，里面可以加描述

  <img src="F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018182115675.png" alt="image-20201018182115675" style="zoom:50%;" />

  平时写代码一定要注意规范。

- 关键字：一共差不多50个关键字，Java所有的组成部分都需要名字。类名、变量名和方法名都被称为标识符。

<img src="F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201018182927338.png" alt="image-20201018182927338" style="zoom:67%;" />

​	所有的标识符都以字符(A-Z或者a-z)，美元符($)、或者下划线(__)开始，首字母之后可以是字母、美元符、下划线或者数字的任意字符组合；

​	不能使用关键字作为变量名或者方法名，*标识符大小写敏感*；

​	可以使用中文命名，但是一般不建议这样去使用，也不建议使用拼音，很Low；

### 数据类型

Java是一种强类型语言，要求变量的使用要严格符合规定，所有变量都必须先定义后使用；

整数类型  : byte  1个字节(8 bit，8位) -128-127， short 2个字符， int 占4个字符  long占8个字符  byte 8 bit  short 16 bit   int 32 bit        long 64 bit

浮点类型 : float 4个字节  double 占8个字节，使我们常用的  float 32 bit  double 64 bit

字符类型: char 占2个字节 char 16 bit

boolean类型，占1位，只有true和false两个值  boolean 1 bit

![image-20201019100248051](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201019100248051.png)



计算机关于存储位的说明

1个字节(byte) = 8 位(bit)  ,例如 11001010，这是一个8bit， 没一个数字占据一个bit



引用数据类型(Reference Type):

​	类，接口，数组，除了八大基础类型之外的，都叫做引用数据类型；



### 类型的转换

 **在java中，整数拓展， 二进制  十进制  八进制 十六进制**

```java
        
低  ------------------------------------>  高

byte,short,char—> int —> long—> float —> double 
    自动类型转换也叫“向上类型转换”，它是低级转为高级，例如 char a='a'; int a1=a; 这时就是向上类型转型，由char类型转为int类型a1=97;
		
		int i =10;
        int i1 =010; //八进制 0开头
        int i2 =0x10; //十六进制 0x开头  0~9  A~F 16进制
		int i3 =0b11 //二进制
```

  **浮点数拓展，**

//浮点数  浮点数有限  离散 舍入误差， 接近但不等于

//最好完全使用浮点数进行比较

// 对于银行业务，我们尽量用类， BigDecimal  数学工具类

```java
public class Demo3 {
    public static void main(String[] args) {
        int i =10;
        int i1 =010; //八进制 0开头
        int i2 =0x10; //十六进制 0x开头  0~9  A~F 16进制
        System.out.println(i);
        System.out.println(i1);
        System.out.println(i2);
        System.out.println("====================");

        //浮点数拓展， 银行业务我们怎么表示钱？
        float f = 0.1f;  //0.1
        double d = 1.0/10; //0.1
        System.out.println(f==d); //false

        float d1 = 232323223232f;
        float d2 = d1+1;
        System.out.println(d1==d2); //返回了true
    }
}

```

 **字符的拓展，我们可以通过强制类型转换，把字符转换为int， 所有的字符本质还是数字**

// 编码 Unicode 有个表，  每个数字对应一个字符，共2个字节，65536  Excel  2^16 =65536

```java
        char c1 ='a';
        char c2 = '中';
        
        char c3 = '\u0061';

        System.out.println(c1);
        System.out.println((int)c1);

        System.out.println(c2);
        System.out.println((int)c2);
```

  **转义字符**

// \t  一个制表符  \n 一个换行  \b 一个空格

```java
String sa = new String("Hello World!");
String sb = new String("Hello World!");
System.out.println(sa==sb); //false

String sc = "Hello World!";
String sd = "Hello World!";
System.out.println(sc==sd);  // true

//从内存分析，sa,sb是完全不同的两个对象，而sc, sd是两个字符串，所以第一个不相等，第二个相等 
```



  **布尔值扩展**

```java
System.out.println("===========================");
boolean flag = true;
if (flag==true){
    System.out.println("这是真");
}
if (flag){
    System.out.println("这是真");
}
//上述的代码是等价的, 代码要精简易读,有经验的程序员直接使用下面的方式
```

**类型转换&&强制转换**

低 ----------------------------------------------------->高

byte, short ,char --->int -->long--->float--->double   //小数的优先级高于整数

运算中，不同类型的数据先要转化为同一个类型，然后再进行运算

```java
int a = 128;
byte demo = (byte)a;  //强制高位转换低位，128超出了byte的表达范围，导致内存溢出 
System.out.println(a);
System.out.println(demo);
//上述就是强制转换  (类型名)变量名  (byte) demo  由高到底，需要强制类型转换，由低到高，不需要进行类型转换

强制类型转换也叫“向下类型转型”，它是高级转为低级，例如int i1 = 123;        byte b = (byte)i1;//强制类型转换为byte

在Java中强制类型转换分为基本数据类型和引用数据类型两种，这里我们讨论的后者，也就是引用数据类型的强制类型转换。

       在Java中由于继承和向上转型，子类可以非常自然地转换成父类，但是父类转换成子类则需要强制转换。因为子类拥有比父类更多的属性、更强的功能，所以父类转换为子类需要强制。那么，是不是只要是父类转换为子类就会成功呢？其实不然，他们之间的强制类型转换是有条件的。

       当我们用一个类型的构造器构造出一个对象时，这个对象的类型就已经确定的，也就说它的本质是不会再发生变化了。在Java中我们可以通过继承、向上转型的关系使用父类类型来引用它，这个时候我们是使用功能较弱的类型引用功能较强的对象，这是可行的。但是将功能较弱的类型强制转功能较强的对象时，就不一定可以行了。

       举个例子来说明。比如系统中存在Father、Son两个对象。首先我们先构造一个Son对象，然后用一个Father类型变量引用它：

       Father father = new Son();

       在这里Son 对象实例被向上转型为father了，但是请注意这个Son对象实例在内存中的本质还是Son类型的，只不过它的能力临时被消弱了而已，如果我们想变强怎么办？将其对象类型还原！

       Son son = (Son)father;

       这条语句是可行的，其实father引用仍然是Father类型的，只不过是将它的能力加强了，将其加强后转交给son引用了，Son对象实例在son的变量的引用下，恢复真身，可以使用全部功能了。

       前面提到父类强制转换成子类并不是总是成功，那么在什么情况下它会失效呢？

       当引用类型的真实身份是父类本身的类型时，强制类型转换就会产生错误。例如：

       Father father = new Father();

       Son son = (Son) father;

       这个系统会抛出ClassCastException异常信息。

    所以编译器在编译时只会检查类型之间是否存在继承关系，有则通过；而在运行时就会检查它的真实类型，是则通过，否则抛出ClassCastException异常。

   所以在继承中，子类可以自动转型为父类，但是父类强制转换为子类时只有当引用类型真正的身份为子类时才会强制转换成功，否则失败
```



`****注意点:`****

1. **`不能对布尔值进行转换;`**
2. **`不能把对象类型转换为不相干的类型`**
3. **`大容量转换为低容量，需要强制转换，低容量转换高容量，不需要强制；`**
4. **`转换时候可能内存溢出或者精度的问题`**

```java
System.out.println((int)23.7);  //23
System.out.println((int)-45.89f);  //-45
```

```java
int money = 10_0000_0000;
int years = 20;
int total = money*years; //-1474992996480  计算的时候溢出了
long total2 = money*years; //-147399996480 ,默认是int，转换之前已经存在了问题
long total3 =money*((long)years); //20000000000，正确，需要先把一个数转换为long
```



### 变量、常量、作用域

> 变量是什么？ 就是可以变化的量，java是一种强类型语言，每个变量都必须声明其类型，java变量是程序中最基本的存储单元，其要素包括变量名，变量类型和作用域

```java
type varName [=value] [{, varname=[value]}];
//数据类型  变量名 =值；可以用逗号隔开来声明多个同类型的变量。	
```

注意事项：

1. 每个变量都有类型，类型可以是基本类型，也可以是衍生类型
2. 变量名必须是合法的标识符
3. 变量声明是一条完整的语句，因此没一个声明都必须以分号结束

```java
//int a,b,c;
// int a=1,b=2,c=3;  //这种方式程序可读性比较差，在实际代码中，不建议这种方式
String name = "qinjiang";
char x = 'X';
double pi =3.14;

```

> 作用域  在java有三种作用域，分别是 类变量，实例变量，局部变量

```java
public class Demo8 {
    //属性:变量

    //类变量 static修饰的变量为类变量
    static double salary = 2500; //可以直接输出，直接使用
  
    //实例变量: 从属于对象，通过这个类才能使用,如果没有初始化，会使用它的默认值, 布尔值默认是false ;
    // 除了基本类型，其他的默认值都是null;
    String name ;
    int age;

    public static void main(String[] args) {
        //局部变量，必须声明和初始化
        int i = 10;
        System.out.println(i);

        //变量类型 变量名字= new Demo8()
        Demo8 demo8 = new Demo8();
        demo8.age = 10;

    }

    //其他方法
    public void add(){
        System.out.println(i); //i是一个局部变量，这个i就在main定义，无法在add使用
    }
}
```

常量可以理解为一个特殊变量，一旦定义不可改变, 一般常量都用大写字符表示，提高可读性

`final 常量名=常量值`

`static final double PI = 3.14`

修饰符不存在先后顺序

`final static double PI=3.14`

变量命名规范

1. 所有变量、方法、类名，见名知意
2. 类成员变量，首字符小写和驼峰原则，: monthSalary，除了第一个单词意外，后面的单词首字母大写
3. 局部变量， 首字符小写和驼峰原则,
4. 常量，大写字母和下划线 : MAX_LENGTH, MAX_VALUE
5. 类名，首字符大写和驼峰原则 , Man, GoodMan
6. 方法名， 首字母小写和驼峰原则  :run(), runRun()

### 基本的运算符

算术运算符  + - * / % ++ --

赋值运算符 =

关系运算符  > ，<，  >=，   <= ，== ，  != ，  instanceof

逻辑运算符: &&与，   || 或， !非

位运算符  & ， |，  ^  ~ >> << >>>  

条件运算法 ?:

扩展赋值运算符   +=   -=  *=  /=

```java
    public static void main(String[] args) {
        //二元运算符
        //Ctrl+D 复制当前行到下一行
        int a = 10;
        int b =20;
        int c = 10;
        int d = 10;

        System.out.println(a+b);
        System.out.println(a-b);
        System.out.println(a*b);
        System.out.println(a/(double)b);

        long e = 12312312313123L;
        int j = 123;
        short f =10;
        byte h =8;

        System.out.println(e+f+j+h); // 输出Long,如果运算中有Long类型，运算结果一定是Long
        System.out.println(f+j+h); //输出int类型 如果运算中有int类型，运算结果一定是int类型
        System.out.println(f+h); //输出int类型
```

**类型向上转换，在同一个运算关系中，含有不同类型，则向上进行类型转换**

> ++  --的运算时机，根据符号所在的变量位置，从而的出值	

```java
package operator;

public class Demo4 {
    public static void main(String[] args) {
        //++ -- 自加，自减 一元运算符
        int a =3;
        int b =a++;
        // a=a+1  先使用，后自加
        
        //a=a+1 先自加，后使用
        int c = ++a;

        //a++ 先使用，后自加  ++a，先自加，后使用；所有自加，自减，原来是的变量值都会发生变化
    }
}
```



> 逻辑运算符  &&与  ||或  !非

```java
package operator;

public class Demo5 {
    public static void main(String[] args) {
        boolean a = true;
        boolean b =false;

        System.out.println("a && b:"+(a&&b));// 逻辑与运算，两个变量都为真，结果才为true
        System.out.println("a || b:"+(a||b)); //逻辑或运算，两个变量中有一个为真，则结果为true
        System.out.println("!(a && b:)"+!(a&&b)); //如果是真，则变为假，如果是假则为真

        //短路运算
        int c=5;
        boolean d = (c<4)&&(c++<4);
        System.out.println(d);
        System.out.println(c); //上述运算执行结束，并没有运算c++<4
    }
}
```

> 三元运算符及小结



 //字符串连接符， +, 左右两侧只有有一个出现String，则最终结果就会出现String

```java
public class demo6 {
    public static void main(String[] args) {
        int a =10;
        int b =20;
        a+=b;
        a-=b;
        System.out.println(a); //输出10
        //字符串连接符  +
        System.out.println(""+a+b); //输出1020
        System.out.println(a+b+"-");  //输出30-
    }
}
```



x ? y : z , 如果x为真，则是Y， 否则就是Z

```java
public class Demo7 {
    //三元运算符
    public static void main(String[] args) {
        //x ? y : z 如果x==true， 则结果为y， 否则结果为z

        int sore = 80;
        String  type = (sore<60) ? "不及格":"几个"; //必须掌握这个用法
        System.out.println(type);
    }
}
```



### 包机制

为了刚好的组织类，java提供了包机制，提供命名空间， 包的本质就是一个文件夹，防止命名冲突

包语言的规范，

1. 一般用公司域名导致作为包名,防止包名重复: www.baidu.com  com\baidu\www   com\baidu\baike  com\baidu\zhidao
2. 包通过package开头，必须在文件开头， package com.baidu.www;
3. 我们需要使用其他包的内容，就需要使用import  import java.util.Date;

推荐看一个内容，阿里巴巴开发手册。



### Java Doc

参数信息 

@author 作者名

@version 版本

@since 指明需要最早使用的jdk版本

@param 参数名

@return  返回值情况

@throws 异常抛出情况

```
package operator;

/**
 * @author luojg
 * @version 1.0
 * @since 1.8
 * 这是一个类的文档解释
 */
public class Doc {
    String name; //这是一个类变量

    /**
     * @author luojg
     * @param 参数
     * @return 返回的值
     * @throws Exception
     */
    public String test(String name) throws Exception {
        return name;
    }

}
```



> 生成Java Doc的指令为javadoc -encoding UTF-8 -charset UTF-8 Doc.java	

![image-20201019163747153](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201019163747153.png)



使用IDEA自动生成JavaDoc过程

![image-20201019164815714](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201019164815714.png)

关键指令： other command line arguments: `-encoding UTF-8 -charset UTF-8 -windowtitle "JAVASE基础语法" -link https://docs.oracle.com/javase/8/docs/api/`

##  3.JAVA流程控制

 

### 用户交互Scanner

scanner, java.util.Scanner，在java5的新特性，我们可以通过Scanner类来获取用户的输入。

基本的语法：  Scanner s = new Scanner(System.in);

通过Scanner类的netx()和nextLine()方法来获取输入的字符串，在读取前我们一般需要使用hasNext()和hasNextLine()判断是否还有输入的数据

```java
package scanner;

import java.util.Scanner;

public class Demo1 {
    public static void main(String[] args) {
        //创建一个扫描器对象，用于接收键盘数据
        Scanner scanner =new Scanner(System.in);
        System.out.println("使用next方式进行接收:");

        //判断用户有没有输入字符串
        if(scanner.hasNext()){
            String str = scanner.next();
            System.out.println("输入的内容为:"+str);
        }

        //凡是属于IO流的类，用完要关掉
        scanner.close();
    }
}
```

next():

1. 一定要读取到有效字符后才可以结束输入。
2. 对输入有效字符之前遇到空白，netx()方法会自动将其去掉
3. 只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符
4. next()不能得到带有空格的字符串

nextLine()：

1. 以Enter为结束符，也就是说，nextLine()方法返回的是输入回车之前的所有字符。
2. 可以获得空白



**Scanner支持很多输入方式，如下图**

```java
package scanner;

import java.util.Scanner;

public class Demo2 {
    public static void main(String[] args) {
        int i = 0;
        float f = 0.0f;

        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入一个整数数据:");

        if(scanner.hasNextInt()){
            i = scanner.nextInt();
            System.out.println("你输入的整数是:"+i);
        }else {
            System.out.println("你输入的不是整数数据!"+i);
        }

    }
}
```



我们实现一个多次循环的结构

```java
package scanner;

import java.util.Scanner;

public class Demo5 {
    public static void main(String[] args) {
        //我们输入多个数字，并求其总和和平均数，每输入一个数字用回车确认，通过输入非数字来结束输入并输出执行结果
        Scanner scanner = new Scanner(System.in);
        double sum = 0;
        int m =0;
        //通过循环判断是否还有输入，并且里面对每一次进行求和和统计
        while (scanner.hasNextDouble()){
            double x =scanner.nextDouble();
            System.out.println("你输入了第"+(m+1)+"个数据, 然后当前结果sum="+sum);
            m = m+1; //m++
            sum = sum+x;
        }
        System.out.println(m+"个数的和为"+sum);
        System.out.println(m+"个数的平均值是:"+(sum/m));

        scanner.close();
    }
}
```



### 顺序结构

语句与语句之间，框与框之间按照从上到下顺序进行的，他是若干次

```java
package struct;

public class ShunXuDemo {
    public static void main(String[] args) {
        System.out.println("Hello World1!");
        System.out.println("Hello World2!");
        System.out.println("Hello World3!");
        System.out.println("Hello World4!");
        System.out.println("Hello World5!");
    }
}
```

### 选择结构

> if 单选择结构

我们很多时候需要去判断一个东西是否可行，然后我们才去执行，这样一个过程在程序中用if语句来表示

语法结构：

```java
if(布尔表达式){
    //如果布尔表达式为true将执行的语句
}
```

示例代码：

```java
package struct;

import java.util.Scanner;

public class ifDemo1 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入内容:");
        String scanner1 = scanner.nextLine();
        //equals ：判断字符串是否相等
        if(scanner1.equals("Hello")){
            System.out.println(scanner1);
        }
        
        
    }
}
```



> if 双选择结构

现在有个需求，公司要收购一个软件，成功了，给人支付100万一，失败了就自己找开人开发。这样的需求用一个if就搞不定，我们需要有两个判断，需要一个双选择结构，所以就有了if-else结构

语法结构:

```java
if(布尔表达式){
	//如果布尔表达式的值为true
}else{
	//如果布尔表达式的值为false
}
```

示例代码

```java
package struct;

import java.util.Scanner;

public class ifDemo2 {
    public static void main(String[] args) {
        //考试分数大于60分就是及格，小于60分就是不及格
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入成绩:");

        int score = scanner.nextInt();

        if(score>60){
            System.out.println("及格");
        }else {
            System.out.println("不及格");
        }
    }
}
```



> if 多选择结构

如果真实情况还可能存在ABCD，存在区间多级判断，比如90-100就是A，80-90就是B....等等，在生活中我们很多时候的选择也不仅仅只有两个，所以我们就需要一个多选择结构来处理这类问题

语法结构

```java
if(布尔表达式 1){
    //如果布尔表达式 1的值为true执行代码
} else if(布尔表达式 2){
    //如果布尔表达式 2的值为true执行代码
}else if(布尔表达式 3){
    //如果布尔表达式 3的值为true执行代码
}else {
    //如果以上布尔表达式都不为true执行这个代码
}
```

只要一个语句满足，其他就不会执行了。

示例代码

```java
package struct;

import java.util.Scanner;

public class IfDemo3 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
		 System.out.println("请输入成绩:");
        int score = scanner.nextInt();

        if (score ==100){
            System.out.println("恭喜满分!");
        }else if (score>=90&&score < 100){
            System.out.println("A 级别");
        }else if (score>=80&&score < 90){
            System.out.println("B 级别");
        }else if (score>=70&&score < 80){
            System.out.println("C级别");
        }else if (score>=60&&score < 70){
            System.out.println("D 级别");
        }else if (score>=0&&score < 60){
            System.out.println("不及格");
        }else {
            System.out.println("成绩不合法!");
        }

    }
}
```



> 嵌套的if结构

使用嵌套if....else语句是合法的，也就是说你可以再另一个if或者 else if语句中使用if或者else if语句，你可以向if语句一样嵌套else if ....else.



语法  

```java
if(布尔表达式 1){
	//如果布尔表达式 1 的值为true执行代码
	if(布尔表达式 2){
	//如果布尔表达式 2的值为true执行代码
	}
}
```

思考，我们需要寻找一个数，在1-100之间，如何进行快速的查找



> switch多选择结构

```java
switch(expression ){
	// case穿透  //switch匹配一个具体的值
	case value:
        //语句
        break;
    case value:
        //语句
        break;
    case value:
    	//语句
        break;
    default: //可选语句
        //语句
        break;
}
```

示例代码

```java
package struct;

public class SwitchDemo {
    public static void main(String[] args) {
        // case 穿透，//switch 匹配一个具体的值
        char grade = 'C';
        switch (grade){
            case 'A':
                System.out.println("优秀");
                break;
            case 'B':
                System.out.println("良好");
                break;
            case 'C':
                System.out.println("良好");
            case 'D':
                System.out.println("良好");
            case 'E':
                System.out.println("挂科");

            default:
                System.out.println("良好");
                break;
        }
    }
}
```

switch语句中的变量类型可以是：

byte shore int 或者char,

**<font color='red'>在jdk 1.7 以后开始，switch支持字符串String类型了</font>**



### 循环结构

> while循环

while是最基本的循环，结构为

`****while(布尔表达式){`****

​	**`// 循环体表达式`**

**`}`**

这个语法的规则是:

1. 只要布尔表达式内容为true，循环就一直执行下去

2. 我们大多数情况下会让循环停止下来，我们需要一个让表达式失效的方式来结束循环

3. 少部分情况需要循环一直执行，比如服务器的请求响应监听等

4. 循环条件一直为true，就会造成一直循环，也就是死循环，我们在正常业务编程中需要尽量里面死循环，会影响程序性能或者造成程序卡死崩溃

示例代码：

```java
package struct;

public class WhileDemo1 {
    public static void main(String[] args) {
        //实现一个简单的 计算1+2+3+.....100=?
        int i = 1, sum = 0;
        while (i <= 100) {
            sum = sum + i;
            i++;
        }
        System.out.println(sum);
    }

}
```



> do...while循环

do...while简单的理解就是先计算，后循环， 和while不同的是，do...while至少会循环一次

`do{`

​	`//循环体代码`

`}while(布尔表达式);`

while和do...while的区别：

	1. while是先判断，后执行，dowhile是先执行后判断!
 	2. do...while总是保证循环体至少被执行一次，这是主要差别



实例代码:

```java
package struct;

public class DoWhile2 {
    public static void main(String[] args) {
        int a = 0, sum = 0;

        while (a<0){
            a++;
            System.out.println(a);
        }

        System.out.println("===========");

        do {
            sum = sum + a;
            a++;
        } while (a <= 100);
        System.out.println(sum);
    }

}
```

> for循环

虽然所有的循环结构都可以使用while和do...while表示，但是java提供了一个更简单的for循环结构，for循环语句是一种支持迭代的通用结构，是一种最灵活，最有效的循环结构

for循环执行的次数是在执行前就确定的，语法：

```java
for(初始化； 布尔表达式； 迭代){
	//循环体代码语句
}
```

示例代码

```java
package struct;

public class ForDemo1 {
    public static void main(String[] args) {
        int oddSum = 0; //奇数
        int evenSum = 0; //偶数
        //初始化 --条件判断---迭代
        for (int i = 0; i <= 100; i++) {
            if (i % 2 == 0) {
                evenSum += i;
            } else {
                oddSum += i;
            }
        }
        System.out.println("evenSum:"+evenSum + "---oddSum:" + oddSum);

    }
}
```

IDEA使用小技巧

`输入 100.for 自动生成一个for循环语句`

知识点：

	1. 最先执行初始化步骤，可以声明一种类型，但可初始化一个或者多个循环控制变量，可以是空语句。
 	2. 然后检测布尔表达式的值，如果为true，循环体被执行，如果是false，循环终止，开始执行循环体后面的语句，布尔表达式也可以去掉
 	3. 再次检测布尔表达式，循环执行上面过程。

示例代码

```java
//死循环
for(;;){
    
}
```

实现一个用for循环设计，打印出来能被5整除的数，且每3行换行一次

```java
package struct;

public class ForDemo2 {
    public static void main(String[] args) {
        int line = 0;
        for (int i = 0; i <= 1000; i++) {

            if (i % 5 == 0) {
                if (line > 2) {
                    System.out.println();
                    line = 0;
                }
                line++;
                System.out.print("-" + i + "-");

            }
        }
    }
}
```

示例代码，打印一个九九乘法口诀

```java
package struct;

public class ForDemo3 {
    public static void main(String[] args) {
        for (int i=1;i<=9;i++){
            for (int j = 1; j <= i; j++) {
                System.out.print(i+"*"+j+"="+(i*j) +"\t");
            }
            System.out.println();
        }
    }
}
```

输出结果:

```java
1*1=1	
2*1=2	2*2=4	
3*1=3	3*2=6	3*3=9	
4*1=4	4*2=8	4*3=12	4*4=16	
5*1=5	5*2=10	5*3=15	5*4=20	5*5=25	
6*1=6	6*2=12	6*3=18	6*4=24	6*5=30	6*6=36	
7*1=7	7*2=14	7*3=21	7*4=28	7*5=35	7*6=42	7*7=49	
8*1=8	8*2=16	8*3=24	8*4=32	8*5=40	8*6=48	8*7=56	8*8=64	
9*1=9	9*2=18	9*3=27	9*4=36	9*5=45	9*6=54	9*7=63	9*8=72	9*9=81	
```

> 增强for循环	

在jdk5引入的，主要用于数组和集合的遍历，语法格式为

```
for(声明语句:表达式){
	//代码句子
}
```

声明语句，用来声明新的局部变量，改变量类型必须和数组元素的类型匹配。作用域限定在循环语句块，值与此事数组元素的值相等，表达式是要访问的数组名，或者是返回值为数组的方法。



```java
package struct;

public class ForDeme4 {
    public static void main(String[] args) {
        int [] numbers = {10,20,30,40,50}; //定义一个数组
        for (int x:numbers){
            System.out.println(x);
        }
    }
}
```

备注：在python里面for循环用户和Java不一样

```python
objs = Book.Objects.all().iterator()
for obj in objs:
	print(obj.title)
for obj in objs:
	print(obj.title)
	
	
for iterating_var in sequence:
   statements(s)
```



### break&continue

	1. break在任何循环语句的主体部分，均可用break控制循环的流程。Break用于强行退出循环，不执行循环中剩余的语句。在switch语句也可以使用
 	2. continue语句用在循环语句体重，用于终止某次循环过程，既跳过循环体还未执行的语句，并接着进行下一次是否执行循环的判断。

在java可以通过continue实现goto的效果，示例代码：

```java
package struct;

public class LabelDemo {
    public static void main(String[] args) {
        //打印101-500之间的所有质数
        //质数是指在大于1的自然数中，除了1和它本身以外不再有其他因数的自然数。
        int count = 0;
        //不建议使用
        outer:
        for (int i = 101; i <= 150; i++) {
            for (int j = 2; j < i / 2; j++) {
                if (i % j == 0) {
                    continue outer;
                }
            }
            System.out.print(i + "\t");
        }

    }
}
```

### 实践练习

使用for循环语句，打印一个三角形

```java
public class TestDemo {
    public static void main(String[] args) {
        //打印三角形
        for (int i = 1; i <= 5; i++) {
            for (int j=5; j>i;j--){
                System.out.print(" ");
            }
            for (int j=1; j<=i;j++){
                System.out.print("*");
            }
            for (int j=1; j<i;j++){
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

程序运行的结果，

![image-20201020115944998](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201020115944998.png)

## 4. JAVA方法



### 什么是方法

设计方法的原则，方法的本意是功能块，就是实现某个功能的语句块的集合。我们设计方法的时候，最好保持方法的原子性，就是一个方法只能完成1个功能，这样有利于我们后期的扩展。

语法规则

```java
修饰符[public private] [static/可选] 返回值类型 方法名(参数类型 参数名，参数类型 参数名.....){
	...
    方法体
    ...
	return 返回值;
}
```

示例代码

```java
package method;

public class Demo01 {
    //main方法
    public static void main(String[] args) {
        int sum = add(1,2);
        System.out.println(sum);
    }
    //修饰符 返回类型 参数列表
    public static int add(int a, int b) {
        return a + b;
    }
}
```



### 方法的定义和调用

用一些代码片段，来完成特定功能，一般情况下定义一个方法包含以下语法

1. 方法包含一个方法头和一个方法体，下面是一个方法的所有部分
   1. 修饰符，可选的，告诉编辑器如何调用这个方法，定义了这个方法的访问类型
   2. 返回值类型 方法可能返回值，returnValueType是方法返回值的数据类型， 有些方法执行所需操作，但没有返回值，这种情况下returnValueType是关键字void
   3. 方法名 方法的实际名称，方法名和参数表共同构成方法前面
   4. 参数类型 参数像是一个占位符，当方法被调用时，传递值给参数。这个值被称为实参或者变量，参数列表是指参数类型、顺序和参数的个数，参数是可选的，方法可以不包含任何参数
      - 形式参数 在方法被调用时用于接收外界输入的数据
      - 实参 调用方法是实际传给方法的数据
   5. 方法体 方法体包含具体的语句，定义该方法的功能



示例代码：

```java
package method;

public class Demo01 {
    //main方法
    public static void main(String[] args) {
        int sum = add(1,2);
        int max = compareMax(3,10);
        System.out.println(sum);
        System.out.println(max);
    }
    //修饰符 返回类型 参数列表
    public static int add(int a, int b) {
        return a + b;
    }

    public static int compareMax(int num1, int num2){
        /*比较两个数大小*/
        if (num1 == num2){
            System.out.println("num1==num2");
            return 0;//终止方法
        }
        if (num1>num2){
            return num1;
        }else {
            return num2;
        }
    }
}
```

**return 0: 除了返回方法，还有终止方法的功能**

**什么是值传递(pass by value)，什么是引用传递(pass by reference)，课后拓展，为什么说JAVA是值传递**



- 值传递： 是指在调用函数时，将实际参数复制一份传递到函数值，这样在函数中如果对参数进行修改，将不会影响到实际参数

- 引用传递(pass by reference) 是指在调用函数时，将实际参数的地址直接传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。

  ```java
  public static void main(String[] args){
      int i = 10;
      pass(i);
      System.out.println("i=" + i);
  }
  public static void pass(int j){
      j = 20;
      System.out.println("j=" + j)
  }
  ```

  ```java
  
  那么，按照上面的定义，有人得到结论：Java的方法传递是值传递。
  public static void main(String[] args) {
      User user = new User();
      user.setName("facker");
      user.setGender("Male");
      pass(user);
      System.out.println("user =" + user);
  }
   
  public static void pass(User user) {
      user.setName("uzi");
      System.out.println("user =" + user);
  }
  经过pass方法执行后，实参的值竟然被改变了，
  那按照上面的引用传递的定义，实际参数的值被改变了，这不就是引用传递了么。
  于是，根据上面的两段代码，
  有人得出一个新的结论：Java的方法中，在传递普通类型的时候是值传递，在传递对象类型的时候是引用传递。
  public static void main(String[] args) {
      String name = "facker";
      pass(name);
      System.out.println("name = " + name);        //facker
  }
  public void pass(String name) {
      name = "uzi";
      System.out.println("name =" + name);        //uzi
  }
  同样传递了一个对象，但是原始参数的值并没有被修改，难道传递对象又变成值传递了？
  上面的参数其实是值传递，把实参对象引用的地址当做值传递给了形式参数。
  所以说，Java中其实还是值传递的，只不过对于对象参数，值的内容是对象的引用
  ```

  

### 方法重载

概念 ， 重载就是在一个类中，有相同的名称，但是形参不同，这个时候就叫做重载



`方法重载的规则：`

1. 方法名字必须相同；
2. 参数列表必须不同(个数不同 或者类型不同 或者参数排列顺序不同等)
3. 方法返回类型可以相同也可以不同
4. 仅仅返回类型不同不足以成为方法的重载。

实现的理论：方法名称相同时，编译器会根据调用方法的参数个数，参数类型等去逐个匹配，以选择对应的方法，如果匹配失败，则编译器报错。



示例代码：

```java
package method;

public class ReLoadDemo {
    public static void main(String[] args) {
        int sum1;
        float sum2,sum3;
        sum1 = add(3,4);
        sum2 = add(3f,4f,5f);
        sum3 = add(3.45f,2.75f);
        System.out.println(sum1+" "+sum2+" "+sum3);

    }

    public static int add(int a, int b){
        return a+b;
    }

    public static float add(float a, float b){
        return a+b;
    }

    public static float add(float a, float b, float c){
        return a+b+c;
    }
}
```



### 命令行传参

有时候我们希望运行一个程序时候，再传递给他消息，这就要靠传递命令参数给main()函数实现。

```java
package method;

public class CommandLine {
    public static void main(String[] args) {
        for (int i=0; i<args.length;i++){
            System.out.println("args["+i+"]:"+args[i]);
        }
    }
}
```



在执行命令行的时候，提示无法加载朱类 CommandLine

![image-20201020215055362](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201020215055362.png)

首先我们需要在项目根路径下，如本题，目录在F:\code\JAVASE\基础语法\src\，

第二执行命令的时候需要执行    **F:\code\JAVASE\基础语法\src>java method.CommandLine -demo,demo2**  不是执行**F:\code\JAVASE\基础语法\src>java method.CommandLine.class -demo,demo2**

### 可变参数

Java 1.5之后，开始支持传递同类型的可变参数给一个方法

在方法声明中，在指定参数类型后面加一个省略号(......)

一个方法中只能指定一个可变参数，

它必须是方法的最后一个参数，任何普通的参数必须在他的之前声明

```java
package method;

public class Demo4 {
    public static void main(String[] args) {
        Demo4 demo4 = new Demo4();

        demo4.method(1,2,3,4,5,6);
    }

    public void method(int x, int...a){
        System.out.println(x);
        System.out.println(a[0]);
        System.out.println(a[1]);
        System.out.println(a[2]);
        System.out.println(a[3]);
    }

    public void method(int i,int j){};
    public void method(int i,int j,int k){};
}

```



### 什么是递归调用

A方法调用B方法我们很容易理解，递归就是A方法调用A方法，就是自己调用自己



利用递归调用可以用简单的程序来解决一些复杂的问题，它通常把一个大型的复杂的问题，层层转化为一个与原问题相似的规模较小的问题来求解，递归策略只需要少量的程序就可以秒输出解题过程所需要的的多次重复计算，大大减少程序的代码量，递归的能力在于用有限的语句来定义对象的无限集合

**递归结构一般包含两个部分**

	- **递归头 什么时候不调用自身方法，如果没有递归头将陷入死循环**
	- **递归体  什么时候需要调用自身方法**

```java
package method;

public class Demo6 {
    //2! 2*1
    //3! 3*2*1
    //5! 5*4*3*2*1

    public static void main(String[] args) {
        System.out.println(f(10));
    }

    public static int f(int n) {
        if (n == 1) {
            return 1;
        } else {
            return n * f(n - 1);
        }
    }
}
```

![image-20201020232252802](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201020232252802.png)

```java
//一个计算器的代码
package method;

import java.util.Scanner;

public class CalcanderDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String ss =null; //初始待输入计算穿
        char c = '+';  //初始计算符号
        double inputNumber=0; //承载输入的数字
        double result=0;

        if (scanner.hasNextLine()){
            ss = scanner.nextLine();
        }
        System.out.println("你输入的整数是:"+ss);

        //示例输入  157-20+10=
        for (int i = 0; i < ss.length(); i++) {
            if (ss.charAt(i)<='9'&&ss.charAt(i)>='0'){
                //如果字符在0-9之间，就拼接成一个输入数字
                inputNumber =inputNumber*10+ss.charAt(i)-'0';
            }else {
                //如果不是数字，那么就是符号，进行符号的选择
                switch (c){
                    case '+':
                        result =add(result,inputNumber);
                        break;
                    case '-':
                        result =sub(result,inputNumber);
                        break;
                    case '*':
                        result = mul(result,inputNumber);
                        break;
                    case '/':
                        result = div(result,inputNumber);
                        break;
                }

                inputNumber =0; //输入数字清空，承接下一个数字
                c = ss.charAt(i); //更改运算符号
                if(c=='='){
                    System.out.println("计算结果是:"+ result);
                    break;
                }
            }
        }
    }
    public static double add(double...num){
        if (num.length==0){
            System.out.println("请确认您的输入");
        }
        int result =0;
        for (int i = 0; i < num.length; i++) {
            result += num[i];
        }
        return result;
    }

    public static double sub(double...num){
        if (num.length==0){
            System.out.println("请确认您的输入");
        }else if (num.length==1){
            System.out.println("只有一个参数，该方法需要两个参数");
        }
        double result =num[0];
        for (int i = 1; i < num.length; i++) {
            result -= num[i];
        }
        return result;
    }

    public static double mul(double...num){
        if (num.length==0){
            System.out.println("请确认您的输入");
        }else if (num.length==1){
            System.out.println("只有一个参数，该方法需要两个参数");
        }
        double result =num[0];
        for (int i = 1; i < num.length; i++) {
            result *= num[i];
        }
        return result;
    }

    public static double div(double...num){
        if (num.length==0){
            System.out.println("请确认您的输入");
        }else if (num.length==1){
            System.out.println("只有一个参数，该方法需要两个参数");
        }
        double result =num[0];
        for (int i = 1; i < num.length; i++) {
            result /= num[i];
        }
        return result;
    }
}
```





## 5.JAVA数组

### 数组概述

数组是相同类型数据的有序集合；

数组描述的是相同类型的若干数据，按照一定的先后次序排列组合而成；

其中每个数据成为一个数组元素，每个数组元素可以通过下标来访问他们；



### 数组声明创建

首先必须声明数组变量，才能在程序中使用数组，声明数组的语法

```java
dataType [] arrayRefVar; //首选的方法
dataType arrayRefVar[];  //效果相同，但不是首选方法	
```

JAVA语言使用new操作符来创建数组，语法如下:

```java
dataType [] arrayRefVar = new dataType[arraySize];	
```

数组的元素通过索引来进行访问，数组索引是从0号开始。

获取数组的长度 arrays.length

示例代码:

```java
package array;

public class Demo2 {
    public static void main(String[] args) {
        int[] nums; //声明一个int类型数组
        nums = new int[10]; //创建一个数组
        int[] num2 = new int[20];
        //给数组赋值
        for (int i = 0; i < 10; i++) {
            nums[i]=i;
        }

        System.out.println(nums[5]);
        int sum=0;
        //计算所有元素的和
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        System.out.println("数组总和为:"+sum);
    }
}
```

> 内存分析

![image-20201021100841741](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201021100841741.png)



![image-20201021104326555](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201021104326555.png)



int[] array=null  //此时只在栈里面，但是还不存在，声明数组，数组并不存在，只有创建的时候，数组才存在

array = new int[10] // 创建数组，在堆里开辟了一个空间，并分成10份

array[0]  =1

array[1]  =1

array[2]  =1

array[3]  =1 //给数组赋值， 在堆里的空间，给地址填上值

数组有三种初始化

静态初始化

```java
int[] a = {10,20,202,20,20,20} //声明+赋值， 一旦定义不可改变
Man[] mans = {new Man(),new Man(),new Man()}
```



动态初始化

```java
int[] array = new int[10];
array[0] = 10;
// 动态初始化，包含默认初始化，比如 array[1]的默认值就是0
```



默认初始化

数组是引用类似，它的元素相当于类的实例变量，其中每个元素也被按照实例变量同样的方式被隐式初始化，比如int默认就是0

> 数组的下标越界及小结

1. 数组长度是确定的，一旦被创建，它的大小就是不可以改变的；
2. 元素必须是相同类型，不允许出现混合类型
3. 数组中的元素可以是任何数据类型，包括基本类型和引用类型
4. 数组变量属于引用类型，数组也可以看成是对象，数组中的每个元素相当于该对象的成员变量
5. 数组本身就是对象，java中对象是在堆中的，因此数组无论保存原始类型还是其他对象类型，数组对象本身就是在堆中





> 小结

1. 数组是相同数据类型的有序集合(数据类型可以为任意类型)
2. 数组也是对象，数组元素相当于对象的成员变量
3. 数组长度是确定的不可变的，如果越界，会爆出ArrayIndexOutofBounds

###  数组的使用

For-Each循环/ 数组作为方法入参 /数组作为返回值

```java
package array;

public class ArrayDemo3 {
    public static void main(String[] args) {
        int[] arrays = {1,2,3,4,5};
        //打印全部数组元素
        for (int i = 0; i < arrays.length; i++) {
            System.out.println(arrays[i]);
        }
    }
}
```

在jdk1.5 不再使用下标来使用数组

```java
package array;

public class ArrayDemo4 {
    public static void main(String[] args) {
        int[] arrays ={1,2,3,4,5};
        for (int array : arrays) {
            System.out.println(array);
        }
    }
}
```

在IDEA可以通过快捷输入 arrays.for来快速生成for循环语句。



在方法中传入数组

```java
package array;

public class ArrayDemo4 {
    public static void main(String[] args) {
        int[] arrays ={1,2,3,4,5};
        for (int array : arrays) {
            System.out.println(array);
        }
        prinArray(arrays);
    }

    public static void prinArray(int[] arrays){
        for (int array : arrays) {
            System.out.println(array);
        }
    }
}

```



返回一个数组

```java
    //数组作为返回
    public static int[] reverse(int[] arrays){
        int[] result = new int[arrays.length];
        //反转操作
        for (int i = 0, j= result.length-1; i < arrays.length ; i++, j--) {
            result[j] = arrays[i];
        }
        return result;
    }
```



### 多维数组

多维数组就可以看成一个数组的数组，

```java
int a[][] = {{1,2,3,4},{1,2,3,4}};
int[][] demo = {{1,2,3,4},{1,2,3,4}};
```

示例代码，输出一个二维数组的信息

```java
package array;

public class ArrayDemo5 {
    public static void main(String[] args) {
        int[][] array = {{1,2},{3,4},{5,6},{7,8},{9,21}};

        for (int i = 0; i < array.length; i++) {
            for (int j = 0; j < array[i].length; j++) {
                System.out.print(array[i][j]+"\t");
            }
            System.out.println();
        }
    }
}
```



实际输出结果：

![image-20201021121745112](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201021121745112.png)



上述案例生成的数组在系统中的内存组织形式

![image-20201021122524005](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201021122524005.png)



### Arrays类

数组的工具类java.util.Arrays

由于数组对象本身并没有什么方法可以供我们调用，但是API中提供了一个工具类Arrays供我们使用，从而可以对数据对象进行一些基本的操作；

我们可以查看jdk的帮助文档

Arrays类中的方法都是static修饰的静态方法，在使用的时候可以直接使用类名进行调用，而不用使用对象来调用，注意：是“不用”而不是"不能"

具有以下常用的功能

	1. 给数组赋值  通过fill方法
 	2. 给数组排序 通过sort方法，按照升序排列
 	3. 比较数组， 通过equals方法比较数组中元素值是否相等
 	4. 查找数组元素，通过binarySearch方法能对排序好的数组进行二分查找法操作；



示例代码

```java
package array;

import java.util.Arrays;

public class ArrayDemo6 {
    public static void main(String[] args) {
        int[] a = {1,2,3,4,9090,0,542,21,3,23};
        System.out.println(a);//直接打印是一个hashCode
        //打印数组元素
        System.out.println(Arrays.toString(a));
        //自己方法打印
        printArray(a);

        Arrays.sort(a);//对数组进行排序
        //System.out.println(Arrays.toString(a));
    }

    public static void printArray(int[] a){
        //写一个和系统的Arrays.toString类似的方法
        for (int i = 0; i < a.length; i++) {
            if (i==0){
                System.out.print("[");
            }
            if (i==a.length-1){
                System.out.println(a[i]+"]");
            }else {
                System.out.print(a[i]+",\t");
            }
        }
    }
}
```

> 冒泡排序算法

冒泡排序是最为出名的排序算法之一，总共有八大排序算法

冒泡排序的代码相当简单，两层循环，外层是冒泡的论述，里面依次进行比较，江湖中人人皆知

我们看到嵌套循环，应该立马可以得到这个算法的时间复杂度为O(n2)

思考：如何去优化冒泡排序

示例代码：

```java
package array;

import java.util.Arrays;

public class ArrayDemo7 {
    public static void main(String[] args) {
        //冒泡排序
        //比较数组中，两个相邻的元素，如果第一个数比第二个数大，就交换他们位置
        //每一次比较，都会产生一个最大或者最小的数字；
        //下一轮就会少一次排序，依次循环，直到结束
        int[] a = {3,5,7,1,20,5};
        int[] sort = sort(a);
        System.out.println(Arrays.toString(sort));
    }

    public static int[] sort(int[] arrays){
        /*冒泡排序*/
        //外层循环，判断我们这个要走多少次
        int temp =0;
        for (int i = 0; i < arrays.length-1; i++) {
            //内层循环，比价判断两个数，如果第一个数比第二个数大，交换位置
            for (int j = 0; j < arrays.length-1; j++) {
                if(arrays[j+1]>arrays[j]){
                temp = arrays[j+1];
                arrays[j+1]=arrays[j];
                arrays[j]=temp;
                }
            }
        }
        return arrays;
    }
}

```







### 稀疏数组

![image-20201021140849306](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201021140849306.png)



![image-20201021140956106](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201021140956106.png)



上述的稀疏数组

11行 11列  3

1       1      1

2       3      2



示例代码：

```java
package array;

public class ArrayDemo8 {
    public static void main(String[] args) {
        //1.创建一个二维数组，11*11,0代表没旗子，1代表黑棋，2代表白棋
        int[][] arrays1 = new int[11][11];
        arrays1[1][2] = 1;
        arrays1[2][3] = 2;
        System.out.println("输出原始的数组:");

        for (int[] array : arrays1) {
            for (int i : array) {
                System.out.print(i + "\t");
            }
            System.out.println();
        }

        //转换为稀疏数组
        System.out.println("将arrays1转化为稀疏数组array2");
        System.out.println("===========================");
        //1.先统计有多少行多少列，多少有效数字
        int row, col, sum = 0;
        row = arrays1.length;
        col = arrays1[0].length;
        for (int i = 0; i < arrays1.length; i++) {
            for (int j = 0; j < arrays1[i].length; j++) {
                if (arrays1[i][j] != 0) {
                    sum++;
                }
            }
        }
        int[][] arrays2 = new int[3][sum + 1];
        //2.初始化稀疏数组第一个元素
        arrays2[0][0] = row;
        arrays2[0][1] = col;
        arrays2[0][2] = sum;

        //3.填充其他元素
        int count = 0; //稀疏矩阵游标
        for (int i = 0; i < arrays1.length; i++) {
            for (int j = 0; j < arrays1.length; j++) {
                if (arrays1[i][j] != 0) {
                    count++;
                    arrays2[count][0] = i;
                    arrays2[count][1] = j;
                    arrays2[count][2] = arrays1[i][j];
                }
            }
        }

        System.out.println("============输出稀疏数组=============");
        for (int i = 0; i < arrays2.length; i++) {
            System.out.println(arrays2[i][0]+"\t"
                    + arrays2[i][1]+"\t"
                    +arrays2[i][2]);
        }

        //还原稀疏矩阵 arrays3
        System.out.println("============还原稀疏数组=============");

        int[][] arrays3 = new int[arrays2[0][0]][arrays2[0][1]];
        for (int i = 1; i < arrays2.length; i++) {
            arrays3[arrays2[i][0]][arrays2[i][1]]=arrays2[i][2];
        }

        for (int i = 0; i < arrays3.length; i++) {
            for (int j = 0; j < arrays3.length; j++) {
                System.out.print(arrays3[i][j]+"\t");
            }
            System.out.println();
        }

    }
}
```





## 6. JAVA面向对象

### 初识面向对象

属性+方法  构成一个类

物以类聚： 分类的思维模式，思考问题首先解决问题需要哪些分类，然后对这些分类进行独立思考，最后，才对某个分类下的细节进行过程的思考

面向过程适合处理复杂的问题，适合处理需要多人协作的问题



对于描述复杂的事务，为了从宏观上把握，从整体上合理分析，我们需要使用面向对象的思路来分析整个系统，但是具体到微观操作，仍然需要面向过程的思路去处理



**面向对象编程的本质是： 以类的方式组织代码，以对象的组织封装数据**



抽象--student manage

一个类里面可以有多个class，但是只能有一个public class，如果一个文件多个public类，错误提示：Class ‘Person1’is public，should be  declared in a file named 'Person1.java'

三大特性：

1； 封装   2；继承   3；多态

从认识事务的角度考虑是先有对象后有类，对象，是具体的事务，类，是抽象的，是对对象的抽象

从代码运行角度考虑，先有类后有对象，类是对象的模板；

创建对象 使用new关键字



### 方法的回顾

方法的定义

- 修饰符  public  private static
- 返回类型  返回八大类型+引用类型   char  byte short int long float double boolean
- break和return的区别   break跳出switch和循环， return代表方法结束了
- 方法名   注意规范，见名知意
- 参数列表  (参数类型 参数名)......可以空参数
- 异常抛出  throws IOException

方法调用：递归

- 静态方法  static修饰符，可以直接调用, 和类一起加载的
- 非静态方法  未加static修饰符的方法，调用需要先把类实例化，然后调用 Student student = new Student(),  student.say(); 类实例化后才存在
- 形参和实参  实际参数和形式参数的类型要一一对应
- 值传递和引用传递  java全部是值传递
- this关键字  代表当前的类或者对象

```java
package oop;

public class Demo01 {
    public static void main(String[] args) {

    }
    /*
    * 修饰符  返回值类型，方法名(....){
    *   //方法体
    *   // return 返回值
    * }
    * */
    public String sayHello(){
        return "HelloWorld";
    }
    public void hello(){
        return;//对于void类型，我们可以不写return ，也可以直接return;
    }

    public int max(int a, int b){
        return a>b?a:b;//三元运算符
    }
}

```



### 对象创建分析

使用new关键字创建对象

- 使用new关键字创建的时候，除了分配内存空间之外，还会给创建好的 对象进行默认的初始化 以及 对类中构造器的调用



类中的构造器也叫作构造方法，在进行创建对象的时候必须要调用的，并且构造器具有如下两个特点

- 必须和类的名字相同
- 必须没有返回类型，也不能写void

**构造器(Construct)的概念必须要掌握**



一个类即使什么都不写，也会存在一个默认的构造方法；

**构造器核心作用：**

	- 使用new关键字，必须要有构造器，如果没有构造器，就会报错，使用new的时候，本质是在调用构造器，实例化的时候，构造一些初始值
	- 一旦定义了有参构造器，那无参构造器就必须显示定义出来
	- 构造器一般用来初始化值

`//在IDEA中，我们可以使用alt+insert 自动生成构造器`

示例代码:

```java
package oop.demo02;

public class Person {
    //属性
    String name;
    String age;
    //方法
    public Person(){
        //构造方法 无参构造，可构造一些初始化信息
        this.name ="秦僵";
    }
    //有参数构造器,一旦定义了有参构造器，无参构造器就必须显示定义
    public Person(String name){
        this.name=name;
    }

    public Person(String name, String age) {
        this.name = name;
        this.age = age;
    }
}

```

总结：

1. 构造器和类名相同，没有返回值，void也不行；
2. new关键词本质是在调用构造方法；
3. 初始化对象的值是他作用；
4. 定义了有参构造后，如果想使用无参构造器，必须显示的定义一个无参构造器；

​    this. 代表是当前类的，后面的值是传入进来的类



对象创建的内存分析

<img src="F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201022160242741.png" alt="image-20201022160242741" style="zoom:67%;" />

小结：

> ```
> /*
> 1.类与对象，类是一个模板，抽象，对象是一个具体的实例
> 2.方法，定义调用
> 3.对象的引用
>     引用类型： 基本类型  byte  shot int long float double boolean char
>     对象是通过引用来操作的,栈--->堆
> 4.对象的属性，其他地方也叫字段Field，或者叫成员变量
>     默认初始化 ，数字：0,0.0  char:u000  boolean :false 引用:null
> 5. 修饰符 属性类型 属性名 =属性值!
> 6. 方法定义格式防止死循环
> 7.对象的创建和使用，
>     - 必须使用new关键字创建对象，需要有构造器，
>     - 对象的属性   new Person().age;
>     - 对象的方法  new Person().sleep();
> 8.类里面
>     - 静态的属性   属性
>     - 动态的行为   方法
>  */
> ```





### 面向对象的三大特性

**封装、继承、多态，是重点  !**

封装： 该露的露，该藏的藏，我们程序设计要追求 `高内聚，低耦合`，高内聚就是类的内部数据操作细节自己完成，不允许外部干涉，低耦合就是仅仅暴露少量的方法给外部使用

封装，通常应该禁止直接访问一个对象中数据的实际表示，而通过操作接口来访问，这称之为信息隐藏

**记住一句话就够： 属性私有，get/set**

示例代码：

```java
package oop.demo4;

public class Sutdent {
    //姓名
    private String name;
    //学号
    private int id;
    //性别
    private int sex;
    //提供一些可以操作这些属性的方法,这时候就需要提供一个public的get,set方法，get是获取属性，set是设置属性值

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getSex() {
        return sex;
    }

    public void setSex(int sex) {
        this.sex = sex;
    }
}

```

封装意义：

1. 提高程序的安全性，保护数据，比如我们类中有一个age属性，我们可以在setAge()方法中进行判断，从而保证数据的安全性

```java
public class Sutdent {
    //姓名
    private String name;
    //学号
    private int id;
    //性别
    private int sex;
    //年龄
    private int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age>120 ||age<0){
            this.age =3;
        }else {
            this.age = age;
        }
    }
```



2. 隐藏代码的实现细节

3. 统一接口，

4. 提高了系统的可维护性,

重载： 方法名相同，参数不同(参数类型不同，参数个数不同 或者顺序不同)，返回值类型可以相同也可以不同，叫做重载

>  继承, 本质是对某一批类的抽象，从而实现对现实世界更好的建模  extends 关键字，意思是扩展，子类是父类的扩展。
>
> Java中的类只有单继承，没有多继承

**封装 继承  多态**



对于普通方法， 如果子类重写了父类的方法，默认使用子类，想使用父类方法，需要使用super.classMethod()

对于构造器的继承:

父无参， 子无参， 子类new一个对象，先父构造器，再子构造器





继承是类与类之间的一种关系，除此之外，类与类之间还有依赖，组合，聚合等

继承关系是两个类，一个是子类（派生类），一个是父类(基类)，子类继承父类，使用关键字extends来表示

子类和父类直接，从意义上讲应该是具有is a 的关系

- object类
- super
- 方法的重写

子类不可继承父类私有的东西。public ->protected->default->private  继承优先级 从高到低

快捷继承键 CTRL+H 查看当前类的所有继承树关系，在JAVA中所有的类都默认直接或者间接继承object类

JAVA只有单继承，没有多继承，一个父类可有多个子类，一个子类只有一个父类；

> super关键字， 可和this进行对比，注意私有的private修饰的不可被继承， 
>
> 在构造器中，子类的构造器默认会调用父类的构造体，且父类的构造器必须放在子类构造器的第一行

super父类属性示例代码

```java
//父类
public class Person {
    protected String name ="kuangshen";
}
//子类
package oop.Demo5;
//子类继承了父类，就会拥有父类的所有方法
public class Student extends Person {
    protected String name ="qinjiang";
    public void test(String name){
        System.out.println(name);
        System.out.println(this.name);
        System.out.println(super.name);
    }
}
//测试类
public class Application {
    public static void main(String[] args) {
        Student student = new Student();
        student.test("秦疆");
    }
}
输出：
    秦僵
    qinjiang
    kuangshen
```

super父类方法的示例代码

```java
//父类方法
public class Person {
    public void print(){
        System.out.println("Person 父类方法执行");
    };
}
//子类方法
public class Student extends Person {
    public void print(){
        System.out.println("Student 子类方法执行");
    }
    public void test(){
        print();
        this.print();
        super.print();
    }
//测试程序
public class Application {
    public static void main(String[] args) {
        Student student = new Student();
        student.test();
    }
}
输出结果：
Student 子类方法执行
Student 子类方法执行
Person 父类方法执行
```

super构造器示例代码

```java
//父类构造器
public class Person {
    public Person() {
        System.out.println("父类无参构造器");
    }
}
//子类构造器
public class Student extends Person {
    public Student() {
        System.out.println("子类无参构造器");
    }
}
//测试类
public class Application {
    public static void main(String[] args) {
        Student student = new Student();

    }
}
输出结果：
    父类无参构造器
	子类无参构造器
```

子类会默认隐藏的调用父类的构造器，如果显示调用父类构造器，必须把父类super()放在第一个

如果父类写了有参构造器，在子类就不能调用父类的无参构造器，只能调用有参构造器，使用方法有两种

```java
//父类Person的构造器是一个有参的构造器
public class Person {
    public Person(String name) {
        System.out.println("父类无参构造器");
    }
}
//方法一.在子类的无参构造器中，super父类的有参构造器
public class Student extends Person {
    public Student() {
        super("秦僵");
        System.out.println("子类无参构造器");
    }
}
//方法二。直接在子类继承父类的有参构造器
public class Teacher extends Person {
    public Teacher(String name) {
        super(name);
    }
}

```

子类的无参构造器，默认调用父类的无参构造器，

```java
public class Student extends Person {
    public Student() {
        super("秦僵");
        System.out.println("子类无参构造器");
    }
}
```



super小结:

```java
- super是调用父类的构造方法，必须在构造方法的第一个
- super必须只能出现在子类的方法或者构造方法中
- super和this不能同时调用构造方法!
```

super和this的对比

```java
- 代表的对象不同，this代表调用者本身这个对象，super代表父类对象的应用
- 使用前提也不同， this 没有继承也可以使用，super只能在继承条件下才可以使用
- 构造方法也不同，this默认调用的是本类的构造，super默认调用的是父类的构造
```

> 重写，        重写都是方法的重写，和属性是无关的

示例代码

```java
//父类的
public class B {
    public  void say(){
        System.out.println("B=>say()");
    }
}

//子类的
public class A extends B {
    @Override
    public void say() {
        System.out.println("A=>say()");
    }
}

//测试类的
public class Application {
    //静态方法和非静态方法区别很大
    //静态方法，方法的调用只和左边有关，定义的数据类型
    //非静态方法，是重写，可以选择重写，而且只能是public
    public static void main(String[] args) {
        //方法的调用只和左边有关，定义的数据类型有关
        A a = new A();//走了子类A类的方法
        a.say();
        //父类的引用指向了子类
        B b = new A();//走了父类B类的方法
        b.say();

    }
}

输出：
    A=>say()
	A=>say()
```

重写（Override）的总结：

- 重写需要有继承关系，是子类重写父类的方法!
- 子类和父类的方法名必须相同，参数列表也必须相同，修饰符不能是static，范围可以扩大，但是不能缩小，比如父类是public,子类不能default 
- 抛出的异常，也有范围，范围可以被缩小，但是不能被扩大。ClassNotFoundException--->Exception(大)

重写，子类的方法和父类的方法必须一致，方法体不同!  

快捷键 Alt+Insert--->override

为什么需要重写呢？

1. 父类的功能子类不一定需要或者不一定满足；

> 多态，  先封装，再继承，最后多态，可以实现动态编译，

动态编译，只有执行代码时候才会编译，多态让程序具有可扩展性

示例代码，

```java
//父类
public class Person {
    public void run(){
        System.out.println("person run");
    }
}
//子类
public class Student extends Person {
    @Override
    public void run() {
        System.out.println("student run");
    }
    public void eat(){
        System.out.println("student eat");
    }
}

//测试类
public class Application {
    public static void main(String[] args) {
        //一个对象的实际类型是确定的
        //可以指向的引用类型就不确定了；父类的引用指向了子类

        //Student子类能调用的方法都是自己的或者继承父类的
        Student s1 = new Student();
        //Person父类型，可以指向子类，但是不能调用子类独有的方法!
        Person s2 = new Student(); //父类的应用指向子类

        Object s3 = new Student();
        //s2对象能执行那些方法，主要看对象左面的类型，和右边关系不大。
        s1.run();
        s2.run(); //一旦子类重写父类的方法，就会走子类的方法
        
        s1.eat();//可以调用执行
        s2.eat();//就不可调用，编辑器会报错
    }
}
执行结果：
    student run
	student run
```



多态的注意事项

- 多态是方法的多态，属性没有多态；
- 父类和子类，有联系，才可以进行多态，没有联系强转会提示  类型转换异常  ClassCastException! 说明符子类有问题
- 存在的必要条件，必须有继承关系，方法需要重写，父类的引用指向的子类对象，father  f1 = new Son()
  - statci方法，属于类，不属于实例，
  - final 方法，是常量，也不能修改重写
  - private， 这个是私有的，也不能重写



自己的理解

```
类A(子类)--->类B(父类)

使用：
A a  = new A()
B b = new A()

b.run() ,如果子类重写方法，执行子类，没重写，执行父类， b可以执行的方法由左边父类B确定，和右边的子类A关系不大，b无法调用子类独有的方法
a.run() 子类，可以调用自己的独有方法或者继承父类的方法
```





>  instanceof  类型转换（），引用类型的转换

instanceof  引用类型，判断一个对象是什么类型

用法

```java
//object>person>student
Object object = new Student()
System.out.println(object instanceof Student); //true
System.out.println(object instanceof Person); //true
System.out.println(object instanceof Object);//true
System.out.println(object instanceof Teacher);//false

（x instanceof y ）; 能否编译通过，就看X指向的实际类型是否是Y的子类型
```

类型之间的基本转换  ，由高向低，需要强制转换 Double float long int->short->byte->char   由第向高，不需要强制转换



由父 --->子

   高-----低

```java
public class Application {
    public static void main(String[] args) {
        //高 --------------低
        Person obj = new Student();
        //student将这个对象转化为Student，我们就可以使用Student类型的方法了!
        Student student = (Student)obj;
       ((Student) obj).go("强制类型转化");

       //低-----高，直接转化
        Student stu = new Student();
        stu.go("测试");
        Person per = stu;
        per.go  //子类转化为父类，可能丢失子类本来的一些方法，这个案例，go就无法走了
        
    }
}
```



多态的总结:

- 父类的引用指向子类的对象；
- 子类的对象转化为父类，此为向上转型，直接转化，但可能丢失子类的一些方法；
- 父类的对象转化为子类，此为向下转换，需要强制转换；
- 方便方法的调用，减少重复的代码，提高利用率，让代码简洁!



抽象，封装，继承，多态都是为了抽象

> static 关键字讲解

示例代码

```java
public class Person {
    private static int age; //静态变量， 在内存只会存储一个
    private double score;  //非静态变量

    public void run(){

    }

    public static void go(){

    }
    //加上static就是静态代码块，只会执行一次
    static {
        //代码块
        System.out.println("静态代码块");
    }
    //这是一个匿名代码块,
    {
        //代码块
        System.out.println("匿名代码块");
    }

    public static void main(String[] args) {
        Person per = new Person();

        System.out.println(Person.age);
        //推荐静态变量直接用类来调用
        System.out.println(per.score);
        System.out.println(per.age);

        //静态方法可以直接调用静态方法，静态方法不能调用非静态方法
        Person.go();  //可以用类直接调用
        go(); //在当前类，static可直接调用
        per.run();
        per.go();

        //静态代码块，最早执行，但只执行一次，然后执行非静态代码块，然后执行构造器,一般代码块用来附初始值
        Person per1 = new Person();
        System.out.println("========");
        Person per2 = new Person();
        
        
    }
}
```



### 抽象类和接口

abstract修饰符可以用来修饰方法也可以修饰类，如果修饰方法，就是抽象方法

示例代码

```java
package oop.demo10;
//abstract 抽象类  本质是一个类  extends (接口可以实现多继承)
public abstract class Action {
    //约束 abstract 抽象方法 只有方法名字，没有方法实现
    public abstract void doSomething();
}

//子类
package oop.demo10;
//抽象类的所有方法，继承了她的子类，都必须实现它的方法，除非子类也是抽象类
public class A extends Action {
    @Override
    public void doSomething() {

    }
}

```

抽象类的特点：

- 不能new出来一个抽象类，只能靠子类实现它，只是一个约束
- 抽象类里面可以写普通的方法
- 抽象方法必须在抽象类中~
- 抽象的抽象: 就是个约束

`思考题:` 抽象类不能new,那存在构造器么？

​			抽象类的意义是什么，为什么需要抽象类？ 比如游戏有很多角色，每个方法都写很麻烦，我们只需要把方法抽象出来，每个角色实现它就行了，提高开发效率



> 接口

普通类  只有具体的实现

抽象类， 具体实现和规范(抽象方法)都有

接口  只有规范，自己无法写方法~ 专业的约束!  约束和实现进行分离。



接口的本质就是规范，定义的是一组规则，体现了现实世界中“如果你是。。。则必须能。。。”的思想

接口的本质是七月，就像我们人间的法律一样，制定好后大家都要遵守

00的精髓，就是对象的抽象，最能体现这一点的就是接口，为什么我们讨论设计模式都只针对具备抽象能力的语言，(c++ java C#)等，就是因为设计模式所研究的，实际上是如何合理的去进行抽象。



声明类的关键字是class 声明接口的关键字是interface

实例代码：

```java

//定义的关键字是interface， 如何能够锻炼自己的抽象思维
public interface UserService {
    //接口中的所有定义其实都是抽象的，public abstract
    //接口中也可以定义属性，定义的属性都是常量，修饰符 publist static final int AGE =99
    int age=99;

    void add(String name);
    void delete(String name);
    void update(String name);
    void query(String name);
}



public interface TimeService {
    void timer();

}

//类可以实现接口，implements
//抽象类  extends
//实现了接口的类，就需要重写接口中的方法
//多重继承，利用接口实现多继承
public class UserServiceImpl implements UserService, TimeService {
    @Override
    public void run(String name) {

    }

    @Override
    public void add(String name) {

    }

    @Override
    public void delete(String name) {

    }

    @Override
    public void update(String name) {

    }

    @Override
    public void query(String name) {

    }

    @Override
    public void timer() {

    }
}

```



接口的一个总结：

- 接口的作用 ， 一个约束，可以让我们定义一些方法，让不同的人实现; 10个员工可能共同完成1个接口，有10中不同的实现方法
- 方法都是 用 public abstract
- 常量都是  public static final
- 接口不能被实例化，接口中是没有构造方法的
- 接口可以实现多个接口，通过implements实现，
- 实现接口，必须要重写接口中的方法~

### 内部类和OOP实战(必修)

内部类就是在一个类的内部定一个一个类，比如A类中定义一个B类，那么B类相对于A类来说就成为内部类，而A类相对于B类来说就是外部类。

- 成员内部类
- 静态内部类
- 局部内部类
- 匿名内部类
- LAMBADA表达式

总会遇到一些奇葩的代码，就是内部类。

`IDEA快捷键： 写了 new Outer()之后，我们按 ctrl +alt +v 能快速的补充前面的字符`



## 7. 异常(Exception)



###  异常体系结构

​	什么是异常   实际工作中，遇到的情况不可能是非常完美的。比如你的某个模块，用户输入的不一定符合你的要求，你的程序要打开某个文件，这个文件可能不存在或者文件格式不对，你要读取数据库的数据，数据可能是空的等，我们的程序再跑着，内存或者硬盘可能满了，等等。

​	软件程序在运行过程中，非常可能遇到刚刚提到的这些异常问题，我们叫做异常，英文叫做`Exception`，意识是例外。这些例外，或者叫异常，怎么让我们写的程序作出合理的处理。

​	异常指程序运行中出现的不期而至的各种情况，如文件找不到，网络连接失败，非法参数等

异常发生在程序运行期间，它影响了正常的程序执行流程。



java把异常当做对象来处理，并定义一个基类 java.lang.Throwable作为所有异常的超类。在java api中已经定义了许多异常类，这些异常类分为两大类，错误Error和异常Exception.

![image-20201023172458753](F:\MD格式学习笔记库\狂神JAVA-1-课堂随记.assets\image-20201023172458753.png)



Error类对象由Java虚拟机生成和抛出，大多数错误与代码编写者所执行的操作无关

Java虚拟机运行错误(Virtual MachineError), 当JVM不再继续执行操作所需要得内存资源时，将出现OutOfMemoryError，这些异常发生时，java虚拟机(JVM)一般会选择县城终止

还有发生在虚拟机师徒执行程序时，如类定义错误(NoClassDefFoundError)、链接错误(LinkageError)。这些错误是不可查的，因为他们在应用程序的控制和处理能力之外，而且绝大多数是程序运行时不允许出现的状态。



RuntimeException

- ArrayIndexOutOfBoundsException(数组下标越界)
- NullPointerException(空指针异常)
- ArithmeticException(算术异常)
- MissingResourceException(丢失资源)
- ClassNotFoundException(找不到类)等异常，这些异常是不检查异常的，程序中可以选择捕获处理，也可以不处理。

这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。

Error通常是灾难性的致命的错误，是程序无法控制和处理的。当出现这些异常时，Java虚拟机一般会选择终止线程，Exception通常情况下是可以被程序处理的，并且在程序中应该尽可能的去处理这些异常。

发现异常和报错，表示自己学习机会，学习了

### 处理异常



如何抛出异常



如何捕获异常



try  catch  finally throw throws

示例代码：

```java
public class Test {
    public static void main(String[] args) {
        int a = 1;
        int b = 0;
        try {//try监控区域
            System.out.println(a / b);
        } catch (ArithmeticException e) {
            System.out.println("程序不能处理，变量B不能为0");
        }finally {//处理善后工作
            System.out.println("finally");
        }
    }
}
```

假设要捕获多个异常，从小到大的捕获，最后面是大的，例如下面如果把Throwable t 放到Exception前面，则编译器会报错

```java
public class Test {
    public static void main(String[] args) {
        int a = 1;
        int b = 0;
        try {//try监控区域
            System.out.println(a / b);
        } catch (Error e) {
            System.out.println("程序不能处理，变量B不能为0");
        }catch (Exception e){
            System.out.println("Exception");
        }catch (Throwable t){
            System.out.println("throwable");
        } finally {//处理善后工作
            System.out.println("finally");
            //finaly可以不要，假设IO，资源，关闭的时候，需要在这里处理
        }
    }
}
```

添加可以使用Ctrl+Alt+T，这样可以快捷添加Try...Catch...

主动抛出异常

public void test(int a ,int b) throws ArithmeticException{

if(b==0){ throw throws

​	throw new ArithmeticException()

}

}

### 自定义异常

使用Java内置的异常类可以描述在编程时出现的大部分异常情况，除此之外

，用户也可以自定义异常，用户自定义异常类，继承Exception类即可

示例代码

```java
package oop.exception;

//自定义异常类
public class MyException extends Exception {
    private int detail;

    @Override
    public String toString() {
        return super.toString();
    }

    public MyException(int a) {
        this.detail = a;
    }
}
```



自定义的异常用的很少，我们基本很少自定义自己的异常类

###  总结



处理运行时异常时，采用逻辑去合理的规避同时辅助try-catch处理

在多重catch块后面，可以加一个catch(Exception)来处理可能会被遗漏的异常

在于不确定的代码，也可以加上try-catch，处理潜在的异常

尽量去处理异常，切忌知识简单的调用printStackTrace()去打印输出

具体如何处理异常，要根据不同的业务需求和异常类型去决定

尽量添加finally语句块去释放占有的资源



## 8. 总结



