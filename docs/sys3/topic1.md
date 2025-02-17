# Fundamentals of computer system

!!! tip
    Every problem in computer science can be solved with another layer of indirection.  - David Wheeler

## 经典体系结构思想

+ 冯诺依曼架构
    + 将程序指令存储器和数据存储器合并在一起的存储器结构。
+ 摩尔定律
    + 集成电路上可容纳的晶体管数目，每隔约两年便会增加一倍。
+ Amdahl定律
    + 我们对计算机系统的某一部分加速的时候，该加速部分对系统整体性能的影响取决于该部分的重要性和加速程度。
    + 记该部分的加速比为 Se ，该部分原本占整体处理时间的占比为 Fe ，则整体的加速比为
        $$ Sn = \frac{1}{(1-Fe)+\frac{Fe}{Se}} $$