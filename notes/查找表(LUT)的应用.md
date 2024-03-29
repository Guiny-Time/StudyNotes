---
tags: ['TA/demo实现']
title: 查找表(LUT)的应用
created: '2022-03-08'
categories: 探索发现
cover: https://s2.loli.net/2022/03/09/lNMtBhpDKQ82LS5.png
---

# 颜色分级
&emsp;&emsp;当我们拿到了一个场景，而我们想要改变它的颜色该怎么办？比如说现在我们有一个暖色调的魔女小屋，但我们希望能够实现一个更阴暗一点的风格。想要置换场景中所有模型的贴图、改变光照显然是不合适的，它的工作量太过庞大，并且如果还想更改色调的话我们又需要重新绘制一套纹理，这显然是划不来的。

<img src="https://s2.loli.net/2022/03/09/POpL6rgFTSAmeWb.png" width=500/><img src="https://s2.loli.net/2022/03/09/vzjg9fp637tT5M8.png" width=500/>

&emsp;&emsp;我们其实很容易的能够联想到一种与之关联的屏幕后处理（Post-Processing）效果，叫做颜色分级（Color Grading）。颜色分级是一个功能非常强大的后处理效果，我们可以利用该效果给屏幕加上各种“滤镜”，得到想要的效果。这个组件比较复杂，在本文中不过多展开，详情可见官方文档：https://docs.unity3d.com/2018.3/Documentation/Manual/PostProcessing-ColorGrading.html

# LUT
&emsp;&emsp;有了这么强大的工具游戏开发者是不是就可以高枕无忧了呢？答案是：不可以！颜色分级虽然强大，但相对的它的开销也是很大的，在移动平台如果依赖颜色分级实现某些效果的话会出现手机发烫、掉电过快等问题，造成不好的用户体验。因此，为了模拟颜色分级的效果，我们需要使用查找表（Lookup Table），简称LUT。在颜色分级诞生之前，开发者基本都是用Custom LUT实现色彩改变的效果的。即使到了今天，在许多注重开销的平台上（例如视频渲染、手机游戏）也是利用Custom LUT来进行优化的。
&emsp;&emsp;在旧版的Unity的Post-Processing Volum中还保存有User LUT的选项（后来被颜色分级所取代），它的面板长这样：

<img src="https://s2.loli.net/2022/03/09/PQhf725DbIHoXWp.png"/>

&emsp;&emsp;可见，输入需要一张用户定义的LUT图，并可以控制其影响程度。那么，LUT图长什么样呢？随便从网上搜一下我们就能得到结果：

<img src="https://s2.loli.net/2022/03/09/lNMtBhpDKQ82LS5.png" width=300/>

&emsp;&emsp;其中左上角和右下角并不陌生，他们分别是第三分量为0和第三分量为1时一组纹理坐标输出在表面的结果。所以易得，查找表其实是以纹理的左上角为原点，向下为正方向对第三分量创建一根数轴得到的结果，单位长度为1（一个格子的长度）。每行的格子其实完全相同，他们对应的第三个分量也是相同的。查找表将索引号与输出值创建联系，上图的LUT（或者称之为**颜色表**）用来确定特定图像中每一像素所要显示的颜色和强度，计算效率很高。那么我们又如何利用这个标准LUT得到自己想要的输出结果呢？

# 制作User LUT
&emsp;&emsp;当我们用绘图软件（例如Photoshop）调试出想要的效果的时候，我们会使用到调整图层。使用调整图层的好处是我们可以随时修改效果的参数，并且可以很方便的添加和删除滤镜而不用担心造成不可逆的后果。还记得文章开头提到的两张风格迥然不同的魔女小屋吗？在Photoshop中，我使用了以下调整图层：

<img src="https://s2.loli.net/2022/03/09/2yHkFUAXsDqBIYf.png"/>

&emsp;&emsp;接着，我们导入标准LUT，放到调整图层之下。隐藏掉魔女小屋的图片之后，我们就会得到经过调整图层处理过的LUT图：

<img src="https://s2.loli.net/2022/03/09/jCDoZ5harAvfiOR.png" width=300/>

&emsp;&emsp;原本输出在屏幕上的颜色经过索引号在查找表上查询到的颜色然后输出。但现在索引号没有变化、查找表本身改变了（被调整图层影响了），因此输出的颜色也就是被影响过的颜色了。当所有颜色经过新的查找表输出，我们就得到了Photoshop里的处理过后的效果。
