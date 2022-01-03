---
tags: [大三/CS264]
title: Chapter15-策略模式
created: '2022-01-03T09:45:28.210Z'
modified: '2022-01-03T14:07:37.872Z'
---

# Chapter15-策略模式
https://www.runoob.com/design-pattern/strategy-pattern.html
定义一系列算法，封装每一个算法，并使它们可互换。策略让算法独立于使用它的客户端而变化。
你现在有许多算法可以使用，而你作为用户，你可以根据需求动态选择算法进行使用。
策略模式建议将算法封装在不同的类中，而我们将这些封装好的算法成为策略。使用策略的对象通常被称为**上下文对象(context object)**，这些策略也可以被称为行为(behavior)。
比如说交通工具有空运（飞机）和海运（轮船），这两种就属于两种不同的策略。在上下文环境中当跨越陆地时，我们选择空运；跨越海洋时，我们可能选择海运。

### 选择组合而非继承的好处
- 将固定行为的代码与易变行为的代码分开。
- 尽可能保障易变化的部分能够自由(易维护)。
- 尽可能允许复用代码。

### 优点
- 这个模式使你的类独立于算法。
- 更容易维护你的代码库。
- 易于扩展。
>- This pattern makes your classes independent from algorithms.
>- Easier maintenance of your codebase.
>- It is easily extendable.

### 缺点
- 上下文类的添加在我们的应用程序中导致了更多的对象。
- 应用程序的用户必须了解不同的策略。
- 当你引入一个新的行为/算法时，你可能也需要改变客户端代码。
>- The addition of context classes causes more objects in our application.
>- Users of the application must be aware of different strategies.
>- When you introduce a new behavior/algorithm, you may need to change the client code also.


## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103202636.png"/>

## 代码实现
交通工具，即上下文环境。当上下文环境变成船的时候，策略（交通媒介）变成水
```Java
public abstract class Vehicle {
    TransportMedium transportMedium;
    public Vehicle(){}
    public void showTransportMedium(){
        transportMedium.transport();
    }
    public void commonJob(){
        System.out.println("We all can be used to transport.");
    }
    public abstract void showMe();
}
// 具体的交通工具
public class Boat extends Vehicle{
    public Boat() {
        // 交通媒介
        transportMedium = new WaterTransport();
    }

    @Override
    public void showMe() {
        System.out.println("I am a boat.");
    }
}
```
交通媒介（策略）
```Java
public interface TransportMedium {
    public void transport();
}
public class WaterTransport implements TransportMedium{
    @Override
    public void transport() {
        System.out.println("Transporting in water.");
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        // 多态性，上下文切换成船
        Vehicle vehicleContext = new Boat();
        vehicleContext.showMe();
        vehicleContext.showTransportMedium();
        System.out.println();

        vehicleContext = new Airplane();
        vehicleContext.showMe();
        vehicleContext.showTransportMedium();
    }
}
```
