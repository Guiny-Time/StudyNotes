---
tags: [大三/CS264]
title: Chapter1-单例模式
created: '2022-01-02T05:54:50.681Z'
modified: '2022-01-02T07:58:54.814Z'
---

# Chapter1-单例模式
这是几大设计模式中最常被使用的模式之一（至少对我来说），在游戏编程的实战中也有很多用途，比如说全局唯一的UI管理器、全局唯一的音效控制器等等。该模式适合针对“有唯一需求，不希望存在多个”的系统。也就是说，**单例模式的实例同时只能存在0个或者1个**，实例化的过程分为懒实例等

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/homework1_1.png"/>

## 代码实现
### final + 懒实例 + 线程锁实现法
由于在C#里没找到给函数上线程锁的关键词，所以没写。Java里再GetUIController方法前需要加上关键词 *synchronized* 。
> **为什么需要线程安全？**
原因显而易见，因为我们的程序可能工作在多线程（multi threaded）环境下。而如果两个线程同时实例化单例类的实例，可能造成线程冲突（thread conflict），从而得到两个单例类的实例，这显然是不行的。
所以，当多线程环境下，当有两个或以上个线程访问程序时，给方法加上线程锁（thread lock）是必要的。这样的结果是：同一时间只能有一个线程访问代码，其余线程需要等待，避免的冲突的形成。

> **为什么是final类？**
主要目的是防止单例类被继承，从而造成多实例化等问题。
如果去掉final，内部类可能会继承单例类，调用内部类的时候会造成多实例化的问题。
C#中的sealed等价于final。

> **什么是懒实例？**
简单地说，延迟初始化是一种延迟对象创建过程的技术。它说，您应该只在需要时创建对象。当您需要处理开销很大的流程来创建对象时，这种方法很有帮助。
In simple terms, lazy initialization is a technique through which you delay the object creation process. It says that you should create an object only when it is required. This approach can be helpful when you deal with expensive processes to create an object.

**优点**
- 线程安全
- 加载时快速

**缺点**
- 代码冗长
- 程序执行过程中需要进行实例化，存在开销问题

```C#
using System;

namespace SingletonPattern
{
    public sealed class UIController
    {
        private static UIController _uiController;
        // 构造函数
        private UIController() { }
        // 懒实例，在需要（调用获取方法）的时候再判断是否需要实例化，对处理实例化开销比较大的环境下很有利
        // 另外，这里没有写线程锁，因为C#的我不会
        public static UIController GetUIController()
        {
            if (_uiController == null)
            {
                _uiController = new UIController();
                Console.WriteLine("A new UI Controller initialized.");
            }
            else
            {
                Console.WriteLine("Already has a UI controller");
            }
            return _uiController;
        }
    }
}
```
```C#
namespace SingletonPattern
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("===单例模式===");
            UIController ui1 = UIController.GetUIController();
            UIController ui2 = UIController.GetUIController();
            // 二者实际上是等价的，全局只存在一个单例的实例
            if (ui1 == ui2)
            {
                Console.WriteLine("These 2 UI Controller re the same.");
            }
        }
    }
}
```
> **输出**
===单例模式===
A new UI Controller initialized.
Already has a UI controller
These 2 UI Controller re the same.

### 饥渴实现法（Eager Initialization）
和懒实例化相对，饥渴实例在程序加载之初就完成了实例化，之后在需要使用相关方法时直接返回实例。
**优点**
- 简单明了的代码
- 线程安全，虽然不用线程锁，但是在开始的时候就实例化完了
- 程序执行过程中开销小

**缺点**
- 加载时间漫长，因为需要实例化所有单例
```C#
using System;

namespace SingletonPattern
{
    public class MusicMgr
    {
        // eager initialized.在Java中，readonly需要被替换成final
        private static readonly MusicMgr _musicMgr = new MusicMgr();

        private MusicMgr()
        {
            Console.WriteLine("A music manager has been initialized.");
        }

        public static MusicMgr GetMusicMgr()
        {
            Console.WriteLine("Game has a music controller now.");
            return _musicMgr;
        }
    }
}
```
### Bill Pugh法
使用静态内部Helper类实现，静态内部类在调用它们的getInstance()方法之前不会加载到内存中。只有当有人调用getCaptain()方法时，才需要考虑Helper类。
如果只是从main()调用任何DummyMethod()，那么这种方法将不会创建任何不想要的输出，就像前面的情况一样。
```C#
using System;

namespace SingletonPattern
{
    public class BillPugh
    {
        private BillPugh()
        {
            Console.WriteLine("Bill Pugh is here now.");
        }

        private static class SingletonHelper
        {
            public static readonly BillPugh _billPugh = new BillPugh();
        }

        public static BillPugh GetBillPugh()
        {
            return SingletonHelper._billPugh;
        }

        public static void DummyMethod()
        {
            Console.WriteLine("I am so handsome.");
        }
    }
}
```

### 双重锁（Double-Checked Locking）
也是使用了线程锁，但是是在程序被调用判断实例化对象为空时锁定。在锁定之后再进行判断，被称为双重判断锁定。
因为C#的lock好像不能锁定整个类，所以直接写伪代码了。换做在Java，lock那句应该被替换成 *synchronized(DoubleCheck.class)* 
```C#
using System;

namespace SingletonPattern
{
    public class DoubleCheck
    {
        private static DoubleCheck _doubleCheck;
        private static int instanceNum = 0;

        private DoubleCheck()
        {
            instanceNum++;
            Console.WriteLine("Instance: " + instanceNum);
        }

        public static DoubleCheck GetDoubleCheck()
        {
            if (_doubleCheck == null)
            {
                lock (这个类本身，但是没这种写法)
                {
                    if (_doubleCheck == null)
                    {
                        _doubleCheck = new DoubleCheck();
                        Console.WriteLine("New Singleton initialized.");
                    }
                    else
                    {
                        Console.WriteLine("You already have a dc singleton");
                    }
                }
                
            }

            return _doubleCheck;
        }
    }
}
```



