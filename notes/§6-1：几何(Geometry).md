---
attachments: [Ch16꞉ 概率模型.md]
tags: [TA/Unity Shader]
title: §6-1：几何(Geometry)
created: '2022-10-15T09:30:54.419Z'
modified: '2022-10-18T03:52:12.323Z'
---

# §6-1：几何(Geometry)
# 隐式几何(Implicit Geometry)
隐式几何给出一或多组关系式，满足该关系式(组)的点在该几何体上。例如空间中有一个半径为1，中心在原点的球，它的表达式为:
$$x^2+y^2+z^2 = 1$$
这个表达式就是球的隐式几何表达的一种，可以画出实际形状（球）。
- 隐式几何提供精确的表达式，在数学上精确的描述了对象，不存在采样错误的问题
- 隐式几何但较为抽象，无法直观的推断出几何形状，**难以描述复杂几何**(例如画一头牛)
- 隐式几何能快速判断点/线/面与当前几何的关系
- 适合光线求焦、处理拓扑结构等应用

## 其他隐式几何的表达方式
在数学公式之外还有别的隐式方法能表示一个几何体:
1. **构造实体几何**
这是一种很有趣的表示几何的方式，它的思维是将几种基础几何形体（例如圆柱、圆锥、球、立方体等）进行交并补等操作得到一个新的几何体。下图的几何体，如果用 $\textcolor{Red}A$ 表示红色立方体，$\textcolor{Blue}B$ 表示蓝色球，$\textcolor{LimeGreen}{C1, C2, C3}$ 表示绿色圆柱，则有$(A\cap B)/((C1\cup C2)\cup C3)$
<img src="https://tva2.sinaimg.cn/large/006UcwnJly1h766f0iqxxj30m80jp0x0.jpg" alt="image" width="400" data-width="800" data-height="709">

2. **距离函数**
空间中任意一个点到几何体都有一个最近的**有向距离(Signed Distance)**，这是可以用公式表示的（作垂线求值等等）。距离是有方向的，例如点在几何体外时为正距离，在几何体内时则为负距离，**有向距离场(Signed Distance Field)**，的概念应运而生。Unity中的TMP(TextMesh Pro)资产、溶球效果、许多文字特效都是基于SDF的概念实现的。
<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h76bih1fmwj30sg0sgabq.jpg" alt="字母A的SDF表示" width="400" data-width="1024" data-height="1024">

3. **分形**
分形几何是一门学问..自然界中有许多天然存在的分形，比如说雪花、螺壳的纹路等等。
<img src="https://tva1.sinaimg.cn/large/006UcwnJly1h76c3yim51j32z01o77wl.jpg" alt="螺壳。2021年6月Bing壁纸" width="852" data-width="3852" data-height="2167">

# 显式几何(Explicit Geometry)
显式几何有两种形式：
1. 画出每个点
2. 参数映射

画出每个点，即在坐标系中画出表达式的形状。比如前面例举的球可以在几何画板中得到以下结果，这就是该球的显式几何：
<img src="https://tva1.sinaimg.cn/large/006UcwnJly1h763yy6h6uj30ev08smym.jpg" alt="image" width="400" data-width="535" data-height="316">

而还有另一种方法，即**参数映射(param mapping)**，定义一个二维平面，上面有一些点，通过一个或多个式子将点映射到三维空间中来.虽然映射公式是以一个“公式”的形式出现的，但由于经过公式处理会得到一个具体的几何体，因此它是显式的。
<img src="https://tva4.sinaimg.cn/large/006UcwnJly1h7660vf5jmj30x10et42u.jpg" alt="image" width="589" data-width="1189" data-height="533">
- 显式几何能轻松的表现几何体的形象
- 显式几何也可以有精确的数学表示（参数映射公式）
- 点线面关系判断变得复杂

有什么表示显式几何的方法呢？
1. **点云**
点云是一种非常直观的表示几何形体的方式。如果在一个二维平面上我们可以把曲线上所有的点都画出来（显然这不可能），我们就能得到这条曲线。当然，连续桦甸市不切实际的，因此这实际上是一种离散的行为。当把这种行为上升到三维时，点云的概念就诞生了。我们不考虑面为面，而是将面上的无数多个点画出来，当点足够密集时也能取得还不错的效果，下图是较稀疏的点云。
理论上点云可以表示任何几何体，只要点云足够密集即可。但这种方法的**开销过大**，需要存储海量的点，点少了**效果又不好**。所以一般获取点云数据之后会将其转化成多边形面（通常没有拓扑结构）。
<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h76cido3ggj30ze0owqgh.jpg" alt="稀疏的点云，并不能完美的表示几何体，通常是扫描得到的初始结果" width="574" data-width="1274" data-height="896">

2. **多边形面**
**最常用**的表示几何的方式没有之一。之所以叫多边形面是因为有**三角形面**(Triangle Mesh)的表示方法也有**四边形面**的表示方法，所以就叫Polygon了。多边形面**非常容易处理**，**数据结构也更为复杂**一些(需要记录点的连线关系、位置等等)。
在.obj类型的文件里，文件定义了模型的所有**顶点**(v)、**顶点法线**(vn)和**顶点纹理坐标**(vt)。在描述一个**面**(f)时，用三个顶点、对应的法线和纹理坐标即可
```obj
# 这个面由第5/1/4个顶点、第1/2/3个纹理坐标、第1个法线组成
f 5/1/1 1/2/1 4/3/1
```

接下来介绍一些显式几何及其表述方式。

## 贝塞尔曲线(de Casteljau算法)
贝塞尔曲线是一种参数表示的显式几何。它应用于很多场合，例如矢量字体、PhotoShop中的钢笔、动画的智能补间/运动轨迹等等。德卡斯特里奥算法是实现贝塞尔曲线的算法之一，他是怎么实现的呢？
给定三个点 $b_0,b_1,b_2$ 就可以画出一条贝塞尔曲线了，这种贝塞尔曲线叫**二次贝塞尔曲线**。假设我们现在要寻找贝塞尔曲线上一点 $t$ 在什么位置：
1. 将贝塞尔曲线“拉直”，画出 $t$ 在曲线上的位置，然后在 $b_0 b_1$ 和 $b_1 b_2$ 两条线段找到 $t$ 的相对位置。其中 $b_0^1 = (1-t)b_0 + tb_1$，$b_1^1 = (1-t)b_1 + tb_2$
<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h76y9i7wd1j30kt0e6dgg.jpg" alt="image" width="349" data-width="749" data-height="510">

2. 链接新得到的两个点 $b_0^1,b_1^1$ 得到新的线段，在上面再找到 $t$ 的相对位置。此时只剩下一个点了，无法再找到其他点与之连线，这就是 $t$ 在贝塞尔曲线上的位置。其中 $b_0^2 = (1-t)b_0^1 + tb_1^1$
<img src="https://tvax2.sinaimg.cn/large/006UcwnJly1h76ydk7xqpj30lv0fbgmd.jpg" alt="image" width="387" data-width="787" data-height="551">

3. 寻找其他离散点的位置，最后连线就可以得到二次贝塞尔曲线
<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h76yf3fy2oj30lr0fd0to.jpg" alt="image" width="383" data-width="783" data-height="553">

对于更多的点，操作方法是类似的迭代，直到仅剩一个点时停止。
<img src="https://tvax2.sinaimg.cn/large/006UcwnJly1h76yhw4j4cj30xu0hcdif.jpg" alt="image" width="418" data-width="1218" data-height="624">

贝塞尔曲线有以下几个**特性**：
- 一定过第一个和最后一个检查点
- 一组检查点经过仿射变换后，绘制出的贝塞尔曲线与变换前一致
- **凸包性质**：贝塞尔曲线一定在控制点形成的凸包(Convec Hull)内
<img src="https://tva4.sinaimg.cn/large/006UcwnJly1h7742hcmayj30ae04wwes.jpg" alt="image" width="300" data-width="374" data-height="176">

### 伯恩斯坦多项式(Bernstein Polynomial)
如果**用代数的方法**表示二次贝塞尔曲线呢？
- 第一层：$b_0^1 = (1-t)b_0 + tb_1$，$b_1^1 = (1-t)b_1 + tb_2$，$b_2^1 = (1-t)b_2 + tb_3$
- 第二层：$b_0^2 = (1-t)b_0^1 + tb_1^1$，$b_1^2 = (1-t)b_1^1 + tb_2^1$
即$b_0^2 = (1-t)((1-t)b_0 + tb_1) + (1-t)b_1 + tb_2$
$b_0^2 = (1-t)^2b_0 + 2t(1-t)b_1 + tb_2$，这个形式特别像 $(1+(1-t))^2$ 的完全平方展开。

是的，贝塞尔曲线上的点与检查点之间存在有趣的多项式关系:
$$b^{n}(t) = b_0^{n}(t) = \sum_{j=0}^{n}b_j B_j^{n}(t)$$
$b_0^{n}(t)$ 表示贝塞尔曲线上的第 $n$ 个点，$b_j$ 表示第 $j$ 个控制点，$B_j^{n}(t)$ 即伯恩斯坦多项式。伯恩斯坦多项式描述了一个**二项分布** [概率模型](@note/Ch16: 概率模型.md)：
$$B_j^{n}(t) = \begin{pmatrix} n \\ i \end{pmatrix} t^i (1-t)^{n-i}$$

## 逐段贝塞尔曲线(Piecewise Bezier Curves)
当有很多检查点的时候，贝塞尔曲线的效果可能不是那么明显（或者说，长得和直线一样，难以描述复杂形状）。因此可以给检查点分组，分别表示多段贝塞尔曲线，然后再把它们连起来。有逐段贝塞尔曲线后就可以表示复杂的曲线了，例如字体、钢笔工具、Adobe Illustrator等等。
<img src="https://tva2.sinaimg.cn/large/006UcwnJly1h774d9412vj30b408ctad.jpg" alt="image" width="350" data-width="400" data-height="300">

上面这张图是用逐段贝塞尔曲线表示字体的一个例子。我们可以在图上看到许多“**控制杆**”，两个杆子之间代表一段独立的贝塞尔曲线。与曲线相连的两个点分别为起止点，通常杆子朝着这段曲线方向的点表示曲线中间的两个点（如果不确定的话试一下就知道了），**一段曲线有四个控制点**。调整控制杆即可调整中间第2或第3个点的位置，实现控制曲线。为什么控制杆是一段直线呢？

<img src="https://tva2.sinaimg.cn/large/006UcwnJly1h774vwyd6dj309z07vdfv.jpg" alt="image" width="159" data-width="359" data-height="283">

逐段贝塞尔曲线第一个点的切线方向和第1、2个点连线的方向是一样的，最后一个点的切线方向和第3、4个点连线（半段控制杆）的方向是一样的。如果要保证逐段贝塞尔曲线连接起来是平滑的，那么在连接点上，两端曲线在该点的切线方向应该一致，而保证一致的做法即前一段曲线的第3、4个点和后一段曲线的第1、2个点在同一直线上。由于前一段曲线的第4个点和后一段曲线的第1个点是重合的，所以形成了共线的“控制杆”。

<img src="https://tva4.sinaimg.cn/large/006UcwnJly1h77506633lj304g05tq2t.jpg" alt="image" width="120" data-width="160" data-height="209">

第一段的终止点和第二段的起点相连的这种情况叫做 $C^0$ 连续。当第一段的第3、4个控制点和第二段的第1、2个控制点之间的距离是一样的时候叫做 $C^1$ 连续。
逐段贝塞尔曲线可以画出复杂的曲线，但它也有“**牵一发而动全身**”的问题。为了避免一点小改动影响整根曲线，接下来介绍的是：

## B-样条
B-样条是奇函数样条的缩写。样条指的是用一系列点规范出一条曲线的方式，听起来和贝塞尔曲线非常像，但相比于贝塞尔曲线**需要更多的点信息**，单一控制点影响的范围非常有限。
好吧...games101没有聊到这个。在接下来清华图形学的学习中会补充这边。

# 曲面(Surface)
## 贝塞尔曲面
贝塞尔曲面由4 * 4一共16个控制点组成，表现为一个3 * 3的网格：

<img src="https://tva1.sinaimg.cn/large/006UcwnJly1h782pobli6j30ea0es41u.jpg" alt="image" width="314" data-width="514" data-height="532">

获取贝塞尔平面的方式为：
1. 针对网格的u方向，取每一条边（一共四条），画出其贝塞尔曲线
2. 画出v方向相同时的边贝塞尔曲线上的点，画出它们的贝塞尔曲线
3. 这条线扫的过程就会得到贝塞尔曲面

<img src="https://tva1.sinaimg.cn/large/006UcwnJly1h782xfavqfj30qz0j241k.jpg" alt="第一步" width="300" data-width="971" data-height="686" style="display:inline"><img src="https://tvax3.sinaimg.cn/large/006UcwnJly1h782ydz35vj30qs0j7wgx.jpg" alt="第二步" width="300" data-width="964" data-height="691" style="display:inline"><img src="https://tva3.sinaimg.cn/large/006UcwnJly1h782z4skdlj30s90jdtdt.jpg" alt="第三步" width="300" data-width="1017" data-height="697" style="display:inline">

## 曲面细分(Mesh Subdivision)
有一类可编程的着色器专门处理细分网格的功能，这就是曲面细分着色器，不过它是一种可选可不选的着色器，不像顶点着色器或片元着色器那么重要。曲面细分有什么实现的方法呢？

## Loop细分

<img src="https://tva1.sinaimg.cn/large/006UcwnJly1h78edfy9cuj308f082t9u.jpg" alt="image" width="303" data-width="303" data-height="290">

{% note info simple %}
**请注意**
**Loop是发明该方法的人的姓，并不代表该细分方法与循环有关。**
{% endnote %}

Loop细分是实现曲面细分的最基本的方法之一。对于任意一个三角形，可以取三边的中点连线，在内部得到一个新的三角形
<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h78cqo6gpqj30jt06xwf0.jpg" alt="image" width="413" data-width="713" data-height="249">

三边中点我们可以划归成“**新的一组顶点**”，之前的三个顶点则是“**旧的一组顶点**”。其中，当两个三角形相接时，一定会有一个中点在两个三角形的共边上：

<img src="https://tva1.sinaimg.cn/large/006UcwnJly1h78cxm4tetj309c08q3yx.jpg" alt="image" width="236" data-width="336" data-height="314">

共边中点的新位置即共边顶点的 $\frac{3}{8}$ 加上非共边顶点的 $\frac{1}{8}$，是一个加权平均的操作。诶那你可能会问剩下两个中点怎么办？那简单呀我们再找和它共边的三角形，然后再对其加权平均就可以了。
处理完“新的顶点”，还剩下“旧的顶点”，该怎么处理旧顶点呢？

<img src="https://tvax3.sinaimg.cn/large/006UcwnJly1h78duqjl03j30rs090mzi.jpg" alt="image" width="800" data-width="1000" data-height="324">

上图右侧的白点表示一个旧的顶点。找到所有与其相连的顶点，定义顶点的度 $n$ (drgree，就是和顶点相连的点的数量，例如上图 $n=6$ )和与度有关系的数 $u$ ，可以对旧顶点原本的位置和相邻旧顶点的位置进行加权平均。

## Catmull-Clark细分

<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h78eaaiufuj30j905w40i.jpg" alt="image" width="693" data-width="693" data-height="212">

Loop细分只能用于三角形面的细分，但有的时候mesh比较不清楚，又有三角形又有四边形，这时候Catmull-Clark细分就能比较好的处理网格。定义两个新概念：
1. 奇异点：一个顶点的$n \ne 4$
2. 非四边面：一个面不是四边形的，可以是三角形也可以是其他多边形

<img src="https://tvax4.sinaimg.cn/large/006UcwnJly1h78el5jg6hj30vr0mpzns.jpg" alt="image" width="443" data-width="1143" data-height="817">

Catmull-Clark细分的连线方式是将面中心的点与面各边的中点相连。对于四边形面，中心的点 $f$ 遵守 $f = \frac{v_1 + v_2 + v_3 + v_4}{4}$ ，即周围四个点的平均。边中点 $e$ 则遵守 $e = \frac{v_1 + v_2 + f_1 + f_2}{4}$ ，即边顶点与中心的点的平均。边顶点 $v$ 遵守 $v = \frac{f_1 + f_2 + f_3 + f_4 + 2(e_1 + e_2 + e_3 + e_4) + 4v}{16}$ 的加权平均。下面三张图反应了位置关系，需要注意的是第三张图的 $m$ 替换成 $e$，&p& 替换成 $v$。

<img src="https://tvax1.sinaimg.cn/large/006UcwnJly1h799g54trzj309l09qmxe.jpg" alt="image" width="145" data-width="345" data-height="350" style="display:inline"><img src="https://tvax3.sinaimg.cn/large/006UcwnJly1h799gvnt9vj30bk09oglz.jpg" alt="image" width="175" data-width="416" data-height="348" style="display:inline"><img src="https://tva2.sinaimg.cn/large/006UcwnJly1h799hsuj95j30ab0an3z8.jpg" alt="image" width="171" data-width="371" data-height="383" style="display:inline">

## 曲面简化(Mesh Simplification)
我在Sketchfab上找到了一个很nice的模型：
<div class="sketchfab-embed-wrapper"> <iframe title="Fox & Robin" frameborder="0" allowfullscreen mozallowfullscreen="true" webkitallowfullscreen="true" allow="autoplay; fullscreen; xr-spatial-tracking" xr-spatial-tracking execution-while-out-of-viewport execution-while-not-rendered web-share src="https://sketchfab.com/models/17c9108a19884b7c9a5f0e65130b4af1/embed"> </iframe> <p style="font-size: 13px; font-weight: normal; margin: 5px; color: #4A4A4A;"> <a href="https://sketchfab.com/3d-models/fox-robin-17c9108a19884b7c9a5f0e65130b4af1?utm_medium=embed&utm_campaign=share-popup&utm_content=17c9108a19884b7c9a5f0e65130b4af1" target="_blank" style="font-weight: bold; color: #1CAAD9;"> Fox & Robin </a> by <a href="https://sketchfab.com/Mayleemouse.?utm_medium=embed&utm_campaign=share-popup&utm_content=17c9108a19884b7c9a5f0e65130b4af1" target="_blank" style="font-weight: bold; color: #1CAAD9;"> Mayleemouse. </a> on <a href="https://sketchfab.com?utm_medium=embed&utm_campaign=share-popup&utm_content=17c9108a19884b7c9a5f0e65130b4af1" target="_blank" style="font-weight: bold; color: #1CAAD9;">Sketchfab</a></p></div>

但是我的游戏是low poly风格的，这个狐狸的面数太高了，直接放到游戏里很违和。我希望狐狸应该是这个样子的：
<div class="sketchfab-embed-wrapper"> <iframe title="Low poly fox by PixelMannen (Animated)" frameborder="0" allowfullscreen mozallowfullscreen="true" webkitallowfullscreen="true" allow="autoplay; fullscreen; xr-spatial-tracking" xr-spatial-tracking execution-while-out-of-viewport execution-while-not-rendered web-share src="https://sketchfab.com/models/371dea88d7e04a76af5763f2a36866bc/embed"> </iframe> <p style="font-size: 13px; font-weight: normal; margin: 5px; color: #4A4A4A;"> <a href="https://sketchfab.com/3d-models/low-poly-fox-by-pixelmannen-animated-371dea88d7e04a76af5763f2a36866bc?utm_medium=embed&utm_campaign=share-popup&utm_content=371dea88d7e04a76af5763f2a36866bc" target="_blank" style="font-weight: bold; color: #1CAAD9;"> Low poly fox by PixelMannen (Animated) </a> by <a href="https://sketchfab.com/tomkranis?utm_medium=embed&utm_campaign=share-popup&utm_content=371dea88d7e04a76af5763f2a36866bc" target="_blank" style="font-weight: bold; color: #1CAAD9;"> tomkranis </a> on <a href="https://sketchfab.com?utm_medium=embed&utm_campaign=share-popup&utm_content=371dea88d7e04a76af5763f2a36866bc" target="_blank" style="font-weight: bold; color: #1CAAD9;">Sketchfab</a></p></div>

但是新找到的模型头上没有小鸟，而且重复找模型真的很麻烦。有没有办法把一个高面数的模型直接降低面数变成low poly风格呢？或者在LOD的应用里，如何获取模型不同面数等级的子模型来实现LOD来降低性能开销呢？

### 边坍缩(Edge Collapsing)
<img src="https://tvax2.sinaimg.cn/large/006UcwnJly1h79ah3kwfdj327k0p87cn.jpg" alt="image" width="864" data-width="2864" data-height="908">

寻找需要坍缩的边，把边两端的顶点捏在一起形成新的顶点，同时减少了面数的一种方法。判断边是否需要探索的算法叫做**二次误差度量(Quadric Error Metrics)**。二次度量误差指探索完的顶点经过一定的调整，找到一个到周围其他面的垂直距离的平方和最小的位置，平方和即误差值，**值越小表示坍缩该边对mesh的影响最小**。那么寻找需要坍缩的边的做法就是：
1. 计算每条边的二次度量误差并排序
2. 坍缩二次度量误差值最小的边
3. 更新坍缩边所影响的边的二次度量误差值（由于坍缩，周围的边也发生的变化，因此需要重新计算），重新排序

这里的动态取最小的数据结构可以是小顶堆或优先队列。









