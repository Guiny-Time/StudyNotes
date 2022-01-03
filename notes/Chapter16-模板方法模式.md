---
tags: [大三/CS264]
title: Chapter16-模板方法模式
created: '2022-01-03T09:48:38.340Z'
modified: '2022-01-03T14:48:27.205Z'
---

# Chapter16-模板方法模式
在操作中定义算法的框架，将一些步骤推迟到子类中。
模板方法模式允许子类**在不改变算法结构的情况下重新定义算法的某些步骤**。
### 和策略模式的区别
- 模板方法模式：通过子类来选择编译时的算法。
- 策略模式：通过控制来选择运行时算法。
>- Template method pattern: compile-time algorithm selection by subclassing.
>- Strategy pattern: run-time algorithm selection by containment.

### 和建造者模式的区别
- 建造者模式：创造型设计模式，客户来决定算法执行的顺序
- 模板方法模式：行为型设计模式，由我来决定代码的行为
>- In Builder Patterns, the clients/customers are the bossthey can control the order of the algorithm.
>- in Template Method pattern, you are the boss-you put your code in a central location and you only provide the corresponding behavior

### 优点
- **只有程序员（我）可以控制基于模板的代码的实现**，客户端无法修改。
- 公共操作被放在一个集中的位置，例如，在一个抽象类中。子类只能重新定义变化的部分，这样就可以**避免冗余代码**。
>- You can control the flow of the algorithms. Clients cannot change them. (Notice that completeCourse() is marked with final keyword in the abstract class BasicEngineering)
>- Common operations are placed in a centralized location, for example, in an abstract class. The subclasses can redefine only the varying parts, so that, you can avoid redundant codes.

### 缺点
- **客户端代码不能指导步骤的顺序**(如果你需要这种方法，你可以遵循Builder模式)。
- 子类可以覆盖父类中定义的方法(即在父类中隐藏原始定义)，这**违背了Liskov替换原则**，基本上是说:如果S是T的子类型，那么T类型的对象可以被S类型的对象替换
- 更多的子类意味着**更分散的代码和更难维护**。
>- Client code cannot direct the sequence of steps (If you need that approach, you may follow the Builder pattern). 
>- A subclass can override a method defined in the parent class (i.e. hiding the original definition in parent class) which can go against Liskov Substitution Principle that basically says: If S is a subtype of T, then objects of type T can be replaced with objects of type S 
>- More subclass means more scattered codes and difficult maintenance. 

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103215907.png"/>

## 代码实现
首先给定一个模板：
```Java
public abstract class Game {
   abstract void initialize();
   abstract void startPlay();
   abstract void endPlay();
 
   // 模板方法，是final
   public final void play(){
 
      //初始化游戏
      initialize();
 
      //开始游戏
      startPlay();
 
      //结束游戏
      endPlay();
   }
}
```
基于这个模板的子类，进行模板方法的具体的实现
```Java
public class Cricket extends Game {
 
   @Override
   void endPlay() {
      System.out.println("Cricket Game Finished!");
   }
 
   @Override
   void initialize() {
      System.out.println("Cricket Game Initialized! Start playing.");
   }
 
   @Override
   void startPlay() {
      System.out.println("Cricket Game Started. Enjoy the game!");
   }
}
```
```Java
public class demo {
   public static void main(String[] args) {
      // 调用模板1
      Game game = new Cricket();
      game.play();
      System.out.println();

      // 调用模板2
      game = new Football();
      game.play();      
   }
}
```




