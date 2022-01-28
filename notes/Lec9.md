---
tags: [大三/CS385]
title: Lec9
created: '2021-11-04T02:45:19.752Z'
modified: '2022-01-12T08:05:43.470Z'
---

# Lec9
现在我们需要开发一个完整的React程序了，比如一个披萨店，用户可以点菜。
首先，我们需要搞到一个pizza的api（当然，我觉得这个自己写更好，但是这毕竟是作业嘛。。）来呈现pizza
然后，我们用bootstrap来让界面更好看
我们还需要加一些功能

## 导入资源
我们需要先在index.js里导入自己需要的包，比如说bootstrap
```JavaScript
import "bootstrap/dist/css/bootstrap.min.css";
```

## 获取API
现在我们需要从网站上获取pizza的API。API中如果包含了多个JSON，那么我们就需要声明多个数组来储存JSON
```JavaScript
constructor(props){
  super(props);
  this.state={
    apiData1: [],
    apiData2: [],
    apiData3: [],
    isFetched: false,
    errorMsg: null
  }
}
```
好了，这玩意不用网络代理，所以直接展示就行了（傻逼czm傻逼czy

## 用下拉列表选择pizza
用户一次只能选一个pizza，不然他会吃不下浪费掉（是否没有考虑过肥宅），所以下拉列表是合适的：
```HTML
<form action="/action.page.php">
  <label for="pizzas">Choose a pizza</label>
  <select name="pizzas" id="pizzas">
    <option value="就是你map的东西啦！">还是你map的东西啦！</option>
  </select>
</form>
```
实际上，key设置成索引是很合适的，方便之后调用findIndex方法找东西
然后还要加一个事件监听上来，就是OnChange

## 点单



