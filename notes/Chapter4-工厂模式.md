---
tags: [大三/CS264]
title: Chapter4-工厂模式
created: '2022-01-02T15:37:17.071Z'
modified: '2022-01-02T17:52:21.258Z'
---

# Chapter4-工厂模式
定义一个创建对象的接口，但**让子类决定实例化哪个类**。工厂方法允许类将实例化延迟到子类，节省了实例化的开销。
工厂模式分为产品和工厂两个主要角色：
- **工厂（接口）**
它是工厂方法模式的核心，独立于应用程序。在模式中创建的任何对象的工厂类都必须实现这个接口。
> It is the core of the factory method pattern and is independent of the application. The factory class of any object created in the schema must implement this interface。 
- **工厂类**
实现了工厂接口的具体工厂类，包含与应用程序密切相关的逻辑，并由应用程序调用以创建产品对象。
> This is a concrete factory class that implements the abstract factory interface, contains logic closely related to the application, and is called by the application to create a product object. 
- **产品（抽象类）**
由工厂方法模式创建的对象的超类型，即产品对象的公共父类或共同拥有的接口。
> The supertype of the object created by the factory method pattern, that is, the common parent class or jointly owned interface of the product object. 
- **产品类**
抽象产品角色定义的接口。一个特定的产品是在一个特定的工厂中创建的，它们通常一个接一个地相互对应。
> This role implements the interface defined by the abstract product role. A specific product is created in a specific factory, and they often correspond to each other one by one.

### 优点
- 不同的**代码是相互分离的**，有助于代码维护。
- 代码的**耦合度低**，可以在不改变现有架构的情况下随意添加新的工厂和产品，**遵守开闭原则**。
>- You are separating code that can vary from the code that does not vary. This technique helps you **<span style="color:red">easily maintain code</span>**. 
>- Your **<span style="color:red">code is not tightly coupled</span>**; so, you can add new classes like Lion, Beer, and so forth, at any time in the system without modifying the existing architecture. So, you have **<span style="color:red">followed the “closed for modification but open for extension” principle</span>**. 
### 缺点
- 如果您需要处理大量的类，那么您可能会遇到维护开销（比如说，一百种产品和一百种工厂）
>- If you need to deal with a large number of classes, then you may **<span style="color:red">encounter maintenance overhead.</span>**

## 参考类图
在类图结构中，可以很轻易的发现两个并行的层次结构。在本例中，AnimalFactory、DogFactory和TigerFactory（工厂们）被放置在一个层次中，而Animal、Dog和Tiger（产品们）被放置在另一个层次中。
因此，工厂和他们创造的产品是并行运行的两个层次结构。
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103005818.png" width=500/>

## 代码实现
工厂的形式为抽象类，包含了生产产品的方法
```Java
public abstract class HumanFactory {
    // 因为有子类继承工厂，所以这里是抽象方法
    public abstract Human creatHuman();
}
public class WonsFactory extends HumanFactory{
    // 制造产品,返回一个人类
    @Override
    public Human creatHuman() {
        return new WonsRetniw();
    }
}

```
产品是接口，包含了产品的功能方法
```Java
public interface Human {
    void Hello();
    void Job();
}
```
```Java
public class demo {
    public static void main(String[] args){
        // 实例化工厂
        HumanFactory wonsFactory = new WonsFactory();

        // 从工厂制造Wons产品
        Human wons = wonsFactory.creatHuman();
        wons.Hello();
    }
}
```

### 进一步封装的写法
我们可以在工厂类中直接加入制作产品的过程，这样工厂被声明时，可以直接调用方法生产产品，无需声明产品实例：
```Java
public abstract class HumanFactory {
    // 创建实例
    public abstract Human creatHuman();
    // 完成产品功能
    public Human makeHuman(){
        Human human = creatHuman();
        human.Hello();
        human.Job();
        return human;
    }
}
public class WonsFactory extends HumanFactory{
    // 制造产品
    @Override
    public Human creatHuman() {
        return new WonsRetniw();
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        HumanFactory wonsFactory = new WonsFactory();
        HumanFactory oceanicFactory = new OceanicFactory();

        // 从工厂制造Wons产品
        wonsFactory.makeHuman();
    }
}

```

