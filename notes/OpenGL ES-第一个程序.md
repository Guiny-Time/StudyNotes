---
tags: [语言学习/OpenGL ES]
title: OpenGL ES-第一个程序
created: '2022-02-23T15:10:59.703Z'
modified: '2022-02-25T05:49:39.457Z'
---

# OpenGL ES-第一个程序
按照安卓的官方文档用OpenGL ES2.0画了第一个三角形，借此来说一下代码结构是什么样的：

<img src="https://s2.loli.net/2022/02/24/XHeJdGq2PzjIW3b.png" width=300/>

环境的搭建需要使用**GLSurfaceView组件**（继承自SurfaceView）和**Renderer类**（GLSurfaceView的内部接口类）搭建一个视图容器：
- GLSurfaceView 是使用 OpenGL 绘制的图形的视图容器，负责触摸事件等逻辑的处理
- GLSurfaceView.Renderer 可控制该视图中绘制的图形，即负责渲染图形图像

# 前置声明
需要使用OpenGL ES 2.0的API时，我们需要在**AndroidManifest.xml**里加上相关声明。同时，如果应用使用了纹理压缩，还必须在清单中声明应用支持哪些压缩格式，以便应用仅在兼容的设备上安装：
```xml
<manifest>

    ...

    <!-- 声明需要使用OpenGL ES 2.0 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />
    <!-- 声明纹理压缩设置 -->
    <supports-gl-texture android:name="GL_OES_compressed_ETC1_RGB8_texture" />
    <supports-gl-texture android:name="GL_OES_compressed_paletted_texture" />
</manifest>
```
> AndroidManifest.xml是每个android程序中必须的文件，它位于整个项目的根目录，是配置程序运行所必要的组件、权限，以及一些相关信息。官方文档称其为清单。

# 核心构成
## Activity
Activity是一个Android组件，作用是提供一个屏幕，用户可以用来交互为了完成某项任务。一个Activity通常就是一个单独的屏幕，我们可以在其中加上想要的组件，但是在声明OpenGL ES应用时，我们需要加上GLSUrfaceView组件：
```Java
package com.example.opengles_study;
import android.app.Activity;
import android.os.Bundle;
import android.opengl.GLSurfaceView;

public class MainActivity extends Activity {

    // 声明GLSurfaceView组件
    private GLSurfaceView gLView;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 创建实例并设置为该Activity所展示的内容
        gLView = new MyGLSurfaceView(this);
        setContentView(gLView);
    }
}

```

## GLSurfaceView
前文说过了， GLSurfaceView 组件是一种专用视图，可以在其中绘制 OpenGL ES 图形。不过对象的实际绘制是由您在此视图中设置的 Renderer 类控制的。在这里我们需要对 GLSurfaceView 进行扩展，因为这样才能捕获触摸事件（**Renderer类负责渲染图形图像，而GLSurfaceView负责触摸事件等逻辑的处理**）：
```Java
package com.example.opengles_study;
import android.content.Context;
import android.opengl.GLSurfaceView;

// 进行扩展
class MyGLSurfaceView extends GLSurfaceView {
    // 声明内部的Renderer类
    private final MyGLRenderer renderer;

    public MyGLSurfaceView(Context context){
        super(context);

        // 创建OpenGL ES 2.0上下文
        setEGLContextClientVersion(2);
        // 实例化Renderer
        renderer = new MyGLRenderer();

        // 设置Renderer
        setRenderer(renderer);
    }
}
    
```
为了节省开销，我们可以把渲染设置成仅在数据发生改变时重新绘制（加在设置Renderer之后）：
```Java
    // Render the view only when there is a change in the drawing data
    setRenderMode(GLSurfaceView.RENDERMODE_WHEN_DIRTY);
```

## Renderer
Renderer是实际用来绘制图像图形的类，控制在与之相关联的 GLSurfaceView 上绘制的内容。Android 系统会调用渲染程序中的以下三种方法来确定在 GLSurfaceView 上绘制什么内容以及如何绘制：
- **onSurfaceCreated()**
类似Unity生命周期函数中的Awake，GLSurfaceView内的Surface被创建时会被调用到
- **onSurfaceChanged()**
类似Unity生命周期函数中的Start（不完全是），当视图的几何图形发生变化（例如当设备的屏幕方向发生变化）时会被调用到；在窗口创建的时候至少调用一次
- **onDrawFrame()**
类似Unity生命周期函数中的Update，每次重新绘制视图时调用

所以一般情况下，首次创建GLSurfaceView时，会按顺序调用onSurfaceCreated、onSurfaceChanged、onDrawFrame这3个方法，然后每绘制一帧都会不停地回调onDrawFrame方法（类似Unity中Update函数的性质）。
```Java
package com.example.opengles_study;
import javax.microedition.khronos.egl.EGLConfig;
import javax.microedition.khronos.opengles.GL10;
import android.opengl.GLES20;
import android.opengl.GLSurfaceView;

public class MyGLRenderer implements GLSurfaceView.Renderer {
    // 创建时调用
    public void onSurfaceCreated(GL10 unused, EGLConfig config) {
        // 设置背景颜色（白色）
        GLES20.glClearColor(0.0f, 0.0f, 0.0f, 1.0f);
    }

    // 每帧调用
    public void onDrawFrame(GL10 unused) {
        // 渲染背景颜色
        GLES20.glClear(GLES20.GL_COLOR_BUFFER_BIT);
    }

    // 屏幕改变时调用
    public void onSurfaceChanged(GL10 unused, int width, int height) {
        // 调整视口大小
        GLES20.glViewport(0, 0, width, height);
    }

}
```

## 渲染模块
完成了基本的搭建之后，现在就开始编写相关的着色代码了。我们要写的第一个代码是实现一个三角形。思路如下：
1. 我们可以用浮点数坐标来定义三角形的三个顶点
2. 将这些坐标逆时针**写入 ByteBuffer 中**，它会传递到 OpenGL ES 图形管道进行处理

> **为什么要逆时针记录**
我们通常用顶点逆时针来表示一个物体的“正面”，用顺时针来表示“背面”。区分正面和背面是很重要的（在数学上我们使用叉乘的结果来进行判断），因为我们之后可能会使用面和剔除的功能。

> **什么是ByteBuffer**
简单来说就是一个字节缓冲区，有HeapByteBuffer和DirectByteBuffer两种形式，在本例中用的是DirectByteBuffer。二者之间的描述和区别如下所示：
<img src="https://s2.loli.net/2022/02/24/Ks4yzECbJpiSweA.png" width=600/>
我们之所以在接下来的代码中使用了DirectByteBuffer是因为手机屏幕就是一个经典的IO设备，通过DirectByteBuffer进行访问的速度更快。

~~~Java
package com.example.opengles_study;
import java.nio.ByteBuffer;
import java.nio.ByteOrder;
import java.nio.FloatBuffer;
import android.opengl.GLES20;

public class Triangle {

    private FloatBuffer vertexBuffer;

    // 此数组中每个顶点的坐标数（x, y, z）
    static final int COORDS_PER_VERTEX = 3;
    // 以逆时针顺序记录顶点的坐标位置
    static float triangleCoords[] = {
            0.0f,  0.622008459f, 0.0f,  // top
            -0.5f, -0.311004243f, 0.0f, // bottom left
            0.5f, -0.311004243f, 0.0f   // bottom right
    };

    // 设置颜色的RGBA值
    float color[] = { 0.63671875f, 0.76953125f, 0.22265625f, 1.0f };

    public Triangle() {
            // 创建DirectByteBuffer并指定容量
            ByteBuffer bb = ByteBuffer.allocateDirect(
                    // (number of coordinate values * 4 bytes per float)
                    triangleCoords.length * 4);
            // 使用设备硬件的本机字节顺序
            bb.order(ByteOrder.nativeOrder());

            // 从ByteBuffer创建一个浮点缓冲区
            vertexBuffer = bb.asFloatBuffer();
            // 把顶点坐标装进去
            vertexBuffer.put(triangleCoords);
            // 设置缓冲区读取第一位
            vertexBuffer.position(0);
    }

}
~~~

# 开始绘制
## 实例化渲染模块
在得到几个核心构成之后，我们就可以开始在屏幕上进行绘制了。首先，我们要把渲染模块在Renderer类中进行实例化：
~~~Java

    ...

    private Triangle mTriangle;

    public void onSurfaceCreated(GL10 unused, EGLConfig config) {
        // 实例化渲染模块-三角形
        mTriangle = new Triangle();
        // 设置背景色
        GLES20.glClearColor(1.0f, 1.0f, 1.0f, 1.0f);
    }

    ...

~~~

## 编写着色器
然后需要在渲染模块中加上顶点着色器代码和片元着色器代码。令人激动人心的部分终于来了，我们终于看到两个属性的名词。在渲染模块中的例子如下所示。
> 您至少需要一个顶点着色程序绘制形状，以及一个片元着色程序为该形状着色。您还必须对这些着色程序进行编译，然后将其添加到之后用于绘制形状的 OpenGL ES 程序中。

~~~Java

    ...

        // 顶点着色器
        private final String vertexShaderCode =
            "attribute vec4 vPosition;" +
            "void main() {" +
            "  gl_Position = vPosition;" +
            "}";

        // 片元着色器
        private final String fragmentShaderCode =
            "precision mediump float;" +
            "uniform vec4 vColor;" +
            "void main() {" +
            "  gl_FragColor = vColor;" +
            "}";

    ...

~~~
翻译成GLSL就是最基本的经典操作（不过这里没有乘上mvp矩阵，可能因为声明的位置就是屏幕坐标系位置的原因）：
~~~glsl
// 顶点着色器
attribute vec4 vPosition;
void main() {
    gl_Position = vPosition;
}
~~~
~~~Java
// 片元着色器
precision mediump float;
uniform vec4 vColor;
void main() {
    gl_FragColor = vColor;
}
~~~

## 编译着色器
前面的着色器代码是用String的形式编写的，要能够正确的编译着色器，我们需要在Renderer类中加上编译着色器的代码：

~~~Java

...

    public static int loadShader(int type, String shaderCode){

        // create a vertex shader type (GLES20.GL_VERTEX_SHADER)
        // or a fragment shader type (GLES20.GL_FRAGMENT_SHADER)
        int shader = GLES20.glCreateShader(type);

        // add the source code to the shader and compile it
        GLES20.glShaderSource(shader, shaderCode);
        GLES20.glCompileShader(shader);

        return shader;
    }
    
...

~~~








