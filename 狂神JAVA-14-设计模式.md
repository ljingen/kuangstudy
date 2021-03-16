# 23种设计模式 DesignPattern

## 1. 设计模式的简介

​	软件设计模式（Design pattern），又称设计模式，是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性、程序的重用性

​	在那与Richard Helm, Ralph Johnson ,John Vlissides合作出版了Design Patterns - Elements of Reusable Object-Oriented Software 一书，在此书中共收录了23个设计模式。这四位作者在软件开发领域里也以他们的匿名著称Gang of Four(四人帮，简称GoF)有时这个匿名GoF也会用于指代前面提到的那本书



**设计模式的六大原则**

- 单一原则(Single Responsibility Principle) 一个类或者一个方法只负责一个职责，尽量做到类的只有一个行为原因引起变化
- 里氏替换原则 (LSP liskov subsitution principle) 子类可以拓展父类的功能，但不能改变原有父类的功能
- 依赖倒置原则(dependence inversion principle)  面向接口编程，(通过接口作为参数实现场景) 上层模块不应该依赖下层模块，两者应依赖抽象，抽象不依赖细节，细节应该依赖抽象，通俗说就是 变量或者传参数，尽量使用抽象类，或者接口
- 接口隔离原则(interface segregation principle) 建立单一接口， 扩展为类也是一种接口，一切皆接口
- 迪米特原则(law of demeter LOD)  最小知道原则，尽量降低类与类之间的耦合，一个对象应该对其他对象有最少的了解
- 开闭原则(open closed principle) 用抽象构建架构，用实现拓展原则



​	**学习设计模式的好的方法**

	- 方法1、学完通过复盘，对已经做了的项目和工程进行理一下思路，进行复盘，看看是否之前用过某种设计模式，但是没有想到
	- 方法2、以后有新的需求，是否可以用我们学到的设计模式进行设计，这样用着用着就熟悉了

## DegisnPattern-01. Strategy策略模式

### 1.概述



### 2.思想



### 3.实例问题

在JAVA李IO流的设计，为什么把BufferedReader设计成  new BufferedReader(new FileReader("f:\test.java"))

而不是设计成

BufferedReader extends FileReader;

然后 new BufferedReader ("f:\test.java")?  

学了装饰者模式就知道为什么这样设计!



`模拟项目` 小明要做一个模拟鸭子游戏，考察各种鸭子，按照抽象、继承多态，他发现鸭子都会叫，会游泳, 他把会叫，会有用放到超类做，但是鸭子头不同，有红头鸭，黑头鸭，绿头鸭，他把不同的放在子类实现，



`项目推出后`，产生了新的需求，看看这个需求的可扩展性

1_添加会飞的鸭子, 有些鸭子确实会飞啊，这时候小明思考放到超类，并把会飞的实现放到了超类，这样也实现了会飞的功能，

这里遇到了一个问题，并不是所有鸭子都会飞，比如红头鸭就不会飞啊， 这样在超类的改动就影响到了子类里面，所有的子类都会飞了，这是不科学的

**继承的问题:对类的局部改动，尤其是超类的局部改动，会影响其他部分，影响会有溢出的效应**



为了解决这个问题，小明使用了面向对象的方法覆盖重写，在超类里面设置fly为no fly,在会飞的鸭子里面重写为fly

`这又衍生一个问题`：如果几十种鸭子，那就需要检查每个鸭子是否会飞，之前写的代码都需要重新过一遍，就需要进行判断然后重写，这工作量很大，而且很可能出错。

那如果把会飞的功能，在超类里面作为抽象函数，让子类实现，需要飞就实现，不需要飞就不实现，是否可行呢？

这仍然是不可行的!  这样做了以后，``代码的复用性就降低了``，比如有20个鸭子都会飞，就需要20种鸭子都实现会飞，并且代码一样，代码可用性降低

`此后，又来了新的需求`，石头鸭子，那我们需要继续填坑

public class StoneDuck extends Duck{.......}

`所以，超类挖的一个坑，每个子类都要来填，增加了工作量，复杂度O(N^2),这不是一个好的设计`



发现的问题: 用OO原则解决新需求不足



### 4.不使用模式实现

```java
//超类
public abstract class Duck {
    public Duck() {
    }

    public abstract void display();

    public void quack() {
        System.out.println("~~gaga~~");
    }

    public void swim() {
        System.out.println("~~i can swim~~");
    }

    //新需求，客户提出鸭子要会飞
    public void fly(){
        System.out.println("~~i can fly~~");
    }
}

//子类
public class GreenHeadDuck extends Duck {
    @Override
    public void display() {
        System.out.println("** Green Head Duck **");
    }
}

//子类
public class RedHeadDuck extends Duck {
    @Override
    public void display() {
        System.out.println("** Red Head Duck**");
    }
}
//使用类
public class StimulateDuck {
    public static void main(String[] args) {
        GreenHeadDuck greenHeadDuck = new GreenHeadDuck();
        RedHeadDuck redHeadDuck = new RedHeadDuck();

        greenHeadDuck.display();
        greenHeadDuck.quack();
        greenHeadDuck.swim();
        //添加新功能，会飞了
        greenHeadDuck.fly();


        redHeadDuck.display();
        redHeadDuck.quack();
        redHeadDuck.swim();
        //添加新功能，会飞了
        redHeadDuck.fly();
    }
}

```



### 5.使用设计模式实现

方法论，如何分析

需要新的设计方式，应对项目的扩展性，降低复杂度

 - 分析项目变化和不变的部分，提取变化的部分，抽象成接口+实现
 - 鸭子那些功能会根据新的需求变化，比如叫声、飞行....这些是变化的，

接口

```java
public interface FlyBehavior {
    void fly();
}

public class BadFlyBehaviorImpl implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("--BadFly--");
    }
}

public class GoodFlyBehaviorImpl implements FlyBehavior {
    public void fly() {
        System.out.println("--GoodFly--");
    }
}
public class NotFlyBehaviorImpl {
    public void fly() {
        System.out.println("--NotFly--");
    }
}


public interface QuackBehavior {
    void quack();
}
public class GaGaQuackBehaviorImpl implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("--GaGa--");
    }
}
public class GeGeQuackBehaviorImpl implements QuackBehavior{
    @Override
    public void quack() {
        System.out.println("--GEGE--");
    }
}
public class NoQuackBehaviorImpl implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("--No Quack--");
    }
}

我们针对于飞的行为，实现一系列飞的行为族，针对叫，我们实现一系列的叫的行为族
```

这样做的好处就是：新增加行为简单，行为类更好的复用，组合更方便，既有继承带来的复用还出，同时没有挖坑， 比如可以用GAGA+坏的飞行，就能组装一个鸭子



策略模式: 分别封装行为接口，实现算法组，超类里面放行为接口对象，在子类里面具体设定行为对象，原则就是，分离变化的部分，封装接口，基于接口编程各种功能，此模式让行为算法的变化独立于算法的使用者

 ![image-20201213002445614](F:\MD格式学习笔记库\狂神JAVA-14-设计模式.assets\image-20201213002445614.png)



### 6.优缺点





### 7.典型应用







## DegisnPattern-13. Rroxy代理模式





