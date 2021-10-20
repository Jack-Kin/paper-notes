## What problem is the paper solving and why is it important?

How to store large-scale data efficiently and reliably has become a very difficult problem. Looking at Google’s internal applications, data access has the following characteristics:

1. Huge data set
2. Mostly sequential access
3. Multi-client concurrent append scenarios are common, and random write behavior is rare
4. Write once, read many times

## What was the previous state of the art?	

Not mentioned.

## How does the paper advanced the state of the art?

Not mentioned.

## How is the system designed?

The GFS cluster includes: a master and multiple chunkservers, and several clients will interact with them

GFS stores 2 kinds of data: metada and data. In GFS, files are divided into Chunks with fixed size and stored seperately.

The two main tasks of the Master are to manage file system Namespace information and file Chunk location information.  When the Master starts up, all metadata is loaded into memory. The advantage is that metadata operation is fast. In addition, due to the importance of metadata, the Master stores file metadata persistently. Every metadata update operation is logged first and then applied to the B+ tree in the memory. In this way, the update will not be lost even if an exception occurs, such as a power failure. To avoid slow startup due to large logs, the Master periodically collects logs (Checkpoint).

For read and write, GFS has specified operations (write uses the concept of lease). Read has 4 steps, and write has 7 steps.

GFS is a relaxed consistency model

GFS uses snapshots to create a file or directory tree backup, which can be used to back up files or create checkpoints (for recovery). GFS uses copy-on-write to write snapshots.

## What are the key insights from the design?

The starting point of this paper is to improve performance. When the amount of data on a single machine is too large, the data needs to be sharded on multiple servers. In order to improve fault tolerance, data needs to be replicated to multiple servers, and data duplication can lead to potential data inconsistencies. To improve consistency often leads to lower performance, which is exactly the opposite of the original intention. GFS discusses the above-mentioned topics: parallel performance, fault tolerance, replication, consistency, and gives the trade-offs Google makes in the production environment.

The master will store the log on the local disk instead of in the database. The reason is: the essence of the database is some kind of B-tree or hash table. In contrast, appending the log will be very efficient; moreover, by creating some checkpoint points in the log , The rebuilding state will also be faster.

## How is the design evaluated, what are the key results?

both  in Micro-benchmarks and in Real World Clusters

## What related problems are still open?

GFS has only 1 maste node, which will bring the following problems:

1. As the number of GFS applications increases, the number of files increases, and eventually the master runs out of memory to store metadata; You can add more memory, but there is always a limit to how much memory a single computer can have.
2. The CPU of a master node can only process hundreds of requests per second. In particular, the number of clients exceeds the capacity of a single master
3. Weak consistency makes it difficult for applications to handle the strange semantics of GFS
4. Failover of the master node is not automatic, requiring human intervention to deal with the master node that has permanently failed and replace it with a new server, which can take tens of minutes or more to process and is too long for some applications

 
