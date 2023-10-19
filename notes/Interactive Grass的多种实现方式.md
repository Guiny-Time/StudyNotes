---
tags: [TA/demo实现]
title: Interactive Grass的多种实现方式
created: '2023-09-11T02:52:22.744Z'
modified: '2023-09-14T04:58:07.545Z'
---

# Interactive Grass的多种实现方式
本文主要介绍了几种实现可交互草地的方式，例如基于定点偏移/GPU Instance/粒子系统的方式。
封面：https://fc.sinaimg.cn/large/006UcwnJgy1hhsgnwyqygg30p80e8kjm.jpg

# 摇曳效果
参考：https://www.patreon.com/posts/quick-game-art-13724221
基本原理：利用 **sin函数** 在世界空间的xz平面画圈，同时利用 **step函数** 在模型空间的y轴限制参数_Yoffset控制摇曳的部分和强度。
```
// world position
float3 wpos = mul(unity_ObjectToWorld, v.vertex).xyz;

// movement in xz plane
float x = sin(wpos.x + (_Time.x * _Speed)) *(v.vertex.y - _YOffset) * 5;
float z = sin(wpos.z + (_Time.x * _Speed)) *(v.vertex.y - _YOffset) * 5;

// apply the movement if the vertex's y above the YOffset
v.vertex.x += step( 0, v.vertex.y - _YOffset, 0.5) * x * _SwayMax;
v.vertex.z += step( 0, v.vertex.y - _YOffset, 0.5) * z * _SwayMax;
```
<img src="https://ae02.alicdn.com/kf/S5860623dc5ac4165b511438f3a331fc70.gif" alt="草3.gif" title="草3.gif" width=400 />

# 基于顶点动画的交互
参考：https://www.patreon.com/posts/19844414
基本原理：在模型周围画圈，在该圈内的顶点在xz面朝径外偏移，并通过脚本获取交互物体的顶点列表

```
for(int i = 0; i < _PositionArray; i++)
{
    // 确定影响范围_Radius
    float3 dis = distance(_Positions[i], wpos);
    float3 radius = 1 - saturate(dis /_Radius);

    // 确定扩张方向sphereDisp，与强度系数_MaxWidth关联
    float3 sphereDisp = wpos - _Positions[i];
    sphereDisp *= radius;
    v.vertex.xz += clamp(sphereDisp.xz * step(_YOffset, v.vertex.y), -_MaxWidth,_MaxWidth); // 这里的step也是控制y轴影响范围的
}
```

C#脚本：
```C#
for (int i = 0; i < objects.Length; i++)
{
    positions[i] = objects[i].transform.position;                   
}
Shader.SetGlobalFloat("_PositionArray", objects.Length);
Shader.SetGlobalVectorArray("_Positions", positions);
```
<img src="https://ae01.alicdn.com/kf/S2b427a16037d46e8aadfdc32bae1b5acD.gif" alt="草5.gif" title="草5.gif" width=400 />

该扩张思路同样可以应用于管道效果，比如之前在b站看到的一期视频：
<img src="https://ae01.alicdn.com/kf/S8b4617c25d22478f864d53725e89b2480.png" alt="image.png" title="image.png" width=250/>

# 基于RT+粒子系统的交互
原理和之前做过的可交互水非常类似：https://www.bilibili.com/video/BV1W14y1n7cv
只不过之前这个项目用的是涟漪的法线贴图 + 粒子系统进行遮罩，而在这个项目中用的则是flowmap + 粒子系统遮罩。

## Flowmap
flowmap可以用于水体、木星表面、太阳表面等具有流动性质的效果上。这是一种记录了2D向量信息的纹理，也被称为向量场（vector field），是一种主体为土黄色的图，更为形象的表示类似于下列中间图片中的风场示意图。常用的绘制flowmap的工具为Flowmap Painter。

<img src="https://ae05.alicdn.com/kf/Sff66419b6d294cf0a29e08fde121992dn.png" alt="image.png" title="image.png" width=250 /><img src="https://ae05.alicdn.com/kf/Sd2cfcf9aff0a42aa9262fa50f49b1fb13.png" alt="image.png" title="image.png" width=250 /><img src="https://ae03.alicdn.com/kf/S702595133f4744d6a6771b2775fe8004Z.png" alt="image.png" title="image.png" width=250 />

正因为flowmap只展示了2D向量信息，因此贴图的第三与第四分量的值默认都为0。默认情况下向量记录的为(0.5,0.5)，对应的RGB颜色是(128,128,0)，也就是贴图主体的土黄色。由于zw分量的空缺，可以往flowmap的空通道中加入其他信息（比如一维的粗糙度贴图）来节省空间。

由于向量场的范围为 [-1,1]，而颜色的范围为 [0,1]，因此需要将 flowmap 的 **xy 分量 * 2 - 1**以得到向量的值。完全没有任何偏移的向量为(0,0)，对应 flowmap 中的(0.5,0.5)，因此 flowmap 的主体是黄色的。

<img src="https://ae03.alicdn.com/kf/S2c4ec551919a4092bd1b04ea794728bby.png" alt="image.png" title="image.png" width=350 />

## RT + Flowmap笔刷
那应该怎么把Render Texture（RT）和通常用在流体环境下的flowmap以及粒子效果相结合呢？它们对草地的交互又有什么作用呢？

<img src="https://ae05.alicdn.com/kf/S56b4560e112b4029ac73bae217c6258eo.jpg" width=100 />

在这个例子中，RT 对应了整片草地的偏移情况，人物对草地的压痕（交互，用flowmap实现）则通过粒子系统叠加到 RT 上。当人物开始行走时，粒子系统在RT上绘制出人物的行走轨迹，草地中在轨迹变化范围中的草的主要方向随向量变化而改变。

而基本的圆形flowmap笔刷可以通过仅包含xy通道的法线贴图实现，即通过eclipse函数创建范围后，通过normalFromHeight方法获取对应的法线，然后舍弃z分量的值。

接下来的步骤类似与可交互水，即创建只渲染flowmap的正交相机和对应的RT、调整相机与草地的位置、创建并调整粒子系统、将RT传入shader中。

然后我们就要根据RT里绘制的结果去偏移草尖的偏移方向了。由于草地的大小和相机视锥体完全对应，因此单根草在草地的模型空间的位置与RT的uv是存在对应关系的，我们可以拿到每根草的位置所对应的向量（例如（1, 0.5）代表往上方倾斜），施加在草的xz平面即可。

这种交互也不是基于物理的，是动画交互的一种。


# 基于GPU Instance生成大量草地
GPU Instance是一种能够高效生成大量实例（大于1000个）的方法，该方法可以实现多种常规方法无法流畅完成的刺激效果，例如10w根草组成的草地。





