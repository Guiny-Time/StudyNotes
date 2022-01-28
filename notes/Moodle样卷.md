---
tags: [大三/CS380]
title: Moodle样卷
created: '2022-01-06T13:17:27.565Z'
modified: '2022-01-06T15:44:02.429Z'
---

# Moodle样卷
# 第一部分
## 描述为移动设备设计的操作系统与为传统个人电脑设计的操作系统相比所面临的三个挑战。
> Describe three of the challenges associated with designing operating systems for mobile devices compared with designing operating systems for traditional personal computers. 

- 较少的存储容量意味着操作系统必须小心管理内存。
- 操作系统还必须小心管理功耗。
- 更少的处理能力加上更少的处理器意味着操作系统必须小心地将处理器分配给应用程序。
-	Less storage capacity means the operating system must manage memory carefully.
-	The operating system must also manage power consumption carefully.
-	Less processing power plus fewer processors mean the operating system must carefully apportion processors to applications.

## 提供一个图表，说明**用户、系统和应用程序、操作系统和计算硬件之间的关系**。
> Provide a diagram that illustrates the relationship between the user or users, the system and application programs, the operating system and the computing hardware. 

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220106221251.png"/>

## **中断的目的**是什么?它们对操作系统有何**重要性**?**陷阱和中断**之间有什么区别?
> What is the purpose of interrupts and how are they important to Operating Systems? What are the differences between a trap and an interrupt?

- 中断是系统中由硬件产生的流的变化。
- 一个中断处理器被调用来处理中断的原因;然后将控制返回给中断的上下文和指令。
-	An interrupt is a hardware-generated change of flow within the system.  
-	An interrupt handler is summoned to deal with the cause of the interrupt; control is then returned to the interrupted context and instruction. 

### 陷阱和中断的差异
- 陷阱是用户程序发出的信号，指示操作系统立即执行某些功能。
- 中断是硬件向CPU发出的一个信号，表示需要立即注意的事件。
- 所有的陷阱都是中断。并不是所有的中断都是陷阱。
- 陷阱可能只发生在软件设备上。中断可能来自硬件和软件设备。
- 一个用户程序指令产生一个陷阱。硬件设备产生中断。
- 陷阱可以用来调用操作系统例程或捕获算术错误。中断可以用来发出I/O完成的信号，以避免设备轮询的需要。
- 	The trap is a signal raised by a user program instructing the operating system to perform some functionality immediately.  
-	The interrupt is a signal to the CPU emitted by hardware that indicates an event that requires immediate attention.  
-	All traps are interrupt.   Not all the interrupts are traps. 
-	Trap may happen only from the software devices. Interrupt may happen from the hardware and software devices. 
-	A user program instruction generates a trap Hardware devices generate interrupts. 
-	A trap can be used to call operating system routines or to catch arithmetic errors. 
-	An interrupt can be used to signal the completion of an I/O to avoid the need for device polling. 

## 描述对称和非对称处理器之间的区别。提供多处理器系统相对于单处理器系统的三个优点
- 对称多处理对所有处理器一视同仁，I/O可以在任何CPU上处理。处理器之间没有通信，因为它们是由主处理器控制的。更便宜。容易设计。
- 非对称多处理有一个主CPU，其余的CPU是从CPU。所有处理器通过共享内存与另一个处理器进行通信。昂贵的、复杂的设计
-	Symmetric multiprocessing treats all processors as equals, and I/O can be processed on any CPU. No Communication between Processors as they are controlled by the master processor. Cheaper. Easier to design.  
-	Asymmetric multiprocessing has one master CPU and the remainder CPUs are slaves. All processors communicate with another processor by a shared memory. Costlier. Complex design 
### 多处理器系统比单处理器系统的优点
- 主服务器在从服务器之间分配任务，I/O通常只由主服务器完成。
- 多处理器可以通过不复制电源、外壳和外设来节省成本。
- 它们可以更快地执行程序，并提高可靠性。
- 它们在硬件和软件方面也比单处理器系统更复杂。
-	The master distributes tasks among the slaves, and I/O is usually done by the master only.  
-	Multiprocessors can save money by not duplicating power supplies, housings, and peripherals. 
-	They can execute programs more quickly and can have increased reliability.  
-	They are also more complex in both hardware and software than uniprocessor systems. 

***

# 第二部分
## 系统程序为程序的开发和执行提供了一个方便的环境。它们可分为文件管理、状态信息、编程语言支持、程序的加载和执行、通信和后台服务。请描述一下

- 文件管理
创建、删除、复制、重命名、打印、转储、列出和操作文件和目录
- 状态信息
有些会向系统询问信息——日期、时间、可用内存数量、磁盘空间、用户数量。其他提供详细的性能、日志记录和调试信息。通常，这些程序将输出格式化并打印到终端或其他输出设备。
- 编程语言支持
有时提供编译器、汇编器、调试器和解释器
- 程序加载和执行
绝对加载器、可重定位加载器、链接编辑器和覆盖加载器，用于高级和机器语言的调试系统
- 通信
提供在进程、用户和计算机系统之间创建虚拟连接的机制。允许用户在彼此的屏幕上发送消息，浏览网页，发送电子邮件消息，远程登录，从一台机器到另一台机器传输文件
- 后台服务
在启动时启动。提供磁盘检查、进程调度、错误日志、打印等功能。在用户上下文中而不是内核上下文中运行。
- **File management**
Create, delete, copy, rename, print, dump, list, and generally manipulate files and directories 
- **Status information**
Some ask the system for info - date, time, amount of available memory, disk space, number of users. Others provide detailed performance, logging, and debugging information. Typically, these programs format and print the output to the terminal or other output devices. 
- **Programming-language support**
Compilers, assemblers, debuggers and interpreters sometimes provided 
- **Program loading and execution**
Absolute loaders, relocatable loaders, linkage editors, and overlay-loaders, debugging systems for higher-level and machine language 
- **Communications**
Provide the mechanism for creating virtual connections among processes, users, and computer systems. Allow users to send messages to one another’s screens, browse web pages, send electronic-mail messages, log in remotely, transfer files from one machine to another 
- **Background Services**
Launch at boot time. Provide facilities like disk checking, process scheduling, error logging, printing. Run in user context not kernel context.  

## 有三种多线程模型：多对一、一对一、多对多。在适当的图表的帮助下解释它们是如何运作的
### Many to one
In this model, we have multiple user threads mapped to one kernel thread. When a user thread makes a blocking system call the entire process blocks. As we have only one kernel thread, only one user thread can access kernel at a time, so multiple threads are not able to access the multiprocessor at the same time. 
The thread management is done on the user level so it is more efficient.
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220106222759.png"/>

### One-to-one
In this model, one to one relationship between kernel and user thread. Multiple threads can run on multiple processors. Problem with this model is that creating a user thread requires the corresponding kernel thread. 
As each user thread is connected to a different kernel, if any user thread makes a blocking system call, the other user threads won’t be blocked. 
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220106222857.png"/>

### Many-to-many
In this model, we have multiple user threads multiplex to same or lesser number of kernel level threads. Number of kernel level threads are specific to the machine. Advantage of this model is if a user thread is blocked we can schedule others user thread to other kernel thread. Thus, System doesn’t block if a particular thread is blocked.
<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220106222914.png"/>

## 工艺控制块(PCB)包含什么信息?
> What information does the Process Control Block (PCB) contain? 

<img src="https://raw.githubusercontent.com/Guiny-Time/PictureBed/main/20220106223008.png"/>

- **指针**
这是一个堆栈指针，当进程从一种状态切换到另一种状态时，需要保存它，以保持进程的当前位置。
- **进程状态**
它存储进程的各自状态。
- **进程号**
每个进程都被分配一个唯一的id，称为进程id或PID，用来存储进程标识符。
- **程序计数器**
它存储一个计数器，该计数器包含进程将要执行的下一条指令的地址。
- **寄存器**
这些是CPU寄存器，包括:累加器，基寄存器，寄存器和通用寄存器。
- **内存限制**
这个字段包含操作系统使用的内存管理系统的信息。这可能包括页表，段表等。
- **Pointer**
It is a stack pointer which is required to be saved when the process is switched from one state to another to retain the current position of the process.
- **Process state**
It stores the respective state of the process.
- **Process number**
Every process is assigned with a unique id known as process ID or PID which stores the process identifier.
- **Program counter**
It stores the counter which contains the address of the next instruction that is to be executed for the process.
- **Register**
These are the CPU registers which includes: accumulator, base, registers and general purpose registers.
- **Memory limits**
This field contains the information about memory management system used by operating system. This may include the page tables, segment tables etc.


## 四个用来描述一个操作系统可能必须为多媒体应用程序保证的服务质量的参数是：吞吐量、延迟、抖动、可靠性。定义这些术语的含义。
> Define what each of these terms means.

- 吞吐量
在特定时间间隔内完成的总工作量。
- 延迟
从第一次提交请求到产生所需结果所经过的时间。
- 抖动
在重放流期间发生的延迟。
- 可靠性
在连续介质的传输和处理过程中如何处理错误。
-	Throughput
The total amount of work completed during a specific time interval.
-	Delay
The elapsed time from when a request is first submitted to when the desired result is produced.
-	Jitter
The delays that occur during playback of a stream.
-	Reliability
How errors are handled during transmission and processing of continuous media.


# 第三部分
## 与应用程序相关联的信息流的传输可以采用五种模式中的一种。单纯形就是其中一种模式。请解释另外4种模式
> The transfer of the information streams associated with an application can be in 1 of the 5 modes. Simplex is one of these modes. Explain the other 4 of these modes. 
- 半双工:双向交替流动
- 全双工:同时双向流(1对1传输)
- 广播:1对all传输
- 组播:1对多传输
-	Half-duplex: flows in both directions but alternately
-	Full-duplex: flows in both directions simultaneously (1-to-1 transmission) 
-	Broadcast: 1-to-all transmission
-	Multicast: 1-to-many transmission

## 找出开源操作系统的两个优点和两个缺点。包括那些认为每个方面是优点或缺点的人的类型。
> Identify two advantages and two disadvantages of open-source operating systems. Include the types of people who would find each aspect to be an advantage or a disadvantage. 

### 优点
- 有很多人在其上工作，很多人调试它们，易于访问和分发，以及快速的更新周期。
- 对于学生和程序员来说，能够查看和修改源代码当然是一种优势。通常，开源操作系统对某些形式的使用是免费的，通常只需要为支持服务付费。
- many people working on them, many people debugging them, ease of access and distribution, and rapid update cycles.
- Further, for students and programmers, there is certainly an advantage to being able to view and modify the source code. Typically, open source operating systems are free for some forms of use, usually just requiring payment for support services. 
### 缺点
- 商业操作系统公司通常不喜欢开源操作系统带来的竞争，因为这些特性很难与之竞争。一些开源操作系统不提供付费支持程序。
- 一些公司避免开源项目，因为它们需要付费支持，这样当出现问题或需要帮助解决问题时，它们就有了负责任的实体。
- 一些人抱怨说，开源操作系统的编码缺乏规则，这意味着缺乏向后兼容性，难以升级，频繁的发布周期迫使用户频繁升级，从而加剧了这些问题。
- Commercial operating system companies usually do not like the competition that open source operating systems bring because these features are difficult to compete against. 
- Some open source operating systems do not offer paid support programs. Some companies avoid open source projects because they need paid support, so that they have some entity to hold accountable if there is a problem or they need help fixing an issue. 
- Finally, some complain that a lack of discipline in the coding of open source operating systems means that backward compatibility is lacking making upgrades difficult, and that the frequent release cycle exacerbates these issues by forcing users to upgrade frequently.

## 解释下列缩写:SMTP\FTP\SSH
- 简单邮件传输协议(SMTP)，它提供了一种机制来在不同的主机之间传输消息;
- 文件传输协议(FTP)，用于在用户命令下将文件从一个系统发送到另一个系统，包含文本和二进制文件;
- secure shell(SSH)，提供安全的远程登录功能。
-	Simple mail transfer protocol (SMTP) which provides a mechanism for transferring messages among separate hosts; 
-	File transfer protocol (FTP) which is used to send files from one system to another under user command with both text and binary files accommodated; 
-	Secure shell(SSH) which provides a secure remote logon capability.

## 可靠的通信是由传输控制协议(TCP)使用不可靠的互联网控制协议(IP)实现的。解释这是如何实现的。
> Reliable communication is achieved by the Transmission Control Protocol (TCP) using the unreliable Internet Control Protocol (IP). Explain how this is achieved.

- 因特网协议(IP)，它为因特网提供不可靠的、无连接的数据包传送。IP是无连接的，因为它独立地处理每个信息包。它是不可靠的，因为它不保证传递(也就是说，它不需要来自发送主机、接收主机或中间主机的确认)。
- tcp提供Internet主机之间可靠的数据流传输。TCP使用底层协议Internet Protocol来传输数据报，并支持在进程端口之间进行连续数据流的块传输。TCP通过确保数据不被损坏、丢失、复制或无序地交付给接收进程来提供可靠的消息传递。它使用序列编号和计时器来确保数据包的可靠传输。TCP允许重新传输丢失的数据包，从而确保所有传输的数据(最终)都被接收。
-	The Internet Protocol (IP), which provides unreliable, connectionless packet delivery for the Internet. IP is connectionless because it treats each packet of information independently. It is unreliable because it does not guarantee delivery (that is, it does not require acknowledgments from the sending host, the receiving host, or intermediate hosts).
-	TCP provides reliable stream delivery of data between Internet hosts. TCP uses Internet Protocol, the underlying protocol, to transport datagrams, and supports the block transmission of a continuous stream of datagrams between process ports. TCP provides reliable message delivery by ensuring that data is not damaged, lost, duplicated, or delivered out of order to a receiving process. It uses sequence numbering and timers to ensure reliable transfer of packets. TCP allows for the retransmission of lost packets, thereby making sure that all data transmitted is (eventually) received.

# 第四部分
## 提供多媒体通讯服务的通讯网络有五种
- 电话网络 Telephone Networks
- 数据网络 Data Networks
- 广播电视网 Broadcast Televison Networks
- 综合业务数字网(ISDN)
- 宽带多业务网络 Broadband multiservice Network
在每一个上面写简短的笔记，解释他们的目的，并描述他们的技术的任何特殊功能。

- **电话网络/Telephone Networks**
为提供基本的交换电话服务而设计。
“切换”意味着用户可以拨打连接到整个网络的任何其他电话。
电话网络以电路模式运行。
对于每个呼叫，在呼叫期间通过网络建立一个单独的电路。
连接电话手持设备到PSTN或PBX的接入电路被设计为携带与呼叫相关的双向模拟信号
虽然现代pstn是以数字模式工作，但调制解调器是用来在模拟接入电路上传输数字信号的。
Designed to provide a basic switched telephone service.
'Switched' means that a subscriber can make a call to any other telephone that is connected to the total network.
Telephone networks operate in circuit mode.
For each call, a separate circuit is set up through the network for the duration of the call.
The access circuits that link the telephone handsets to a PSTN or PBX were designed to carry the 2-way analog signals associated with a call
Though modern PSTNs operate in a digital mode, a modem is used to carry a digital signal over the analog access circuits.

- **数据网络/Data Networks**
它们旨在提供基本的数据通信服务，如电子邮件和一般文件传输。
这类网络中部署最广泛的两个是X.25网络和Internet。
X.25网络仅限于相对低比特率的数据应用程序。
互联网由大量相互连接的网络组成，这些网络都使用同一套通信协议运行。
所有的数据网络都以分组模式运行。之所以使用这种模式，是因为与数据应用程序相关联的数据格式通常是离散的文本块或二进制数据块，每个块之间的时间间隔是不同的。
These are designed to provide basic data communication services such as email and general file transfer.
Two most widely deployed networks of this type are the X.25 network and the Internet.
The X.25 network is restricted to relatively low bit rate data applications only.
The Internet is made up of a vast collection of interconnected networks all of which operate using the same set of communication protocols.
All data networks operate in packet mode. This mode is used because the format of the data associated with data applications is normally in the form of discrete blocks of text or binary data with varying time intervals between each block.

- **广播电视网络/Broadcast Television Networks**
它们的设计是为了支持模拟电视(和广播)节目在广大地区的传播。
它通常与有线网络提供的低比特率返回信道一起工作，以提供一系列的附加服务。
These are designed to support the diffusion of analog television (and radio) programs throughout wide geographical areas.
It generally works with a low bit rate return channel offered by a cable network for interaction purposes to provide a range of additional services.

- **综合业务数字网络/Integrated Services Digital Networks**
为PSTN用户提供附加业务的功能。
这是通过以下方法实现的:(1)将连接用户设备到网络的接入电路转换成全数字形式，
(ii)在这些电路上提供两个独立的通信通道。全数字接入电路被称为数字接入电路
subscriber line (DSL)用户线(DSL)用户线可选业务有:BRA和PRA。
Designed to provide PSTN users with the capability of having additional services.
This was achieved by (i) converting the access circuits that connect user equipment to the network into an all-digital form,
and (ii) providing 2 separate communication channels over these circuits. The all-digital access circuit is known as a digital
subscriber line (DSL)There are service options BRA and PRA.
· 
- **宽带多业务网络/Broadband Multiservice Networks**
设计于80年代中期，用于公共交换网络
支持广泛的多媒体通讯应用。·“宽带”意味着它可以支持比ISDN支持的比特率更高的比特率(&gt;;2Mbps)。这些网络中使用的交换和传输方法必须更加灵活，因为它们被设计成支持多种业务。所有的媒体类型都被转换成数字形式并集成在一起，产生的流被分成固定大小的分组，称为单元。交换固定大小的单元比交换可变长度的包要快得多。不同的多媒体应用程序产生不同速率的细胞流，因此细胞在网络中的传输速率是不同的。这种传输模式称为异步传输模式(ATM)。
Designed in the mid-80s for use as public switched networks
to support a wide range of multimedia communication applications. · "Broadband" means it can support a bit rate higher than that an ISDN can support (>2Mbps). Switching and transmission methods that are used in these networks must be more flexible as they are designed to support multiple services. All media types are converted into digital form and integrated together, and the resulting stream is divided into fixed-sized packets known as cells. Switching fixed-sized cells can be carried out much faster than switching variable-length packets.  Different multimedia applications generate cell streams of different rates and hence the rate of transfer of cells through the network varies. This mode of transmission is known as asynchronous transfer mode (ATM).


## 列出系统设计中微内核方法的两个主要优点和两个主要缺点
> List two main advantages and list two main disadvantages of the microkernel approach to system design.

### Advantages
- 添加新服务不需要修改内核，
- 用户模式下的操作比内核模式下的操作多，安全性更高
- 更简单的内核设计和功能通常会带来更可靠的操作系统。
- 微内核架构小而隔离，因此功能更好。
- 微内核是模块化的，不同的模块可以被替换，重新加载，修改，甚至不需要触及内核。
- 与单片系统相比，系统崩溃更少。
-	adding a new service does not require modifying the kernel, 
-	it is more secure as more operations are done in user mode than in kernel mode, and 
-	a simpler kernel design and functionality typically results in a more reliable operating system.
-	Microkernel architecture is small and isolated therefore it can function better.
-	Microkernels are modular, and the different modules can be replaced, reloaded, modified without even touching the Kernel.
-	Fewer system crashes when compared with monolithic systems.

### Disadvantages
- 微内核架构的主要缺点是与进程间通信相关的开销
- 频繁使用操作系统的消息传递功能，以使用户进程和系统服务能够相互交互。
-	the primary disadvantages of the microkernel architecture are the overheads associated with interprocess communication and 
-	the frequent use of the operating system’s messaging functions in order to enable the user process and the system service to interact with each other.

## 噪声信道的最大传输速率的状态香农定律。计算普通电话线的理论最高比特率是可能的。
电话线的带宽通常是3kHz。信噪比通常为3162。在这种情况下，信道容量是多少?
> State Shannon’s law for the maximum transmission rate of a noisy channel. It is possible to calculate the theoretical highest bit rate of a regular telephone line.
A telephone line normally has a bandwidth of 3kHz. The signal-to-noise ratio is usually 3162. What is the channel capacity in this case?

C = W.log2(1+S/N)bps
C=3000log2(1+3162)=3000x11.63=34881bps

