## What problem is the paper solving and why is it important?

In the 90s, the CPU is becoming faster and faster, and the RAM size is becoming larger and larger, so reading is not the primary problem, writing matters because it mush be reflected on the disk. Also, for increasing disk performance,  access time is limited to the mechanical design  that is hard to improve. 

On the other hand,  the existing file systems at that time have too much small accesses. For example, Unix FFS takes 5 disk I/Os to create a new file. These file systems also write synchronously, and for workloads with many small files, the disk traffic is dominated by synchronous metadata writes. 

LFS is proposed to improve the write performance by buffering and writing, and convert the small synchronous random writes into large asynchronous random writes to utilize near 100% disk bandwidth.

 

## What was the previous state of the art?

Unix FFS. It can be improved with logging, delayed writes and disk request sorting to achieve 25% of the bandwidth.

## How does the paper advanced the state of the art?

This paper proposed a new structure called Log-Structured File System. *Write Cost* is used to compare cleaning policies. 

When the segments cleaned has a utilization of less than 0.8, the log-structured file system outperforms the current Unix System. When less than 0.5, the log-structured file system outperforms an improved Unix System.

## How is the system designed?

The basic idea is: The entire disk is treated as an append only log, and always write sequentially. Whenever we write a new file, we always append to the end of the log sequentially.

In LFS, free space is managed by fixed-size segments. The hard disk is divided into fixed-size segments; write operations are first written to the memory; when the data cached in the memory exceeds the segment size, LFS writes the data to the free segment at a time.

In LFS segments, file contents are stored in fixed-size data blocks. The segments also contains dynamically allocated inode (including file's attributes and the address of the data blocks), and an index of inode called inode map. 

LFS segments also contains a segment summary block to distinguish live blocks. A segment summary includes the file number and the according block number. It is used to do segment cleaning.

LFS stores Checkpoint on the entire hard drive. In Checkpoint, LFS stores the address of the first Segment and the last Segment of the linked list, so simply reading Checkpoint can restore the entire file system.

## What are the key insights from the design?

The basic assumption of the log structure file system-low read cost, high write cost- is very true even today. 

The idea of using segment and the segment cleaning mechanism is important.

How is the design evaluated, what are the key results?

*Write Cost* and *cost-benefit* is used to compare cleaning policies. 

hot and cold segments should be treated differently by the cleaner: Free space in a cold segment is more valuable than free space in a hot segment.

The cost-benefit approach is important.

## What related problems are still open?

If you read a file for the first time, then it takes time to construct the inode map in the memory. 

It is assumed that most reads will be optimized through the ever-expanding memory cache. This assumption is not always true:

- On magnetic media, seek is expensive, the LFS may make reads much slower because of the fragments.
- In flash memory, seek times are usually negligible, write fragmentation is less influential on write throughput.

