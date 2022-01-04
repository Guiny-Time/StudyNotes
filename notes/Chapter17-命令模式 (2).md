---
tags: [大三/CS264]
title: Chapter17-命令模式
created: '2022-01-03T16:24:22.691Z'
modified: '2022-01-03T17:43:53.841Z'
---

# Chapter17-命令模式
将一个请求封装成为一个对象，这样允许用不同的请求、日志、队列等来对客户进行参数化，并且**支持撤销操作**
封装了一个方法调用，大体来说，这个封装对象和四个主体关联：**客户程序(client)**，**调用器(invoker)**，**命令(command)**和**命令接收者(receiver)**
- 命令应当根据命令接收者的对象调用命令接收者中的方法，命令接受者接下来进行一系列操作来执行这个命令。
- 命令被单独交给调用器以调用命令。
- 客户程序管理着调用器和命令的对象，负责通过将命令传递给对象以用于决策

> **为什么需要调用器(Invoker)？**
&emsp;
  实际上，调用器不是强制性的，但是在这个模式中，是个很值得一用的工具。
  在面向对象编程中，程序员封装的是对象的数据结构与相应的方法(method)。但是在命令模式中，我们封装的是命令。
  如果没有调用器，当我们需要想办法处理一些命令相关的数据结构比如，队列、日志等，我们不得不将这些数据塞入到客户程序中，从而导致代码臃肿。同时，这种强行塞入的做法会导致新的客户程序如果想有这些功能也必须重写相应数据结构，或者采取继承(这是最糟糕的)。而此时，调用器使得命令相关的数据结构与客户程序解耦，同时调用器也更加支持组合而非继承。

### 优点
- 命令模式将**请求与执行过程解耦**，客户程序不需要知道一个命令具体会有什么行为。
- 可以这样**创建宏**(一系列命令)
- 可以在**不修改现有代码的情况下增加新的命令**
- **支持撤销**等操作
>- **Requests for the creation and the ultimate execution are decoupled.** Clients may not know how an invoker is performing the operations. 
>- You can **create macros** (sequence of commands). 
>- New commands can be **added without affecting the existing system.**
>- Most importantly, you can **support the undo/redo operations.**

### 缺点
- **每增加一个新的命令，会增加一个类**，这样会增加维护成本
- 因为我们将命令与客户程序解耦了。当**发生错误或者异常的时候，如何正确处理返回返回值会变得棘手**，多线程的状况下，这个会更麻烦。

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220104012842.png"/>

## 代码实现
命令接口和具体命令（以undo为例）
```Java
interface Command {
  void executeCommand();
}

class MyUndoCommand implements Command {
  // 命令接收者
  private Receiver receiver;
  public MyUndoCommand(Receiver receiver) {
    this.receiver=receiver;
  }

  // 执行
  @Override
  public void executeCommand() {
    //Perform any optional task prior to UnDo
    receiver.doOptionalTaskPriorToUndo();
    //Call UnDo in receiver now
    receiver.performUndo();
 }
}
```
命令接收者
```Java
class Receiver {
  // 执行undo
  public void performUndo() {
    System.out.println("Performing an undo command in Receiver.");
  }
  // 执行redo
  public void performRedo() {
    System.out.println("Performing an redo command in Receiver.");
  }

  public void doOptionalTaskPriorToUndo() {
    System.out.println("Executing -Optional Task/s prior to execute undo command.");
  }
  public void doOptionalTaskPriorToRedo() {
    System.out.println("Executing -Optional Task/s prior to execute redo command.");
  }
}
```
调用器
```Java
class Invoker {
  Command commandToBePerformed;

  //设置需要调用的命令
  public void setCommand(Command command) {
    this.commandToBePerformed = command;
  }

  //调用命令
  public void invokeCommand() {
    commandToBePerformed.executeCommand();
  }
}
```
客户端
```Java
public class CommandPatternExample {
 public static void main(String[] args) {

 Receiver intendedReceiver = new Receiver();
 MyUndoCommand undoCmd = new MyUndoCommand(intendedReceiver);

 Invoker invoker = new Invoker();
 invoker.setCommand(undoCmd);
 invoker.invokeCommand();

 MyRedoCommand redoCmd = new MyRedoCommand(intendedReceiver);
 invoker.setCommand(redoCmd);
 invoker.invokeCommand();
 }
}
```



