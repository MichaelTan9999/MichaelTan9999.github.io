---
title: 软考笔记 - 知识产权与标准化
date: 2023-03-30 20:00:00 +0800
categories: 软考
math: true
---
2022年5月左右的预测。

![2022-May-system-configuration-and-performance-evaluation.png](https://s2.loli.net/2023/03/30/54D2uVJvLAhqfWY.png)

## 系统性能

- 可靠性或可用性（计算机系统能正常工作的时间）：MTTF以及这个故障时间在总时间的占比
- 处理能力或效率：吞吐率（如单位时间内能处理的正常作业数）、响应时间（系统得到输入到给到输出的时间）、资源利用率（给定区间中软硬件的使用时间站总区间的比例）

### 常见的评价指标分类

- 计算机：时钟频率（主频）、数据处理速率（PDR）、吞吐率；
- 路由器：端口吞吐量、丢包率、时延、抖动；
- 交换机：支持协议和标准
- 网络：吞吐量
- 操作系统：系统吞吐率（量）、系统响应时间
- 数据库管理系统：最大并发事务处理能力、负载均衡能力、最大连接数
- Web服务器：最大并发连接数、响应延迟、吞吐量

主频 = 外频 * 倍频

$$
\text{MIPS} = \frac{指令条数}{执行时间\times10^6}=\frac{主频}{\text{CPI}}=主频\times\text{IPC}
$$

$$
\text{MFLOPS} = \frac{浮点操作次数}{执行时间\times10^6}
$$

CPI: clock per instruction，平均每条指令的时钟周期个数

IPC: instruction per clock，每周期运行的指令条数

MIPS: million instructions per second，每秒执行的每百万条指令数

MFLOPS：million floating-point operations per second，每秒执行的每百万个浮点操作

### 阿姆达尔（Amdahl）解决方案

（系统性能）提升加速比计算公式：

$$
R=\frac{T_p}{T_i}=\frac{1}{(1-F_e)+\frac{F_e}{S_e}}
$$

其中$T_p$表示不使用改进组件完成整个任务的执行时间，$T_i$表示使用改进组件完成整个任务的执行时间。$F_e$是能被改进的部分占改进前执行总时间的比例，$S_e$是改进前被改造组件部分的执行时间与改进后被改造组件部分的执行时间的加速比值。

### 性能评价方法

关于CPU：时钟频率法、指令执行速度法（MIPS）、等效指令速度法/Gibson Mix吉普森混合法（考虑指令比例不同的问题）

不止针对CPU：数据处理速率法（考虑CPU+内存）、综合理论性能法（考虑理论性能，MTOPS）、基准程序法（benchmark）

测试精确度排名：真实的程序、核心程序、小型基准程序、合成基准程序。

TPC（Transaction Processing Council，事务处理委员会）基准程序系列：

- TPC-A测试OLTP环境下的数据库和硬件的性能
- TPC-B测试不包括网络的纯事务处理，模拟企业计算环境
- TPC-C测试联机订货系统
- TPC-D、TPC-H、TPC-R测试决策支持系统，其中TPC-R允许附加优化选项
- TPC-E测试大型企业信息服务系统
- TPC-W测试服务器

评估方法主要分为测量方法和模型方法，其中模型方法分为模拟方法（系统建模）和分析方法（数学建模）。
