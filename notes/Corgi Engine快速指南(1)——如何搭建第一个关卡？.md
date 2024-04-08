---
tags: [游戏开发/CorgiEngine]
title: Corgi Engine快速指南(1)——如何搭建第一个关卡？
created: '2024-02-28T13:21:08.880Z'
modified: '2024-03-01T04:25:02.933Z'
---

# Corgi Engine快速指南(1)——如何搭建第一个关卡？

{% btn 'https://corgi-engine-docs.moremountains.com/',引擎文档,far fa-hand-point-right,blue larger %}
{% btn 'https://corgi-engine-docs.moremountains.com/API/index.html',引擎API文档,far fa-hand-point-right,red larger %}
{% btn 'https://www.youtube.com/watch?v=3BRDH7H3bcA&list=PLopcAUHZHov3s5Eo9GOWRwny9UBjuBIDg&index=3',视频教程,far fa-hand-point-right,purple larger %}

# 首先得有一个场景

<img src="https://img10.360buyimg.com/ddimg/jfs/t1/106900/40/48267/18613/65df33f1F17526d0a/d62fc47aa4fbaaa2.jpg" alt="一个最基础的关卡" title="一个最基础的关卡" />

在Corgi Engine中如何搭建最简单的场景？引擎已经为我们准备了配置好基础数据的预制件，我们可以直接拖入新建场景中，一个场景最少所需的预制件分别是：
{% tabs 一个场景最少所需的内容 %}
<!-- tab GameManagers -->
    路径：CorgiEngine/Common/Prefabs/LevelManagers/GameManagers
    
    包含了Game Manager、Sound Manager、Achievement Rules 和 Time Manager，以组件的形式添加在物体上。
    
    负责处理得分、时间系数（游戏时间快慢）、暂停、设置解锁成就/增加进度的条件等功能。如果不需要其中的某些管理器，可以删除。
<!-- endtab -->

<!-- tab UI Camera -->
    路径：CorgiEngine/Common/Prefabs/GUI/UICamera
    
    不论游戏是否有UI，UI相机都需要加入场景中，因为预制件上附加了InputManager，这是玩家控制角色的关键组件（我也不知道为什么引擎不把这个mgr放到GameManagers上）。
    
    你可以把所有GUI相关的元素放到这个相机底下的Canvas里。
<!-- endtab -->

<!-- tab LevelManager -->
    这是个例外，我们无法找到LevelManager预制件，所以需要在场景中自行创建。
    
    新建空物体，然后添加“LevelManager”脚本即可。在脚本中可以设置游戏关卡的边界
<!-- endtab -->

<!-- tab Camera -->
    路径：CorgiEngine/Common/Prefabs/Camera/RegularCamera
    
    我们还需要往游戏中加入相机，这样才能看到关卡样貌。Corgi Enging提供了名为CameraController的脚本，可以追踪我们放在游戏中的角色，让视觉效果更好。
    
    引擎提供了两个相机，其中MainCamera支持后处理效果，RegularCamera实现了常规相机的功能。
<!-- endtab -->

<!-- tab Level -->
    一个简单的平台即可，这样玩家就可以有地方站立了。
    
    to do so，创建一个sprite再添加box collider 2d即可。
<!-- endtab -->
{% endtabs %}

# 然后得有一个角色
我们最好按照官方文档推荐的结构创建新角色，这有助于统一角色层级并方便后续管理。当然也可以按自己想的来，这并没有太多影响。
1. 创建空对象 **MyCharacter**
2. 在 **MyCharacter** 节点下创建 **MyModel** 空节点并加上 *SpriteRenderer* 组件。该组件用于渲染角色的sprite
3. 在 **MyCharacter** 上添加 *Character* 脚本，可以找到一个叫 <u>AutoBuildPlayerCharacter</u> 的按钮，如果你想图方便，直接点它！
<img src="https://img12.360buyimg.com/ddimg/jfs/t1/120178/36/43284/12461/65e07a19F21b7af86/ec9d3b07c17092f1.jpg" alt="image.png" title="image.png" width=300 />
4. 将 **MyModel** 赋值到 **MyCharacter** 的 *Character* 脚本的 <u>CharacterModel</u> 字段上（如果有动画，记得在Character Animator上也赋值）
<img src="https://img12.360buyimg.com/ddimg/jfs/t1/160775/27/42499/9365/65e07b91Fc329b5a2/19ca93c7aa64851d.jpg" alt="image.png" title="image.png" width=300 />
5. 如果你刚才没有点 <u>AutoBuildPlayerCharacter</u> 按钮的话，现在在 **MyCharacter** 上添加 *BoxCollider2D* 、 *Rigidbody2D* 和 *CorgiController* 组件。确保前两个是这样的：
<img src="https://img12.360buyimg.com/ddimg/jfs/t1/231698/30/13893/31735/65e07b20F159bdd2a/2af26801c5b19700.jpg" alt="image.png" title="image.png" width=200 />

现在，你已经创建好了一个角色的雏形了，你可以选择将其进一步设置为**可玩角色（玩家）**或**AI（敌人等）**。
<img src="https://img12.360buyimg.com/ddimg/jfs/t1/240289/13/4842/12222/65e07be1F6752cad1/c041309ad030c84d.jpg" alt="image.png" title="image.png" />

### 在场景中自动生成的可玩对象
我们刚才创建了一个基础角色 **MyCharacter** 。现在，我们将尝试将其设置为关卡自动生成和管理的可玩角色。
在经过这个设置之后，关卡管理器就可以在指定的开始位置生成玩家，并在玩家死亡后回收并重新生成:P
1. 确保CharacterType为Player
<img src="https://img14.360buyimg.com/ddimg/jfs/t1/97635/2/43747/5973/65e07ca1Ff07c975a/51ebb973aeb8597e.jpg" alt="image.png" title="image.png" width=300 />
2. 将 **MyCharacter** 制作成预制件（直接拖到文件系统里就好
3. 找到基础场景（上一节所创建的）里的 *LevelManager* 脚本，将预制件赋值到 <u>PlayerPrefabs[0]</u> 和 <u>SceneCharacter</u> 中
<img src="https://img14.360buyimg.com/ddimg/jfs/t1/234637/1/13133/4694/65e07c20F2fb6c0a9/a1bc04873ba147ca.jpg" alt="image.png" title="image.png" width=300 />

这样，我们在开始游戏之后就会发现场景中生成了玩家（的预制件）！
#### 代码引用
```C#
LevelManager.Instance.Players[0]
```

### 在场景中创建AI
设置CharacterType为AI

## 好吧，那我们怎么让角色动起来呢？
有以下三种方法能让AI/可玩角色在场景中动起来：
- 手动创建
- 自动创建
- copy一个（有什么不行的？

### 手动创建
该方法要求我们通过附加组件的方式为角色添加我们所需要的功能，我们可以有所选择，不用一股脑全加上去。虽然有点麻烦，但是是最贴合实际需求的定制化方法。
1. 为你的角色 **MyCharacter** 添加 *CorgiController* 组件，在之后的章节中我们将详细介绍如何配置该组件
2. 确保你的角色 **MyCharacter** 有 *Character* 组件，在之后的章节中将详细介绍其中的配置
3. 添加 *RigidBody2D* 组件.需要注意的是该组件需设置为 <u>Kinematic</u>
4. 如果玩家有血量需求，添加 *Health* 与 *HealthBar* 组件
5. 添加你所需要的角色能力，如：

### 自动创建
当我们点击 *Character* 脚本上的 <u>AutoBuildPlayerCharacter</u> 按钮时，系统就自动创建了一系列玩家能力。我们可以根据实际需求删掉不需要的组件。

### copy一个
比较调整合适的能力很复杂，有没有办法直接复制已经调好的能力呢？
直接复制配置好的预制件，然后改名、换贴图就好了（这真的是文档上写的
