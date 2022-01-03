---
tags: [大三/CS264]
title: Chapter14-观察者模式
created: '2022-01-03T08:28:47.681Z'
modified: '2022-01-03T09:45:08.837Z'
---

# Chapter14-观察者模式
定义对象之间的一对多依赖关系，这样当一个对象改变状态时，它的所有依赖关系都会被通知并自动更新。
在游戏开发中，观察者模式是一种很常见的设计模式。我们通常会对一个事件进行监听，然后在事件发生后执行回调函数内的内容，常用于成就系统等。
观察者模式中有两种类型：
- **订阅者（Observe）**
订阅/注册事件监听，等待事件触发后执行相关函数。也可以随时随地移除监听。
- **发布者（Subject）**
发布/广播事件，通知所有订阅者。订阅者可以有多种类型

### 应用情景
当对象中**存在一对多的关系**时，我们应该使用观察者模式。例如，当一个对象(目标对象)被修改时，依赖对象(观察者对象)将被自动通知。
> When there is a one-to-many relationship in objects, we should use the observer pattern. For example, when an object (target object) is modified, the dependent objects (observer object) will be notified automatically.

### 优点
- 发布者及其注册用户(观察者)正在创建一个**松散耦合的系统**。他们不需要明确地了解对方。
- 当你从通知列表中添加或删除观察者时，不需要修改发布者。
- 可以随时独立添加或删除观察者。
>- The subject and its registered users(observers) are making a loosely coupled system. They do not need to know each other explicitly. 
>- No modification is required in subjects when you add or remove an observer from its notification lists. 
>- Also, you can independently add or remove observers at any time. 

### 缺点
- 内存泄漏是最大的问题(Memory Leak)。在此上下文中，自动垃圾收集器可能并不总是能帮助您。您可以考虑这样一种情况，即移除监听操作没有正确执行。
- 通知顺序(Order of Notification)不可靠。
- Java对观察者模式的内置支持有一些关键的限制，其中之一迫使您选择继承而不是组合，因此它显然违背了一个倾向于相反方向的关键设计原则。

### 与责任链模式的区别
在责任链模式中，通知从链头传入后，开始沿链向链尾移动，方向固定。
在观察者模式中，通知发送到链头后，可以通过广播方式进行发送。如何交付消息取决于处理消息的逻辑。
>- In the chain of responsibility pattern, after a notification is passed in from the chain head, it starts to move along the chain to the end of the chain, and the direction is fixed.
>- The observer pattern is different. After a notification is passed to the chain head, it can be transmitted in broadcast mode. How to deliver the message depends on the logic of processing the message.

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103172615.png" width=300/><img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103172632.png" width=600/>

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103170758.png"/>

## 代码实现
发布者
```Java
import java.util.LinkedList;
// 发布者接口
public interface SubjectInterface {
    // 注册观察者
    void register(Observer observer);
    // 移除观察者
    void unRegister(Observer observer);
    // 通知观察者
    void notify(int notifiedValue);
}
// 具体发布者
public class Subject implements SubjectInterface{
    private LinkedList<Observer> currentRigisters = new LinkedList<Observer>();
    @Override
    public void register(Observer observer) {
        currentRigisters.add(observer);
    }

    @Override
    public void unRegister(Observer observer) {
        currentRigisters.remove(observer);
    }

    @Override
    public void notify(int notifiedValue) {
        for(Observer o : currentRigisters){
            o.update(notifiedValue);
        }
    }
}
```
观察者
```Java
// 观察者接口
public interface Observer {
    void update(int value);
}

// 具体观察者
public class ObserverA implements Observer{
    String name;
    public ObserverA(String name){
        this.name = name;
    }
    @Override
    public void update(int value) {
        System.out.println(name + " receive value: " + value);
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        // 实例化观察者们和发布者
        Observer ob1 = new ObserverA("Wons");
        Observer ob2 = new ObserverA("Orion");
        Observer ob3 = new ObserverB("Oceanic");
        Subject subject = new Subject();

        // 注册、通知、移除、通知
        subject.register(ob1);
        subject.register(ob3);
        subject.notify(2);

        subject.register(ob2);
        subject.unRegister(ob1);
        subject.notify(5);
    }
}
```
