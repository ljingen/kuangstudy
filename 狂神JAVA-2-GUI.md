# GUI编程入门到游戏实战

- 这是什么
- 他怎么玩
- 该如何去在我们平时运用？
- 组件
- - 窗口
  - 弹窗
  - 面板
  - 文本框
  - 列表框
  - 按钮
  - 图片
  - 监听事件
  - 鼠标
  - 键盘事件
  - 破解工具

做完就可以用来做外挂

## 简介

Gui的核心技术： Swing  AWT  是GUI的核心编程工具，现在不流行，因为不美观，还需要我们的jre环境! 太大了!

为什么我们要学习，因为他是MVC的基础，可以写出自己心中想要的小工具，工作的时候也可能维护到swing界面，概率极小，主要是为了了解MVC架构，了解监听!





## AWT

### AWT介绍

1. abstract windows toolkit.  GUI ,包含了很多类和接口

2. 元素  窗口 按钮 文本框

组件(Component) ----按钮Button---TextArea 文本域---Label标签---.......

​                          ----容器Container----Windows---Frame()+Dialog（弹窗）

​															---面板Panel----applet

按钮等存放在容器内(add)

![image-20201024112758548](F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201024112758548.png)





### 2.1组件和容器

示例代码

```java
package top.aigoo.lesson1;

import java.awt.*;

//GUI的第一个界面
public class TestFrame {
    public static void main(String[] args) {
        //Frame jdk ,查源码
        Frame frame = new Frame("我的第一个java图形界面窗口!");
        //需要设置可见性，需要设置可见性，
        
        //方法类名直接通过. 不要看全部帮助文档.
        frame.setVisible(true);

        //设置窗口大小
        frame.setSize(400,400);

        //设备背景颜色 Color
        frame.setBackground(new Color(239, 202, 81));

        //弹出的初始位置
        frame.setLocation(200,200);

        //设置大小不可改变
        frame.setResizable(false);
    }
}

```

操作思想， 直接读源码，直接.找方法

执行的代码结果

![image-20201024114635436](F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201024114635436.png)



发现一个问题：发现窗口关闭不掉，停止java运行就可以关闭掉窗口

回顾一下封装的代码

```java
public class TestFrame2 {
    public static void main(String[] args) {
        MyFrame myFrame1 = new MyFrame(100,100,200,200,Color.red);
        MyFrame myFrame2 = new MyFrame(300,100,200,200,Color.yellow);
        MyFrame myFrame3 = new MyFrame(100,300,200,200,Color.blue);
        MyFrame myFrame4 = new MyFrame(300,300,200,200,Color.black);
    }
}


class MyFrame extends Frame{
    static int id =0; //可能存在多个窗口，我们需要一个计数器

    public MyFrame(int x, int y, int width, int height, Color color){
        super("Myframe:"+(++id));
        setBounds(x,y,width,height);
        setBackground(color);
        setVisible(true);
    }
}
```



### 2.2 面板Panel

Panel不能单独存在，需要内嵌到容器上

面板还需要布局

23种设计模式；

解决了窗口的关闭事件



```java
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

public class TestPanel {
    public static void main(String[] args) {
        Frame frame = new Frame();
        //布局的概念
        Panel panel = new Panel();
        //设置布局
        frame.setLayout(null);
        //左边
        frame.setBounds(300,300,500,500);
        //颜色
        frame.setBackground(new Color(40,161,35));
        //panel设置坐标，是相对于frame
        panel.setBounds(50,50,400,400);
        panel.setBackground(new Color(191,15,60));

        frame.add(panel);
        frame.setVisible(true);
        //监听事件，监听窗口关闭事件 System.exit(0)

        frame.addWindowListener(new WindowAdapter() {
            //点击窗口关闭的时候要做的事情
            @Override
            public void windowClosing(WindowEvent e) {
                //窗口关闭
                System.exit(0);
            }
        });
    }
}
```

![image-20201024191044280](F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201024191044280.png)

### 2.3 AWT布局

- 流式布局 FlowLayout 上下或者左右，
- 东西南北中 BorderLayout
- 表格布局 GridLayout

先看看流式布局，示例代码如下

```java
package top.aigoo.lesson3;

import java.awt.*;

public class TestFlowLayout {
    public static void main(String[] args) {
        Frame frame = new Frame();
        Button button1 = new Button("button1");
        Button button2 = new Button("button2");
        Button button3 = new Button("button3");

        //设置为流式布局
        //frame.setLayout(new FlowLayout())
        frame.setLayout(new FlowLayout(FlowLayout.LEFT));

        frame.setSize(200,200);
        frame.setVisible(true);

        frame.add(button1);
        frame.add(button2);
        frame.add(button3);
    }
}

```

东西南北中定位布局

```java
public class TestBorderLayout {
    public static void main(String[] args) {
        Frame frame = new Frame("BorderLayout");

        Button east = new Button("East");
        Button west = new Button("West");
        Button south = new Button("South");
        Button north = new Button("North");
        Button center = new Button("Center");


        frame.add(east,BorderLayout.EAST);
        frame.add(west,BorderLayout.WEST);
        frame.add(south,BorderLayout.SOUTH);
        frame.add(north,BorderLayout.NORTH);
        frame.add(center,BorderLayout.CENTER);

        frame.setVisible(true);
        frame.setSize(400,400);
    }
}
```

表格布局Grid

```java
public class TestGridLayout {
        public static void main(String[] args) {
            Frame frame = new Frame("BorderLayout");

            Button east = new Button("East");
            Button west = new Button("West");
            Button south = new Button("South");
            Button north = new Button("North");
            Button center = new Button("Center");

            frame.setLayout(new GridLayout(2,3));
            frame.add(east);
            frame.add(west);
            frame.add(south);
            frame.add(north);
            frame.add(center);
            frame.pack();
            frame.setVisible(true);
            frame.setSize(400,400);
        }

}
```

上述的布局都是自动布局，其他布局都是由这些构成



练习题，生成如下图的图形

![image-20201026115549280](F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201026115549280.png)





```java
public class Exercise1 {
    public static void main(String[] args) {
        Frame frame = new Frame("excrsise");

        frame.setLayout(new GridLayout(2,1));
        frame.setSize(400,400);
        frame.setVisible(true);
        frame.setBackground(Color.MAGENTA);


        Panel panel1 = new Panel(new BorderLayout());
        Panel panel2 = new Panel(new GridLayout(2,1));

        panel1.add(new Button("pa1-btn-1"), BorderLayout.EAST);
        panel1.add(new Button("pa1-btn-2"), BorderLayout.WEST);
        panel2.add(new Button("pa2-btn-1"));
        panel2.add(new Button("pa2-btn-2"));
        panel1.add(panel2, BorderLayout.CENTER);

        frame.add(panel1);


        Panel panel3 = new Panel(new BorderLayout());
        Panel panel4 = new Panel(new GridLayout(2,2));

        panel3.add(new Button("pa3-btn-east"), BorderLayout.EAST);
        panel3.add(new Button("pa3-btn-west"), BorderLayout.WEST);

        for (int i = 1; i <= 4; i++) {
            panel4.add(new Button("pa4-btn-"+i));
        }
        panel3.add(panel4,BorderLayout.CENTER);

        frame.add(panel3);


    }
}
```

小结：

- Frame 是一个顶级窗口，
- panel是无法单独显示，必须添加到某个容器中
- 布局管理器有FlowLayout, BorderLayout,GridLayout
- 含有大小、定位、背景颜色、可见性、监听

### 2.4 事件监听



事件监听，当某个事情发生的时候，该干什么？ 这叫做事件监听。

```java
public class ActionEvent2 {
    public static void main(String[] args) {
        Frame frame = new Frame("两个按钮一个监听");

        Button button1 = new Button("btn-start");
        Button button2 = new Button("btn-stop");
        button2.setActionCommand("stop the button");

        MoniterListener listener = new MoniterListener();
        button1.addActionListener(listener);
        button2.addActionListener(listener);

        frame.add(button1, BorderLayout.NORTH);
        frame.add(button2, BorderLayout.SOUTH);
        frame.pack();
        frame.setVisible(true);
    }
}

class MoniterListener implements ActionListener{

    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("按钮被点击了=>"+e.getActionCommand());
    }
}
```



### 2.5 输入框



```java
package top.aigoo.lesson4;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TestText1 {
    public static void main(String[] args) {
        new MyFrame();
    }

}

class MyFrame extends Frame {
    public MyFrame() throws HeadlessException {
        TextField textField = new TextField();
        add(textField);
        //监听这个文本输入框的输入文字
        MyActionListener2 listener2 = new MyActionListener2();

        textField.addActionListener(listener2);

        pack();
        setVisible(true);
    }
}

class MyActionListener2 implements ActionListener{
    @Override
    public void actionPerformed(ActionEvent e) {
        TextField field = (TextField) e.getSource();//向下强制类型转换
        System.out.println(field.getText());//获取文本框输入

    }
}
```





### 2.6 简易计算器，组合+内部类回顾复习



OOP原则：组合是大于继承，优先使用组合

```java
public class TestCalc {
    public static void main(String[] args) {
        new Calcalator1();
    }
}

//计算器类
class Calcalator1 extends Frame{
    public Calcalator1(){
    //三个文本框
        TextField textField1 = new TextField(10); //字符数
        TextField textField2 = new TextField(10); //字符数
        TextField textField3 = new TextField(20); //字符数
        //1个按钮
        Button button = new Button("=");
        //添加按钮的监听
        button.addActionListener(new MyActionListener3(textField1,textField2,textField3));
        //1个标签
        Label label = new Label("+");

        setLayout(new FlowLayout());

        add(textField1);
        add(label);
        add(textField2);
        add(button);
        add(textField3);

        pack();
        setVisible(true);

    }
}
//监听器类
class MyActionListener3 implements ActionListener{
    private TextField num1,num2,num3;
    //获取三个变量
    public MyActionListener3(TextField num1,TextField num2,TextField num3) {
        this.num1=num1;
        this.num2=num2;
        this.num3=num3;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        //获取加数和被加数
        int n1= Integer.parseInt(num1.getText());
        int n2= Integer.parseInt(num2.getText());
        //将这个值+法运算后放到第三个框
        num3.setText(""+(n1+n2));
        //清除前两个框
        num1.setText("");
        num2.setText("");

    }
}
```





使用组合方式实现计算器

```java
public class TestCalc2 {
    public static void main(String[] args) {
        new Calculator2().loadFrame();
    }
}

class Calculator2 extends Frame{
    //属性
    TextField num1,num2,num3;

    //方法
    public void loadFrame(){
        num1 = new TextField(10); //字符数
        num2 = new TextField(10); //字符数
        num3 = new TextField(20); //字符数
        Button button = new Button("=");
        Label label = new Label("+");

        //添加按钮的监听
        button.addActionListener(new MyActionListener4(this));

        setLayout(new FlowLayout());

        add(num1);
        add(label);
        add(num2);
        add(button);
        add(num3);
        pack();
        setVisible(true);
    }
}


class MyActionListener4 implements ActionListener{
    public Calculator2 calculator;

    public MyActionListener4(Calculator2 calculator) {
        this.calculator = calculator;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        int n1= Integer.parseInt(calculator.num1.getText());
        int n2= Integer.parseInt(calculator.num2.getText());
        //2.将这个值+法运算后放到第三个框
        calculator.num3.setText(""+(n1+n2));
        //3. 清除前两个框
        calculator.num1.setText("");
        calculator.num2.setText("");
    }
}
```

我们直接优化为使用内部类方式

```java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TestCalc4 {
    public static void main(String[] args) {
        new Calculator4().loadFrame();
    }
}

class Calculator4 extends Frame{
    //属性
    private TextField num1,num2,num3;

    public void loadFrame(){
        num1 = new TextField("");
        num2 = new TextField("");
        num3 = new TextField("");
        Label label = new Label("=");
        Button button = new Button("=");
        button.addActionListener(new MyActionListener4());

        setLayout(new FlowLayout());
        add(num1);
        add(label);
        add(num2);
        add(button);
        add(num3);

        pack();
        setVisible(true);
    }

    private class MyActionListener4 implements ActionListener{

        @Override
        public void actionPerformed(ActionEvent e) {
            int n =Integer.parseInt(num1.getText())+Integer.parseInt(num2.getText());
            num3.setText(""+n);
            num1.setText("");
            num2.setText("");
        }
    }
}
```





### 2.7 画笔

```java
public class TestPaint {
    public static void main(String[] args) {
        new MyPaint().loadFrame();
    }
}

class MyPaint extends Frame{
    public void loadFrame(){
        setTitle("画板");
        setVisible(true);
        setBounds(200,200,600,500);
    }
    @Override
    public void paint(Graphics g) {
        g.setColor(Color.red);
        for (int i = 100; i <599 ; i++) {
            for (int j = 100; j <499 ; j++) {
                g.fillOval(i,j,100,100);
            }
            repaint();
        }

        g.setColor(Color.BLACK);
    }
}
```



### 2.8 鼠标监听

目的，想要实现鼠标去画画！

```java
public class TestMouseListener {
    public static void main(String[] args) {
        new MouseFrame("画图");
    }
}

class MouseFrame extends Frame{
    //画画需要画笔
    //需要鉴定鼠标当前位置
    //需要集合存储这个点
    private ArrayList points;

    public MouseFrame(String title) {
        super(title);
        setBounds(200,200,400,300);
        //鼠标监听器是在窗口
        //存鼠标的点
        this.points= new ArrayList();
        this.addMouseListener(new MyMouseListener());
        setVisible(true);
    }

    @Override
    public void paint(Graphics g) {
        super.paint(g);
        //画画监听鼠标事件
        Iterator iterator = points.iterator();

        while (iterator.hasNext()){
            Point point = (Point) iterator.next();
            g.setColor(Color.BLUE);
            g.fillOval(point.x, point.y,10,10);
        }

    }
    //添加一个点到我们的界面上
    public void addPoint(Point point){
        this.points.add(point);
    }

    //适配器模式
    private class MyMouseListener extends MouseAdapter {
        @Override
        public void mousePressed(MouseEvent e) {
            MouseFrame frame = (MouseFrame) e.getSource(); //由高到低 object-->Frame 强制转换
            //这里点击时候，就会在界面产生一个点!
            //这个点就是鼠标的点
            frame.addPoint( new Point(e.getX(),e.getY()));
            //每次点击鼠标都需要重新画一次
            frame.repaint();//点击鼠标，刷新页面
        }
    }
}
```



原理

![image-20201026204656027](F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201026204656027.png)





### 2.9 窗口监听

示例代码

```java
public class TestWindow {
    public static void main(String[] args) {
        new TestWindowListener();
    }
}

class TestWindowListener extends Frame {
    public TestWindowListener()  {
        setBackground(Color.blue);
        setBounds(100,100,200,200);
        setVisible(true);
        //addWindowListener(new MyWindorListener());
        //使用匿名内部类
        this.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                //关闭窗口
                System.out.println("windowClosing");
                System.exit(0);
            }

            @Override
            public void windowActivated(WindowEvent e) {
                //激活窗口
                System.out.println("windowActivated");
            }
        });
    }

//    class MyWindorListener extends WindowAdapter{
//        @Override
//        public void windowClosing(WindowEvent e) {
//            setVisible(false); //隐藏窗口，通过按钮隐藏窗口
//            System.exit(0);//正常退出
//        }
//    }
}
```



### 2.10 键盘监听



```java
public class TestKeyListener {
    public static void main(String[] args) {
        new MyKeyFrame();
    }
}

class MyKeyFrame extends Frame{
    public MyKeyFrame() {
        setBounds(100,100,600,500);
        setVisible(true);
        setBackground(Color.pink);
        this.addKeyListener(new KeyAdapter() {
            @Override
            public void keyPressed(KeyEvent e) {
                int keyCode = e.getKeyCode();
                System.out.println(keyCode);
                if(keyCode==KeyEvent.VK_UP){
                    System.out.println("按下了向上按键");
                }
            }
        });
        this.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}
```





## Swing

### 3.1 窗口JFrame，面板

标签居中

```java
public class JFrameDemo {
    //init() 初始化
    public void init(){
        JFrame frame = new JFrame("这是JFrame窗口");
        frame.setVisible(true);
        frame.setBounds(100,100,200,200);
        frame.setBackground(Color.cyan);
        //设置文字
        JLabel label = new JLabel("欢迎来到狂神说JAVA系列节目");
        frame.add(label);
        //容器实例化
        frame.getContentPane();
        //关闭事件
        frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

    }
    public static void main(String[] args) {
        //建立一个窗口
        new JFrameDemo().init();
    }
}


public class JFrameDemo2 {
    public static void main(String[] args) {
        new MyJFrame2().init();
    }
}

class MyJFrame2 extends JFrame{
    public void init(){
        this.setVisible(true);
        this.setBounds(100,100,200,200);

        JLabel label = new JLabel("欢迎来到狂神说JAVA系列节目");
        label.setHorizontalAlignment(SwingConstants.CENTER);
        this.add(label);


        //获得一个容器,JFrame需要依靠容器进行渲染
        Container container=this.getContentPane();
        container.setBackground(Color.BLUE);
    }
}

```



### 3.2 弹窗 JDialog



```java
public class DialogDemo extends JFrame {
    public DialogDemo(){
        this.setVisible(true);
        this.setSize(700,500);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

        //JFrame 放东西，需要容器
        Container contentPane = this.getContentPane();
        //绝对布局
        contentPane.setLayout(null);
        //按钮
        JButton button = new JButton("点击弹出一个对话框!");//创建一个按钮
        button.setBounds(30,30,200,50);

        //点击按钮的时候，弹出一个弹出层
        button.addActionListener(new ActionListener() {//监听器
            @Override
            public void actionPerformed(ActionEvent e) {
                //弹出弹窗
                new MyDialogDemo();
            }
        });
        contentPane.add(button);

    }
    public static void main(String[] args) {
        new DialogDemo();
    }
}
//弹窗的窗口
class MyDialogDemo extends JDialog{
    public MyDialogDemo() {
        this.setVisible(true);
        this.setBounds(100,100,500,500);
        //this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        Container contentPane = this.getContentPane();

        contentPane.add(new Label("秦老师带你学JAVA"));

    }
}
```

### 3.3 标签JLabel

label

``` java
new Label("xxx");
new JLabel("xxx");
```

图标Icon  添加一个标签有图片的内容，示例代码

```java
package top.aigoo.lesson6;

import javax.swing.*;
import java.awt.*;
import java.net.URL;

public class ImageIconDemo extends JFrame {
    public ImageIconDemo() {
        JLabel jLabel = new JLabel("Imageicon");

        URL url =ImageIconDemo.class.getResource("icon.png");

        ImageIcon imageIcon = new ImageIcon(url);

        jLabel.setIcon(imageIcon);
        jLabel.setHorizontalAlignment(SwingConstants.CENTER);

        Container contentPane = getContentPane();
        contentPane.add(jLabel);

        setVisible(true);
        setBounds(100,100,300,300);
        setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new ImageIconDemo();
    }
}
```



### 3.4 面板 JPanel

普通面板

```java
public class JPanelDemo extends JFrame {
    public JPanelDemo(){ //构造器
        Container contentPane = this.getContentPane();
        //后面参数意思是间距
        contentPane.setLayout(new GridLayout(2,1,10,10));

        JPanel jPanel1 = new JPanel(new GridLayout(1, 3));
        JPanel jPanel2 = new JPanel(new GridLayout(1, 3));
        JPanel jPanel3 = new JPanel(new GridLayout(1, 3));

        jPanel1.add(new JButton("1"));
        jPanel1.add(new JButton("1"));
        jPanel1.add(new JButton("1"));

        jPanel2.add(new JButton("1"));
        jPanel2.add(new JButton("1"));
        jPanel2.add(new JButton("1"));

        jPanel3.add(new JButton("1"));
        jPanel3.add(new JButton("1"));
        jPanel3.add(new JButton("1"));

        contentPane.add(jPanel1);
        contentPane.add(jPanel2);
        contentPane.add(jPanel3);

        this.setVisible(true);
        this.setSize(500,500);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);


    }

    public static void main(String[] args) {
        new JPanelDemo();
    }
}
```

带有上下滚动的面板  JScorllPanel

```java
public class JScrollDemo extends JFrame {
    public JScrollDemo() {
        Container contentPane = this.getContentPane();
        //文本域
        JTextArea jTextArea = new JTextArea(20, 50);
        jTextArea.setText("这是设置文本框默认文本!");

        JScrollPane jScrollPane = new JScrollPane(jTextArea);

        contentPane.add(jScrollPane);

        this.setVisible(true);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        this.setBounds(100,100,300,150);
    }

    public static void main(String[] args) {
        new JScrollDemo();
    }
}
```





### 3.5 按钮JButton, JRadioButton,JCheckBox

- 图片按钮 JButton

```java
public class JButtonDemo1 extends JFrame {

    public JButtonDemo1(){
        Container contentPane = this.getContentPane();
        URL url = JButtonDemo1.class.getResource("icon.png");
        ImageIcon imageIcon = new ImageIcon(url);

        //把图标放在按钮上面

        JButton button = new JButton();
        button.setIcon(imageIcon);
        button.setText("测试");
        button.setToolTipText("这是图标按钮");

        contentPane.add(button);
        this.setVisible(true);
        this.setSize(500,300);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JButtonDemo1();
    }
}
```

- 单选按钮 JRadioButton

```java
public class JButtonDemo2 extends JFrame {
    public JButtonDemo2(){
        Container contentPane = this.getContentPane();
        //将一个图片变为图标
        URL url = JButtonDemo2.class.getResource("icon.png");
        Icon icon = new ImageIcon(url);

        //单选按钮
        JRadioButton jRadioButton1 = new JRadioButton("JRadioButton1");
        JRadioButton jRadioButton2 = new JRadioButton("JRadioButton2");
        JRadioButton jRadioButton3 = new JRadioButton("JRadioButton3");
        //由于单选框只能选择一个，分组进行,一个组只能选择一个

        ButtonGroup buttonGroup = new ButtonGroup();
        buttonGroup.add(jRadioButton1);
        buttonGroup.add(jRadioButton2);
        buttonGroup.add(jRadioButton3);

        //添加到容器里面
        contentPane.add(jRadioButton1, BorderLayout.CENTER);
        contentPane.add(jRadioButton2, BorderLayout.SOUTH);
        contentPane.add(jRadioButton3, BorderLayout.NORTH);

        this.setVisible(true);
        this.setBounds(100,100,500,400);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

    }
    public static void main(String[] args) {
        new JButtonDemo2();
    }
}
```



- 复选按钮 JCheckBox

```java
public class JButtonDeme3 extends JFrame {
    public JButtonDeme3(){
        Container contentPane = this.getContentPane();
        //将一个图片变为图标
        URL url = JButtonDemo2.class.getResource("icon.png");
        Icon icon = new ImageIcon(url);

        //多选框
        JCheckBox jCheckBox1 = new JCheckBox("忘记密码");
        JCheckBox jCheckBox2 = new JCheckBox("找回密码");

        //添加到容器里面
        contentPane.add(jCheckBox1, BorderLayout.NORTH);
        contentPane.add(jCheckBox2, BorderLayout.SOUTH);


        this.setVisible(true);
        this.setBounds(100,100,500,400);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

    }
    public static void main(String[] args) {
        new JButtonDeme3();
    }
}
```

### 3.6 列表JList+JCompose

- 下拉框

  ```java
  public class JComposeDemo extends JFrame {
      public JComposeDemo() {
          Container container = this.getContentPane();
  
          JComboBox objectJComboBox = new JComboBox();
  
          objectJComboBox.addItem("最新电影");
          objectJComboBox.addItem("热映中");
          objectJComboBox.addItem("正在上映");
          objectJComboBox.addItem("已下架");
  
          container.add(objectJComboBox);
          
          this.setVisible(true);
          this.setBounds(100,100,500,400);
          this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
      }
  
      public static void main(String[] args) {
          new JComposeDemo();
      }
  }
  ```

  

- 列表框

```java
public class JListDemo extends JFrame {
    public JListDemo()  {
        Container container = this.getContentPane();
        String[] strings = new String[20];
        String[] contents = {"1","2","3"};
        JList jList = new JList(contents);
        int[] ints = new int[100];
        container.add(jList);

        this.setVisible(true);
        this.setBounds(100,100,500,400);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JListDemo();
    }
}
```

应用场景：

- 下拉框 ：选择地区，或者一些单个选项，一般超过2个可以用下拉框
- 列表：展示一些信息，一般动态扩容的





### 3.7 文本框JTestField+passwordField+JTextArea

- 文本框

```java
public class JTextDemo extends JFrame {
    /*文本框，密码框，文本域*/

    public JTextDemo() {
        Container container = this.getContentPane();

        JTextField textField = new JTextField("demo");
        JTextField textField2= new JTextField("world",20);

        container.add(textField, BorderLayout.NORTH);
        container.add(textField2, BorderLayout.SOUTH);

        //密码输入框


        this.setVisible(true);
        this.setBounds(100,100,500,400);
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        new JTextDemo();
    }
}
```



- 密码框

- ```java
  import java.awt.*;
  
  public class JPasswordDemo extends JFrame {
      public JPasswordDemo() {
          Container container = this.getContentPane();
          
          //密码输入框
          JPasswordField passwordField = new JPasswordField();
          passwordField.setEchoChar('*');
          
          container.add(passwordField);
          
          this.setVisible(true);
          this.setBounds(100,100,500,400);
          this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
      }
  
      public static void main(String[] args) {
          new JPasswordDemo();
      }
  }
  ```

  

- 文本域





## 贪吃蛇

> 帧  如果时间片足够小，就是动画，一秒30帧，人眼就已经看得是动画了。连起来就是动画，拆开就是静态图片
>
> 定时器

### 游戏截图如下：

![image-20201028102206654](F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201028102206654.png)



###  游戏流程:

​	1,画窗口class StartGame{}) ---画面板(class GamePanel{})-----添加静态数据(class Data{})->游戏类-->逻辑类



界面JFrame

大小，不可调整大小，默认关闭监听，可见

GamePanel extends JPanel{

​	//游戏所有东西都是用paintComponet  

​	// 

}

public class  Data{}  数据中心  

相对路径  tx.png ，是相对当前java

绝对路径  / 相对于当前的项目

### 1. 设计游戏主启动类

先设计class StartGame{}  实现定义一个窗口，窗口里面需要设置 窗口大小，窗口的关闭监听，窗口不可设置大小，窗口可见

```java
//游戏主启动类, 创建一个游戏主窗口
public class StartGame {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame();
        jFrame.setTitle("狂神说JVA-贪吃蛇小游戏");
        jFrame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        jFrame.setResizable(false);//窗口大小不可变       
        jFrame.setVisible(true);
        jFrame.setBounds(10,10,900,720);
        //正常游戏界面都应该在面板上
    }
}
```

### 2.设计游戏面板

我们设计的游戏，所有游戏元素都放在游戏面板上，这个面板和窗口一样的大小

```java
// 首先在当前包创建一个新的类GamePanel，并继承自JPanel
public class GamePanel extends JPanel{
    public void init(){ 
    }
    public GamePanel() {
    }
}
// 进入到StartGame{}类，添加我们的游戏面板
public class StartGame {
    public static void main(String[] args) {
		.......
        jFrame.add(new GamePanel());
        ......
    }
}
```

### 3.绘制我们游戏的静态画面

将游戏元素的页头和游戏区域 放到面板中 class GamePanel extends JPanel{} ,最终目的是生成一个头部带有广告图的游戏窗口

```java
// 第一步 先重写paintComponent(Graphics g) 
public class GamePanel extends JPanel {
    public void init(){
    }
    public GamePanel() {
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        setBackground(Color.WHITE);
        g.fillRect(25, 75, 850, 600);//默认的游戏界面  x轴23，Y轴75
    }
}
//第二步， 为了实现绘制面板，我们建立一个数据类，并绘制图标，在这里，需要记住相对路径 “tx.png”，是在当前java文件的目录，绝对路径 /tx.png，相当于当前项目的目录
//下面这个地址的目录为/基础语法/AWT/statics
public class Data {
    //相对路径  "tx.png" 在当前java文件的目录
    //绝对路径  /tx.png ，相对于当前项目的目录
    public static URL headerURL = Data.class.getResource("/statics/header.png");
    public static URL bodyURL = Data.class.getResource("/statics/body.png");
    public static URL downURL = Data.class.getResource("/statics/down.png");
    public static URL foodURL = Data.class.getResource("/statics/food.png");
    public static URL leftURL = Data.class.getResource("/statics/left.png");
    public static URL rightURL = Data.class.getResource("/statics/right.png");
    public static URL upURL = Data.class.getResource("/statics/up.png");

    public static ImageIcon header = new ImageIcon(headerURL);
    public static ImageIcon body = new ImageIcon(bodyURL);
    public static ImageIcon down = new ImageIcon(downURL);
    public static ImageIcon food = new ImageIcon(foodURL);
    public static ImageIcon left = new ImageIcon(leftURL);
    public static ImageIcon right = new ImageIcon(rightURL);
    public static ImageIcon up = new ImageIcon(upURL);

}
//第三步 将头部的广告条绘制到游戏面板paintComponent(Graphics g)
protected void paintComponent(Graphics g) {		
         ...
        //绘制头部的广告画
        Data.header.paintIcon(this,g,25,11);
    }
}

```



<img src="F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201028132624093.png" alt="image-20201028132624093" style="zoom:50%;" />

### 4. 绘制静态的Snake

将小蛇绘制到面板,通过观察，可以知道需要如下信息

​	首先小蛇要有长度length, 

​	然后有蛇头，蛇身体，蛇头有上下左右四个方向，蛇身体有两节，

​	每个蛇有一个xy坐标控制位置，蛇头初始化的x,y应该是分别100,100，每个图片25*25，所以左面第一节蛇身体坐标是 [75,100]，第二节蛇身体坐标为[50,100]

​	后续移动，我们需要把蛇坐标存储起来，需要一个int数组

```java
//第一步，定义我们需要的类属性  GamePanel
int length;//小蛇长度
int[] snakeX = new int[600];//蛇的X坐标 25*25
int[] snakeY = new int[600];//蛇的Y坐标 25*25

//第二步，在GamePanel中，定义init(),并把上述属性初始化
public void init(){
    length=3;
    snakeX[0] = 100;snakeY[0] = 100; //脑袋坐标
    snakeX[1] = 75;snakeY[1] = 100; //第一个身体坐标
    snakeX[2] = 50;snakeY[2] = 100;  //第二个身体坐标
}

//第三步 ,在paintComponent(Graphics g)绘制蛇头 社身体
    protected void paintComponent(Graphics g) {
		....
        //绘制小蛇
        Data.right.paintIcon(this, g, snakeX[0], snakeY[0]);
        for (int i = 1; i <length; i++) {
            Data.body.paintIcon(this,g,snakeX[i],snakeY[i]);
        }
    }
//第四步 一定记住，在自己的构造体里面调用初始化方法 
//构造体    
public GamePanel() {
        init();
}
```

### 5 实现蛇头方向静态的控制

蛇头是跟随键盘上下左右键，会进行更换的，当用户按下上，则向上移动，用户按下下，则向下移动

默认在页面显示 "请按下空格开始", 这个也需要监听键盘，当按下游戏开始动

```java
//第一步 定义类属性，
 boolean isStart; //是否开始
//第二步  在init()初始化
    public void init(){
		.....
        isStart = false; //默认游戏没有开始
    }

//第三步 在paintComponent(Graphics g)画上这个属性
if (isStart==false){
    g.setColor(Color.WHITE);
    g.setFont(new Font("微软雅黑", Font.BOLD,40));//设置字体
    g.drawString("按下空格开始游戏",300,300);
}

//第四步，为了实现切换是否开始，我们还需要实现KeyListener
    public void keyPressed(KeyEvent e) {
        int keyCode = e.getKeyCode();

        if (keyCode==KeyEvent.VK_SPACE){
            //用户按空格键
            isStart = !isStart; //取反，空格开始，空格结束
            repaint(); //把前面的paintComponent()执行一次
        }

        if(keyCode==KeyEvent.VK_R||keyCode==KeyEvent.VK_D){

            direction= "R";
        }else if (keyCode==KeyEvent.VK_L||keyCode==KeyEvent.VK_A){
            direction= "L";
        }else if (keyCode==KeyEvent.VK_U||keyCode==KeyEvent.VK_W){
            direction= "U";
        }else if (keyCode==KeyEvent.VK_D||keyCode==KeyEvent.VK_S){
            direction= "D";
        }
    }

//第五步 在paintComponent(Graphics g)画上这个属性
    if (direction.equals("R")){
        Data.right.paintIcon(this, g, snakeX[0], snakeY[0]);
    }else if (direction.equals("L")){
        Data.left.paintIcon(this, g, snakeX[0], snakeY[0]);
    }else if (direction.equals("U")){
        Data.up.paintIcon(this, g, snakeX[0], snakeY[0]);
    }else if (direction.equals("D")){
        Data.down.paintIcon(this, g, snakeX[0], snakeY[0]);
    }
这样我们就可以实现切换，目前还没有重新绘制画板，所以没法实现蛇头变换
```



### 6. 实现小蛇的移动

实现小蛇移动，首先需要有一个定时器，这样每一定时间产生一个定时器事件，

然后需要实现ActionListener这个接口，处理方法中，当事件发生，如果当前是向右，蛇头的x +25, Y不变，身体依次向前移动一个坐标

就是第一控制蛇头方向，第二我们每次移动加25像素，第三我们身体跟随蛇头进行移动

```java
//首先 在类属性中，添加计时器
    Timer timer = new Timer(100,this); //监听当前对象，100ms执行一次
//然后进入到初始化方法中，启动定时器
    public void init(){
		....
        timer.start();
    }

//实现ActionListener，完成actionPerformed方法，小蛇移动相关事务在这个方法中进行处理
    public void actionPerformed(ActionEvent e) {
        if (isStart){
            //移动蛇身体
            for (int i = length-1; i >0 ; i--) {
                snakeX[i] = snakeX[i-1];
                snakeY[i] = snakeY[i-1];
            }
            //判断方向，移动蛇头
            if (direction.equals("R")){
                snakeX[0] +=25;
                if (snakeX[0]>850){ snakeX[0]=25; }
            }else if (direction.equals("L")){
                snakeX[0] -=25;
                if (snakeX[0]<25){ snakeX[0]=850; }
            }else if (direction.equals("U")){
                snakeY[0] -=25;
                if (snakeY[0]<75){ snakeY[0]=650; }
            }else if (direction.equals("D")){
                snakeY[0] +=25;
                if (snakeY[0]>650){ snakeY[0]=75; }
            }
            repaint();
        }
        timer.start();//定时器开启
    }
}

```



### 7.填充食物，并设置食物的随机坐标

定义食物坐标，使用随机数，添加坐标为随机坐标

```java
//第一步.GameJpanel
int foodx ,foody;
Random random = new Random();

//第二步.init()
foodx=25+25*random.nextInt(34); //25是边界
foody = 75+25*random.nextInt(24);//食物

//第三步.paintComponent(Graphics G)
Data.food.paintIcon(this,g,foodx,foody);

//第四步. actionPerformed(ActionEvent e)
if (isStart){
    //吃食物
    if (snakeX[0]==foodx&&snakeY[0]==foody){
        length++;//小蛇长度+1
        //再次随机生成食物
        foodx=25+25*random.nextInt(34); //25是边界
        foody = 75+25*random.nextInt(24);//食物
    }
    ...
}
```

### 8.设置游戏失败条件，当小蛇撞到自己身体时候游戏失败



```java
// 第一步 设置游戏失败状态 GameJPanel 属性
boolean isFaile; //游戏失败状态，默认不失败
//第二步 init() 初始化游戏状态 
isFaile = false;
//第三步 判断游戏是否失败，如果失败，就要在面板画上红色文字，paintComponent(Graphics g)
if (isStart == false) {
    g.setColor(Color.white);
    g.setFont(new Font("微软雅黑", Font.BOLD, 40)); //设置字体
    g.drawString("按下空格开始游戏", 300, 300);
}

//第四步 ，键盘监听事件，按下空格时，是失败还是暂停，如果是失败，那么数据初始化 keyPressed(keyEvent e){}
if (keyCode == KeyEvent.VK_SPACE) {//如果按下控制
    if (isFaile) {
        //是否是重新开始
        isFaile = false;
        init();//初始化数据
    } else {
        isStart = !isStart; //取反，空格开始，空格结束
    }
    repaint();  //会把前面paintComponent()重新执行一次
}

//第五步 事件监听事件，首先，需要当前游戏isStart==true并且ifFaile==false,才可移动 actionPerformed(ActionEvent e){}

 if (isStart && isFaile == false){
     ....
     .....
 }
	然后加入是否失败的判断
        //失败判定，撞到自己就是失败
        for (int i = 1; i < length; i++) {
            if (snakeX[0] == snakeX[i] && snakeY[0] == snakeY[i]) {
                isFaile = true;
            }
        }
```



### 9.积分系统



```java
//第一步  在类中设置游戏积分和游戏长度属性
int score; //成绩

//第二步  到初始化方法里面初始化score
score =0; //成绩初始化，吃一个长10分
//第三步 到paintComponent(Graphics g) 把分数画在面板上
g.setColor(Color.white);
g.setFont(new Font("微软雅黑", Font.BOLD, 14)); //设置字体
g.drawString("长度:"+length,750,35);
g.drawString("分数:"+score,750,50);

//第四步 进入事件监听方法，成功+10分
//吃食物
if (snakeX[0] == foodx && snakeY[0] == foody) {
    length++;//小蛇长度+1
    //每吃一次食物分数加10
    score +=10;
    //再次随机生成食物
    foodx = 25 + 25 * random.nextInt(34); //25是边界
    foody = 75 + 25 * random.nextInt(24);//食物
}

//第五步 在事件监听方法中，失败重置时，分数为0

```



### 10.可以继续优化的

```java
// 可以增加等级。分数到一定程度，游戏速度更快
初始等级1,500ms一次 越来越快

//游戏的撞墙没有判定，可以增加撞墙的判定


//小蛇回头可以判定 只能向左
    
    
//有毒食物，食物多个，小蛇多节
    

//游戏界面优化
    
//方向鼠标拖拽
    
//是否可以设置到数据库，可以取盘
    
//联机对战
    
//登录 弹窗登录
    
学过的东西一定要用

```

## 总结

![image-20201028191420266](F:\MD格式学习笔记库\狂神JAVA-2-课堂随记.assets\image-20201028191420266.png)