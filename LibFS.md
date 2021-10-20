 

## What problem is the paper solving and why is it important?

In traditional operating systems, only privileged servers and the kernel can manage system resources. Untrusted applications are restricted to the interfaces and implementations of this privileged software. This organization is flawed because application demands vary widely. An interface designed to accommodate every application must anticipate all possible needs. The implementation of such an interface would need to resolve all tradeoffs and anticipate all ways the interface could be used. The exokernel architecture solves this problem by giving untrusted applications as much control over resources as possible.

## What was the previous state of the art?	

https://i.imgur.com/qGREjSc.png

Exokernel Principles:

1. Separate protection and management
2. Expose allocation (applications allocate resources explicitly)
3. Expose names (physical names, reduce costly translations)
4. Reduce revocation (each application has control over its set of physical resources)
5. Expose information

 The exokernel architecture:

1. Kernel support for protected abstraction (Xok)
   1. performs access control on all resources in the same manner
   2. provides software abstractions to bind ahrdware resources together
   3. some of Xok's abstractions allow applications to download code
2. Protected sharing with 4 mechanisms when meeting 3 levels of trust including Mutual trust, Unidirectional trust and Defensive programming for mutual distrust

XN:

1. Disk-block-level multiplexing
2. Self-descriptive metadata
3. Template-based description

## How does the paper advanced the state of the art?

XN provides access to stable storage at the level of disk blocks.

XN’s novel solution employs UDFs

## How is the system designed?

1. Ordered disk writes
2.  The buffer cache registry
3.  C-FFS: a library file system

## What are the key insights from the design?

the novel design of untrusted deterministic functions.

How is the design evaluated, what are the key results?

using Xok/ExOS.

## What related problems are still open?

To implement a range of file systems (log-structured file systems, RAID, and memory-based file systems), thus testing if the XN interface is powerful enough to support concurrent use by radically different file systems. 

To investigate using lightweight protected methods like UDFs to implement the simple protection checks required by higher-level abstractions.
