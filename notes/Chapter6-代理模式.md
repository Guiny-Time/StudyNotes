---
tags: [大三/CS264]
title: Chapter6-代理模式
created: '2022-01-02T19:14:50.264Z'
modified: '2022-01-02T20:03:57.893Z'
---

# Chapter6-代理模式
嗯...就是代理，平常我们经常用代理服务器，某种意义上和这是一样的。有一个境外的服务器，现在在这个服务器的基础上形成一个国内的代理服务器，我们就可以在国内通过代理服务器完成境外服务器可以完成的工作（即翻墙）了。
代理模式则是直接使用代理类的实例，在代理类中，对真实对象进行懒实例化。代理模式**专注于控制对对象的访问**。
## 代理类型
- 远程代理。**隐藏在不同地址空间中的实际对象**。例如：调用远程服务器上的实际对象的方法的开销很大，在本地服务器上形成一个代理可以降低开销。
- 虚拟代理。执行优化技术，例如**根据需要创建一个重对象**。例如：避免多次加载一个大图像。
- 保护代理。处理**不同的访问权限**。例如：组织中的安全团队可以实现一个保护代理来阻止Internet对特定网站的访问。
- 智能参考。当客户端访问对象时，执行**额外的管理工作**。一个典型的操作是计算在特定时刻对实际对象的引用数量。
>- Remote proxies. **<span style="color:red">Hide the actual object that stays in a different address space</span>**.
>- Virtual proxies. Perform optimization techniques, such as the **<span style="color:red">creation of a heavy object on a demand basis</span>**.
>- Protection proxies. Deal with **<span style="color:red">different access rights</span>**.
>- Smart reference. **<span style="color:red">Performs additional housekeeping work</span>** when an object is accessed by a client. A typical operation is counting the number of references to the actual object at a particular moment.
### 优点
- 分离目标对象:代理模式可以将代理对象与实际的被称为目标对象分离。
- 降低耦合:在一定程度上降低了系统耦合，具有良好的可扩展性。
- 保护目标对象:代理类表示目标对象的业务逻辑。客户端直接与代理类交互，并且客户端与实际目标对象之间没有关联。
- 增强目标对象:代理类可以在目标对象的基础上添加新的函数。
>- Separate the target object: The proxy pattern can **separate the proxy object from the real called target object.**
>- Reduce coupling: To a certain extent, it reduces the system coupling and has **good expansibility.**
>- Protect target object: The proxy class represents the business logic of the target object. The client directly interacts with the proxy class, and there is no association between The client and the actual target object. 
>- Enhance the target object: the proxy class can add new functions based on the target object. 

### 缺点
- 增加类的数量: 代理模式会增加系统中类的数量。系统的复杂性增加了。
- 性能下降: 客户端和目标对象之间增加了代理对象，导致请求处理速度变慢。
>- Increase in the number of classes: The proxy pattern will increase the number of classes in the system. **The complexity of the system is increased.**
>- Performance degradation: A proxy object is added between the client and the target object, resulting in **slower request processing speed.** 

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103031718.png"/>

## 代码实现
真实的对象（以图片为例）
```Java
public class Image extends Subject{
    @Override
    public void doSomething() {
        System.out.println("I am real image.");
    }
}
```
代理的对象
```Java
public class ProxyImage extends Subject{
    // 这里的Subject是对象的抽象类，图片和代理图片都继承了它
    static Subject cs;
    @Override
    public void doSomething() {
        System.out.println("I am the proxy image.");
        // 懒实例化
        if(cs == null){
            cs = new Image();
        }
        // 调用真实图片的方法
        cs.doSomething();
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        ProxyImage proxy = new ProxyImage();
        proxy.doSomething();
    }
}

```
### 非懒实例化的实现方式
可以直接在代理的构造函数中完成对真实对象的实例化。如：
```Java
public class ProxyImage extends Subject{
    static Subject cs;
    // 这里的Subject是对象的抽象类，图片和代理图片都继承了它
    public ProxyImage(){
      cs = new Image();
    }
    @Override
    public void doSomething() {
        System.out.println("I am the proxy image.");
        // 调用真实图片的方法
        cs.doSomething();
    }
}
```
但如果遵循此设计，则每当实例化代理对象时，都需要实例化一次Image类。因此，这个过程可能最终会创建不必要的对象，造成资源浪费。


