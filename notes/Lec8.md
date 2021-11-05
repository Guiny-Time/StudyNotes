---
tags: [CS385]
title: Lec8
created: '2021-10-19T01:56:19.420Z'
modified: '2021-11-04T02:41:09.136Z'
---

# Lec8
## 获取网络上的JSON文件
我们需要在属性中声明三个变量：
- this.state.apiData（储存来自API的JSON）
- this.state.isFetched（是否获取成功的bool值）
- this.state.errorMsg（存储错误信息）
```JavaScript
constructor(props){
  super(props);
  this.state={
    apiData: [],
    isFetched: false,
    errorMsg: null
  }
}
```
接着，我们开始获取文件：
```JavaScript
async componentDidMount(){
  try{
    // 获取地址
    const API_URL = 'https://jsonlaceholder.typicode.com/users';
    // 获取结果
    const jsonResult = await response.json();

    // 储存结果到本地变量
    this.setState({apiData:jsonResult});
    this.setState({isFetched: true});
  }catch(error){
    // 获取失败
    this.setState({isFetched: false});
    this.setState({errorMsg: error});
  }
}
```
### 为什么是异步获取？
JS是单线程，如果同步会卡死在获取上，导致页面的其他功能无法正常加载使用，甚至导致网页崩溃
componentDidMount()方法是一个生命周期方法，它允许React作出反应，向调用环境指示当前组件已成功挂载。使用async关键字意味着我们使函数或方法componentDidMount成为一个异步函数

### 什么是Fetch
就是抓取的意思，这是React内置的函数，允许用户从指定的url中获取JSON文件
接下来，我们只需要打印JSON的内容就好了（你想干嘛都行）

React居然不用配置网络代理吗。。。
