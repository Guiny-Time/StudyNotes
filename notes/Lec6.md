---
tags: [CS385]
title: Lec6
created: '2021-10-19T01:55:57.444Z'
modified: '2021-10-20T15:51:28.103Z'
---

# Lec6
## 给JSON排序函数
原生的arr.sort()根本不够用，所以需要自己写比较函数来进行排序
```JavaScript
arr.sort(你自己搞的比较函数)
```
### 数字排序
首先，写出排序方法：
```JacaScript
compareWithNum(ObjectA, ObjectB){
  let comparison = 0;
  // 使用对象的id作为数字进行比较
  if(ObjectA.id > ObjectB.id) comparison = 1;
  else if(ObjectA.id < ObjectB.id)  camprison = -1;
  else  comprison = 0;

  return comparison;
}
```
使用排序方法：
```JavaScript
{
  ObjArr.sort(this.compareWithNum).map((a)=>(
    //具体的展示操作
))}
```

### 字符串排序
排序方法：
```JacaScript
compareWithString(ObjectA, ObjectB){
  let comparison = 0;
  // 使用对象的id作为数字进行比较
  if(ObjectA.name.toLowerCase() > ObjectB.name.toLowerCase()) comparison = 1;
  else if(ObjectA.name.toLowerCase() < ObjectB.name.toLowerCase())  camprison = -1;
  else  comprison = 0;

  return comparison;
}
```
使用它：
```JavaScript
{
  ObjArr.sort(this.compareWithString).map((a)=>(
    //具体的展示操作
))}
```

### 排序方法结合过滤器
排序和Filter是可以一起用的，如：
```JavaScript
{
  ObjArr.sort(this.排序方法).filter(this.过滤器方法(输入参数)).map((a)=>(
    //具体的展示操作
  ))
}
```

#### 升序/降序？
我们可以注意到，我们返回1/-1或者0来进行排序，这三个数就是程序判断大小的依据。因此，在排序方法中改变返回值就可以自由控制升序降序。

#### JSX、虚拟DOM、条件渲染
- JSX
JSX是基于JS的一层抽象，能更好的帮助你写代码
比如说我们遇到过：
```JavaScript
{
  ObjArr.sort(this.排序方法).filter(this.过滤器方法(输入参数)).map((a)=>(
    <b>ObjArr.name</b>
  ))
}
```
在js代码中出现了html语言，这就是JSX。
- 虚拟DOM
虚拟DOM是独立于真实的DOM之外的副本，当虚拟DOM发生改动时，真实的DOM响应式的发生更改，这是一种牺牲空间换取时间的行为。
虚拟DOM的修改很快，我们只需要渲染真实DOM上需要修改的部分而不需要整个重新渲染。
- 条件渲染
我们一般把需要条件判断是否显示的html语句放在JSX区域的花括号里，如：
```JavaScript
render(){
  return(
    <div className="App">
      // 当今天是周末时，显示p标签内容
      {this.isWeekend() && (
        <p>Today is Weekend</p>
      )}
    </div>
  )
}
```

## 用户定义函数
实际上，我们已经写过好几个函数了，比如过滤器函数、比较函数等等。不过，对于函数写在哪里很有讲究，比如：
```JavaScript
import React,{Component} from "react";
// 位于App这个组件之外，全局
function f1(){
  return new Date().toLocalString(); 
}

class App extends Component{
  // 位于App组件内
  function f2(obj){
    return new Date().toLocalString(); 
  }
  render(){
    // const f2 = this.props.F;,这是放子组件里这个位置的，在父组件用的时候赋值
    // 通过这个做法，子组件可以使用父组件的函数
    return(<div className="App">
      <p>{f1()}</p>
      <p>{this.f2()}</p>
    </div>);
  }
}
export default App;
```

### 向数组中添加新的对象
```JavaScript
arr.concat(newObject);
```













