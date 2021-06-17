---
attachments: [Clipboard_2021-05-26-11-04-12.png]
tags: [CS280]
title: VII-14-可用性检查方法
created: '2021-05-26T02:43:46.287Z'
modified: '2021-06-15T14:46:39.898Z'
---

# VII-14-可用性检查方法
## 认知走查(Cognitive Walkthrough)
认知演练是一种以任务为中心的启发式评估方法，一个或多个可用性专家通过一系列任务，从用户的角度提出一系列问题。这种方法源于这样一个概念，即用户更喜欢通过使用系统来完成任务来学习系统，而不是学习手册。
### 执行认知走查
- 从接口要支持的任务套件中选择特定的任务。
- 为该任务确定一个或多个正确的操作序列。
- 在接口提供的上下文中检查这些序列。
- 评估假设的用户是否能够在每个点上选择适当的操作。

### 认知走查的主要特点
- 由分析师执行，反映了分析师的判断。
- 检查特定的用户任务。
- 分析操作的正确顺序，以及用户是否会遵循这些操作。
- 识别界面中可能出现的故障点，并提出可能的原因。
- 通过追踪假想用户可能的心理过程来识别问题

### 认知走查表格
#### 横轴：四个问题
- 用户会尝试并实现正确的结果吗?
- 用户会注意到正确的操作对他们可用吗?
- 用户会将正确的操作与他们期望实现的结果联系起来吗?
- 如果执行了正确的操作;用户是否能看到朝着预期结果前进的进展?
> Will the user try and achieve the right outcome?
Will the user notice that the correct action is available to them?
Will the user associate the correct action with the outcome they expect to achieve?
If the correct action is performed; will the user see that progress is being made towards their intended outcome?
#### 纵轴：用户步骤
用户在这个场景完成某个动作需要经历哪些步骤？
#### 样例图
这里只有三个问题(早期认知走查模型)，现在已经被上述横轴的四个问题替代
![](@attachment/Clipboard_2021-05-26-11-04-12.png)

### 认知走查的优缺点
#### 优点
- 允许在原型阶段或没有模型的情况下对设计进行早期评估。
- 帮助设计师评估其设计的功能如何配合以支持用户的工作。
- 提供关于动作序列的有用反馈。
- 协助设计人员为故障区域提供原因。
- 提供用户心理过程的指示，这有助于建立一个成功的界面，以适应用户。
#### 缺点
- 依赖于分析而不是用户测试。
- 提供特定任务的详细检查，而不是接口的概览。

## 启发式分析(Heuristic Evaluation)





