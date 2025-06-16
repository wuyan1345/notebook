---
count: true
---

# Instruction-Level Parallelism(ILP)

??? tip "有趣的图"
    + 单周期 vs 流水线，你们知道吗？

        <img src="../3.png" style="max-width: 60%; height: auto;">

## Dependences
+ 如果想要指令并行，我们必须知道有哪些指令可以并行，不能并行的指令之间的关系称为依赖。
+ Data Dependences
    ```asm hl_lines="1 2"
    FLD F0,0(X1)
    FADD.D F4,F0,F2
    FSD F4,0(X1)
    ```
+ Name Dependences
    ```asm linenums="1" hl_lines="1 2"
    FDIV.D F2,F6,F4
    FADD.D F6,F0,F12
    FSUB.D F8,F6,F14
    ```
    + 如果将第2、3条指令中的F6命名成别的寄存器则第1、2条指令之间就不存在依赖
+ Control Dependences
    + 在分支中包含上述依赖，也就是说，在条件满足/不满足的时候指令依赖情况不同。

## Hazard
+ 在流水线CPU中需要处理的冒险问题。
+ Structure hazards
    + A required resource is busy.
+ Data hazard (RAW, WAR, WAW)
    + Need to wait for previous instruction to complete its data read/write.
+ Control hazard
    + Deciding on control action depends on previous instruction.
+ 可以通过重新规划来避免冒险的问题。

## Branch Prediction
+ Predict outcome of branch
    + Only stall if prediction is wrong
    + 因此只要预测分支的成功率越高，流水线停止的次数就越少，效率就越高。
+ Static branch prediction
    + Based on typical branch behavior
    !!! example "loop and if-statement branches"
        + Predict backward branches taken
        + Predict forward branches not taken
    + Delayed branch（使用延迟插槽）
+ Dynamic branch prediction （基于历史跳转情况）
    + Hardware measures actual branch behavior
    + Assume future behavior will continue the trend 
    !!! note 
        + use Branch Prediction buffer(aka branch history table) + Branch-Target Buffers
        + 转换方式

            <img src="../4.png" style="max-width: 80%; height: auto;">
        + 1-Bit Predictor
            + 效率过低
        + 2-Bit Predictor
            
            <img src="../5.png" style="max-width: 60%; height: auto;">

            + 相当于(-2,-1,1,2)四种程度，如果预判对了就加强判断，否则削减判断的力度。
        + n-Bit Predictor

+ Branch Target Buffer
    + 记录分支指令的地址和目标地址
    + 通过分支指令的地址来预测目标地址
    + may solve the problem for 
        + Even with predictor, still need to calculate the target address for 1-cycle penalty for a taken branch

## Overlapping Execution
+ conflict in access memory
    + adding instruction buffer between memory and instruction decode unit

## Dynamic Scheduling
+ 流水线运行的弊端在于如果指令之间存在依赖，流水线就必须停止运行直到相关操作处理完成。
+ 实现动态规划，相当于将指令重新排列但不改变其功能以解决冒险。
+ 将经典流水线的 ID 阶段拆分成 IS 和 RO 
    + IS: Check structural hazards
        + 检查目标功能单元有无空位（如整数 ALU、浮点 ALU、加载/存储单元、分支单元）
    + RO: Check data hazards
+ Scoreboard algorithm
    + 工作四个阶段
        + Issue (IS)
            + 如果功能单元无空位或者WAW就不发射
        + Read Operands (RO)
            + 如果有RAW就等待
        + Execute
        + Write Result
            + 如果有WAR就等待
    + 硬件三部分
        + Instruction status
        + Functional unit status
            + Busy 部件是否忙碌
            + Op 操作类型
            + Fj, Fk 源寄存器地址
            + Rj, Rk 源寄存器状态
            + Qj, Qk 产生结果的功能单元
        + Register result status
    <img src="../6.png" style="max-width: 80%; height: auto;">

    + In-order issue; out-of-order execute & commit
    
+ Tomasulo's Approach
    + It tracks when operands for instructions are available to minimize RAW hazards;
    + It introduces register renaming in hardware to minimize WAW and WAR hazards.
    + 三个阶段
        + Issue
            + 检查功能单元是否有空位
            + 如果有空位则将指令放入功能单元并将寄存器重命名
        + Execute
            + 执行指令
        + Write Result
            + 将结果写回寄存器
    <img src="../7.png" style="max-width: 80%; height: auto;">

+ Reorder Buffer
    + in order commit
    + 解决了Tomasulo's Approach中指令执行顺序和提交顺序不一致的问题
    + Add a step: Commit—update register with reorder result

## Achieve CPI < 1
<img src="../8.png" style="max-width: 80%; height: auto;">

+ SuperScalar
    + The number of instructions which are issued in each clock cycle is not fixed.
+ VLIW
    + The number of instructions which are issued in each clock cycle is fixed (4-16), and these instructions constitute a long instruction or an instruction packet
+ Super Pipeline