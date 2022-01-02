---
tags: [大三/CS264]
title: Chapter3-建造者模式
created: '2022-01-02T08:57:17.261Z'
modified: '2022-01-02T15:04:57.854Z'
---

# Chapter3-建造者模式
如果需要创建一个**复杂**的对象，该对象涉及构建过程中的各个步骤，同时**产品需要是不可变的**，那么建造者模式是一个很好的选择。
一个对象的创造机制应该独立于这些部分。施工过程并不关心这些部件是如何组装的。同样的构造过程必须允许我们创建对象的不同表示形式。
建造者模式有四个角色：指导（Director）、建造者（Builder）、具体建造者（ConcreteBuilder）、产品（Product）
### 优点
- **可以创建一个复杂的对象**，一步一步，并改变步骤。可以通过隐藏复杂构造过程的细节来促进封装。当整个施工结束时，主管可以从建造者那里取回最终产品。一般来说，在高层次上，似乎只有一种方法可以制作完整的产品。其他内部方法只涉及部分创建。所以，你**可以更好地控制施工过程**。
- 使用这种模式，**同样的施工过程可以生产出不同的产品**。
- 因为你**可以改变构建步骤**，你**可以改变产品的内部表示**。
>- You **can create a complex object**, step by step, and vary the steps. You promote encapsulation by hiding the details of the complex construction process. The director can retrieve the final product from the builder when the whole construction is over. In general, at a high level, you seem to have only one method that makes the complete product. Other internal methods only involve partial creation. So, you have finer control over the construction process. 
>- Using this pattern, the **same construction process can produce different products**. 
>- Since you can **vary the construction steps**, you can **vary product’s internal representation**.

### 缺点
- **产品是不可塑的**，生产完成之后的产品无法被更改
- 你可能**需要复制部分代码**。这些重复可能在某些上下文中产生重大影响，并转变为反模式。
- 建造者专注于一种特定类型的产品。所以，**要创造不同类型的产品，需要想出不同的建造器**。
- **对于结构非常复杂的产品，建造者模式才更有意义**。
>- **It is not suitable if you want to deal with mutable objects** (which can be modified later). 
>- You may **need to duplicate some portion of the code**. These duplications may have significant impact in some context and turn into an antipattern. 
>- A concrete builder is dedicated to a particular type of product. So, **to create different type of products, you may need to come up with different concrete builders**. 
>- **The approach makes more sense if the structure is very complex**. 

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/3-1.png"/>

## 代码实现
```Java
// Director class
class Director
{
  // 实例化建造者
  Builder builder;
  public void construct(Builder builder)
  {
    //执行具体建造步骤 
    this.builder = builder;
    builder.startUpOperations();
    builder.buildBody();
    builder.insertWheels();
    builder.addHeadlights();
    builder.endOperations();
  }
}
```
```Java
interface Builder
{
  // 所有建造方法
  void startUpOperations();
  void buildBody();
  void insertWheels();
  void addHeadlights();
  void endOperations();
  /*The following method is used to retrieve the object that is constructed.*/
  Product getVehicle();
}
```
对于一个具体的产品应该怎么组装：
```Java
//Car class
class Car implements Builder
{
  private String brandName;
  private Product product;
  public Car(String brand)
  {
    product = new Product();
    this.brandName = brand;
  }
  public void startUpOperations()
  {
    //Starting with brand name
    product.add(String.format("Car model is :%s",this.brandName));
  }
  public void buildBody()
  {
    product.add("This is a body of a Car");
  }
  public void insertWheels()
  {
    product.add("4 wheels are added");
  }
  public void addHeadlights()
  {
    product.add("2 Headlights are added");
  }
  public void endOperations()
  { //Nothing in this case
  }
  public Product getVehicle()
  {
    return product;
  }
}
```
```Java
// Product class
class Product
{
  private LinkedList<String> parts;
  public Product()
  {
    parts = new LinkedList<String>();
  }
  public void add(String part)
  {
    //Adding parts
    parts.addLast(part);
  }
  public void showProduct()
  {
    System.out.println("\nProduct completed as below :");
    for (String part: parts)
    System.out.println(part);
  }
}
```
```Java
public class BuilderPatternExample {
  public static void main(String[] args) {
    System.out.println("***Builder Pattern Demo***");
    Director director = new Director();

    Builder fordCar = new Car("Ford");
    // Making Car
    director.construct(fordCar);
    Product p1 = fordCar.getVehicle();
    p1.showProduct();
  }
}
```
