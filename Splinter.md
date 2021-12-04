# Splinter: Bare-Metal Extensions for Multi-Tenant Low-Latency Storage



## What problem is the paper solving and why is it important?

Kernel-bypass key-value stores offer < 10μs latency, > Mops/s throughput. But is it fast just because they're dump? This simplicity results in inefficient data movement between storage and compute and costly client-side stalls. Granularity of compute is steadily decreasing, for example,  Virtual machines → Containers → Lambdas. We have extremely high tenant density, and we want to allow tenants to extend data model at runtime.

## What was the previous state of the art?	

N/A



## How does the paper advanced the state of the art?

Splinter is a multi-tenant inmemory key-value store with a new approach to pushing compute to storage servers. Splinter preserves the low remote access latency (9 µs) and high throughput (3.5 Mops/s) of in-memory storage while adding native-code runtime extensions and the dense multi-tenancy (thousands of tenants) needed in modern data centers.

Tenants can install and invoke extensions at runtime • Extensions written in Rust • Rely on type and memory safety for isolation, avoids context switch • Implemented in ~9000 lines of Rust • Supports two RPCs → install(ext_name) & invoke(ext_name) • Also supports regular get() & put() RPCs → “Native” operations

## How is the system designed?

Splinter uses Tenant Locality And Work Stealing to avoid cross-core coordination while avoiding hotspots. Splinter uses Lightweight Cooperative Scheduling to prevent long running extensions from starving short running ones. Splinter uses Low cost isolation for no forced data copies across trust boundary



## What are the key insights from the design?

to customize the storage tier of each application to support specialized data types. But it's hard to customize the data structure for multi-tenant services. So Splinter takes a different approach: it allows applications to push small pieces of native compute (extensions) to stores at runtime. These extensions can implement richer data types and operators, avoiding extra round trips and reducing data movement.



## How is the design evaluated, what are the key results?

First it tests the  latency with tenant skew. The server runs near saturation at 4 Mops/s in each case. Without work stealing, tail latency under high skew increases from 138 μs to 330 μs.
Without tenant locality, median and tail latencies are affected. From the graph, the higher tenant skew, the fewer active tenants. If there is no work stealing, it results in poor tail Latency under high skew.

Second, it investigates the impact of mixing short operations with cooperative longer-running operations. Yielding frequently can help improve median latency from 38 μs to 22 μs.
However, yielding too frequently hurts median latency. It also shows how uncooperative extensions impact system performance. System throughput stays constant at 3 Mops/s throughout. For fractions of uncooperative requests greater than 1 every million, tail latency is significantly affected.



## Open problems? 

Low-latency RDMA-based Storage Systems. 

