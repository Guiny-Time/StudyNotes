---
tags: [大三/CS264]
title: Chapter5-抽象工厂模式
created: '2022-01-02T17:53:52.261Z'
modified: '2022-01-02T19:13:28.139Z'
---

# Chapter5-抽象工厂模式
提供一个接口来创建一系列相关或依赖的对象，而无需指定它们的具体类。
抽象工厂模式相当于在工厂模式的基础上再加了一层抽象，属于“工厂的工厂”。比如说，雪冬和远洋两个人有神明版本和普通人版本，所以可以有两种工厂（神明工厂和普通人工厂）
### 优点

### 缺点
- 抽象工厂中的任何变化都迫使我们传播对具体工厂的修改。如果您遵循的设计理念是编程到接口，而不是到实现，那么您需要为此做好准备。这是开发人员始终牢记的关键原则之一。在大多数情况下，开发人员不希望更改他们的抽象工厂。
- 整体架构可能看起来很复杂。此外，在某些场景中，调试变得很棘手
>- Any change in the abstract factory forces us to propagate  the modification of the concrete factories.   If you follow the  design philosophy that says program to an interface, not to an  implementation, you need to prepare for this. This is one of the  key principles that developers always keep in mind. In most scenarios, developers do not want to change their abstract factories. 
>- The overall architecture may look complex. Also, debugging becomes tricky in some scenarios 

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103015628.png"/>

### 简单工厂模式、工厂模式和抽象工厂模式的区别
简单工厂根据动物工厂，可以直接得到对应的动物
> HumanFactory humanFactory = new HumanFactory();
wons = humanFactory.creatWons();

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103025827.png"/>

工厂模式在工厂实例化之后，需要调用具体的动物工厂的制造方法得到具体的动物
> HumanFactory wonsFactory = new WonsFactory();
  Human wons = wonsFactory.creatHuman();

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103025840.png"/>

抽象工厂模式更多一层抽象，一个工厂对应多种动物
> HumanFactory humanFactory = new GodHumanFactory();
  wons = humanFactory.creatWons();

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103025902.png"/>

#### 总结
1. 使用简单的工厂，您可以将不同于其余代码的代码分开(基本上，您可以解耦客户机代码)。这种方法可以帮助您轻松地管理代码。这种方法的另一个关键优点是客户机不知道对象是如何创建的。因此，它**促进了安全性和抽象**。但它**可能违反开闭原则**。
> With simple factory, you can separate the code that varies from the rest of the code (basically, you decouple the client codes). This approach helps you easily manage your code. Another key advantage of this approach is that the **<span style="color:red">client is unaware of how the objects are created</span>**. So, it promotes both security and abstraction. But it can **<span style="color:red">violate the open-close principle</span>**. 
2. 您可以使用工厂方法模式来克服这个缺点，该模式**允许子类决定如何完成实例化过程**。换句话说，您将对象创建委托给实现工厂方法来创建对象的子类。
> You can overcome this drawback using the factory method pattern that **<span style="color:red">allows subclasses to decide how the instantiation process is completed</span>**. In other words, you delegate the objects creation to the subclasses that implement the factory method to create objects. 
3. 抽象工厂基本上是**工厂中的工厂**。它创建了一系列相关对象，但并不依赖于具体的类。
> The abstract factory is basically a **<span style="color:red">factory of factories</span>**. It creates the family of related objects but it does not depend on the concrete classes. 

工厂方法促进了继承;它们的子类需要实现工厂方法来创建对象。抽象工厂模式促进了对象组合，您可以使用抽象工厂的具体实例组合类。这些工厂都**通过减少对具体类的依赖来促进松散耦合**。
> The factory method promotes inheritance; their subclasses need to implement the factory method to create objects. The abstract factory pattern promotes object composition, where you compose classes using the concrete instances of an abstract factory. Each of these factories **<span style="color:red">promote loose coupling by reducing the dependencies on concrete classes.</span>**

## 代码实现
工厂（接口），里面包含了能够生产的相关产品
```Java
public interface HumanFactory {
    Wons creatWons();
    Oceanic creatOceanic();
}
```
工厂类(神明工厂)，生产对应的神明。如果没有某种神，可以把实现方法写成空。普通人工厂类似
```Java
public class GodHumanFactory implements HumanFactory{
    @Override
    public Wons creatWons() {
        // 返回对应的产品（神明雪冬）
        return new GodWons();
    }

    @Override
    public Oceanic creatOceanic() {
        return new GodOceanic();
    }
}
```
产品（接口）。对于一种产品，写一个他能干什么的接口
```Java
public interface Oceanic {
    void Hello();
    void Job();
}
```
产品类（神明产品）
```Java
public class GodOceanic implements Oceanic{
    @Override
    public void Hello() {
        System.out.println("Hi. I am Oceanic the god");
    }

    @Override
    public void Job() {
        System.out.println("I am god of ocean.");
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        HumanFactory humanFactory;
        Wons wons;
        Oceanic oceanic;

        humanFactory = new GodHumanFactory();
        wons = humanFactory.creatWons();
        oceanic = humanFactory.creatOceanic();
        wons.Hello();
        wons.Job();
        oceanic.Hello();
        oceanic.Job();

        humanFactory = new NormalHumanFactory();
        wons = humanFactory.creatWons();
        oceanic = humanFactory.creatOceanic();
        wons.Hello();
        wons.Job();
        oceanic.Hello();
        oceanic.Job();
    }
}
```





