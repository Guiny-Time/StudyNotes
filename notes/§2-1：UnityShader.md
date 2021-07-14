---
tags: [图形学]
title: §2-1：UnityShader
created: '2021-07-14T17:16:39.785Z'
modified: '2021-07-14T18:20:00.569Z'
---

# §2-1：UnityShader
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
