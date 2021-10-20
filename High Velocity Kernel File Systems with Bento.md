 

https://www.usenix.org/conference/fast21/presentation/miller

## What problem is the paper solving and why is it important?

High development velocity for file systems is critical and challenging. It allows developers to quickly distributes new features to users (arch). 

The kernel code is too large, difficult to debug, prone to introducing bugs, unable to avoid service interruptions during redeployment,high performance penalty......

## What was the previous state of the art?

​		Performance 	Safety		General Programmability 	Live Upgrade 		Easy Debugging

VFS     Yes					No		  		    Yes									No							No

FUSE	No					Yes					Yes									No							Yes

eBPF	Yes					Yes					 No									No							No				

## How does the paper advanced the state of the art?

1. enables file systems written in safe Rust
2. errors are sandboxed to the file system
3. can be replaced without disruption, upgrades in a cloud server setting
4. supports user space debugging

- Bento performs similarly to VFS-native ext4 on a variety of benchmarks ,while outperforms a FUSE version by 7x on ‘git clone’. 
- Bento can dynamically add file provenance tracking to a running kernel file system with only 15ms of service interruption.

## How is the system designed?

Bento is composed of modules such as BentoFS, libBentoFS and libBentoKS. Among them, the C language used by BentoFS is developed as a separate module, and the other two are developed by rust and compiled into the file system module (the file system and these two modules are compiled together into a static file ). Beno’s design goal is to support file systems written in rust

- BentoFS receives the calls from the VPS and forwards the calls to the correct file system (BentoFS maintains multiple active file systems). When the file system is loaded, it applies to BentoFS for registration and is added to the file system list.
- libBentoFS is located between BentoFS and File System and is responsible for converting unsaft C calls from BentoFS into saft Rust calls (C type to rust type. Pointer converted to reference).
- libBentoKS is located between Filesystem and the rest of the kernel modules, and is responsible for providing a secure abstraction layer for kernel services.
- Upgrade is an online update module that supports hot upgrades.
- User-level debug support, using the fuse module to forward the request to the user space
- User mode execution, file system can be compiled in user space

## What are the key insights from the design?

- The idea of using Rust for a file system
- Separate Bento into BentoFS, libBentoFS, and libBentoKS.

## How is the design evaluated, what are the key results?

Can Bento support competitive performance?

## How does live upgrade impact availability?

Baselines:

- Ext4 wth data  journaling (data=journal)
- Ext4 with default metadata journaling (data=ordered)

Benchmarks:

- Untar, tar, grep on Linux Git clone on xv6
- Bento-fs similar to ext4
- Fuse is slower

## What related problems are still open?

- Extend software verification to handle concurrency and high performance file systems.
- Design a stackable file systems supporting complex file system data structures and provide safety guarantees about the C code.
