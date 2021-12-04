# Learning Relaxed Belady for Content Distribution Network Caching



## What problem is the paper solving and why is it important?

Caching in CDNs. Whenever a user requests content that is not currently cached, the CDN must fetch this content across Internet Service Providers (ISPs) and WANs. While CDNs are paid for the bytes delivered to users, they need to pay for the band-width required to fetch cache misses. These bandwidth costs constitute an increasingly large factor in the operational cost of large CDNs. CDNs are thus seeking to minimize these costs, which are typically measured as byte miss ratios



## What was the previous state of the art?	

Heuristic-based algorithms:  LRU, LRUK, GDSF, ARC, 

ML-based adaptation of heuristics: UCB, LeCAR

Belady’s MIN algorithm



## How does the paper advanced the state of the art?

 Learning Relaxed Belady (LRB) mimics Belady using machine learning to find objects to
evict based on past access patterns. It's not build on heuristics, nor try to optimize heuristics-based algorithms. 



## How is the system designed?

Relaxed Belady Algorithm & Good Decision Ratio.  Belady boundary is introduced, so that Relaxed Belady evicts an objects beyond boundary. Also the paper introduces an eviction decision quality metric, the good decision ratio, that evaluates if the next request of an evicted object is beyond the Belady boundary, so that quality of decisions made by an ML architecture without running a full simulation can be evaluated. 

The cache track past object within the memory window, and periodically sample object to generate a label training data set. After the object is accessed again or passes the memory window, we label the corresponding training data  

ML Architecture: Features include Object size, Object type, Inter-request distances (recency) and Exponential decay counters (long-term frequencies). The model is Gradient boosting decision trees because it's lightweight.

To select evict candidates, it randomly samples k candidates from the cache. And a small sample can mimic relaxed Belady if we can find 1 object beyond the boundary. k is chosen as 64.



## What are the key insights from the design?

Relaxed Belady gives us a way to avoid individual eviction decision.



## How is the design evaluated, what are the key results?

Simulator implementation : LRB + 14 other algorithms. Prototype implementation: C++ on top of production system (Apache Traffic Server)  and also appli ed many optimizations

Traffic reduction: 20% traffic reduction over B-LRU and 10% reduction over the best SOTA on Wikipedia traces.

 overhead of LRB vs CDN production system: Throughput achieves 11.7 Gbps, which is close to the production system. The peak CPU rate is 16%, which is higher than the 9% on the production system.



## Open problems? 

How to better  tuning LRB’s hyper parameters

