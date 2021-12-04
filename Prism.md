 

# Efficient Migrations Between Storage Tiers with PrismDB



## What problem is the paper solving and why is it important?

Most of the  new architectures choose one point in the design space: fast but expensive / cheap but slower. This paper want to achieve a Pareto efficient balance between performance and cost-per-bit. This paper rethinks the entire design of key-value stores to fully exploit fast and slow storage tiers, in order to realize more optimal trade-offs between performance, endurance, and cost. PrismDB’s primary design goals are to maximize read and write performance, while minimizing the amount of I/O (writes)  issued to flash.



## What was the previous state of the art?	

RocksDB



## How does the paper advanced the state of the art?

This paper designed a new key-value store, PrismDB. Compared to the standard use of RocksDB on flash in datacenters today, PrismDB’s average throughput on tiered storage is 3.3× faster and its read tail latency is 2× better, using equivalently-priced hardware.



## How is the system designed?

PrismDB uses a hybrid data layout, where hot objects are stored in slab-based files on NVM where random access is fast, and cold objects are stored in a sorted log on flash where sequential writes are prioritized.  To estimate the access frequency of objects, PrismDB uses a lightweight object popularity mechanism based on the clock algorithm.  PrismDB uses a partitioned, shared-nothing architecture, minimizing the amount of synchronization between threads.



## What are the key insights from the design?

A novel migration mechanism for multi-tiered storage systems that captures the relationship between key popularity and write amplification on multitiered storage.



## How is the design evaluated, what are the key results?

YCSB experiments (Table 5) are run using 8 concurrent clients. Compare PrismDB against three baselines:
RocksDB, RocksDB that uses NVM as an L2 read cache (labeled rocksdb-l2c) and Mutant.

For regular RocksDB, NVM outperforms single-tier TLC NAND, which outperforms the single-tier
QLC NAND setup, which uses denser and slower flash. Similarly, across the multi-tier setups, the higher the proportion of NVM, the better the performance.

PrismDB is able to provide a throughput benefit under all distributions. PrismDB provides better read latency

## Open problems? 

