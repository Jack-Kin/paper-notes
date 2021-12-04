# QD Interview Preparation

## 5 ways to speed up

1. stack vs heap:  use stack as much as possible. 

   Stack is local, heap is shared among multiple threads

   avoid `new`, `unique pointer`, `shared pointer`

2. short circuiting

   ```c++
   if (A && B) // good
   if (B && A) // bad
   if (A || B) // good
   if (B || A) // bad
   ```

3. minimize the branch (if...else...)

   - fetch instructions takes a lot of time

   - A  Good Way instead of branch  (CPU is good at handling 0 & 1)
      ```C++
     uint32_t errorCodes = WriteToFile();
     if (errorCodes)
         HandleError(errorCodes);
     else
     {
         // Do something else...
     }
     ```

4.  Not use Dynamic Polymorphism

   Stay away virtual methods, inheritance from classes that are abstract, etc

   Stay away from dynamic dispatch

   Reasons:

   1. Virtual methods cannot be inlined
      - Inlining a method makes the function calls a lot faster and comes at cost of a larger binary
   2. CPU cannot accurately predict the target of the virtual call at runtime, may lead to cache miss

5. Keep Cache warm





## BQ

### **Infiniband**

首先，**IB和以太网要解决的问题完全不同，所以在底层协议设计时的考虑也完全不一样。**

以太网设计初衷是，多个不同的系统，如何让他们之间流畅的进行信息交换？这是一种典型的network，它的设计中优先考虑的是兼容性与分布式。

IB要解决的问题是，在一套系统内部，如何将多个分开的部件整合起来，如同一台设备一样工作。我们可以把这种网络称为一种fabric，是一种互联技术，它的设计时考虑的优先是低延迟。

限制：

- IB网络的地址数量，称为LID号，是有限的，地址空间65535个

- 需要有个叫做subnet manager的调度中心来进行管理和调度，所以IB天生就是一个SDN网络

对于10G以上的高速网络流量，如果所有报文都由CPU处理封包解封包将会占用非常多的资源，这样相当于用昂贵CPU的大量资源资源仅仅是处理网络传输的简单任务，在资源配置的角度上看是一种浪费。 因而借鉴DMA技术的思路，旁路CPU，不仅提高了的数据传输带宽而且减轻的CPU的负担，定义了**RDMA**技术，从而解决了这一问题，使得在高速网络传输中对CPU实现了卸载，同时提高了网络的利用率。



### RDMA

DMA(直接内存访问)是一种能力，允许在计算机主板上的设备直接把数据发送到内存中去，数据搬运不需要CPU的参与。

RDMA是一种概念，在两个或者多个计算机进行通讯的时候使用DMA， 从一个主机的内存直接访问另一个主机的内存。

RDMA是一种host-offload, host-bypass技术，允许应用程序(包括存储)在它们的内存空间之间直接做数据传输。具有RDMA引擎的以太网卡(RNIC)--而不是host--负责管理源和目标之间的可靠连接

传统的TCP/IP技术在数据包处理过程中，要经过操作系统及其他软件层，需要占用大量的服务器资源和内存总线带宽，数据在系统内存、处理器缓存和网络控制器缓存之间来回进行复制移动，给服务器的CPU和内存造成了沉重负担。尤其是网络带宽、处理器速度与内存带宽三者的严重"不匹配性"，更加剧了网络延迟效应。

RDMA是一种新的直接内存访问技术，RDMA让计算机可以直接存取其他计算机的内存，而不需要经过处理器的处理。RDMA将数据从一个系统快速移动到远程系统的内存中，而不对操作系统造成任何影响。

优势：

1. 零拷贝(Zero-copy) - 应用程序能够直接执行数据传输，在不涉及到网络软件栈的情况下。数据能够被直接发送到缓冲区或者能够直接从缓冲区里接收，而不需要被复制到网络层。
2. 内核旁路(Kernel bypass) - 应用程序可以直接在用户态执行数据传输，不需要在内核态与用户态之间做上下文切换。
3. 不需要CPU干预(No CPU involvement) - 应用程序可以访问远程主机内存而不消耗远程主机中的任何CPU。远程主机内存能够被读取而不需要远程主机上的进程（或CPU)参与。远程主机的CPU的缓存(cache)不会被访问的内存内容所填充。
4. 消息基于事务(Message based transactions) - 数据被处理为离散消息而不是流，消除了应用程序将流切割为不同消息/事务的需求。
5. 支持分散/聚合条目(Scatter/gather entries support) - RDMA原生态支持分散/聚合。也就是说，读取多个内存缓冲区然后作为一个流发出去或者接收一个流然后写入到多个内存缓冲区里去。



### PMEM

https://zhuanlan.zhihu.com/p/391089447

![pmem_compare](https://pic3.zhimg.com/v2-3f6e2da315fb39126b6ba5e1779e2afe_b.jpg)

持久内存主要优势场景是什么？

**场景1：大内存低成本解决方案**

1. 基于内存性能的考量，你必须使用基于内存的解决方案，而不可能使用基于磁盘的方案，比如内存数据库（Redis, MemSQL）。

2. 虽然你的应用可以接受基于磁盘而带来的性能损耗，但是显然如果内存扩大，你的应用可以跑得更快更加节省时间，比如基于 Spark 搭建的应用。

**优势：**持久内存在单位价格上约为普通内存的一半，并且可以在单台机器上轻松达到 1.5 TB，甚至 3 TB 的内存大小。因此比如你目标需要 20 TB 的总内存容量，持久内存可能只需要10台机器即可满足，但是基于DDR内存的集群可能需要40台甚至更多。考虑机器投入以及运营上带来的成本，持久内存所带来的低成本解决方案的优势是显而易见的。



**场景2：高性能持久化需求的应用**

1. 消息队列：大家熟悉的开源消息队列 Kafka，由于其消息持久化逻辑的存在，其吞吐最终会卡在硬盘 IO 上。目前的解法是不断堆机器来扩展整个 Kafka 集群的吞吐。

2. 搜索系统：类似于 Kafka，流行的开源搜索系统 Elasticsearch，也将部分的数据结构存放在磁盘上。那么最终影响整体延迟和吞吐的将会是磁盘 IO 的性能。

3. 数据库或者KV存储引擎：比如 MySQL 或者 RocksDB，都具有重要的面向外存的数据持久化逻辑。

4. 分布式文件系统：在人工智能场景中，常常会有大量的小文件存在。比如在 Ceph 的文件系统中，在 metadata server 上对大量小文件的管理常常由于大量随机读写的存在而产生性能问题。

**优势**：显然，对于有高速持久化读写需求的场景，持久内存引入直接有了数量级的性能提升。在吞吐方面，由于单机吞吐提升，因此总的机器数量规模可以大量减少，在延迟方面则是提供了另一给维度的优势。具体性能比对可以参照上一节末尾给出的性能比对表格。



### Memcached

免费开源、高性能、分布式内存对象缓存系统，本质上是通用的，但旨在通过减轻数据库负载来加速动态web应用程序。Memcached是一个内存中的键值存储，用于存储来自数据库调用、API调用或页面呈现结果的小块任意数据(字符串、对象)。Memcached简单但功能强大。其简单的设计促进了快速部署，易于开发，并解决了大数据缓存面临的许多问题。它的API适用于大多数流行的语言。
