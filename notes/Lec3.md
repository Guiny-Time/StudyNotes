---
tags: [大三/CS385]
title: Lec3
created: '2021-10-19T01:55:46.889Z'
modified: '2022-01-12T08:06:00.854Z'
---

# Lec3
### 对象
你感觉这很像JSON，确实，我也感觉...
```javaScript
import React,{Component} from "react";

function fliter(obj){
  return(obj.id > 1); // 对象过滤器，返回id大于1的对象
}

class App extends Component{
  render(){
    let myObj = [id: 1, name: "2", age: 3.4];
    console.log(myObj);     // 会完整打印整个对象
    return(<div className="App">
      <p>myObj's name is: {myObj.name}</p>    // 获取对象的某个属性
    </div>);
  }
}
export default App;
```

### 对象数组
由对象组成的数组。
**若元素具有相同的键会导致报错，请规避这种情况**
```javaScript
import React,{Component} from "react";
function fliter(obj){
  return(obj.id > 1); // 对象过滤器，返回id大于1的对象
}
class App extends Component{
  render(){
    let myObjArr = [{id: 1, name: "2", age: 3.4}, {id: 2, name: "4", age: 3.8}];
    console.log(myObj);     // 会完整打印整个对象
    return(<div className="App">
      {myObjArr.fliter(myObjCars).map(
        (element) => (
          <div key={element.id}>Name: {element.name}</div>
        )
      )}
    </div>);
  }
}
export default App;
```

### Debug
```javaScript
import React,{Component} from "react";

class App extends Component{
  render(){
    let x = 10;       // JS中的变量是弱类型
    let y = "abc";
    let myArr = [1,"2",3.4];  // JS的数组也是弱类型
    console.log(myArr);   // 控制台输出x的值。这对数组或者对象很有用，会逐个打印：
    /*
    [1,"2",3.4]
    0: 1
    1: "2"
    3: 3.4
    */
    return(<div className="App">
      <p>The first element in array is: {myArr[0]}</p>  // 获取元素的方式和JAVA一样
    </div>);
  }
}
export default App;
```

### 导入外部JS文件
```javaScript
import React,{Component} from "react";
import {carOwners} from ‘./car_owners’;   // 从项目目录下的car_owers.js中导入文件，并命名成carOwers
const aaa = carOwners;    // 弄个变量

class App extends Component{
  render(){     // 渲染函数
    return(<div className="App"></div>);    // 返回类名为App的div。这里是空的，也可以自己加html代码
  }
}
export default App;
```
