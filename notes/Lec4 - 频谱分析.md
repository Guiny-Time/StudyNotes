---
tags: [大四/CS425 音频与语音处理]
title: Lec4 - 频谱分析
created: '2022-11-03T13:52:22.531Z'
modified: '2023-01-03T18:30:23.517Z'
---

# Lec4 - <font color="red">频谱分析</font>
## 频谱
虽然一个信号通过观察其时间波形（**时域信号**）可能难以理解/分析，但其信号的频域部分，也就是**频谱**能够揭示出更多关于这个信号是如何形成的信息。因此我们需要对信号（比如说声音）进行频域上的分析。

> 你知道人耳是怎么听到声音的吗？
声音在现实世界表现为大气中气压的振动。在振动传入人的内耳中时，鼓膜振动，引起中耳中一系列的三块小骨头（锤骨、砧骨和镫骨）振动。振动被转移到内耳的蜗牛状耳蜗。
<img src="https://tvax3.sinaimg.cn/large/006UcwnJgy1h9anch9ek9j30jq0be0xm.jpg" alt="image" width="310" data-width="710" data-height="410"><img src="https://tvax4.sinaimg.cn/large/006UcwnJgy1h9ane8c1bwj30hl0f5n0v.jpg" alt="image" width="310" data-width="633" data-height="545">
这种振动然后沿着耳蜗的基底膜（basilar membrane）传递，并由耳蜗中的毛细胞（hair cells）转化为神经冲动。神经冲动到达中枢神经系统的听觉处理区，听觉处理区识别声音，并将这一知识与来自其他感觉模式（如视觉）的信息结合起来。
人们认为，耳蜗的功能是通过对沿其长度方向不同空间位置的不同频率作出反应来分离传入的声音频率，基底膜上的每个区域都是一个滤波器组。
<img src="https://tva4.sinaimg.cn/large/006UcwnJgy1h9anndvi5rj30go09cq8l.jpg" alt="image" width="600" data-width="600" data-height="336">
滤波器组是一组滤波器，每个滤波器都有不同的中心频率，并通过不同频谱区域的能量。

- **频谱分析器(Spectrum Analyser)**
频谱分析器是一种发现和检查构成信号的**各个频率成分的能量**的工具。用户可以自己选择一组特定的频率，不过在许多情况下，默认的方法是选择一组间隔相等的频率。一旦选定完成，频谱分析器就会隔离并测量每个频率周围的能量。
<img src="https://tvax2.sinaimg.cn/large/006UcwnJly1h9bie2jiz4j31as0pmgp8.jpg" alt="image" width="600">

- **带通滤波器(BandPass Filter)**
带通滤波器这个名字听起来听陌生的，但在现实世界中，可调谐带通滤波器最经典的一个例子是收音机上的旋钮。当旋钮转动到电台频率时，带通滤波器只让这个电台相关的频率通过，而过滤掉了所有其他的频率。
所以平常我们通过移动带通滤波器的中心频率，以找到我们要找的电台。  
<img src="https://tvax1.sinaimg.cn/large/006UcwnJly1h9big7s1xtj31hc0u01d7.jpg" alt="image" width="620" data-width="1920" data-height="1080">

- **关系？**
可以用带通滤波器来进行频谱分析的工作。具体来说，我们可以通过过滤其他频率的方式隔离出每个感兴趣的频率然后展开分析。最适合频谱分析的带通滤波器是具有窄通带的带通滤波器（BPF）。信号通过BPF后，我们可以在其输出端测量能量。
<img src="https://tva3.sinaimg.cn/large/006UcwnJly1h9bj7k5agsj30e60anjsp.jpg" alt="image" width="500" data-width="510" data-height="383">

## 傅里叶变换

频谱分析仪是怎么把时域的信号转化成频域展示出来的呢？其背后的原理是由傅里叶理论描述的。傅里叶理论，或者说**傅里叶变换**（Fourier Transform）解释了如何计算一个信号的频谱。通过使用傅里叶方程，我们可以找到任何可以用数学描述的信号的频谱。

$$F(\omega) = \int_{-\infty}^{\infty}f(t)e^{-j\omega t}dt$$
其中，$\omega$ 是弧度频率，$f(t)$ 是时域信号，$e^{jwt}$ 是复数指数。这里 $j = 2\pi i$ 。同时，上述公式还可以写作：
$$F(\omega) = \int_{-\infty}^{\infty}f(t)(cos(\omega t) - j sin(\omega t)) dt$$
可见：
$$e^{-j\omega t} = cos(\omega t) - j sin(\omega t)$$
傅里叶变换本质上是测量信号 $f(t)$ 和频率为 $\omega$ 的复数指数信号之间的**相似性**（correlation），复数指数信号是由余弦和正弦组成的。实部是余弦的部分，虚部是正弦的部分。

从本质上讲，傅里叶认为有可能从一系列频率增加的正弦和余弦项的**总和**中形成任何周期函数 $f(t)$ 。通过傅里叶变换，你是在测量波形 $f(t)$ 与频率为 $\omega$ 的$cos(\omega t)$ 和$sin(\omega t)$ 的每个波之间的相似度。这些**相似度越高，就意味着在$\omega$ 下的能量越大**（代表频域图中的峰值）。

如果我们测量信号 $f(t)$ 与频率为 $\omega$ 的许多不同的余弦和正弦值之间的相似性，那么我们就有了所谓的频谱。那么如何选择要测量的 $\omega$ 值？最常见的方法是选择在频率轴上**均匀分布**的 $\omega$ 值。

以下是一个信号的时域展示和拆分的正余弦信号:

<img src="http://tvax4.sinaimg.cn/large/006UcwnJly1h9bjlv2jwfj30fc0bi0tv.jpg" alt="image" width="550" data-width="552" data-height="414">

<img src="http://tvax3.sinaimg.cn/large/006UcwnJly1h9bjm2f9k2j30f90anzlt.jpg" alt="image" width="350" data-width="549" data-height="383"><img src="https://tvax3.sinaimg.cn/large/006UcwnJly1h9bjnpca40j30e60anac6.jpg" alt="image" width="350" data-width="510" data-height="383">

这与以下事实有关：
- 波形的形状可以完全是一个偶函数(cos)或奇函数(sin)，也可以是它们之间的某种组合。

仅用余弦波的描述不可能得出准确的正弦波，相对的仅用正弦波的描述不可能得出准确的余弦波，因为它们在时间上的起点完全不同。有的时候一个波形很复杂（比如$x(t) = 0.25sin{(\omega t)} + cos{(\omega t)}$)，我们需要余弦和正弦来描述所有可能发生的时间序列类型。这也是为什么傅里叶变换公式中使用了复数指数。

复数指数 $2\pi i$ 和相位的概念有关，相位描述了曲线偏移零点的程度，一般用 $\phi$ 表示，比如说：
$$x(t) = sin{(\omega t + \phi)}$$

#### 一些例子
- 锯齿波
<img src="https://tvax2.sinaimg.cn/large/006UcwnJly1h9bs6e2oomj30m608bmzr.jpg" alt="image" width="298" data-width="798" data-height="299">

- 方波
<img src="https://tvax1.sinaimg.cn/large/006UcwnJly1h9bs8mz96xj30m608b0v5.jpg" alt="image" width="298" data-width="798" data-height="299">

- 双锯齿
<img src="https://tvax1.sinaimg.cn/large/006UcwnJly1h9bs9a2c6jj30m608bn00.jpg" alt="image" width="298" data-width="798" data-height="299">

- 抛物线
<img src="https://tvax4.sinaimg.cn/large/006UcwnJly1h9bs9yhxxcj30m608b76m.jpg" alt="image" width="298" data-width="798" data-height="299">

### 频谱
频谱有两个重要的组成参数，即**magnitude of the spectrum**和**phase of the spectrum**。因为傅里叶变换公式里指数是复值的（即有实部和虚部），所以 **频谱 $F(\omega)$ 也是一个复数** 。因此，我们需要进一步处理以找到频谱的 **幅度** 和 **相位**。

幅度的计算公式是找到 $F(\omega)$ 的**绝对值**：
$$|F(\omega)| = \sqrt{real(F(\omega))^{2} + imaginary(F(\omega))^{2}}$$
相位的计算公式是求**反正切值**（夹角）：
$$\angle F(\omega) = tan^{-1}(\frac{imaginary(F(\omega))}{real(F(\omega))})$$

两个参数都能单独成谱，不过，大多数时候我们感兴趣的是看幅度谱而不是相位谱，因为信号中不同频率成分的大小决定了我们是否能听到它。

举个例子，
$$x = sin(2\pi \omega t) + 0.5 * sin(2 \pi 2\omega t) + 2 * sin(2 \pi 3\omega t) + 0.25 * sin(2\pi 6\omega t)$$
其中 $\omega = 20Hz$。这代表在 $\omega$、$2\omega$、$3\omega$ 和 $6\omega$ 这四个频率上各有一个sin函数（虚部）。
所以在 $\omega = 20Hz$ 处，幅度为2。但幅度谱存在正负两个方向，所以虽然总幅度为2，但要分出一半均摊给正负两个方向。所以在正轴上幅度表现为1。同理可得在 $\omega$ 为40、60、120处的幅度谱分别为0.25、1、0.125：

<img src="https://tvax3.sinaimg.cn/large/006UcwnJgy1h9drdrufezj30dn0aojsi.jpg" alt="image" width="491" data-width="491" data-height="384">

但很明显，幅度谱难以反馈不同频率的函数是正弦还是余弦函数。因此还需要相位谱的辅助。正弦和余弦之间的区别就在相位上，如果用余弦来表示正弦，只需简单的移动 $\frac{\pi}{2}$，也就是半个周期。
$$sin(\omega t) = cos(\omega t - \frac{\pi}{2})$$

在这个例子中，可以把原式的sin函数改写成cos形式：
$$x = cos(2\pi \omega t - \frac{\pi}{2}) + 0.5 * cos(2 \pi 2\omega t - \frac{\pi}{2}) + 2 * cos(2 \pi 3\omega t - \frac{\pi}{2}) + 0.25 * cos(2\pi 6\omega t - \frac{\pi}{2})$$

在图像上表示为20、40、60、120这几个点的相位值为 $-\frac{\pi}{2}$。相位谱同样有正负之分。在负数轴上，所有sin的部分的相位值为 $\frac{\pi}{2}$，组合在一起呈现出中心对称。

需要非常注意的是，函数式的每一项**都应该是以加号相连的**。因此比如说出现了$-cos(2\pi \omega t)$，需要改写成 $cos(2\pi \omega t + \pi)$。写成 $cos(2\pi \omega t - \pi)$ 也没问题，不过约定俗成的规则是加号。

## 计算傅里叶变换
### 离散傅里叶变换
在计算机中，数据往往是离散而非连续的，因此用连续傅里叶变换不切实际。这里介绍一下离散傅里叶变换（DFT）进行计算。

$$X[k] = \sum_{n = 0}^{N - 1} x[n]e^{-j\frac{-2\pi k}{N}n}, k = 0,\cdots ,N-1$$

其中 $n$ 指代第 $n$ 个样本，$k$ 指代所截取的频率，也就是 $\omega$。

离散傅里叶变换使用了微积分的思想。通过无限细分的矩形，我们可以用来表示一个曲线的积分。同样的道理，离散傅里叶变换中所有**经过处理的数据点的和与连续傅里叶变换的积分几乎是相同的**。

关于为什么是求和，一个浅显易懂的说明：

<img src="https://tva2.sinaimg.cn/large/006UcwnJgy1h9e85b3ukoj30bb0csgod.jpg" alt="image" width="300" data-width="407" data-height="460">

### 快速傅里叶变换
DFT的原理浅显易懂，但计算起来非常令电脑难受，因为当你的数据点越多，每个点需要计算的时间就会增长，加到所有点上就是 &O(n^2)& 的恶心。而**快速傅里叶变换**(FFT)可以帮助我们降低时间复杂度到 $O(n log_{2}n)$，并且在**数据量为 $2^n$ 时效果最好**，**数据量为质数时没有任何效果**。

不过FFT写起来也是真复杂，因为需要分组、需要画蝶形图七七八八...大部分音频处理软件都自带了FFT的库，可以直接调用（没事造什么轮子！用现成的不香吗）。

现在来试试吧！假设我们有一组处理过的信号点，原型是频率为1024Hz的余弦波，采样频率为32768Hz，有32768个数据点经过FFT计算，他的频谱图和分贝频谱图如下所示：

<img src="https://tva1.sinaimg.cn/large/006UcwnJgy1h9e97en2agj30e60an0tj.jpg" alt="image" width="310" data-width="510" data-height="383"><img src="https://tvax1.sinaimg.cn/large/006UcwnJgy1h9e96ew5kpj30e60an75k.jpg" alt="image" width="310" data-width="410" data-height="383">

你开始觉得莫名其妙，为什么余弦波经过处理出现了**两个峰值**？明明只应该在频率1024Hz的地方出现一条线才对，但结果却有两条。这就是**FFT的混叠问题**。在大于一半的采样频率，也就是 $\frac{F_{s}}{2}$ 的部分都是FFT的混叠区域，混叠区域和有效区域是呈现**镜面对称**的（分贝频谱图展示的比较明显）。混叠区域是一种冗余，通常会被隐藏。

但其实FFT认为信号是无限延申的，我们只是对有限的数据进行了处理而已。上述例子的频谱图是可以被拉伸到“无限”的范围的，当然他们也是冗余：

<img src="https://tvax3.sinaimg.cn/large/006UcwnJgy1h9e9j3j5hoj30e60anwft.jpg" alt="image" width="410" data-width="510" data-height="383">

另外引入**频率分辨率**的概念： $\Delta f = \frac{F_s}{N}$，这是FFT能检测到的最小频率变化，当小于 $\Delta f$ 时，频率会落在采样点之间而不被发现。在这个例子中，$\Delta f = \frac{32768Hz}{32768} = 1$

另外是**频率仓**的概念。频率仓 $k = \frac{f_{in}}{\Delta f}$，$f_{in}$ 是输入的余弦波信号的频率，也就是1024Hz。

如果我们把余弦波的频率调成1024.5Hz，一些奇怪的现象便出现了：

<img src="https://tvax1.sinaimg.cn/large/006UcwnJgy1h9fdspgkatj30e60angmi.jpg" alt="image" width="410" data-width="510" data-height="383">

注意看，在峰值附近出现了一些**曲线**。分贝频谱则更加明显：

<img src="https://tvax2.sinaimg.cn/large/006UcwnJgy1h9fdscwx96j30e60ant9r.jpg" alt="image" width="410" data-width="510" data-height="383">

这种外观上的变化是由于被分析的信号被输入到FFT时，所分析的频率并不是一个完整的周期。当输入**信号的频率是采样频率的整数倍数**时，才不会出现这样的曲线。比如例子里 $\frac{32768}{1024} = 32$，但 1024.5Hz 就不行，导致原本峰值被分散到周边的分量，这也被成为**频率泄露**，导致真实频谱与噪声无法很好的区分。

比较这两个信号，很明显，1024Hz信号的末端可以连接到它的起点，而对于1024.5Hz的信号则不行。

<img src="https://tvax3.sinaimg.cn/large/006UcwnJgy1h9feajper1j30ek0anwg6.jpg" alt="image" width="310" data-width="524" data-height="383"><img src="https://tva2.sinaimg.cn/large/006UcwnJgy1h9febrif98j30e60andi4.jpg" alt="image" width="310" data-width="510" data-height="383">

#### 窗口化(Windowing)
**窗函数**(windowing function)能够强制修改数据集中之外的频域信息为0。通过窗口化能够最大限度的降低频率泄露带来的影响。

<img src="https://tvax3.sinaimg.cn/large/006UcwnJgy1h9felzllt0j315c0kmany.jpg" alt="image" width="688" data-width="1488" data-height="742">

有许多不同的窗函数，例如矩形窗(Rectangulae window)、汉宁窗(Hann window)、Hamming窗、凯撒窗(Kaiser window)、Blackman-Harris窗等等。下图是不同窗函数的原型（时域信号）和其频谱：

<img src="https://tvax3.sinaimg.cn/large/006UcwnJgy1h9ff0rlb4bj30ey0an0u0.jpg" alt="image" width="338" data-width="538" data-height="383"><img src="https://tva1.sinaimg.cn/large/006UcwnJgy1h9ff1d6dkbj30ey0anwfq.jpg" alt="image" width="338" data-width="538" data-height="383">

<img src="https://tva3.sinaimg.cn/large/006UcwnJgy1h9fezmi6e8j31hc0nytic.jpg" alt="image" width="520" data-width="1920" data-height="862">

请注意：
- 使用**矩形窗可以获得尖锐的峰值，但在这些峰值的底部有很多冗余/泄露的频谱**。
- **使用类似一半sin函数的窗函数（比如Hann窗），会可以得到更宽的峰值，但泄露幅度的占比要低得多。**

窗口化信号，实际上就是把信号与窗函数相乘，从而改变信号的形状，并进一步改变信号的频谱。可以说，新的频谱是信号的频谱与窗口的频谱的**卷积**。是这样算的：
$$y[n] = \sum^{N-1}_{k=0}h[k]x[n-k]$$
其中 $h$ 是窗函数， $x$ 是信号， $y$ 是处理过的信号。

举个栗子吧！现在我们有一组数据点：$x[n] = {2,6,-1,-2}$，并且窗函数 $h[n] = {5,-3,1}$

<img src="https://tva2.sinaimg.cn/large/006UcwnJgy1h9ffn6yq7dj30i608dgo1.jpg" alt="image" width="654" data-width="654" data-height="301">

#### 零填充(Zero Padding)
就像在图像领域对需要卷积的图像周边填充0一样，也可以在需要处理的信号两端添加空白信号以**凑齐 $2^n$ 的长度**，最好的发挥FFT的优势。幅度输出的结果将在外观上更加平滑，包含了更多细节。但在严格意义上，它不提供更好的频率分辨率 $\Delta f$。以下是使用零填充与不使用的对比：

<img src="https://tva1.sinaimg.cn/large/006UcwnJgy1h9fgm613rrj30jm0andgv.jpg" alt="image" width="606" data-width="706" data-height="383">

## FFT处理非周期性信号
现实世界中，绝大多数音频都天然的不是周期信号，直接用FFT处理势必会造成大量冗余数据，窗函数处理必不可免。同时有些信号可能非常长，所以计算FFT的操作数量会非常大。那么最好的办法就是**将这些信号分成小段**。

音频信号首先被缓冲成通常是**重叠**的帧，帧的长度通常是 $2^n$。该长度将反映出对信号中的频率内容静止的时间长度，例如10或20ms。然后窗口化（**通常使用半正弦形状的窗函数**）、零填充后用FFT对每段信号进行单独分析。

处理完的结果最后会输出一张二维或三维的深度图，通过颜色深浅反映不同片段的频谱。




