## Concepts

**What is blk-mq:**

<iframe frameborder="0" allowfullscreen="true" src="https://www.kernel.org/doc/html/latest/block/blk-mq.html"></iframe>

**What is NIC:**

Network Interface Controller

**What is numa:**

Non-uniform memory access

where the memory access time depends on the memory location relative to the processor.

## What problem is the paper solving and why is it important?

To design a storage stack that can both achieve us-scale tail latency and high throughput. 

It is important because none of the existing SOTA storage stack can achieve the two goals simultaneously.

## What was the previous state of the art?

Two SOTA storage stacks: Linux and SPDK.

- Linux: suffer from high tail latencies due to head-of-line blocking. Require careful orchestration of compute, storage and network.
- SPDK: Good for 1 application 1 core. But when  multiple applications share a core, storage stacks and kernel CPU scheduler interacts frequently.

## How does the paper advanced the state of the art?

To simultaneously enable μs-scale tail latency for L-apps and high throughput for T-apps

In comparison to SPDK, blk-switch improves the average and P99 tail latency by as much as 12× and 18×, respectively, while achieving comparable or higher throughput

## How is the system designed?

**Switched-architecture (load balancing):**

1. Blk-switch introduced a per-core, multi-egress queue block layer architecture. Each egress queue is mapped into a driver queue.  The egress queues are assigned priorities to the kernel thread based on application performance goals (Latency-sensitive/Throughout-bound).
2. Blk-switch can decouple request processing from application cores in order to efficiently utilize all cores.  Requests submitted at a core can be steered from the ingress core to another core's egress queue. After processing, the response would be routed back to the original application core.

**Request Steering (handling transient loads):**

1. Steers T-app request at ingress queues of transiently overloaded cores to other cores' egress queues.
2. No implementation on remote storage server side because it's unnecessary.

**Application Steering (handling persistent loads):**

1. Steers threads from persistently overloaded cores to underutilized cores.
2. Implemented at both the application side and the remote storage server.

## What are the key insights from the design?

1. Linux’s per-core block layer queues + modern multi-queue storage + network hardware -> makes the storage stack conceptually similar to network switches.
2. Decouple ingress (application-side block layer) queues from egress (device-side driver) queues.
3. Different timescales for different Steering(T-app request scale for Request Steering and coarse-grained timescales for Application Steering)

## How is the design evaluated, what are the key results?

**Goal**: μs-scale average and tail latency &  high throughput for T-apps

**Evaluation Setup**

1. **Evaluated Systems:** , Linux, SPDK, blk-switch
2. **Performance metrics:**  average and tail latency(P99 & P99.9) for L-apps, total throughput of all applications, and throughput-per-core
3. **Default workload:** Linux-FIO, SPDK-perf, blk-switch-RocksDB
4. **To push the bottleneck to the storage stack processing:** Two 32-core servers connected directly over 100Gbps

**Achievements**

1.  Increase 

   L-apps

    competing for host compute and network resources with T-apps:

   - - Linux: high throughput but high latency: HOL blocking 
     - SPDK: increasing Lapps, inflated latency and degraded throughput: undesirable interactions between the storage stack and the kernel CPU scheduler
     - Another reason: increased L3 cache miss rate. Higher cache miss rates lead to an increase in the per-byte CPU overhead for kernel TCP processing (mainly dusre to data copy), resulting in lower throughput-per-core.
     - Blk-switch: 
       - - Application steering: isolate Lapps and Tapps
         - Request steering: utilize unused Lapps cores 
         - Prioritizing

2. Increase **L-app threads** competing for host compute and network resources with T-apps:

3. Increase the load induced by T-apps:

4.  blk-switch maintains its μs-scale average and tail latency for L-apps with varying number of cores, even when scheduling across NUMA nodes

5. blk-switch is able to maintain low average and tail latencies even when applications operate at throughput close to **200Gbps**.

6. blk-switch latency is largely overshadowed by SSD/RocksDB  access latency

## What related problems are still open?

1. How to improve the performance of blk-switch on SSD & RocksDB?
2. How to accommodate blk-switch to harware-level isolation?
3. Is it possible to use blk-switch in Linux?

[optional] What questions do you have?

The concept of head-of-line is not very clear for me. 
