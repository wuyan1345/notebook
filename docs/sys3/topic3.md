---
count: true
---

# Memory Hierarchy(Cache)

+ Different Types of Memory
    + **Registers**: Fastest, smallest, used for immediate data.
    + **Cache**: Faster than main memory, stores frequently accessed data.
    + **Main Memory (RAM)**: Slower than cache, larger capacity.
    + **Secondary Storage**: Slowest, used for long-term storage (e.g., HDD, SSD).

## Cache
!!! tip
    a safe place for hiding or storing things.

+ whether split
    + Split Cache: Separate instruction and data caches.
    + Unified Cache: Combined instruction and data cache.
+ Block Size
    + A fixed-size collection of data containing the requested word, retrieved from the main memory and placed into the cache
    + Larger blocks can exploit spatial locality but may increase miss penalty.
+ Block placement
    + **Direct Mapped**: Each block maps to exactly one cache line.
    + **Fully Associative**: Any block can go into any cache line.
    + **Set Associative**: A compromise between direct mapped and fully associative.
+ Block identification
    + **Tag**: Identifies which block is stored in a cache line.
    + **Index**: Identifies the cache line.
    + **Offset**: Identifies the specific word within a block.
+ Block replacement
    + **LRU (Least Recently Used)**: Replaces the least recently used block.
    + **FIFO (First In, First Out)**: Replaces the oldest block.
    + **Random**: Randomly selects a block to replace.
    + **OPT (Optimal)**: Replaces the block that will not be used for the longest time in the future (theoretical).
+ Write Streategy
    + **Write-Through**: Writes data to both cache and main memory.
    + **Write-Back**: Writes data only to cache, updating main memory later.
+ Cache size: block size + tag size + valid
+ flip-flop
    + A flip-flop is a basic memory element that can store one bit of data.
    + It consists of two stable states, representing binary 0 and 1.
    + Flip-flops are used in cache to store the valid bit and tag information.

## Memory System Performance
<img src="../9.png" style="max-width: 80%; height: auto;">