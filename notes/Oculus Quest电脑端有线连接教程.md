---
title: Oculus Quest电脑端有线连接教程
created: '2021-10-25T13:40:32.977Z'
modified: '2021-10-25T13:58:30.745Z'
---

# Oculus Quest电脑端有线连接教程
**由于无线连接需要代理路由或代理wifi，所以走有线**
**本篇教程使用github作为图床，使用代理来加载图片**
## 基础步骤
### 安装Oculus电脑客户端
点击群文件的oculus setup就可以直接安装，如下：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20211025214452.png"/>
安装建议使用科学上网，一个好的代理可以在半小时内下完5G多的资源，否则要五个小时。安装好之后桌面上会出现客户端图标：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20211025214715.png"/>
这时候使用之前注册的开发者账户登录会出错，原因是国内墙的dns污染导致无法连接oculus的服务器。解决方式是打开电脑文件目录：
```
C:\WINDOWS\System32\drivers\etc
```
里面有一个hosts文件，以记事本打开它，在末尾加上：
```
157.240.217.51 graph.oculus.com
157.240.195.51 graph.oculus.com
157.240.199.54 graph.oculus.com
```
保存之后，打开cmd输入命令刷新dns：
```
ipconfig/flushdns
```
完成之后重启客户端，这时候就可以登陆了
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/U7%25IF51%7ECX75%7BD%7BE%25E8OQN7.png"/>

### 连接Quest设备
首先，给手柄装上电池，然后拿出Quest自带的usb-C连接线。长得比较奇怪的一边连接设备，另一边连接电脑的usb3.0接口或者usb-C接口
完成之后，打开Oculus客户端，在设备中添加头部设备：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20211025215441.png"/>
选中Quest之后，选择有线，之后等待配对成功。
初次连接需要检查连接：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20211025215641.png"/>
完成之后，连接就成功了。

### 连接steam VR游戏
嗨，我们没有在玩游戏
不过相关链接：https://www.youtube.com/watch?v=wGQDVZkoK7U
