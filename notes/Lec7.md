---
tags: [大三/CS385]
title: Lec7
created: '2021-10-19T01:56:14.124Z'
modified: '2022-01-12T08:05:47.059Z'
---

# Lec7
## 在React中使用BootSrtap
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20211020130327.png" width=400/>

### BootStrap？
BootStrap是一个CSS第三方库，旨在于提供更好、更快、更美观的响应式页面设计，适合手机端开发。
除此之外，还有LayUI、ElemntUI、ant-design等CSS第三方库可供选择。不过BootStrap的响应式设计为一个网页在多个平台上跨越提供了保障。
#### 导入 
```JavaScript
import "bootstrap/dist/css/bootstrap.css"
```
#### 使用
我们主要通过对类的声明来使用bootstrap，例如：
```HTML
<button type="button" class="btn btn-primary btn-lg">这是一个使用了bootstrap样式的按钮</button>
```
对于想要选择的样式，可以去官方文档找找看

## 使用Node.JS开发
这是js的运行环境，包含了写js所需要的一切包（在npm里，一个包管理器）

## api
我们可以通过api来获取其他url上的信息，比如JSON等等
```JavaScript
async componentDidMount() {
    try {
      // 从给定的url上获取信息
      const API_URL = "https://randomuser.me/api/?results=10";
      // 等待响应，储存JSON
      const response = await fetch(API_URL);
      const jsonResult = await response.json();
      // 更新成功信息
      this.setState({ apiData: jsonResult.results });
      this.setState({ isFetched: true });
    } catch (error) {
      // 获取失败的情况
      this.setState({ isFetched: false });
      // 展示错误信息
      this.setState({ errorMsg: error });
    }
  }
```
