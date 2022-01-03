---
tags: [大三/CS264]
title: Chapter7-装饰器模式
created: '2022-01-02T20:04:28.150Z'
modified: '2022-01-02T21:07:14.528Z'
---

# Chapter7-装饰器模式
动态地将额外的责任附加到对象上。装饰器为扩展功能提供了一个灵活的替代子类。
这个模式表示类必须**对修改关闭，但对扩展开放**。也就是说，**可以在不干扰现有功能的情况下添加新功能**。
当我们想要向特定对象(而不是整个类)添加特殊功能时，装饰器模式非常有用。在这个模式中，我们尝试使用对象组合的概念，而不是继承。因此，当我们掌握了这项技术后，就可以在不影响底层类的情况下向对象添加新的职责。比如说，我们有一个房子，我们可以用彩灯装饰器和烟囱装饰器在原有的房子的基础上加上彩灯和烟囱。
**注意**：装饰器应符合单一职责原则（SOLID），这样更方便于简单的添加/删除装饰器。
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103042449.png" width=300/><img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103042505.png" width=300/>

### 优点
- **现有的结构是原封不动的**，所以你不会在那里引入bug。
- **新功能可以很容易地添加**到现有的对象。
- 在初始设计阶段，你**不需要预测/实现所有支持的功能**。你可以增量开发(例如，一个一个地添加装饰器对象来支持增量需求)。您必须承认这样一个事实:如果首先创建一个复杂的类，然后尝试扩展功能，这将是一个冗长乏味的过程
>- The existing structure is untouched, so that you are not introducing bugs there. 
>- New functionalities can be easily added to an existing object. 
>- You do not need to predict/implement all the supported functionalities at the initial design phase. You can develop incrementally (e.g., add decorator objects one by one to support incremental needs). You must acknowledge the fact that if you make a complex class first, and then you try to extend the functionalities, it will be a tedious process 

### 缺点
- 如果您在系统中创建了太多的装饰器，将很难维护和调试，会造成不必要的混乱。
>- If you create too many decorators in the system, it will be hard to maintain and debug. So, in that case, it can create unnecessary confusion. 

### 复合是如何促进继承不能实现的动态行为的?
- 当派生类从父类继承时，它只继承基类那个时候的行为。尽管不同的子类可以以不同的方式扩展基类/父类，但这种类型的绑定在编译时是已知的，所以本质上是静态的。但是您在示例中**使用组合概念的方式让您可以试验动态行为**。
- 当我们设计一个父类时，我们可能没有足够的可见性来了解我们的客户在以后的阶段需要什么样的额外责任。我们的约束是**我们不应该频繁地修改现有的代码**。在这种情况下，对象组合不仅优于类的继承，还确保了我们不会向现有体系结构引入bug。**继承中出 bug，我们可能得往父类回溯。而组合我们只需要看是什么模块出问题了。**
- 最后，在这种情况下，您必须记住一个关键的设计原则(开闭原则): **类应该对扩展开放，但对修改关闭**。
- 另外，使用继承可能会造成多继承的问题。**多重继承有运行时的编译时间，会导致运行速度变慢。**

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103042053.png"/>

## 代码实现
组件（造房子）
```Java
public abstract class Component {
    public abstract void makeHouse();
}
```
房子本体，之后的装饰都是基于这个的
```Java
public class ConcreteComponent extends Component{
    @Override
    public void makeHouse() {
        System.out.println("Original House here.");
    }
}
```
抽象装饰器
```Java
public abstract class AbstractDecorator extends Component{
    protected Component component;
    // 设置被装饰的组件（房子）
    public void setTheComponent(Component c){
        component = c;
    }

    // 建造，装饰
    @Override
    public void makeHouse() {
        if(component!=null){
            component.makeHouse();
        }
    }
}
```
具体的装饰器（挂彩灯）
```Java
public class LampDecorator extends AbstractDecorator{
    // 组合
    @Override
    public void setTheComponent(Component c) {
        super.setTheComponent(c);
    }

    // 造房子，加入了彩灯方法
    @Override
    public void makeHouse() {
        super.makeHouse();
        System.out.println("Lamp decorator begin.");
        addLamp();
    }

    // 彩灯方法
    public void addLamp(){
        System.out.println("Lamp is adding...");
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        ConcreteComponent house = new ConcreteComponent();
        house.makeHouse();

        System.out.println("Now let's add some lamps.");
        LampDecorator lampDecorator = new LampDecorator();
        // 设置被装饰的组件
        lampDecorator.setTheComponent(house);
        // 添加彩灯，装饰完成
        lampDecorator.makeHouse();
    }
}
```







