---
tags: [CS423]
title: 'Week5-1: Blender的纹理技术'
created: '2021-10-11T05:55:40.299Z'
modified: '2021-10-11T06:09:06.532Z'
---

# Week5-1: Blender的纹理技术
当决定对一个物体使用纹理，前置的准备需求有两个：
1. 一张纹理贴图
2. 需要使用纹理的物体（模型）
## 基础纹理（单张/图片纹理）
在新建物体之后，创建材质球，随后在材质球选项中的baseColor处点击小圆点选择Image Texture（图片纹理,下图）进行纹理映射
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20211011140114.png" width = 400/>

这时候可能会发现什么也看不到，这是因为视口模式选择的问题，需要切换到渲染模式才可以正常的显示纹理（通常模式下显示的是白模）。

### UV编辑
在上方工具栏有UV编辑的选项，打开它！
接下来，选中我们需要修改的面，在纹理贴图上将会显示它影响的范围。通过更改顶点在贴图上对应的位置，我们可以得到不同的映射结果：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20211011140639.png" width = 400/>
