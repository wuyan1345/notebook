---
count: true
---

# Fundamentals of computer system

!!! tip
    Every problem in computer science can be solved with another layer of indirection.  - David Wheeler

## 前言
+ 冯诺依曼架构
    + 将程序指令存储器和数据存储器合并在一起的存储器结构。
+ Flynn分类法
    + Flynn分类法是一种广泛应用于并行计算机分类的方法。它根据指令流(Instruction Stream)和数据流(Data Stream)两个独立的维度进行分类，从而得出四种不同类型的计算机体系结构。
    + 四种类型为SISD/SIMD/MISD/MIMD。

## 计算机系统架构的优化趋势
+ Instruction-level parallelism(ILP)
    + Single processor performance improvement ended in 2003.
+ New models for performance:
    + Data-level parallelism(DLP)
    + Thread-level parallelism(TLP)
    + Request-level parallelism(RLP)
+ Multiprocessors
    + 实现的难点：程序编写/加载平衡/同步与数据互通

## 性能比较
+ 评判性能的对象
    + Algorithm
    + Programming language, compiler, architecture
    + Processor and memory system
    + I/O system
+ 规定 Performance = 1/Execution time
+ CPU Clocking
    + Clock period: 时钟周期的长度
    + Clock frequency/rate： 一秒钟内有多少个时钟周期
+ CPU Time = CPU Clock Cycles * Clock Cycle Time
+ Average cycles per instruction(CPI)
    + CPU Time = Instruction count * CPI / Clock Rate
+ Weighted average CPI
    $$ CPI = \sum CPI_i \times \frac{Inst Count_i}{ Inst Count } $$
+ Amdahl定律
    + 我们对计算机系统的某一部分加速的时候，该加速部分对系统整体性能的影响取决于该部分的重要性和加速程度。
    + 记该部分的加速比为 Se (speedup)，该部分原本占整体处理时间的占比为 Fe (fraction)，则整体的加速比为
        $$ Sn = \frac{1}{(1-Fe)+\frac{Fe}{Se}} $$

## 经典体系结构思想
+ 用摩尔定律设计
    + 集成电路上可容纳的晶体管数目，每隔约两年便会增加一倍。
+ 抽象化简化设计
+ 加快普通情况处理
+ 并行性能优化
+ 流水线优化
    + Break tasks into stages so that multiple tasks can be simultaneously performed in different stages.
+ 分支预测优化
    + Sometime faster to assume a particular result than waiting until the result is known.
+ 使用分等级的内存访问优化
+ 去冗余(redundancy)优化

## 谬论(Fallacies)和陷阱(Pitfalls)
??? danger "谬论"
    + 多处理器(Multiprocessors)是万金油。
    + 硬件优化既提升了性能又能提高能源效率（至少保证能量效率不变）
    + 基准值(benchmark)将无限期有效
    + 磁盘故障的额定平均时间近140年，所以磁盘运作几乎没有失败
    + 峰值能表示性能
+ 陷阱
    + 故障检测可以降低可用性