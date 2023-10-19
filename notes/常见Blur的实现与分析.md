---
tags: [TA/Unity Shader]
title: 常见Blur的实现与分析
created: '2023-05-12T10:32:56.516Z'
modified: '2023-05-12T14:07:43.817Z'
---

# 常见Blur的实现与分析

最近和嘟嘟猪有一个新的空岛游戏的想法，在场景设计的讨论上我提出可以用移轴模糊实现出low poly小人国的效果（不知道实际应用好不好看，因为模型本身就比较小），正好可以借此机会尝试一下写一些后处理shader，在此祭出浅墨老师的常见blur效果对比分析图：

<img src="https://image.baidu.com/search/down?url=https://tvax3.sinaimg.cn/large/006UcwnJly1hdvs5o3b6yj31130r71gk.jpg" alt="image.png" title="image.png" width=600 />

下图为接下来效果演示用到的图片(https://pxhere.com/en/photo/1391057)，作品版权遵守CC0协议，仅供学习参考使用。

<img src="https://c.pxhere.com/photos/4e/d3/street_crowd_people_car_road-1391057.jpg!d" srcset="https://c.pxhere.com/photos/4e/d3/street_crowd_people_car_road-1391057.jpg!d" alt="pedestrian, people, road, street, car, night, city, crowd, cityscape, downtown, transport, evening, vehicle, lane, lighting, infrastructure, metropolis, urban area, residential area, human settlement, metropolitan area, Free Images In PxHere" width=500>

<img src="https://image.baidu.com/search/down?url=https://tvax4.sinaimg.cn/large/006UcwnJly1hdvsfezop3j30en0g245w.jpg" alt="image.png" title="image.png" />

## Gaussian Blur
高斯模糊（Gaussian Blur）是图像经过高斯核处理得到的结果。下图是一个高斯核的例子：

<img src="https://image.baidu.com/search/down?url=https://tvax4.sinaimg.cn/large/006UcwnJly1hdvsxoncuej309k0683yp.jpg" alt="image.png" title="image.png" width=300 />

在二维空间中，高斯滤波的过程被定义为：

$$G(u,v) = \frac{1}{2\pi \sigma^2} e^{-(u^2 + v^2)/(2\sigma^2)}$$
其中uv代表输入图像的长宽，σ表示高斯分布的标准差。依照高斯分布的3σ原则，我们只需要处理$(6\sigma + 1)\times (6\sigma + 1)$的矩阵就可以保证相关像素影响。关于图像边界部分的处理有很多种方法，想偷懒的话可以直接用最简单的取0法进行填充。在py中，可以直接用opencv库调用高斯滤波：

```python
blur = cv2.GaussianBlur(img,(5,5),0)
```

下面是浅墨老师提供的片元着色器中的高斯核卷积过程：

```cg
float4 FragGaussianBlur(v2f i): SV_Target
{
    half4 color = float4(0, 0, 0, 0);

    color += 0.40 * SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, i.uv);
    color += 0.15 * SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, i.uv01.xy);
    color += 0.15 * SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, i.uv01.zw);
    color += 0.10 * SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, i.uv23.xy);
    color += 0.10 * SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, i.uv23.zw);
    color += 0.05 * SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, i.uv45.xy);
    color += 0.05 * SAMPLE_TEXTURE2D(_MainTex, sampler_MainTex, i.uv45.zw);

    return color;
}
```
显然color代表每个正在被像素最终输出的结果，而color

在PS中应用半径为30的高斯模糊得到以下效果：

<img src="https://image.baidu.com/search/down?url=https://tvax4.sinaimg.cn/large/006UcwnJly1hdvt082ibjj30ru0ifjzd.jpg" alt="image.png" title="image.png" width=500/>




















