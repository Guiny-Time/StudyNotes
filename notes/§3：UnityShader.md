---
attachments: [Clipboard_2021-07-15-12-31-34.png, Clipboard_2021-07-15-12-34-59.png, Clipboard_2021-07-15-12-35-17.png, Clipboard_2021-07-15-13-10-29.png, Clipboard_2021-07-15-13-10-41.png, Clipboard_2021-07-15-13-40-12.png, Clipboard_2021-07-15-13-41-03.png, Clipboard_2021-07-15-14-31-22.png]
tags: [TA/Unity Shader]
title: §3：UnityShader
created: '2021-07-14T17:16:39.785Z'
modified: '2021-11-21T06:06:10.334Z'
---

# §3：UnityShader
## UnityShader概述
我们如何才能使用UnityShader的魔法呢？答案是我们需要一个材质。在依托于材质之后，shader才能发挥自己的能力。

### Unity内置的Shader模板
创建Shader时，Unity会提供四种内置的Shader模板：
- **Standard Surface Shader**
产生一个包含标准光照模型的表面着色器模板
- **Unlit Shader**
产生一个不包含光照但是包含雾效的基本顶点/片元着色器
- **Image Effect Shader**
实现各种屏幕后效果
- **Compute Shader**
利用GPU的并行性来处理一些与常规渲染无关的计算

## ShaderLab
ShaderLab是Unity提供的一层抽象，它帮助我们更好的编写Shader
### ShaderLab结构
ShaderLab有四个主要结构
- Shader{}
第一层是Shader{}，它定义了整个Shader代码块，同时可以声明Shader的位置和命名
- Properties{}
第二层是属性，它定义了Shader中需要使用的属性，并且会在材质球的检查器中显示
- SubShader{}
第三层是自渲染器，定义了一系列Pass以及可选的状态和标签
- Fallback{}
最后一层是回滚，它定义了GPU性能不行的时候的第二套解决方案

#### 命名与存放位置
当我们创建了一个Shader，我们需要在材质球上使用它。材质球选择Shader位于检查器上方的位置，如：
![](@attachment/Clipboard_2021-07-15-12-31-34.png)
在我们创建Shader的时候，我们实际上可以自定义Shader位于这下面的哪个分类里、叫什么名字。比如我在Guiny分类下创建了一个叫“Shader命名测试”的Unlit Shader，那么我们就可以在检查器的对应位置找到我们的shader
```ShaderLab
Shader "Guiny/Shader命名测试"
```
![](@attachment/Clipboard_2021-07-15-12-35-17.png)

#### 属性(Property)
属性是Shader与材质球的联系，它们将会显示在材质的面板中
定义属性的方式如下：
```ShaderLab
Properties
{
    //属性名("展示在面板上的名字", 属性类型) = 默认值
    _mainText("Texture", 2D) = "white"{}
    _guiny("时光光自定义Texture", 2D) = ""{}
}
```
![](@attachment/Clipboard_2021-07-15-13-10-41.png)
你会发现属性名的开头都有一个下划线，这是shader属性的命名习惯，实际上你也可以不这么做。
属性语义块支持的所有属性类型如下所示：
```ShaderLab
    Properties
    {
        //属性名("展示在面板上的名字", 属性类型) = 默认值
        _Int("Int类型", Int) = 2
        _Float("Float类型", Float) = 1.5
        _Range("Range类型", Range(0.0,5.0)) = 3.0   //Range类型默认内部为float，在声明属性类型的时候就要加上范围值
        _Color("Color类型", Color) = (0.2,0.3,1,1)  //Color是一个四元组，表示RGBA
        _Vector("Vector类型", Vector) = (1,2,3,4)   //Vector同样是四元组，表示一个四维向量
        _2D("2D类型", 2D) = ""{}                    //纹理类型，双引号内是内置纹理名称，花括号是纹理属性
        _Cube("Cube类型", Cube) = "white"{}         //纹理类型
        _3D("3D类型", 3D) = "black"{}               //纹理类型
    }
```
在检查器中，它们表现为：
![](@attachment/Clipboard_2021-07-15-13-41-03.png)
你会发现虽然这些属性很强大，但是它可能无法满足所有需求(比如需要一个bool值判断是否开启了某个功能)，这时候我们可以重载材质编辑面板，通过自定义着色器GUI来获得想要的属性，如![](@attachment/Clipboard_2021-07-15-14-31-22.png)

#### 子渲染器(SubShader)
每个UnityShader可以包含多个子渲染器，主要是因为鉴于不同显卡的性能差异问题，一些针对高性能显卡编写的SubShader可能无法在较低性能的显卡上使用，因此多个SubShader加回滚可以避免低配置就没法好好玩游戏的情况
子渲染器包括了Pass、状态(RenderSetup)和标签(Tags)

- Pass
Pass语义块定义了一次完整的渲染流程。在一个子渲染器中可以有多个Pass，但如果Pass数目过多则会造成渲染的性能下降，因此应该尽可能使用数量最少的Pass
```ShaderLab
Pass{
  Name "GuinyFirstPass" // Pass的名字，方便再其他地方用UsePass调用
  tags{"LightMode"="ForwardBase"}// Pass内的标签，与子渲染器的不同
  Cull Front            // Pass内的状态设置
  // 具体的代码
}
```
- 状态设置
状态设置主要设置的是**显卡的各种状态**，比如是否开启深度测试、如何进行深度测试、是否开启混合等等。当在一个子渲染器中设置了渲染状态，这些状态设置将会应用到这个子渲染器的所有Pass语义块中。如果想独立影响不同的Pass语义块，请在Pass內部进行对应的设置

- 标签(Tags)
标签是一个键值对，并且KV都是字符串类型，用来告诉渲染引擎应该怎样渲染对象。一个标签的花括号里可以包含多个KVP。标签只能在子渲染器中声明，不能在Pass语义块中声明(Pass语义块中的标签不同于子渲染器的标签类型)

#### 回滚(FallBack)
在之前的TMP使用中也存在一个Fallback，指的是字体回滚，当在字体库中找不到某个字体的时候回滚到预备字体库中的选择。Shader的回滚则表示当所有的子渲染器GPU都无法执行的时候的终极操作，也就是最低级的Shader
```ShaderLab
Fallback "VertexLit"  // 使用VertexLit渲染器作为最低级的Shader
```
<br>

## 几种着色器
所有的着色器类型都需要定义在Cg/HLSL语义块中，也就是CGPROGRAM与ENDCG中间。但是需要注意，有的着色器定义在Subshader中，而有的Shader定义在Pass中。
如果项目中使用了较多的光源，那么更适合使用表面着色器，但其在移动应用中的效果一般
如果项目中的光源较少，或者你希望能自定义设置渲染器更多一些，那么顶点着色器或者表面着色器就更为合适

### 表面着色器(Surface Shader)
使用表面着色器的渲染代价比较大，本质上表面着色器和顶点着色器或者片元着色器是一样的，可以将表面着色器理解成顶点/片元着色器的抽象
使用表面着色器的方式：
```ShaderLab
Shader "Lec3/表面着色器"{
  Subshader{                        // 定义在Subshader语义块中

    CGPROGRAM
    #pragma surface surf Lambert    // 定义这个子渲染器使用了表面着色器与兰伯特光照模型

    // 输入的结构体
    struct Input{
      float4 color : COLOR;
    };

    // 一个无返回值的方法
    void surf (Input IN, inout SurfaceOutput o){
      o.Albedo = 1; //反射率
    }
    ENDCG

  }
  Fallback "Diffuse"
}
```

### 顶点/片元着色器
想不到吧，这两个着色器在Unity中是用Cg或者HLSL写的(那我学GLSL干嘛)，它们比表面着色器更为复杂，但灵活性更高
使用顶点着色器和片元着色器的方式：
```ShaderLab
Shader "Lec3/顶点和片元着色器"{
  SubShader{
    Pass{                         // 定义在Pass语义块中
      CGPROGRAM

      #pragma vertex vert         // 声明使用顶点着色器
      #pragma fragment frag       // 声明使用片元着色器

      float4 vert(float4 v : POSITION) : SV_POSITION{
        return mul(UNITY_MATRIX_MVP, v);
      }
      fixed4 frag() : SV_Target{
        return fixed4(1.0, 0.0, 0.0, 1.0);
      }

      ENDCG
    }
  }
}
```

### 固定函数着色器(Fixed Function Shader)
它适用于特别老旧的、不支持可编程管线着色器的设备，能实现的效果非常有限
固定函数着色器被定义在Pass语义块中，需要完全使用ShaderLab的语法来编写








