---
attachments: [Clipboard_2021-07-14-23-39-02.png, Clipboard_2021-07-14-23-52-07.png, Clipboard_2021-07-14-23-55-22.png, Clipboard_2021-07-14-23-56-53.png, Clipboard_2021-07-14-23-58-01.png, Clipboard_2021-07-15-00-01-04.png, Clipboard_2021-07-15-00-02-39.png, Clipboard_2021-07-15-00-03-06.png, Clipboard_2021-07-15-00-04-37.png]
tags: [Shader/Unity Shader]
title: §1-3：MVP矩阵变换
created: '2021-07-14T13:17:56.003Z'
modified: '2021-09-27T02:15:24.423Z'
---

# §1-3：MVP矩阵变换
<center><img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210924154412.png" width=800px/>
<p>从观察者的观察空间投影到屏幕空间</p></center>

&emsp;&emsp;什么是MVP矩阵？MVP矩阵的三个字母指的是模型矩阵(Model)、观察矩阵(View)和投影矩阵(Projection)。回想一个模型从导出到最终呈现在屏幕上的过程，我们发现模型的顶点的坐标经历了这么些事：
- 在建模软件中，模型顶点的坐标是以模型中心为原点计算的
- 在把模型拖入场景之后，模型顶点的坐标是以世界中心为原点计算的
- 从摄像机出发观察模型
- 将模型投影到摄像机上，进行渲染，最终呈现在玩家眼前

&emsp;&emsp;实际上，这个变换就是使用了MVP矩阵。从模型空间的顶点，我们最终得到了投影在相机上的结果。再想想写shader的时候，顶点着色器往往包含以下语句：
```ShaderLab
v2f o;                                  // 传输到片元着色器
o.pos = UnityObjectToClipPos(v.vertex); // 将顶点坐标从模型空间(Object)转换到裁剪空间(Clip)
```
&emsp;&emsp;我们知道，顶点着色器最基本的功能就是将顶点坐标从模型空间转换到裁剪空间。这里的UnityObjectToClipPos函数的作用即将顶点坐标乘上MVP矩阵进行变化，实现了一个物体从自己的模型空间(也成为局部空间/Local Space)经过模型矩阵(M)，转换到世界空间(World Space)；再经过观察矩阵(V)，进入到摄像机所观察到的视觉空间(View Space)；最后通过投影矩阵转换到裁剪空间(Clip Space)的过程。

## 模型空间--->世界空间
&emsp;&emsp;模型空间就是在建模的时候以模型自身中心为中心的坐标系；世界空间指的是以世界中心为中心的坐标系。
&emsp;&emsp;感觉好像对模型空间很生疏？实际上，当一个物体有子对象的时候，父物体的坐标是世界空间下的坐标，而子物体的坐标其实就是模型空间下的。举个例子，现在我们的地图中有一个电脑作为父物体，而显示器、键盘都是子物体。当我们聚焦整个父物体时，我们发现它的Transform组件下的坐标就是世界空间下的坐标:
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926155901.png" width=200/><img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926160002.png" width=310/>
&emsp;&emsp;而当我们聚焦电脑下的组件（比如键盘）时，Transform组件显示的坐标就是整个电脑模型空间下的坐标：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926160252.png" width=200/><img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926160322.png" width=310/>

&emsp;&emsp;再比如说，我们在处理模型的时候经常会遇到上(Vector3.up)、左(Vector3.left)、前(Vector3.forward)等方向性的概念，而萌新使用这些概念的时候往往无法得到正确的结果，比如我们希望这个模型通过Vector3.up明确方向实现向上移动，但是它怎么朝着莫名其妙的方向动了呢？这是因为搞混了模型空间和世界空间。Unity脚本API中对Vector3.up的描述是：用于编写 Vector3(0, 1, 0)的简便方法。然而这里的(0, 1, 0)是对物体的模型空间而言的，而不是世界空间。举个例子：


// 请在这里插入手绘示意图


&emsp;&emsp;这就是出错的原因了。我们提到的上下左右前后几种方向被称为“自然方向”，由于Unity使用左手系的缘故，这些方向和坐标轴的对应关系为：
- +x ---> 右/right
- +y ---> 上/up
- +z ---> 前/forward
### 模型矩阵M
&emsp;&emsp;那么，我们该如何将点/向量从模型空间转换到世界空间呢？回想一下刚刚我们提到的父对象的Transform坐标！
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926160002.png" width=310/>
&emsp;&emsp;父对象的Transform的坐标指的就是这个模型原点在世界空间下的坐标，从(0, 0, 0)变成(-2.918, -3.026, 0.091)了呢，还有旋转变换和缩放变换。实际上，我们需要的就是一个矩阵的复合变换来实现这个过程。变换方式如下(顺序不能改)：
- 进行缩放
- 进行旋转（绕各轴旋转的顺序可以打乱，比如说我下面写的是先转z然后y然后x）
- 进行平移
$$M = 
\begin{pmatrix}
   1         & 0        & 0        & t_{x} \\
   0         & 1        & 0        & t_{y} \\
   0         & 0        & 1        & t_{z} \\
   0         & 0        & 0        & 1
\end{pmatrix}  \begin{pmatrix}
   1         & 0              & 0             & 0 \\
   0         & cos\theta_{x}  & -sin\theta_{x}& 0 \\
   0         & sin\theta_{x}  & cos\theta_{x} & 0 \\
   0         & 0              & 0             & 1
\end{pmatrix}  \begin{pmatrix}
   cos\theta_{y}  & 0        & sin\theta_{y}  & 0 \\
   0              & 1        & 0              & 0 \\
   -sin\theta_{y} & 0        & cos\theta_{y}  & 0 \\
   0              & 0        & 0              & 1
\end{pmatrix}  \begin{pmatrix}
   cos\theta_{z}  & -sin\theta_{z}  & 0       & 0 \\
   sin\theta_{z}  & cos\theta_{z}   & 0       & 0 \\
   0              & 0               & 1       & 0 \\
   0              & 0               & 0       & 1
\end{pmatrix}  \begin{pmatrix}
   k_{x}     & 0        & 0        & 0 \\
   0         & k_{y}    & 0        & 0 \\
   0         & 0        & k_{z}    & 0 \\
   0         & 0        & 0        & 1
\end{pmatrix}
$$
代入数据得：
$$M = 
\begin{pmatrix}
   1         & 0        & 0        & -2.918 \\
   0         & 1        & 0        & -3.026 \\
   0         & 0        & 1        & 0.091 \\
   0         & 0        & 0        & 1
\end{pmatrix}  \begin{pmatrix}
   1         & 0              & 0             & 0 \\
   0         & \cos{44.003}  & -\sin{44.003}& 0 \\
   0         & \sin{44.003}  & \cos{44.003} & 0 \\
   0         & 0              & 0             & 1
\end{pmatrix}  \begin{pmatrix}
   \cos{85.543}  & 0        & \sin{85.543}  & 0 \\
   0              & 1        & 0              & 0 \\
   -\sin{85.543} & 0        & \cos{85.543}  & 0 \\
   0              & 0        & 0              & 1
\end{pmatrix}  \begin{pmatrix}
   \cos{-94.927}  & -\sin{-94.927}  & 0       & 0 \\
   \sin{-94.927}  & \cos{-94.927}   & 0       & 0 \\
   0              & 0               & 1       & 0 \\
   0              & 0               & 0       & 1
\end{pmatrix}  \begin{pmatrix}
   1.8     & 0        & 0        & 0 \\
   0         & 1.8    & 0        & 0 \\
   0         & 0        & 1.8    & 0 \\
   0         & 0        & 0        & 1
\end{pmatrix}
$$

&emsp;&emsp;具体结果是什么我就不算了，这庞大的计算量。。。总之，在计算的最后我们会得到一个4*4矩阵，这就是针对该模型的模型矩阵 $M$ 。利用这个矩阵，我们现在可以求得子物体Transform坐标（模型空间坐标）的世界空间坐标了！
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926160322.png" width=310/>
&emsp;&emsp;将键盘的模型坐标$\begin{pmatrix}-0.005815\\0.0167358\\0.1960645\\1\end{pmatrix}$乘上刚刚算好的模型矩阵M，我们就能得到就能得到键盘的世界坐标。

## 世界空间--->观察空间
&emsp;&emsp;观察空间就是以摄像机为中心的坐标系。我们可以认为观察空间也是模型空间的一种，**想象一下所有被观察的物体都是相机的子物体**，那么我们要做的就是某种意义上的“从世界坐标重新转换成模型坐标”的过程。这不是恰好和刚刚进行的“从模型空间坐标转换成世界空间坐标”的操作正好反过来了吗？
&emsp;&emsp;没错！从模型空间到世界空间，$M$ 矩阵的操作是先缩放、旋转，最后平移。那么反过来的过程就应该是**先平移再旋转**。由于缩放对相机视野并不产生影响，所以我们不考虑缩放的选项。不过到这里还没有结束，我们还**需要对z轴进行逆变换，因为观察空间是右手系**（也是几个坐标空间中唯一的右手系），而世界空间是左手系。为什么呢？因为我们希望相机的前方是z轴的负方向，这样一来观察空间中每个能够进入摄像机视野的物体的z值都是正数，并且表示了该物体和摄像机的距离。
&emsp;&emsp;为什么非要搞这一出呢？在接下来的学习中我们会接触到**深度值**的概念。在对每个物体进行渲染之前，我们会将物体片元的深度值写入深度缓冲中（关闭深度写入另谈）。当同一个位置上出现多个片元时，我们会只渲染深度值最小的那个片元。当z值（深度）都是正值时，我们能够对数据更直观的分析。
> **另一种思考模式**
&emsp;&emsp;我们可以想象将摄像机移动到世界中心，再将相机的坐标轴与世界空间的坐标轴对齐。这样处理之后得到的观察矩阵和前文提到的相同。
&emsp;&emsp;相机从世界中心原点移动到它所在的位置，是先平移，再旋转的。那么移动回来，就需要先旋转，再平移。

### 观察矩阵V
&emsp;&emsp;以这个摄像机为例：
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926171746.png" width=190/><img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20210926171828.png" width=441/>
$$V = 
\begin{pmatrix}
   1         & 0      & 0     & 0 \\
   0         & 1      & 0     & 0 \\
   0         & 0      & -1    & 0 \\
   0         & 0      & 0     & 1
\end{pmatrix}
\begin{pmatrix}
   1         & 0              & 0             & 0 \\
   0         & cos\theta_{x}  & -sin\theta_{x}& 0 \\
   0         & sin\theta_{x}  & cos\theta_{x} & 0 \\
   0         & 0              & 0             & 1
\end{pmatrix}  \begin{pmatrix}
   cos\theta_{y}  & 0        & sin\theta_{y}  & 0 \\
   0              & 1        & 0              & 0 \\
   -sin\theta_{y} & 0        & cos\theta_{y}  & 0 \\
   0              & 0        & 0              & 1
\end{pmatrix}  \begin{pmatrix}
   cos\theta_{z}  & -sin\theta_{z}  & 0       & 0 \\
   sin\theta_{z}  & cos\theta_{z}   & 0       & 0 \\
   0              & 0               & 1       & 0 \\
   0              & 0               & 0       & 1
\end{pmatrix}  \begin{pmatrix}
   1         & 0        & 0        & t_{x} \\
   0         & 1        & 0        & t_{y} \\
   0         & 0        & 1        & t_{z} \\
   0         & 0        & 0        & 1
\end{pmatrix}
$$
代入数据得:
$$V = 
\begin{pmatrix}
   1         & 0      & 0     & 0 \\
   0         & 1      & 0     & 0 \\
   0         & 0      & -1    & 0 \\
   0         & 0      & 0     & 1
\end{pmatrix}
\begin{pmatrix}
   1         & 0              & 0             & 0 \\
   0         & \cos{-29.049}  & -\sin{-29.049}& 0 \\
   0         & \sin{-29.049}  & \cos{-29.049} & 0 \\
   0         & 0              & 0             & 1
\end{pmatrix}  \begin{pmatrix}
   \cos{-24.138}  & 0        & \sin{-24.138}  & 0 \\
   0              & 1        & 0              & 0 \\
   -\sin{-24.138} & 0        & \cos{-24.138}  & 0 \\
   0              & 0        & 0              & 1
\end{pmatrix}  \begin{pmatrix}
   \cos{23.288}  & -\sin{23.288}  & 0       & 0 \\
   \sin{23.288}  & \cos{23.288}   & 0       & 0 \\
   0              & 0               & 1       & 0 \\
   0              & 0               & 0       & 1
\end{pmatrix}  \begin{pmatrix}
   1         & 0        & 0        & 5.24 \\
   0         & 1        & 0        & -4.19 \\
   0         & 0        & 1        & -8.29 \\
   0         & 0        & 0        & 1
\end{pmatrix}
$$

&emsp;&emsp;同样，由于计算太过繁琐，我用 $V$ 来指代运算的结果，这是个4*4的矩阵。将物体的世界坐标乘以 $V$ 就可以得到观察空间下的坐标。

## 观察空间--->裁剪空间
这一步并不是真正的投影(三维的游戏物体投影到屏幕的二维坐标)，只是为投影做准备。裁剪空间的目的是判断顶点是否在可见范围内
变换步骤：
- 对x、y、z分量进行缩放，用w分量作为范围值
- 如果x、y、z分量都在w范围内，则该点位于裁剪空间内

#### 透视投影
透视投影常用于3D游戏，其特点是实现近大远小的效果。其P矩阵如下：
![](@attachment/Clipboard_2021-07-14-23-52-07.png)
这就是一个恒定的公式，推导过程挺复杂的(我相信我没有这个兴趣)
> 公式中的符号解析
>- **Near**
摄像机的近裁平面
>- **Far**
摄像机的远裁平面
>- **FOV**
摄像机的可视范围
![](@attachment/Clipboard_2021-07-14-23-55-22.png)
>- **近裁平面高度、远裁平面高度**
其实就是简单的数学计算
![](@attachment/Clipboard_2021-07-14-23-56-53.png)
>- **Ascept**
是一个比值，近裁平面宽比上近裁平面高或者远裁平面宽比上远裁平面高
![](@attachment/Clipboard_2021-07-14-23-58-01.png)

之后，当三个分量的范围都在w范围内则该点位于裁剪空间内
![](@attachment/Clipboard_2021-07-15-00-01-04.png)

#### 正交投影
正交投影常用于2D游戏，因为不论远近同一个物体看起来都是一样大的。其P矩阵如下：
![](@attachment/Clipboard_2021-07-15-00-04-37.png)

> 公式中的符号解析
>- **Near**
摄像机的近裁平面
>- **Far**
摄像机的远裁平面
>- **Size**
摄像机的可视范围
![](@attachment/Clipboard_2021-07-15-00-02-39.png)
>- **近裁平面高度、远裁平面高度**
其实就是简单的数学计算
![](@attachment/Clipboard_2021-07-15-00-03-06.png)
>- **Ascept**
是一个比值，近裁平面宽比上近裁平面高或者远裁平面宽比上远裁平面高
![](@attachment/Clipboard_2021-07-14-23-58-01.png)

之后，当三个分量的范围都在w范围内则该点位于裁剪空间内
![](@attachment/Clipboard_2021-07-15-00-01-04.png)






