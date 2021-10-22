# Fast Crash Recovery in RAMCloud



## What problem is the paper solving and why is it important?

DRAM's role in storage is increasing. But it's difficult for developers to use DRAM effectively. RAMCloud is a general-purpose storage system that makes it easy for developers to harness the full performance potential of large-scale DRAM storage. It keeps all data in DRAM all the time, so there are no cache misses. RAMCloud storage is durable and available, so developers need not manage a separate backing store. RAMCloud is designed to scale to thousands of servers and hundreds of terabytes of data while providing uniform low-latency access (5-10 μs round-trip times for small read operations). Durability and availability without impacting system performance is important



## What was the previous state of the art?	

RAMCloud keeps only a single copy of data in DRAM; redundant copies are kept on disk or flash, which is both cheaper and more durable than DRAM. However, this means that a server
crash will leave some of the system’s data unavailable until it can be reconstructed from secondary storage.



## How does the paper advanced the state of the art?

RAMCloud’s solution to the availability problem is fast crash recovery: the system reconstructs the entire contents of a lost server’s memory (64 GB or more) from disk and resumes full service in 1-2 seconds. 



## How is the system designed?

Harnessing scale:  Each server scatters its backup data across all of the other servers, allowing thousands of disks to participate in recovery. Hundreds of recovery masters work together to avoid network and CPU bottlenecks while recovering data. 

 Log-structured storage provides high performance and simplifies many issues related to crash recovery.

Use Randomization to make decisions in a distributed and scalable fashion.

Use Tablet profiling to track the distribution of data within tables.



## What are the key insights from the design?

In memory storage. Using some database optimization kind of way in the recovery strategy.



## How is the design evaluated, what are the key results?

networking: infiniband. High bandwidth and low latency: NIC, bypassing kernel.

+ The presence of deleted versions will, if anything, make recovery faster, but deleted versions may not need to be rereplicated.
+ With large numbers of disks, the speed of recovery is limited by outbound network bandwidth on the recovery master
+ the full Segment Scattering algorithm improves recovery time by about 33% over a purely random placement mechanism

