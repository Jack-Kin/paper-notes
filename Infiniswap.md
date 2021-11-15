# INFINISWAP



## What problem is the paper solving and why is it important?

Memory-intensive applications are widely used today for low-latency services and data-
intensive analytics alike. These applications experience rapid performance deteriorations when their working sets do not fully fit in memory.



## What was the previous state of the art?	

SOTA:

Memory Blade; HPBD/NBDX; RDMA key-value service (HERD, FaRM), Intel Rack Scale Architecture (RSA)



Some previous works include:

Accelio based network block device

An exploration of network RAM

Leveraging endpoint flexibility in data-intensive clusters

Cashmere-VLM: Remote memory paging for software distributed shared memory

The network RamDisk: Using remote memory on heterogeneous NOWs

Swapping to remote memory over Infiniband: An approach using a high performance network block device

Nswap: A network swapping module for Linux clusters



## How does the paper advanced the state of the art?

INFINISWAP is a new scalable, decentralized remote memory paging solution that
enables efficient memory disaggregation. It is designed specifically for RDMA networks to perform remote memory paging when applications cannot fit their working sets in local memory.  Unlike existing solutions, it does not incur high remote CPU overheads, scalability concerns from central coordination to find machines with free memory, and large performance loss due to evictions from, and failures of, remote memory.



## How is the system designed?

INFINISWAP consists of two primary components – INFINISWAP block device and INFINISWAP daemon. 

The I NFINISWAP block device exposes a conventional block device I/O interface to the virtual memory manager (VMM), which treats it as a fixed-size swap partition. The entire address space of this device is logically partitioned into fixed-size slabs. Slabs from the same device can be mapped to multiple remote machines’ memory for performance and load balancing. All pages belonging to the same slab are mapped to the same remote machine.

If a slab is mapped to remote memory, INFINISWAP synchronously writes a page-out request for that slab to remote memory using RDMA WRITE, while writing it asynchronously to the local disk. If it is not mapped, INFINISWAP synchronously writes the page only to the local disk. For page-in requests or reads, INFINISWAP consults the slab mapping to read from the appropriate
source; it uses RDMA READ for remote memory.

The INFINISWAP daemon runs in the user space and only participates in control plane activities. Specifically, it responds to slab-mapping requests from INFINISWAP block devices, preallocates its local memory when possible to minimize time overheads in slab-mapping initialization, and proactively evicts slabs, when necessary, to ensure minimal impact on local applications. 



## What are the key insights from the design?

It uses Remote paging to avoid hardware design and application modification.

It also uses Local backup disk to have fault-tolerance

The Slab management is novel.



## How is the design evaluated, what are the key results?

Having 50% working sets in memory, application performance is improved by 2-16x.

Under 90 containers (applications), mixing all applications and memory constraints, Cluster memory utilization is improved from 40.8% to 60% by using INFINISWAP.



## Open problems? 

 Paging-aware data structure placement (by modifying VoltDB) 

 INFINISWAP’s meta-data management overhead due to large slabs, Selecting the optimal slab
size to find a good balance between management overhead and memory efficiency

Trade-off in fault-tolerance( Fault-tolerance vs. space-efficiency): Local disk is the bottleneck and Multiple remote replicas

