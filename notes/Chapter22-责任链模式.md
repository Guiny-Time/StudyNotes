---
tags: [大三/CS264]
title: Chapter22-责任链模式
created: '2022-01-03T09:45:49.575Z'
modified: '2022-01-03T16:12:01.042Z'
---

# Chapter22-责任链模式
通过给多个对象处理请求的机会，避免请求的发送方和接收方之间的耦合。**将接收到的对象串联起来，沿着链传递请求，直到有对象处理它为止**。
和观察者不同的是，在观察者模式中，所有注册用户都同时收到通知；但是在责任链模式中，以顺序的方式一个接一个地通知链中的对象。

### 优点
- 我们可以**有多个对象来用于处理一个请求**。
- 我们**可以动态地改变责任链**，以更好面对具体问题。
- **一个处理对象只需要做自己的工作**，而不需要考虑别的处理对象是干什么的，**解决不了请求直接传到下一个**就是了。
- 这个模式支持**低耦合**，因为它将发送请求的对象与处理请求的对象之间进行解耦。

### 缺点
- 我们没办法保证我们能够完整处理这个请求，因为责任链到了末尾后就不会再处理请求了。
- 这样的话 debug 可能会比较麻烦。

> **如果传递到了责任链的末尾依然没有解决问题怎么办？**
&emsp;
如果没有人能够处理请求，您可以**用适当的消息抛出异常**，并在您的预期捕获块中捕获异常，以引起您的注意(或以某种不同的方式处理它)。一个简单的解决方案是使用try/catch(或try/finally或try/catch/finally)块。您可以将处理程序放在这些结构中。
您还可以**将请求记录在一个文件中**（log），**或者将其存储在一个队列中**，以便以后处理。

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103233152.png"/>

## 代码实现
抽象责任链
```Java
public abstract class AbstractLogger {
   public static int INFO = 1;
   public static int DEBUG = 2;
   public static int ERROR = 3;
 
   protected int level;
 
   // 责任链中的下一个元素
   protected AbstractLogger nextLogger;
   // 设置下一环
   public void setNextLogger(AbstractLogger nextLogger){
      this.nextLogger = nextLogger;
   }
 
   // 判断有没有能力log信息出来
   public void logMessage(int level, String message){
      if(this.level <= level){
         write(message);
      }
      // 办不到，传给下一环
      if(nextLogger !=null){
         nextLogger.logMessage(level, message);
      }
   }
   // log信息
   abstract protected void write(String message);
   
}
```
具体的责任链环：
```Java
public class ConsoleLogger extends AbstractLogger {
   // 赋予等级值
   public ConsoleLogger(int level){
      this.level = level;
   }
 
   @Override
   protected void write(String message) {    
      System.out.println("Standard Console::Logger: " + message);
   }
}
```
```Java
public class ChainPatternDemo {
   
   private static AbstractLogger getChainOfLoggers(){
      // 赋予等级
      AbstractLogger errorLogger = new ErrorLogger(AbstractLogger.ERROR);
      AbstractLogger fileLogger = new FileLogger(AbstractLogger.DEBUG);
      AbstractLogger consoleLogger = new ConsoleLogger(AbstractLogger.INFO);
      // 设置责任链的下一个
      errorLogger.setNextLogger(fileLogger);
      fileLogger.setNextLogger(consoleLogger);
 
      return errorLogger;  
   }
 
   public static void main(String[] args) {
      // 获取责任链
      AbstractLogger loggerChain = getChainOfLoggers();

      // 按责任链顺序进行处理
      loggerChain.logMessage(AbstractLogger.INFO, "This is an information.");
      loggerChain.logMessage(AbstractLogger.DEBUG, "This is a debug level information.");
      loggerChain.logMessage(AbstractLogger.ERROR, "This is an error information.");
   }
}
```
