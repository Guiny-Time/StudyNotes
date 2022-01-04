---
tags: [大三/CS264]
title: Chapter11-组合模式
created: '2022-01-03T14:12:13.124Z'
modified: '2022-01-03T16:55:17.888Z'
---

# Chapter11-组合模式
组件模式是将多个对象组合**在类树状结构上以形成部分-整体关系的层次结构**，如下图所示。组合模式允许用户统一地处理单个对象与对象之间的组合。
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220104003736.png" width=500/>

### 优点
- 在类树状结构中，我们可以**统一地处理对象之间的组合(分支结构)和单个对象(叶结构)**。
- 其非常适合**整体-局部的层次关系**。
- 我们可以比较**轻易地在整体结构中添加与删除组件**。

### 缺点
- 如果要在组件模式下**维持组件的相对位置顺序，则需要一些特殊的操作**。
- 如果处理的东西有变异性，那么我们可能**无法比较顺利地删除变异组件**。
- 随着**结构越来越大，结构也会越来越难以维护**。对于一些比较特殊的结构，我们甚至可能需要支出用于动态维护的开销。

## 类图
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220104004837.png" width=400/>

## 代码实现
员工类
```Java
import java.util.ArrayList;
import java.util.List;
 
public class Employee {
   private String name;
   private int salary;
   private List<Employee> subordinates;
 
   //构造函数，参数为员工姓名、工资
   public Employee(String name, int sal) {
      this.name = name;
      this.salary = sal;
      // 下属链表
      subordinates = new ArrayList<Employee>();
   }

   // 添加下属
   public void add(Employee e) {
      subordinates.add(e);
   }

   // 移除下属
   public void remove(Employee e) {
      subordinates.remove(e);
   }

   // 获取下属
   public List<Employee> getSubordinates(){
     return subordinates;
   }
 
   public String toString(){
      return ("Employee :[ Name : "+ name 
      +", salary :" + salary+" ]");
   }   
}
```
```Java
public class CompositePatternDemo {
   public static void main(String[] args) {
      // 实例化所有员工
      Employee CEO = new Employee("John", 30000);
 
      Employee headSales = new Employee("Robert", 20000);
 
      Employee headMarketing = new Employee("Michel", 20000);
 
      Employee clerk1 = new Employee("Laura", 10000);
      Employee clerk2 = new Employee("Bob", 10000);
 
      Employee salesExecutive1 = new Employee("Richard", 10000);
      Employee salesExecutive2 = new Employee("Rob", 10000);

      // 添加下属（作为树状结构的子节点）
      CEO.add(headSales);
      CEO.add(headMarketing);
      // 按照顺序继续添加下属
      headSales.add(salesExecutive1);
      headSales.add(salesExecutive2);
 
      headMarketing.add(clerk1);
      headMarketing.add(clerk2);
 
      //打印该组织的所有员工
      System.out.println(CEO); 
      for (Employee headEmployee : CEO.getSubordinates()) {
         System.out.println(headEmployee);
         for (Employee employee : headEmployee.getSubordinates()) {
            System.out.println(employee);
         }
      }        
   }
}
```
