## What problem is the paper solving and why is it important?

Storage devices have lagged behind networking devices in achieving high bandwidth and low latency.

Goal: a standard **OS-supported** mechanism in Linux 

## What was the previous state of the art?	

Kernel-bypass frameworks ,e.g. SPDK  and near-storage processing reduce kernel overheads. However, there are clear drawbacks to both such as significant, often bespoke,

application-level changes, lack of fine-grained isolation, wasteful busy waiting, and the need for specialized hardware in the case of computational storage

## How does the paper advanced the state of the art?

networking community: by using storage BPF， the average latency breakdown is reduced.

## How is the system designed?

envision similar use of BPF for storage by removing the need to traverse the kernel’s storage stack and move data back and forth between the kernel and user space when issuing dependent storage requests.

## What are the key insights from the design?

Using the idea of exokernel file system designs to allow users to enable the kernel to understand their data layout

## How is the design evaluated, what are the key results?

They designed a benchmark that executes lookups on an on-disk B+-tree

## What related problems are still open?

interactions of BPF with the cache 

interactions of BPF with the scheduler policies
