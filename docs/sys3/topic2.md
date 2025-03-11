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
+ Data hazard
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
        + use Branch Prediction buffer(aka branch history table)
        + 转换方式

            <img src="../4.png" style="max-width: 80%; height: auto;">
        + 1-Bit Predictor
            + 效率过低
        + 2-Bit Predictor
            
            <img src="../5.png" style="max-width: 60%; height: auto;">

            + 相当于(-2,-1,1,2)四种程度，如果预判对了就加强判断，否则削减判断的力度。
        + n-Bit Predictor

## Overlapping Execution
+ conflict in access memory
    + adding instruction buffer between memory and instruction decode unit

## Dynamic Scheduling
+ 流水线运行的弊端在于如果指令之间存在依赖，流水线就必须停止运行直到相关操作处理完成。
+ 实现动态规划，相当于将指令重新排列但不改变其功能以解决冒险。
+ 将经典流水线的 ID 阶段拆分成