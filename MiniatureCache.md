# Cache Modeling and Optimization using Miniature Simulations



## What problem is the paper solving and why is it important?

Caching is important and ubiquitous. Techniques for accurate and efficient cache modeling are especially valuable,  because performing optimizations without having good cache models is impossible. A key problem is the lack of any prior solution that's general and lightweight enough to be used for online optimization.



## What was the previous state of the art?	

First is the brute force method dating back to at least the 1960s, and that's to construct a discrete MRC by running separate simulations for each and every cash size of interest. Now, this always works, but it's very expensive in both time and space.

Similar work by Mason and his colleagues at IBM in 1960 produced a single pass method that computes an entire MRC in one shot, but this only works for stack algorithms that exhibit an inclusion property like LRU.

Counterstacks from 2014 requires logarithmic space.

The authors'  work on shards from 2015 was the first constant space algorithm, and the AET algorithm from 2016 ATC also runs in constant space. The resurgence of interest in this area has been very exciting, but there are no existing methods that are both general and efficient, which is what prompted us to develop miniature simulation.

## How does the paper advanced the state of the art?

Simulate a large cache by using a tiny one. Miniature simulation scales down both the reference stream and the cache size. We use hashing to randomly sample the key space. Since the simulationruns an unmodified algorithm, we can model any Caching algorithm.



## How is the system designed?

1. Scaling Down: We can scale down an emulated cache size to a smaller mini size using a sampling Rad, so the mini size is simply R * the emulated size.
2. Mini-Sim Cache Tuning: It's a Dynamic multi-model optimization, which simulates candidate configurations online and periodically applies best to actual cache.
3. SLIDE: Sharded List with Internal Differential Eviction. It uses single unified cache, no hard partitions. It defers partitioning decisions until eviction. It avoids resizing, migration and complexity issues. There is also a new SLIDE list abstraction, which replaces internal LRU/FIFO building blocks.



## What are the key insights from the design?

Scaling Down, Mini-Sim Cache Tuning and SLIDE. Mini-sim is extremely effective nad it can optimizes workloads and policies automatically.



## How is the design evaluated, what are the key results?

1. Scaling Down: the Mini-Sim approximations are extremely close to the exact curves. The average absolute miss ratio error is below one percentage point of the standard box plots for three algorithms is below 1%. For time and space, Mini-Sim makes 200 times space smaller and 10 times faster with R=0.001
2. Mini-Sim Cache Tuning: LIRS S stack size, 5 mini-sims with f = 1.1— 3. 2Q A1out size, 8 mini-sims with Kout = 50% — 300%. R = 0.005, epoch = 1M refs
3.  SLIDE: it constructs MRC online and updates SLIDE settings periodically



## What problems are still open?

It is not clear how to apply their shadow-queue technique to more complex caching policies. Non-monotonicity may also present problems; even a small local bump in the MRC could be misinterpreted as the single cliff to be removed.

