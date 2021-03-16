# JAVA网络编程

## 1.1 概述

地球村：你在西安，你朋友在美国都能听到。



什么叫做计算机网络？



网络编程的目的？

电台，上网干的事儿都是网络可以干的



想要达到这个效果需要做什么？

1、如何准确的定位网络上的一台主机，192.168.16.124：端口，定位到这个计算机上的某个资源

2、找到这个主机，如何传输数据呢？



javaweb  网页编程 b/s



网络编程，tcp:ip c/s









## 1.2  网络通信的一些要素



人工智能  智能汽车，工厂，人少？

如何实现网络通信，就需要实现通信双方的地址(ip、端口号)， 

规则： 网络通信的协议

TCP/IP参考模型,  OSI七层参考模型及TCP/IP模型

![image-20201101121834518](F:\MD格式学习笔记库\狂神JAVA-4-课堂随记.assets\image-20201101121834518.png)



网络编程中有两个主要的问题

- 如何准确的定位到网络上一台或者多台主机
- 找到主机之后如何进行通信

网络编程中的要素

- ip和端口号， IP类，端口类
- 网络通信写协议   udp类，tcp类

## java的网络编程

IP地址类:    Class InetAddress  唯一定位一台网络上的计算机 127.0.0.1 (localhost)

ip地址分类

	- ip地址分类法   ipv4, ipv6   127.0.0.1 由4个字节组成，每个字节长度为0-255，大概42亿个网址，2011年已经用尽， ipv6:fe80::1d81:a72c:e4ab:f9de%27  每个是128位，8个无符号整数来表示 240e:342:9b45:da00:6cba:afb1:6617:d590
	- 公网-私网分类法
	- - 192.168

域名

- 

## 端口

端口表示计算机上的一个进程的进程

不同进程有不同的端口号!端口号不能冲突! 用来区分软件电脑最多只能跑到0~65535个端口

TCP,UDP端口 ，每个都是65535  ,可以tcp用了80，但是udp继续使用80，这是没问题，单个协议下，端口号不可重复

端口分类：

​	共有端口  0~1023   例如Http 80  https 443  ftp 21 ssh 22 telent23 

​	程序注册的接口  1024-49151 ,分配给用户或者程序 tomcat 8080  mysql 33-6 oracle 1521

​	动态私有端口  49152~65535

> 查看端口的命令  netstat -ano # 查看所有的端口
>
> tasklist | findstr "8696" #查看指定端口哪个进程在使用



## 通信协议

协议  约定，就好比我们现在说的是普通话

网络通信协议，速率  传输码率，代码结构 传输控制

问题  非常的复杂？

重要两个协议  TCP(用户传输协议) UDP用户数据报协议

> TCP和UDP的对比
>
> - TCP连接稳定，UDP不连接 不稳定
> - TCP三次握手，四次分手 ，分为客户端和服务端，UDP没有明确界限，即可客户端和服务端，也可客户端和客户端
> - TCP传输完成，释放连接，效率低，UDP不管有没有准备好，都可以发给你....
> - DDOS攻击



> ->建立一个TCP连接，最少需要三次握手
>
> A : 你愁啥  B: 瞅你咋地  A: 干一场!
>
> ->TCP连接如果想断开，需要4次沟通:
>
> A: 我要断开了!
>
> B:我知道你要断开了(这时候没有断开)
>
> B:你真的断开了吗()
> A: 我真的要断开了!
>
> 



<img src="F:\MD格式学习笔记库\狂神JAVA-4-课堂随记.assets\image-20201101152450750.png" alt="image-20201101152450750" style="zoom:50%;" />



四次挥手

<img src="F:\MD格式学习笔记库\狂神JAVA-4-课堂随记.assets\image-20201101152525177.png" alt="image-20201101152525177" style="zoom:50%;" />



## 实现TCP聊天

> 在Java中，把不同类型的输入输出源抽象为流，其中输入和输出的数据称为数据流（Data Stream）。数据流是Java程序发送和接收数据的一个通道，数据流中包括输入流（Input Stream）和输出流（Output Stream）。通常应用程序中使用输入流读出数据，输出流写入数据。 流式输入、输出的特点是数据的获取和发送均沿数据序列顺序进行。相对于程序来说，输出流是往存储介质或数据通道写入数据，而输入流是从存储介质或数据通道中读取数据，一般来说关于流的特性有下面几点

创建一个TCPServer的思路，打开一个服务器，当客户端连接上来，把客户端输入的内容给打印出来。

​	step1. 使用ServerScoket 创建服务器socket；

​	step2.监听客户端的连接  Socket socket = serverSocket.accept()

​	step3. 获取输入流，

​		InputStream inputStream = socket.getInputStream()

​		ByteArrayOutputStream baos = new ByteArrayOutputStream ();

​		byte[] buffer = new byte[1024]

​		int len

​		while((len=inputStream(buffer)!=-1){

​			baos.write(buffer,0,len);

​		}

​		 System.out.println(baos.toString());

​	step4. 按照使用先后次序逆序关闭各流及通道



服务端示例代码

```java
//服务端
public class TCPServerDemo1 {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream inputStream = null;
        ByteArrayOutputStream baos = null;
        try {
            //1.建立一个服务器地址
            serverSocket = new ServerSocket(9999);
            //2.等待客户端链接过来
            while (true) {
                System.out.println("通道已经建立");
                socket = serverSocket.accept();
                //3.读取客户端的消息
                inputStream = socket.getInputStream();
                //4.管道流
                baos = new ByteArrayOutputStream();
                byte[] bytes = new byte[1024];
                int len;
                while ((len = inputStream.read(bytes)) != -1) {
                    baos.write(bytes, 0, len);
                }
                System.out.println(baos.toString());
            }


        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //5. 关闭资源流
            if (baos != null) {
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (inputStream != null) {
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (serverSocket != null) {
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }
}
```

客户端思路：

​	step1. 创建一个Socket , 指明服务器ip 和端口 Socket socket = new Socket("localhost", 9999)

​	step2.获取输出流

​	OutputStream ots = socket.getOutputStream()

​	step3. 发送消息

​	ots.write("你好，欢迎学习狂神说java".getBytes())

​	step4.使用完成后需要关闭流



客户端示例代码

```java
//客户端
public class TCPClientDemo1 {
    public static void main(String[] args) {

        Socket socket=null;
        OutputStream outputStream =null;
        //1.要知道服务器地址，端口号
        try {
            InetAddress serverIp = InetAddress.getByName("127.0.0.1");
            //2.端口号
            int port = 9999;
            //3.创建一个socket连接
            socket = new Socket(serverIp, port);
            //4.发送消息 IO流
            outputStream = socket.getOutputStream();
            outputStream.write("你好，欢迎学习狂神说JAVA".getBytes());

        } catch (UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(outputStream!=null){
                try {
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}
```

### 文件上传

![image-20201101161618214](F:\MD格式学习笔记库\狂神JAVA-4-课堂随记.assets\image-20201101161618214.png)



文件上传的思路

[客户端]

​	step1. 先创建socket

​	step2. 从socket获取输出流

​	step3. 读取文件

​	step4. 写出文件

​	step5. 确定服务器接收完毕，才能够断开连接 

​	step6. 关闭资源

[服务端]

​	step1. 先创建服务，指定服务端口9999

​	ServerSocket serverSocket = new ServerSocket(9999);

​	step2 .监听客户端的连接

​	Socket socket = serverSocket.accept();

​	step3. 从socket获取输入流

​	InputStream inputStream = socket.getInputStream();

​	

​	step4. 文件输出

```java
FileOutputStream fos = new FileOutputStream(new File("receive"));
 byte[] buffer = new byte[1024];
int len;
while ((len=inputStream.read(buffer))!=-1){
    fos.write(buffer,0,len);
}
```

​	step5 通知客户端接收完毕

​	

​	step6.关闭资源连接 

实例代码:

```java
//客户端代码
public class TCPClientDemo2 {
    public static void main(String[] args) throws Exception {
        //step 1. 创建一个socket，指明连接到地方和端口
        Socket socket = new Socket("localhost",9999);
        //step 2. 创建一个输出流
        OutputStream os = socket.getOutputStream();

        //step3 .硬盘读取文件
        FileInputStream fis = new FileInputStream(new File("touxiang.jpg"));
        //step4. 写出文件
        byte[] buffer = new byte[1024];
        int len;

        while ((len=fis.read(buffer))!=-1){
            //流程，先用FileInputStream的read从本地文件读取，每次读到buffer，并赋值给len，
            os.write(buffer,0,len);//读取结束后，从内存写入到socket
        }

        //通知服务器，我已经结束了
        socket.shutdownOutput(); //我已经传输完了
        
        //确定服务器接收完毕，才能够断开连接
        InputStream inputStream = socket.getInputStream();
        //String byte[]
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        byte[] buffer2 = new byte[1024];
        int len2;
        while ((len2=inputStream.read(buffer2))!=-1){
            byteArrayOutputStream.write(buffer,0,len2);
        }

        System.out.println(byteArrayOutputStream.toString());

        //step 5. 关闭资源
        byteArrayOutputStream.close();
        inputStream.close();
        fis.close();
        os.close();
        socket.close();

    }
}

//服务端代码
public class TCPServerDemo2 {
    public static void main(String[] args) throws Exception {
        //step 1.创建一个服务器socket，指定服务器监听端口为9999
        ServerSocket serverSocket = new ServerSocket(9999);
        //step2.监听客户端的连接请求
        Socket socket = serverSocket.accept();
        //从socket中获取输入流
        InputStream inputStream = socket.getInputStream();
        //转换流，把字节流转换为文件流
        FileOutputStream fos = new FileOutputStream(new File("receive.jpg"));
        //step4. 写文件
        byte[] buffer = new byte[1024];
        int len;
        while ((len=inputStream.read(buffer))!=-1){
            fos.write(buffer,0,len);
        }
        //通知客户端我接收完毕
        OutputStream outputStream = socket.getOutputStream();
        outputStream.write("我接收完毕，你可以断开了".getBytes());

        //step5 关闭输入流
        outputStream.close();
        fos.close();
        inputStream.close();
        socket.close();
        serverSocket.close();
    }
}
```

### Tomcat



服务端 ，自定义，tomcat服务器（未来重心JAVA后台开发用别人的服务器）

客户端, 自定义，浏览器B





### UDP

思路

- step1 建立一个socket    DatagramSocket
- step2 建个包 并打包   DatagramPacket
- step3 发送消息     send()
- Step4. 关闭流

服务端，开放端口

​	**step1.** 开放端口  

​	DatagramSocket socket = new DatagramSocket(9090)

​	**step2**. 接收数据， 需要等待客户端连接  

​	byte[] buffer = new byte[1024]

​	DatagramPacket packet = new DatagramPacket (buffer,0,buffer.length)

​	socket.receive(packet);  //阻塞接收

​	System.out.println(new String(packet.getData(),0,packet.getLength()));

​	**step3** 关闭流 

​	socket.close()

> 发送短信不需要服务器



发送端接收端的案例



### 聊天工具

聊天工具的思路

发送端，接收端，多线程,控制台

发送端   UDPSenderDemo1.java

​	step1 创建一个socket, DatagramSocket

​	step 1-1 准备数据  

​				  BufferReader reader = new BufferReader(new InputStreamReader(System.in)))

​					String data = reader.readLine()

​					byte[] dates = data.getBytes()

​	Step2 创建一个packet发送

​				DatagramPacket packet =new  DatagramPacket(datas, 0,datas.length,new InetSocketAddress("localhost",6666))

​				socket.send(packet)

​	Step3 . 关闭流 socket.close()

​	

接收端端   UDPReceoveDemo1.java

​	step1 创建一个socket, DatagramSocket

​	step 2 准备接收包裹数据  

​		  while(true){}

​			byte [] container = new byte[1024]

​			DatagramPacket  packet=new  DatagramPacket(container , 0,container .length)

​	step3 阻塞式接收

​			socket.receive(packet)

​	step4. 断开连接 bye

​		 byte []data = socket.getData();

​		 String receiveData = new String(data,0, data.length)

​	   System.out.println(receiveData)				 

​       if receiveData .equales("bye"){

​			   break;		

​       }

### 实现多人在线咨询

两个人可以是发送方，也可以都是接收方

实例代码

```java
public class TalkSend implements Runnable {
    DatagramSocket socket = null;
    BufferedReader reader = null;
    private int fromPort;
    private String toIP;
    private int toPort;


    public TalkSend( int fromPort, String toIP, int toPort) {
        this.toIP = toIP;
        this.toPort = toPort;
        this.fromPort = fromPort;
        try {
            socket = new DatagramSocket(fromPort);
            reader = new BufferedReader(new InputStreamReader(System.in));
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        while (true) {
            try {
                String data = reader.readLine();
                byte[] datas = data.getBytes();
                DatagramPacket packet = new DatagramPacket(datas, 0, datas.length, new InetSocketAddress(this.toIP, this.toPort));
                socket.send(packet);
                if (data.equals("bye")) {
                    break;
                }

            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        socket.close();
    }
}


、、
    public class TalkReceiver implements Runnable {
    DatagramSocket socket = null;
    private int port;
    private String msgFrom;

    public TalkReceiver(int port, String msgFrom) {
        this.port = port;
        this.msgFrom = msgFrom;
        try {
            socket = new DatagramSocket(port);
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        while (true) {
            //准备接收包裹
            try {
                byte[] container = new byte[1024];
                DatagramPacket packet = new DatagramPacket(container, 0,
                        container.length);
                socket.receive(packet);//阻塞式接收包裹

                //断开连接 Bye
                byte[] data = packet.getData();
                String receiverData = new String(data,0,data.length);
                System.out.println(msgFrom+":"+receiverData);

                if (receiverData.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        socket.close();
    }
}
//学生聊天

public class TalkStudent {
    public static void main(String[] args) {
        //开启两个线程
        new Thread(new TalkSend(7777,"localhost",9999)).start();
        new Thread(new TalkReceiver(8888,"老师")).start();
    }
}

//老师聊天
public class TalkTeacher {
    public static void main(String[] args) {
        //开启两个线程
        new Thread(new TalkSend(5555,"localhost",8888)).start();
        new Thread(new TalkReceiver(9999,"学生")).start();
    }
}
```



## JAVA IO流的介绍

作者：匿名用户
链接：https://www.zhihu.com/question/67535292/answer/1248887503
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



可以沿着这条路想一想：

1，学IO流之前，我们写的程序，都是在内存里自己跟自己玩。比如，你声明个变量，创建个数组，创建个集合，写一个排序算法，模拟一个链表，使用一些常用API，现在回想一下，是不是在只是自己在内存里玩一玩？计算机组成包括运算器，控制器，存储器，输入设备，输出设备。那么你前面的工作，仅仅够你的程序和内存以及CPU打打交道，如果你需要操作外部设备呢？比如键盘，显示器，再比如，最常见的外设：硬盘？甚至未来世界里的每家每户都有的机器人，“如何让你的程序和机器人进行交互呢？”

2，所以程序设计语言必须要提供程序与外部设备交互的方式，这就是IO框架的由来。我们需要和外部设备进行数据的交互。那么，计算机是通过什么和外部进行交互的呢？很简单就能想到：数据线。数据线里传播的是什么呢？一个词：比特流。比特就是bit的谐音，计算机中“位”的意思，代表0或1。1位或者1bit，就是一个0或一个1。但是，毕竟0或1不能表示什么，所以计算机更常见的基本单位是字节，也就是用8位0或1组成的一段数据。以上是对比特流的由来做一个简单地解释。（比特流一词来自于计算机网络原理中，对物理层传输内容的描述：物理层（网线）中传输的是“比特流”，在这里借用这个名词代指数据的表示形式，帮助理解）

上面两段话的意思，其实是为了下文做铺垫，帮助理解输入输出最重要的概念：方向性。输入还是输出，是相对于程序或者说相对于内存而言的。数据从外流到内存，就是输入（读），数据从内存出去，就是输出（写）。

3，既然计算机和外界进行信息的输入和输出交互，用的是比特流，那么很容易就能想到IO流名字的由来了。就是比喻输入输出的数据像流一样。我们可以这么认为，任何外部设备与内存之间输入输出的操作，都是需要输入输出流（IO流）来完成的，这里的IO流，指的就是比特流（或者称字节流）。这些外部设备，包括，键盘（标准输入设备），显示器（标准输出设备），音响，网络上另一台主机，甚至你玩游戏用的游戏手柄，以及各种各样的信号传感器，都可以叫做外部设备，和这些设备之间进行数据交互，显然不可能靠之前学习的那些数组，集合，常用类，String等等来完成。而是要靠和外界数据交换的类来完成。靠什么来进行数据交换，就是前面说的，比特流，或者说IO流类。

4，那么，既然要学习IO流，就得针对某一个输入输出设备来学习。哪种输入输出设备最重要同时也最常见？当然是硬盘。硬盘在这里的含义也可以理解为文件系统。（Java程序是运行在某操作系统平台上的应用软件JVM上的，实际上Java程序可见的并不是硬盘，而是操作系统提供的文件系统，因此此处可直接理解为文件系统）。因此，我们学习IO流的时候，基本上是学习的Java如何操作文件系统，除了文件系统，我们还能够了解Java操作标准输入输出设备，如[http://System.in](https://link.zhihu.com/?target=http%3A//System.in)和System.out。

5，知道了学习的方向，是要使用Java操作文件系统，那么首先要学习的就是文件的表示，即File类。然后，我们要操作做文件，虽然我们大部分操作都是操作文件系统，但是要明白IO流的概念不仅仅局限在操作文件上，前面我已经提到了，我们的编程语言是要能操作所有的输入输出，因此，API提供了两个顶层抽象类，用来表示操作所有的输出输出：InputStream，OutputStream。并且，这两个类表示字节的输入输出，因为输入输出的本质是字节流。这里注意体会一句话“字节流是最最基本的流”，这句话的由来就是因为计算机底层传递的就是字节。那么，当我们要操作文件的时候，就需要具体的对文件系统操作的IO实现类，于是我们需要学习FileInputStream和FileOutputStream，它们是文件输入输出字节流。这里之所以FileInputStream/OutputStream作为子类出现，按照面向对象思想理解就是，将来还有别的字节流来操作别的设备（比如将来需要通过操作网络设备获取网络数据，再比如需要操作机器人，那么或许就会再来个RobotInputStream和RobotOutputStream，这些新的需求也就都可以继承这个体系）

（这里顺便提一句架构设计思想，其中有一种设计原则叫“开闭原则”，其核心是：一个对象对扩展开放，对修改关闭。就是说，一旦写好了某个类，就不要去轻易改动他，而是要保证它一直能运行下去，而面对新的功能需求时，只要在原有代码上增加即可，而不是修改原有代码。要做到开闭原则，就需要分清需求中未来哪些部分是稳定的，哪些是很可能变化的，而往往抽象的部分是最稳定的，把稳定的内容分离出来，就能满足开闭原则。这就是为什么Java的类设计的如此之琐碎，为什么我们要从继承关系角度去理解JavaIO流的设计）

6，学了文件IO字节流之后，我们会发现原始的字节流对象用起来没那么高效，因为每个读或写请求都由底层操作系统处理，这些请求往往会触发磁盘访问、网络活动或其他一些相对昂贵的操作。不带缓冲区的流对象，只能一个字节一个字节的读，每次都调用底层的操作系统API，非常低效，而带缓冲区的流对象，可以一次读一个缓冲区，缓冲区空了才去调用一次底层API，这就能大大提高效率。所以又有了BufferedInputStream和BufferedOutputSteam，他们的用法是把字节流对象传入后再使用，也相当于把它俩套在了字节流的外面，给字节流装了个“外挂”，让基本字节流如虎添翼。

7，说到操作文件，就不得不提到文件的分类和编码格式。文件分为二进制文件和文本文件，二进制文件是用记事本打开后看不懂的，他们的编码格式是特殊的，比如pdf文件，exe文件。记事本打开后人能看懂的只有纯文本文件，我们处理文件（或者说处理任何的字节流），就免不了处理一些文本文件（或文本字节流）。如果是英语国家的人还好说，因为他们是用的常用字符用一张ASCII码表就能表示得出来，用一个字节就能表示一个字母。但是显然，对非英语国家的人来说，一个字节的大小无法表示他们所有的文字。因此，人们需要有能够处理字符的类，或者说这个类提供一个功能：就是把输入的字节转成字符，把要输出的字符转成计算机可以识别的字节。所以，你需要两个转换流：InputStreamReader和OutputStreamWriter。这两个类的作用分别是把字节流转成字符流，把字符流转成字节流。但是这两个流需要套在现成的字节流上才能使用，当中用到的设计模式也就是常说的装饰模式。当字节流被转成字符流之后，恭喜你，你可以不必操作字节流了，而是可以用人类的方式read和write各种“文字”。

（那么，我们为什么还要学习字节流？因为字节流依然有它的作用范围。首先，所有的流都是建立在字节流之上的，比如字符流。字节流或许可以读任何字节，但是他处理不了Unicode（万国码），他处理不了Data流，Object流，也就是说，它做不了高级的事情，只能读写最原始的东西。字节流好比动物，能看，能听，能汪汪叫，但是他不能读书，不能写字，不能理解更高级的知识。其次要注意的是，字符流只能用来处理文本文件，也就是只能来处理字符，如果出来用来处理二进制文件，会带来错误，所以处理二进制文件只能用字节流）

8，还是回到文件系统，我们最常见的是和文件系统打交道，那么针对如此常见的用途，读取文本文件能不能用一种方便的方式呢？当然，大牛们替你想到并提供了。FileReader和FileWriter这两个流对象可以直接把文件转成读取、写入流。让你省去了创建字节流，再套上转换流的步骤。看看这类名起的，实际上很形象，xxxReader和xxxWriter，明摆着告诉你“阅读和书写”都是“人可以做的”也就是他们表示的是字符流。同理上面的InputStreamReader和OutputStreamWriter，表示的是把字节流转成人可读的，把字节流转成人可写的。因此他们的顶层抽象类：Reader和Writer，表示的是所有人类可读可写的字符流统称。

10，同上面说的缓冲区的作用，再把Reader和Writer做成高效的，就需要BufferedReader和BufferedWriter，把它们套在Reader和Writer上，就能实现高效的字符流。

11，讲到这里，IO流的大概思想已经说的的差不多了，是不是觉得之前混乱的那些类，现在知道他们的作用和设计思想以后，稍稍清晰了许多呢？可以简单的记，字节流是基础，理论上可用于所有的输入输出场景，内容是文字的字节流可以通过转换流转成字符流，转换流是字节流和字符流之间相互转换的桥梁，把字节流转成字符流，离不开转换流，字符流是对于字符功能的增强可用来处理“文字”，操作文件系统应用范围最广，所以JDK提供了现成的FileXXX类，用来方便编程使用。另外，还有许多类是“在内存里自己和自己玩的”比如ByteArrayReader/Writer，PipedWriter/Reader，它们虽然也称为“流对象”但是他们的数据不出内存，所以它们的close()方法可有可无。以及其他带有某些功能的类，比如序列化流，比如数据输入输出流，等等。IO流对象的用法和作用大同小异，其使用环境和意义取决于具体需要，用到了再具体分析即可。

这里先写到这了。这里作为Java编程入门应该可以了，主要介绍了JavaIO框架的设计思想，但具体底层实现细节，还需要学习JVM相关知识，以及微机原理和接口技术等等底层的课程。





![preview](https://pic2.zhimg.com/v2-adf9568db6289098b6db2a469b44aa5d_r.jpg?source=1940ef5c)



## URL



url 统一资源定位符

DNS 域名解析  www.baidu.com xxx.xxx.xxx.xxxx

> 协议:// ip地址:  端口号  /项目名 /资源   URL一般由5部分组成，可以少，但是不能多



示例代码

```java
public class TestURLDown {
    public static void main(String[] args) throws Exception {
        //1.下载地址
        URL url = new URL("https://m10.music.126.net/20201101230310/7be60" +
                "f227e58c75e41c9b5095352121c/yyaac/obj/wonDkMOGw6XDiTHCmMOi/4110449541" +
                "/a8fb/f595/6168/15da44f1938939ee86835ecc4cfb4f5b.m4a");
        //2.连接到这个资源的HTTP
        HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();
        InputStream inputStream = urlConnection.getInputStream();
        FileOutputStream fos = new FileOutputStream("text.m4a");

        byte[] buffer = new byte[1024];
        int len;
        while ((len=inputStream.read(buffer))!=-1){
            fos.write(buffer,0,len); //写出这个数据
        }
        fos.close();
        inputStream.close();
        urlConnection.disconnect();//断开连接
    }
}
```