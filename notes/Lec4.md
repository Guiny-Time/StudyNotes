---
tags: [大三/CS385]
title: Lec4
created: '2021-10-07T02:47:27.570Z'
modified: '2022-01-12T08:05:54.440Z'
---

# Lec4
### 父组件与子组件
```javaScript
import React,{Component} from "react";
// 父组件
class App extends Component{
  render(){     // 渲染函数
    return(<div className="App">
      <Up />    // 使用子组件
    </div>);    // 返回类名为App的div。这里是空的，也可以自己加html代码
  }
}
// 子组件
class Up extends Component{
  render(){     // 渲染函数
    return(<div className="App">I am Up</div>);    // 返回类名为App的div。这里是空的，也可以自己加html代码
  }
}
export default App;
```

### 状态属性
```javaScript
import React,{Component} from "react";

class App extends Component{
  // 构造器声明
  constructor(props){
    super(props);
    // 声明两个状态up和dowm
    this.state = {up: 0, down: 0};
  }

  render(){
    return(<div className="App"></div>);
  }
}
export default App;
```

#### 修改状态（this.setState）
```javaScript
this.setState({状态名: 新的值});
```

## 事件
```javaScript
import React,{Component} from "react";
// 父组件
class App extends Component{
  constructor(props){
    super(props);
    this.state = {up: 0};
    // 声明点击事件
    this.handlerOnClick = this.handerOnClick.bind(this);
  }

  // 点击执行函数
  handerOnClick(){
    console.log("大猫猫万岁");
  }

  render(){
    return(<div className="App">
      <Up buttonHandler={this.handlerOnClick}/>    // 使用子组件
    </div>);
  }
}

// 子组件
class Up extends Component{
  render(){
    // 注册事件Handler
    const buttonHandler = this.props.buttonHandler;
    return(<div className="App">
      I am Up！
      // 挂载事件Handler
      <button onClick={buttonHandler}></button>
    </div>);
  }
}
export default App;
```


