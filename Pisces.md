# Performance Isolation and Fairness for Multi-Tenant Cloud Storage



## What problem is the paper solving and why is it important?

most systems today provide weak performance isolation and fairness between tenants



## What was the previous state of the art?	

Network Sharing: SecondNet, Oktopus, NetShare, DaVinci

Service Sharing: Parda, mClock, Argon, Stout

Amazon DynamoDB do not provide fairness, assume uniform load distributions
across tenant partitions, and are not work conserving



## How does the paper advanced the state of the art?

Pisces is a system for achieving datacenter-wide per-tenant performance isolation and fair-
ness in shared key-value storage.  Today’s approaches for multi-tenant resource allocation are based either on per-VM allocations or hard rate limits that assume uniform workloads to achieve high utilization. Pisces achieves per-tenant weighted fair shares (or minimal rates) of the aggregate resources of the shared service, even when different tenants’ partitions are co-located and when demand for different partitions is skewed, time-varying, or bottlenecked by different server resources.



## How is the system designed?

Four complementary mechanisms: partition placement, weight allocation, replica selection, and weighted fair queuing. To achieve system-wide fairness, partitions are placed with respect to demand and node capacity constraints. Local weights  give tenants throughput where they need it most. Replicas are selected in a weight-sensitive manner. And request queuing is enforce dominant resource fairness



## What are the key insights from the design?

Novel mechanism decomposition and novel algorithms



## How is the design evaluated, what are the key results?

achieves nearly ideal (0.99 Min-Max Ratio) weighted fair sharing, strong performance isolation, and robustness to skew and shifts in tenant demand.



## Open problems? 

NA
