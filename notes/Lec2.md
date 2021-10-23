---
tags: [CS385]
title: Lec2
created: '2021-10-06T15:53:53.930Z'
modified: '2021-10-19T12:25:32.459Z'
---

# Lec2
## 代码框架
```javaScript
import React,{Component} from "react";

class App extends Component{
  render(){     // 渲染函数
    return(<div className="App"></div>);    // 返回类名为App的div。这里是空的，也可以自己加html代码
  }
}
export default App;
```

### 声明并使用变量
```javaScript
import React,{Component} from "react";

class App extends Component{
  render(){
    let x = 10;       // JS中的变量是弱类型
    let y = "abc";
    let z = Math.floor(Math.random() * 1000);   // 生成三位随机数
    // 使用x变量的值，用{}来显示值
    return(<div className="App">
            <p>x's value is {x + x}</p> 
            <p>y's uppercase: {y.toUpperCase()}</p> 
            <p>Random!!!{z}</p>
          </div>);
  }
}
export default App;
```

#### 针对数组的map函数
map函数能通过箭头函数逐个对数组元素进行修改并打印到屏幕（不是控制台）上。
```javaScript
import React,{Component} from "react";

class App extends Component{
  render(){
    let myArr = [1,"2",3.4];  // JS的数组也是弱类型

    return(<div className="App">
      {myArr.map(
        (element) => (
          <div key={element}>Array Element: {element}</div>
        )
      )}
    </div>);
  }
}
export default App;
```


