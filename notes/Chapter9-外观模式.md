---
tags: [大三/CS264]
title: Chapter9-外观模式
created: '2022-01-03T07:25:23.697Z'
modified: '2022-01-03T08:26:59.938Z'
---

# Chapter9-外观模式
为子系统中的一组接口提供统一的接口。Facade定义了一个更高级的接口，使子系统更容易使用。
这相当于一个用户图形界面（API），用户只要简单的点击某个按钮就可以完成对应的交互而不需要访问深层的代码。
它是支持松散耦合的模式之一。在这里，您强调抽象，并通过公开一个简单的接口来隐藏复杂的细节，代码变得更清晰，更有吸引力。**facade不封装子系统类或接口。它只是提供了一个简单的接口(或层)，使您的工作更轻松。**
> Facade pattern does not encapsulate the subsystem classes or interfaces. It just provides a simple interface (or layer) to make your life easier.

### 优点
- 如果一个系统由许多子系统组成，管理所有这些子系统将变得非常困难，客户可能会发现他们很难单独与这些子系统进行通信。facade模式**为客户端提供了一个简单的接口**，而不是复杂的子系统。
- 外观模式通过**将客户机与子系统分离来促进弱耦合**。
- 它还可以帮助你**减少客户端需要处理的对象的数量**。
- 使用组合，可以轻松地访问每个子系统中的方法。
>- If a system consists of many subsystems, managing all those subsystems becomes very tough and clients may find their life difficult to communicate separately with each of these subsystems. Facade patterns **<span style="color:red">provides a simple interface to clients, instead of presenting complex subsystems.</span>**
>- This approach also **<span style="color:red">promotes weak coupling by separating a client from the subsystems.</span>** 
>- It can also help you to **<span style="color:red">reduce the number of objects that a client needs to deal with.</span>** 
>- Use the compositions, which can easily access the methods in each subsystem.

### 缺点
- 子系统与外观层相连。因此，您**需要考虑一个额外的编码层**(即，您的代码库增加了)。
- 当子系统的内部结构发生变化时，你也需要将这些**变化**合并到facade层中。
- **开发人员需要了解这个新层**，而他们中的一些人可能已经知道如何有效地使用子系统/api
>- Subsystems are connected with the facade layer. So, you **<span style="color:red">need to take care of an additional layer of coding</span>** (i.e., your codebase increases). 
>- When the internal structure of a subsystem changes, you **<span style="color:red">need to incorporate the changes in the facade layer</span>** also. 
>- **<span style="color:red">Developers need to learn about this new layer</span>**, whereas some of them may already be aware of how to use the subsystems/APIs efficiently 

***

### 与适配器模式的区别
- 在适配器模式中，我们更改了接口，以便客户机不会感觉到接口之间的差异。
- 在外观模式中，我们简化了接口。它们为客户端提供了一个简单的交互接口(而不是复杂的子系统)。
>- In the adapter pattern, you try to alter an interface so that the clients do not feel the difference between the interfaces. 
>- The facade pattern simplifies the interface. They present the client a simple interface to interact with (instead of a complex subsystem).

### 与中介者模式的区别
- 在中介者模式中，子系统可以识别中介。他们互相交谈。
- 在外观模式中，子系统不知道外观层的存在，并且提供了从外观层到子系统的单向通信（调用）。
>- In a mediator pattern implementation, subsystems are aware of the mediator. They talk to each other. 
>- In a facade, subsystems are not aware of the facade and the one-way communication is provided from facade to the subsystem(s).

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/9-2.png"/>

## 代码实现
子系统们：
```Java
// 制造头发的子系统
public class Hair {
    public void createWonsHair(){
        System.out.println("Brown hair.");
    }
    public void createOceanicHair(){
        System.out.println("Golden hair.");
    }
}
// 制造身体的子系统
public class Body {
    public void createWonsBody(){
        System.out.println("Wear a blue coat.");
    }
    public void createOceanicBody(){
        System.out.println("Wear a shirt.");
    }
}
```
外观层
```Java
public class HumanFacade {
    // 组合的写法
    Hair hair;
    Body body;
    public HumanFacade(){
        hair = new Hair();
        body = new Body();
    }

    // 把复杂的子系统抽象到一个方法内
    public void createWons(){
        hair.createWonsHair();
        body.createWonsBody();
        System.out.println();
    }

    public void createOceanic(){
        hair.createOceanicHair();
        body.createOceanicBody();
        System.out.println();
    }
}
```
客户端
```Java
public class demo {
    public static void main(String[] args){
        HumanFacade humanFacade = new HumanFacade();

        humanFacade.createWons();
        humanFacade.createOceanic();
    }
}
```


