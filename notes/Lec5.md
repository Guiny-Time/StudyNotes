---
tags: [CS385]
title: Lec5
created: '2021-10-19T01:55:53.190Z'
modified: '2021-10-19T15:49:46.480Z'
---

# Lec5
## 处理用户输入
### 输入事件
```javaScript
import React,{Component} from "react";
// 父组件
class App extends Component{
  constructor(props){
    super(props);
    this.state = {input: ""};
    // 声明输入事件
    this.handlerOnChange = this.onChangeEvent.bind(this);
  }

  // 输入事件执行函数
  onChangeEvent(event){
    // 获取用户输入的值并存入属性
    this.setState({input: event.target.value});
    // 方便后续使用
    let searchTerm = this.state.input;
  }

  render(){
    return(<div className="App">

      // 使用子组件
      <SearchInput 
        searchTerm={this.state.input}
        onChange={this.handlerOnChange} />

    </div>);
  }
}

// 子组件
class SearchInput extends Component{
  render(){
    // 注册事件Handler
    const changeHandler = this.props.onChangeHandler;
    // 输入项
    const inputItem = this.props.inputFromUser;

    return(<div className="UserInput">

      // 挂载Handler、传递输入值
      <form>
        <input type="text" 
        value = {inputTerm} 
        onChange = {onChangeHandler} />
      </form>

    </div>);
  }
}
export default App;
```

### 子组件中的map过滤器
需要注意，我们在使用带有过滤器的组件时，需要给它的几个props赋值（尤其是数据库那个），不然会报错无法访问过滤器
```JavaScript
class ComponentB extends Component {
  // 过滤器
  nameSearchFilter(searchTerm) {
    return function (spotifyObj) {
      let songName = spotifyObj.title.toLowerCase();
      return (
        searchTerm !== "" &&
        songName.includes(searchTerm) &&
        searchTerm.length >= 3
      );
    };
  }

  render() {
    // 用户输入和数据
    const searchTerm = this.props.userInput.toLowerCase();
    const songDataBase = this.props.spotifyDataBase;
    const count = this.props.countNum;
    return (
      <div className="ComponentB">
        <hr />
        <h2>Searching Reasult: </h2>
        {songDataBase
          .filter(this.nameSearchFilter(searchTerm))
          .map((element) => (
            <div key={element.ID}>Name: {element.title}</div>
          ))}
      </div>
    );
  }
}
```
