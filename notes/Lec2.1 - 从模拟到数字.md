---
attachments: [Clipboard_2023-01-04-02-04-56.png, Clipboard_2023-01-04-02-12-48.png]
tags: [大四/CS425 音频与语音处理]
title: Lec2.1 - 从模拟到数字
created: '2022-11-02T05:33:21.351Z'
modified: '2023-01-03T18:48:28.062Z'
---

# Lec2.1 - <font color="red">从模拟到数字</font>


# 数字化潮流
随着电子信息的发展，歌曲被记录到了CD、硬盘里。但CD也容易被划拉，并且内容可能损坏丢失。
声卡以USB的形式连接到电脑。它支持不同的采样率(44100kHz, 96000kHz等)、采样信号的比特数(16bit, 8bit, ...)、设备的频率响应、支持的输入和输出的数量。声卡支持双向的输入输出，例如录制人声到电脑（Analog -> Digital）、播放音频（Digital -> Analog）。**<font color="red">流程图如下所示</font>：**

<img src="https://i0.hdslb.com/bfs/article/56fb7a9530a3319c0576653591a515de8f0cbf1f.png" alt="image" width="886" data-width="986" data-height="254">
<img src="https://i0.hdslb.com/bfs/article/ad3c5694b14469abf8b2f0b6b0a5a03e7c6c53e1.png@942w_249h_progressive.webp" alt="image" width="852" data-width="852" data-height="219">

## 录音
当频率高于最大输入电平时，超出的部分会被截平(clipping)，造成失真。截平有两种，一种是硬裁剪，即直接一刀切（硬剪辑与数字系统有关，被认为会给数字录音带来一种刺耳、不愉快的质量。）；另一种是软裁剪，实现了一种更为圆润平滑的过渡。有的时候为了实现特殊的“失真”效果会故意在电子乐器中使用硬裁剪。

<img src="https://i0.hdslb.com/bfs/article/ed6b5be5ff5b828590364d5dcd2b702b4ee7ccbd.png" alt="image" width="400" data-width="1000" data-height="660"><img src="https://i0.hdslb.com/bfs/article/5de90a8e5726e39fd94ba148b67e998c9b005772.png" alt="image" width="500" data-width="1000" data-height="660">

## 采样率
根据香农定理和奈奎斯特频率，采样率有一个下限，即$f_{max} \leq F_s/2$。如果信号中存在的**最高频率(<font color="red">奈奎斯特频率</font>**) $f_{max}$ 不超过**采样频率** $F_s$ 的一半，那么任何信号都可以从其采样中准确重建。高于最高频率的部分会被**抗混叠滤波器**过滤，防止音频产生问题。下图是一个<font color="red"><b>用啁啾信号来说明混叠现象的实验</b></font>：

<img src="https://i.ytimg.com/vi/3vB53hoLsls/maxresdefault.jpg" width=500 />

**<font color="red">为什么即使用奈奎斯特频率进行采样时还要采用抗混叠滤波器？</font>**

抗混叠滤波器（AAF）是在信号采样器前使用的一种滤波器，用于限制信号的带宽，以满足奈奎斯特-香农采样定理在感兴趣的频段上的要求。由于该定理指出，当奈奎斯特频率以上的频率功率为零时，才有可能从其采样中明确地重建信号，因此砖墙滤波器是一个理想化但不实用的AAF。
> An anti-aliasing filter (AAF) is a filter used before a signal sampler to restrict the bandwidth of a signal to satisfy the Nyquist–Shannon sampling theorem over the band of interest. Since the theorem states that unambiguous reconstruction of the signal from its samples is possible when the power of frequencies above the Nyquist frequency is zero, a brick wall filter is an idealized but impractical AAF.[a] A practical AAF makes a trade off between reduced bandwidth and increased aliasing. 

标准的采样率是44.1kHz，44.1 kHz的采样率可以记录最大频率为22.05 kHz的音频信号。频率低于标准频率会造成一定程度的失真（听起来闷闷的），更高的采样率意味着更多的信息。<font color="red"><b>为什么标准采样率是44.1kHz？</b></font>有两个原因，一个是技术原因，另一个是与心理声学有关。
- **技术原因**
早期的数字录音机是由录像机制成的--在60赫兹视频中，有35条空白线，每帧留下490条线，或每场245条线用于采样。如果每行存储三个样本，采样率就变成了60 × 245 × 3 = 44.1 kHz....
Early digital audio recorders were made from video machines - In 60 Hz video, there are 35 blanked lines, leaving 490 lines per frame, or 245 lines per field for samples. If three samples are stored per line, the sampling rate becomes 60 × 245 × 3 = 44.1 kHz.…
- **心理声学原因**
人类可以听到20赫兹和20千赫之间的频率。实际上，随着年龄的增长，大多数人失去了听高频的能力，只能听到不超过15 kHz-18 kHz的频率。22050Hz的奈奎斯特频率大于20kHz，所以它意味着从样本中完美地重建音频是可能的。同时，通过将奈奎斯特频率置于大于我们的听觉范围，我们可以使用设计要求不那么严格的抗混叠滤波器来消除信号中大于20kHz的频率能量。
Humans can hear frequencies between 20 Hz and 20 kHz. Actually, most people lose their ability to hear high frequencies as they get older and can only hear frequencies no greater than 15 kHz–18 kHz. 

<img src="https://tvax1.sinaimg.cn/large/006UcwnJly1h7ryyayt8kj30f409jwhu.jpg" alt="image" width="544" data-width="544" data-height="343">

### 其他采样率
- 8kHz
往往用于对音质要求较低的演讲信号采样中，例如英语听力。因为不包含高频信息，这类音频通常比较小，比较能听到在说什么东西就行了。
- 48kHz
是另一个较为常见的采样率，一般用于 "专业音频 "的背景下。例如，它是视频音频中的标准采样率。这种采样率将一半的采样率值移到24kHz。
- 更高的采样率
一些工程师选择以更高的采样率工作，这往往是44.1 kHz或48 kHz的倍数。88.2 kHz、96 kHz、176.4 kHz和192 kHz的采样率是可用的，这意味着超音频可以被数字化。

但是要记住一件事：更多的样本 = 更多的数据 = 需要更多的存储空间 = 需要处理更多的数据!

## 比特位
经常会在一些音乐底下看到8bit标签，听起来都有一种独特的电子感。8bit是21世纪初红白机上的游戏配乐的比特位，现在在一些像素游戏中8bit风格也很流行（因为像素独特的复古感和童年记忆，更重要的是8bit很小，不过相应的能表示的数据量也小）。
比特位是以8为倍数递进的，现在的音乐文件基本采用32位：
- 8bit
能表达4,096种数据
- 16bit
能表达65,536种数据
- 24bit
能表达16,777,216种数据
- 32bit
能表达4,294,967,296种数据

量化的过程将连续的信号量化成离散的电平。更大的**位深度**意味着在量化时采样更多的数据，数字化版本中的振幅变化就越细微，同时也意味着更大的内存开销。以下是4bit的量化情况：

![](@attachment/Clipboard_2023-01-04-02-12-48.png)

## 信噪比(SNR)
信噪比指量化的采样音频中声音信号与噪声的比值，它是衡量音频质量的一个参数。SNR与位深度有关，任何以特定位深度编码的信号都会有相同的信噪比。下图是位深度、信号与噪声间的关系：

<img src="https://tvax1.sinaimg.cn/large/006UcwnJly1h7s61w9k4mj30dk09ijt8.jpg" alt="image" width="488" data-width="488" data-height="342">

SNR与**动态范围**(Dynamic Range)有关。动态范围指的是给定位深度下，音频信号的最大振幅与最小振幅的比值，即录音中最向量和最柔和的部分之间的差异。
<font color="red">SNR的**计算公式**为</font>：
$$SNR = 6.03 * N_{bits} + 1.76$$
单位为dB。例如16位深度的音频信噪比为 $SNR = 6.03 * 16 + 1.76 = 98dB$ 。实际上，由于现实生活中的电子元件性能，16位深度的动态范围更接近于89dB（ $= 6.02 * N_{bits} - 7.26$ ）dB。
不过人耳的听觉范围可是从20Hz到20000Hz，换而言之动态范围有差不多96dB。不过需要注意的是：
- 耳朵疼痛：130分贝。
- 听力损伤> 90分贝（长期暴露）。
- 正常对话：60-70分贝。
- 典型的教室背景噪音: 20-30分贝。
- 通常的声音在500Hz到2000Hz之间

进入声卡的模拟信号的最大允许振幅受到声卡规格的限制--如果声卡过载，通常有一个LED指示灯显示红色。因此，无论输入的音频被量化为8位、16位或24位，最大允许的振幅值都是一样的。

## <font color="red">动态范围与响度</font>
响度与动态范围有关。它是整个记录波形的**平均水平，而不是峰值**，它测量的是时间上的一个单一实例。也就是说，响度取决于一段时间内测量的音量，而不仅仅是波形的最响点。

它与听录音时的整个体验的响度有关。改变录音的响度，改变了录音的动态范围，使最安静的信号上升到某个阈值以上。

提高响度（加大音量）使我们对听众的体验有了一定的控制，所以在给定的环境条件下也能听到音频。

因此，在声卡输出的DAC（数模转换器）之后，放大器/扬声器的音量设置决定了音频的最终输出音量（响度）。如果扬声器的最小音量被设置为低于听觉阈值，那么就不能获得信号动态范围的全部好处。然而，调高音量会提高最大声的信号的音量，也会提高最安静的信号的音量。这可能会导致震耳欲聋的响声或失真，当信号超过了音频设备准确再现的能力。

因此，对于听觉环境的最佳响度水平，有一个妥协。自己调节最合适的音量吧。

> 虽然16位的音频可以达到89dB的动态范围，但并不是所有的生产商都能利用这个宽广的范围。
事实上，音频经常被 "动态压缩"，使音频中最安静和最响亮的部分之间的差异很小。这意味着，他们可以提高音频的整体音量。
为什么这样做？ 许多制片人希望他们的音乐听起来更宏大，失去所有的微妙之处

