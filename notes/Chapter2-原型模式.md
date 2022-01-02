---
tags: [大三/CS264]
title: Chapter2-原型模式
created: '2022-01-02T08:02:57.307Z'
modified: '2022-01-02T14:43:52.728Z'
---

# Chapter2-原型模式
一种通过克隆的方式降低实例化开销的设计模式。通常来说，我们会克隆一个原型并稍作修改，得到一个新的实例。
## 类图
weeklyLog是以Log为原型的克隆版本：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/homework2_2.png"/>

## 优点
•当创建一个复杂或无聊的类的实例时，它很有用。你可以专注于其他关键的活动。
•您可以在运行时包含或丢弃产品。
•您可以以更低的成本创建新实例。
>- It is useful when creating an instance of a class is a complicated (or boring) process. Instead, you can focus on other key activities.
>- You can include or discard products at runtime.
>- You can create new instances at a cheaper cost.

## 缺点
每个继承了大类的子类都需要实现新的克隆机制。当克隆中存在循环时，这一切将会变得充满挑战。

### 代码实现
```Java
// 实现了可克隆接口
public abstract class BasicCar implements Cloneable
{
  // 基本参数。在克隆之后可以修改
  public String modelName;
  public int basePrice,onRoadPrice;
  public String getModelname() {
    return modelName;
  }
  public void setModelname(String modelname) {
    this.modelName = modelname;
  }
  public static int setAdditionalPrice()
  {
    int price = 0;
    Random r = new Random();
    //We will get an integer value in the range 0 to 100000
    int p = r.nextInt(100000);
    price = p;
    return price;
  }
  // 克隆方法实现
  public BasicCar clone() throws CloneNotSupportedException
  {
    return (BasicCar)super.clone();
  }
}
```
```Java
// 具体的车型
class Nano extends BasicCar
{
  //A base price for Nano
  public int basePrice=100000;
  public Nano(String m)
  {
    modelName = m;
  }
  @Override
  public BasicCar clone() throws CloneNotSupportedException
  {
    // 克隆方法实现
    return (Nano)super.clone();
    //return this.clone();
  }
}
```
```Java
public class PrototypePatternExample
{
  public static void main(String[] args) throws CloneNotSupportedException
  {
    System.out.println("***Prototype Pattern Demo***\n");
    // 实例化一辆Nano，并改参数
    BasicCar nano = new Nano("Green Nano") ;
    nano.basePrice=100000;

    BasicCar bc1;
    //bc1克隆了Nano
    bc1 =nano.clone();
    //Price will be more than 100000 for sure
    bc1.onRoadPrice = nano.basePrice+BasicCar.setAdditionalPrice();
    System.out.println("Car is: "+ bc1.modelName+" and it's price is: "+bc1.onRoadPrice);
  }
}
```

## 深拷贝（deep copy）与浅拷贝（shallow copy）
在原型模式中，我们使用的clone类型是浅拷贝。下面将对于这三个物体之间的引用展开讨论。
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220102164631.png" width=500/>

### 浅拷贝
浅拷贝是对象的按位拷贝，它用原始对象属性值的精确拷贝创建一个新对象。如果属性是基本类型，则复制基本类型的值;如果属性是内存地址(引用类型)，那么副本就是内存地址。
**因此，如果一个对象改变了这个地址，就会影响到另一个对象**。也就是说，默认的复制构造函数只是对象的浅拷贝(逐个复制成员)，也就是说，只复制对象空间，而不复制资源。
>- Shallow copy is a bitwise copy of an object, which creates a new object with an exact copy of the original object attribute value. If the attribute is a basic type, the value of the basic type is copied; If the attribute is a memory address (reference type), the copy is the memory address. 
Therefore, if one object changes this address, it will affect the other object. That is, the default copy constructor is only a shallow copy of the object (copy one member by one), that is, **only the object space is copied, not the resources.** 

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220102164708.png" width=500/>

### 深拷贝
在复制引用类型成员变量时，为引用类型的数据成员创建一个独立的内存空间来复制真实的内容。如果一个对象改变了这个地址，不会影响到另一个对象。
>- Deep copy is when copying reference type member variables, it creates an independent memory space for the data members of reference type to copy the real content. 

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220102164734.png" width=500/>

### 应用场景
- 浅拷贝更快，消耗更低。如果对象只有字段的话很合适
- 深拷贝更慢，但是是完全独立的。如果对象有复杂引用的话很合适









