---
tags: [TA/Unity Shader]
title: Compute Shader介绍与使用
created: '2022-10-12T09:10:57.670Z'
modified: '2022-10-13T12:46:46.401Z'
---

# Compute Shader介绍与使用
# 什么是Compute Shader
**计算着色器**（Compute Shader，下文简称CS）是跑在GPU上的一类shader程序，它独立于渲染管线，是在GPU的另一块独立空间运算的。由于GPU的高并行计算性能，CS很适合用来计算大量需要并行计算的数据，比如说CNN神经网络或者挖矿，避免由CPU处理太多计算。
如何使用CS呢？以及如何对CS进行debug呢？

# 基本构成
在Unity中新建一个CS，命名为TestCS（创建方法为右键/Create/Shader/Compute Shader），双击打开后默认内容如下所示，并且Unity贴心的加了注释：
```HLSL
// Each #kernel tells which function to compile; you can have many kernels
#pragma kernel CSMain

// Create a RenderTexture with enableRandomWrite flag and set it
// with cs.SetTexture
RWTexture2D<float4> Result;

[numthreads(8,8,1)]
void CSMain (uint3 id : SV_DispatchThreadID)
{
    // TODO: insert actual code here!

    Result[id.xy] = float4(id.x & id.y, (id.x & 15)/15.0, (id.y & 15)/15.0, 0.0);
}

```

- 第一行声明了名为CSMain的"kernel"。kernel是内核或者核（比如说卷积核，也是用kernel表示的）的意思，这一行即把一个名为CSMain的函数声明为内核，或者称之为**核函数**。核函数最终会在GPU中被执行，一个CS中**允许存在多个核函数**，并且**至少要有一个核函数才会被唤起**。声明核函数的方法即：
```HLSL
#pragma kernel functionName
```

- 第二行创建了一个带有**enableRandomWrite**标志的RT。RWTexture2D的意思是可以被CS读写的2D纹理，RW是RandomWrite的简写。**RWTexture2D与Texture2D的区别在于能否写入**，后者是只读不写的。
由于RWTexture2D本身是纹理，它是由多个像素组成的，可以通过Result[id.xy]的形式访问值为float4类型的像素。

- 第三行定义了一个线程组(Thread Group)中可以被执行的线程（Thread）总数量。将numthreads(x,y,z)中的三个数相乘即线程的总数量，如例子中有8 * 8 * 1 = 64个线程。X、Y 和 Z 值指示特定方向的线程组的大小，X * Y * Z 的总数为组中的线程数。 指定跨三个维度的线程组大小的功能允许以逻辑 2D 和 3D 数据结构的方式访问各个线程。
需要注意的是，**每个核函数前都要定义线程组**，否则就会编译报错。
```HLSL
[numthreads(8,8,1)]
void test1 (uint3 id : SV_DispatchThreadID){}

[numthreads(8,8,1)]
void test2 (uint3 id : SV_DispatchThreadID){}
```

- 线程组下面的部分就是对应的核函数
```HLSL
void CSMain (uint3 id : SV_DispatchThreadID)
{
    // TODO: insert actual code here!

    Result[id.xy] = float4(id.x & id.y, (id.x & 15)/15.0, (id.y & 15)/15.0, 0.0);
}
```
这个函数的意思是给输出纹理Result写入新的像素。在单线程的C#里如果要逐行的给纹理画上像素，我们一定会写两个for循环来赋值：
```C#
for(int i = 0; i < texture.width; i++){
  for(int j = 0; j < texture.height; j++){
    //给第i行第j个像素赋值
  }
}
```
这是很慢的，时间复杂度为 $O(N^2)$，必须进行纹理宽度次数的内循环，当纹理较大时一定会造成卡顿。但核函数里一样的操作只写了一行代码，这是怎么回事呢？因为这一行是针对单一线程写的，它只需要对一个像素进行处理。有很多线程在并行的做这件事，因此只需要一行代码而不是写一个内外循环，处理速度直接起飞。

## 脚本调用
CS需要用C#脚本调用才能使用。可以在脚本中声明CpmputeShader类型的变量与CS相关联，创建RT用于debug：
```C#
public ComputeShader computeShader;
public RenderTexture RT;
```
创建用于CS的RT，开启读写：
```C#
RenderTexture mRenderTexture = new RenderTexture(256, 256, 16);
mRenderTexture.enableRandomWrite = true;
mRenderTexture.Create();
```
将RT赋值给C#脚本，下文中的kernelIndex变量即核函数下标，每个核函数的下标独一无二，可以通过名字的方式来查找并赋值。
```C#
int kernelIndex = computeShader.FindKernel("CSMain");
computeShader.SetTexture(kernelIndex, "Result", mRenderTexture);
```
为当前核函数分配线程组，按照创建RT的实际大小(256*256)进行分配。在Dispatch函数中传入的三个整型参数指明了我们要创建的线程组的数量。回想一下，每个线程组的大小是我们在compute shader中已经定义了的，所以在上面的例子中，线程的数量我们遵循如下的计算：
1. 要创建的线程组数量：256（纹理的宽高皆为256）/8 =32;
2. 每个组的线程数目：8*8=64;
3. 线程总数量：32*32（线程组）*64（个线程每个组） = 65536（线程数量）
4. RT一共有这么多像素：256*256 = 65536;
```C#
computeShader.Dispatch(kernelHandle, 256/8, 256/8, 1);
```
运行程序后可以在RT出按住ctrl查看渲染结果，这也是一种用来debug CS的方式(可以单独查看每个通道输出的结果)：

<img src="https://tvax4.sinaimg.cn/large/006UcwnJly1h73pp3l7ooj30kx0b343e.jpg" alt="image" width="500" data-width="753" data-height="399">

那可不可以把处理的结果与材质相关联呢？当然可以。现在创建一个新材质命名为CSTest，在C#脚本里加上材质变量，现在一共有三个变量：
```C#
public ComputeShader computeShader;
public RenderTexture RT;
public Material mat;
```
给材质写一个mainTexture，并在C#中赋上RT的值：
```C#
mat.mainTexture = tex;
```
运行后材质结果如下所示：

<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h73puagkqmj30a90a0n1f.jpg" alt="image" width="300" data-width="369" data-height="360">

# 高级功能
## Structured Buffers
一个 Structured Buffer是一个包含单一数据类型的一组数据。声明Structured Buffer的方式如下：
```HLSL
StructuctedBuffer<float> floatBuffer;         // 创建一个float类型的buffer，里面只能有float类型
RWStructuredBuffer<int> readWriteIntBuffer;   // 创建一个int类型的buffer，里面只能有int类型

struct VecMatPair         // 创建一个结构体，可以包含多种数据类型
{
    float3 pos;           // 顶点
    float4x4 mat;         // 矩阵
};
RWStructuredBuffer<VecMatPair> dataBuffer;    // 创建一个VecMatPair类型的buffer，里面只能有VecMatPair结构体类型
```
类似于可读写RT，可以在C#中将值传递给CS计算。不过与RT不同的是，对于一个Structured buffer，**需要去指定一个元素中有多少字节**，并将这个数据跟compute buffer对应的buffer储存到一起。对于我们的例子结构，里面有：
## 










