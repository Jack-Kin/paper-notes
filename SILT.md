因为SILT是基于固态盘的单节点存储系统，了解固态盘读写的性能是至关重要的:

## What problem is the paper solving and why is it important?

Presents a new flash-based key-value storage system to reduce per-key memory consumption with predictable system performance

and lifetime.

As key-value stores scale in both size and importance, index memory efficiency is increasingly becoming one of the most important factors for the system’s scalability and overall cost effectiveness. Flash capacity is growing rapidly from 2008 to 2011. 

## What was the previous state of the art?

BufferHash@NSDI10，FlashStore@VLDB10，ChunkStash@ATC10，SkimpyStash@SIGMOD11，SILT@SOSP11，BloomStore@MSST12

There is a memory overhead and lookup performance of the recent key-value stores.

## How does the paper advanced the state of the art?

 SILT requires approximately 0.7 bytes of DRAM per key-value entry and uses on average only 1.01 flash reads to handle lookups. 

## How is the system designed?

LogStore is a log-structure, responsible for processing Put and Delete operations. LogStore uses an in-memory hash table to map the key to its location on the solid-state disk. The hash table can function as a filter and an index at the same time.Because each key requires a 4-byte pointer, the memory overhead is high. 

Once the LogStore is full, it will be converted to HashStore. HashStore is temporary. HashStore is introduced to avoid frequent merging of SortedStore. HashStore stores data in the hash order of keys, so no memory index is needed, but a filter with less memory overhead is used. 

When the number of HashStore reaches the upper limit, it will be merged into SortedStore in batches. SortedStore is sorted according to the natural order of keys, and then represented by a prefix tree. By combining the compression algorithm, a very low memory overhead can be achieved.

For each insert operation, the KV pair is written to the LogStore. The delete operation is equivalent to inserting a "delete". For each query operation, the LogStore, HashStore, and SortedStore are queried in turn. Once found, the search stops immediately and the result is returned. If "delete" is found, indicating that the data has been deleted, return.

## What are the key insights from the design?

Multi-Store design.

## How is the design evaluated, what are the key results?

Full System Benchmark

​	Throughput: SILT can sustain an average insert rate of 3,000 1 KiB key-value pairs per second, while simultaneously supporting 33,000 queries/second, or 69% of the SSD’s random read capacity.

​		With no inserts, SILT supports 46 K queries per second (96% of the drive’s raw capacity), and with no queries, can sustain an insert rate of approximately 23 K inserts per second. 

​	Latency: 367 us on average for 100% GET queries for 1KiB key-value entries.

## What related problems are still open?

Borrow SILT's idea to develop new modular storage systems.
