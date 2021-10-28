# A large scale analysis of hundreds of in-memory cache clusters at Twitter



## What problem is the paper solving and why is it important?

In-memory caching is ubiquitous in the modern web services. The effectiveness and performance of in-memory caching can be workload dependent.   However, there remains a significant gap in the understanding of current in-memory caching workloads. This paper tries to answer the questions such as how are in-memory caches used and do existing assumptions about in-memory still hold.



## What was the previous state of the art?	

 Firstly, there has been a lack of comprehensive studies covering the wide range of use cases in today’s production systems. Secondly, there have been new trends in in-memory caching usage since the publication of previous work. Thirdly, some aspects of in-memory caching received little attention in the existing studies, but are known as critical to practitioners. Lastly, there has been a lack of open-source in-memory caching traces. Researchers have to rely on storage caching trace, key-value database benchmarks or synthetic workloads to evaluate in-memory caching systems.

## How does the paper advanced the state of the art?

This paper bridges the  gap in the understanding of current in-memory caching workloads by collecting and analyzing workload traces from 153 Twemcache clusters at Twitter



## How is the system designed?

There is no new design part. This is  a paper focusing on the anaylsis of Twitter.





## What are the key insights from the design?

Minimizing object metadata to increase effective cache size.

Size distribution changes pose challenges to memory management. 

Innovations needed on better memory management techniques.

Efficient proactive expiration techniques are more important than evictions 

Innovation needed on efficient TTL expiration

## How is the design evaluated, what are the key results?

Twitter's object sizes are small (median 230 bytes), while the metadata size is large (56 bytes per-obj metadata). For future research, we need to minimizing object metadata to increase effective cache size. Size distribution can be dynamic as well. Besides, sudden changes are not rate, there are several heat map showing this. The implication for future research is that: 1. size distribution changes pose challenges to memory management. 2. Innovations needed on better memory management techniques. TTL use cases include bounding inconsistency, periodic refresh and implicit deletion. A figure shows mean TTL across clusters. TTLs are usually short. For short TTLs, there is no need for a huge cache size if expired objects can be removed in time. Production caches have small miss ratio and small variations. Request spikes are not always caused by hot keys

