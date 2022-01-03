---
tags: [大三/CS264]
title: Chapter8-适配器模式
created: '2022-01-02T21:08:20.649Z'
modified: '2022-01-03T06:55:01.568Z'
---

# Chapter8-适配器模式
将类的接口转换为客户期望的另一个接口。适配器允许由于接口不兼容而无法协同工作的类。
这是一种很常用的设计模式，当系统升级导致一些接口不能好好使用的时候，就需要适配器来调和这些矛盾完成升级。比如说耳机转换头就是经典的适配器。
再比如说，计算矩形的面积有一个计算的类，但是要用来计算三角形的面积，就需要写一个三角形面积计算适配器。适配器的重点在于适配器内的转换操作。
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103133915.png" width=500/>
### 优点
- 适配器的工作简单直接，并且回报巨大，特别是对于那些**不能更改但仍然需要使用**它们以保持稳定性的遗留系统，来适应新的系统。

### 缺点
- 需要编写一些额外的代码。
- 适配器的工作应该仅限于执行简单的接口转换，不应该附加其他的任务（这样做也违反了单一职责原则）。

### 适配器的类型
- **对象适配器（Object Adapters）**
对象适配器**通过对象组合进行适配(Object Compositions)**。例如，三角形适配器实现了矩形接口的方法，三角形是Adaptee（被适配到矩形接口中）接口，**适配器保存了adaptee实例**。
> **基本实现步骤**
可以参见代码实现部分的接口实现法：
>- 继承适配目标的接口，或继承适配目标的抽象类
>- 在构造函数中提到您正在适配的类，并在实例变量中存储实例
>- 改写接口方法进行适配

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103142757.png" width=500/>

- **类适配器（Class Adapter）**
类适配器**通过子类进行适配**。它们是多重继承的促进者。但是在Java中不支持通过类进行多重继承。(需要接口来实现多继承的概念。)
**随着需要适配的类越来越多，继承的内容也会越来越多，造成结构上的复杂化。**
> **基本实现步骤**
可以参见代码实现部分的类适配器方法：
>- 继承适配目标的类，实现目标接口
>- 改写接口方法进行适配

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220103142818.png" width=500/>

#### 总结
对象适配器使用组合，相较于使用继承的类适配器更为灵活，适用范围也更广。

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/8-2.png"/>

## 代码实现
原本的类（三孔插座，只能查三孔插头）
```Java
public class Plug {
    public void usePlug(TrianglePlug t){
        System.out.println(t.hole + " holes plug");
    }
}
```
插座适配器（让两孔插头也能使用三孔插座）
```Java
public class TwoPlugAdapter {
    public void usePlug(TwoPlug t){
        Plug p = new Plug();
        TrianglePlug tri = new TrianglePlug();

        // 重点是三孔怎么用两孔去表示
        tri.hole = t.hole + 1;
        p.usePlug(tri);
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        TwoPlug two = new TwoPlug();
        TwoPlugAdapter twoPlugAdapter = new TwoPlugAdapter();
        twoPlugAdapter.usePlug(two);
    }
}
```
### 接口实现法
为了严格遵守面向对象的设计原则，我们可以使用接口实现法来实现适配器模式。
对物体抽象出一层接口，随后物体实现接口的相关内容：
```Java
public interface RectangleInterface {
    void aboutRectangle();
    float calculateAre();
}
public class Rectangle implements RectangleInterface{
    public float base;
    public float height;
    public Rectangle(float b, float h){
        this.base = b;
        this.height = h;
    }
    @Override
    public void aboutRectangle() {
        System.out.println("The rectangle base is " + base);
        System.out.println("The rectangle height is " + height);
    }

    @Override
    public float calculateAre() {
        return base * height;
    }
}
```
三角形适配器直接实现了矩形的接口，然后在里面的相关方法内调用了三角形自己的方法
```Java
// 继承适配目标的接口
public class TriangleAdapter implements RectangleInterface{
    // adaptee实例
    Triangle triangle;
    // 在构造函数中提到正在适配的类
    public TriangleAdapter(Triangle t){
        this.triangle = t;
    }

    // 改写接口方法进行适配
    @Override
    public void aboutRectangle() {
        triangle.aboutTriangle();
    }

    @Override
    public float calculateAre() {
        return triangle.calculateArea();
    }
}
```
```Java
public class demo {
    public static void main(String[] args){
        Rectangle rectangle = new Rectangle(10,20);
        Triangle triangle = new Triangle(3,4);
        TriangleAdapter adapter = new TriangleAdapter(triangle);

        rectangle.aboutRectangle();
        rectangle.calculateAre();
        adapter.aboutRectangle();
        adapter.calculateAre();
    }
}
```
### 类适配器
采用了继承的方式，再实现了目标的接口。这样可能造成多继承的问题，如果Triangle类是final类也不能被继承。
```Java
// 继承适配目标的类，实现目标接口
class TriangleClassAdapter extends Triangle implements RectInterface{
  public TriangleClassAdapter(double base, double height) {
    super(base, height);
  }

  // 改写接口方法进行适配
  @Override
  public void aboutRectangle(){
    aboutTriangle();
  }

  @Override
  public double calculateAreaOfRectangle(){
    return calculateAreaOfTriangle();
  }
}
```






