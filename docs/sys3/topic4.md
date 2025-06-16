---
count: true
---

# DLP and TLP

## DLP (Data-Level Parallelism)
+ SIMD
    + Vector architecture
        + Single Instruction, Multiple Data (SIMD) allows the same operation to be performed on multiple data points simultaneously.
        + Example: Vector addition where the same operation is applied to each element of two vectors.
        + Array processors
            <img src="../10.png" style="max-width: 80%; height: auto;">
    + Multimedia instructions
        + Instructions designed to handle multimedia data types (e.g., images, audio).
        + Examples: MMX, SSE, AVX instructions in x86 architecture.
    + Graphics Processing Units (GPUs)
        + Highly parallel architecture designed for processing large data sets.
        + GPUs can execute thousands of threads simultaneously, making them suitable for tasks like image processing and machine learning.

## TLP (Thread-Level Parallelism)
+ MIMD
    + Multiple Instruction, Multiple Data (MIMD) allows different instructions to be executed on different data streams.
    + Example: Superscalar processors that can execute multiple instructions from different threads simultaneously.
+ UMA
    + Uniform Memory Access (UMA) architecture where all processors share a common memory space.
    + Example: Multi-core processors where each core can access the same memory.
+ NUMA
    + Non-Uniform Memory Access (NUMA) architecture where memory access time depends on the memory location relative to the processor.
    + Example: Multi-socket systems where each socket has its own local memory, and accessing remote memory is slower.