## What problem is the paper solving and why is it important?

For distributed systems, the storage hierarchy extends to rack, cluster, and datacenter levels.

- The bandwidth is limited by the network, so the bandwidth of memory and disk is the same.
- The memory latency changes relatively large, and the disk latency itself has a relatively large base, so the change is not obvious.

Dirstibuted systems are a must.

Increase Scalability

Decrease Latency

1. more interactives
2. data is stored distributedly

Hardware

1. price doesn't go linerly
2. single node failure

## What was the previous state of the art?	

E.g.: BigTable System

How does the paper advanced the state of the art?

Bigtable's improvement over the original paper:

Improved performance isolation

Improved protection against corruption

## How is the system designed?

1. GFS
2. Making Applications Robust Against Failures
   1. Canary requests
   2. Backend detection
   3. More aggressive load balancing when imbalance is more severe
   4. Better to give users limited functionality than an error page
3. Add Sufficient Monitoring/Status/Debugging Hooks
   1. Export HTML-based status pages for easy diagnosis
   2. Export a collection of key-value pairs via a standard interface
   3. Support low-overhead online profiling 
   4. RPC subsystem collects sample of all requests
4. BigTable
   1. Replication
   2. Coprocessors

## What are the key insights from the design?

Jeff gives a view of how to build large distributed systems in multi-aspects.  Hardware, Software, Reliability, Application Robustness, System Monitoring, the file system and how the data are stored all matters when designing a large distributed system.

## How is the design evaluated, what are the key results?

Not mentioned

## What related problems are still open?

E.g: Design Goals for Spanner

– zones of semi-autonomous control

– consistency after disconnected operation

– users specify high-level desires

General model of consistency choices, explained and codified

Easy-to-use abstractions for resolving conflicting updates to multiple versions of a piece of state 
