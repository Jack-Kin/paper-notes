 

# LinnOS: Predictability on Unpredictable Flash Storage with a Light Neural Network

## What problem is the paper solving and why is it important?

More and more data will be real-time, which need large-volume data with microsecond-level access latencies. However, SSD's internal complex complexity is a significant factor that results in bad latency. It manages all of its internal resources with background operations such as
garbage collection, buffer flushing, wear leveling, and read repairs, which  poses a threat to latency predictability



## What was the previous state of the art?	

While/gray-box, such as re-architect device internals. But it needs to modify hardware

Black-box, such as SSD-aware file systems and applications. But is needs considerable re-design in software stack.



## How does the paper advanced the state of the art?

Speculative execution can mitigate every slow I/O by sending a duplicate I/O to another node or device, but it is agnostic, and it needs to passively wait due tot black-box. LinnOS can proactively infer the black-box by using a lightweight neural network for per-I/O speed inference.

Latency stability at even p99.99, and average latency improved by up to 80%.



## How is the system designed?

The input features include:  the number of pending I/Os when an incoming I/O arrives,  the latency of the R most-recently completed I/Os, and the number of pending I/Os at the time when each of the R completed I/Os arrived. The network has 3 layers. Input layer is supplied with the 31 features. The raw information from the block layer is converted to the feature format, in an offline way for training and an online way for live inference. Next, the hidden layer consists of 256 regular neurons. This layer uses RELU activation functions for its low computation cost and ability to support non-linear modeling.  Lastly, the output layer has two neurons with linear activation functions. They use an argmax operator to convert the output to a binary decision





## What are the key insights from the design?

Let the device be the device (black-box) and do not redesign the file systems or applications, but learn the device behavior.



## How is the design evaluated, what are the key results?

87-97% accuracy with 4-6 us overhead



## Open problems? 

How to further lower the inference cost to support faster devices and higher throughput?

 Can advanced accelerators help accelerate OS kernel operations?

Can near-storage/data processing help? 

Can we skip the inference when the outcome is highly assured?

Can we cache the approximation results for popular predictions?

How should “ML-for-system” solutions mask the cases that machine learning fails to catch, while still benefiting from its generality? 

Is marrying learning and heuristic a powerful option that exploits the advantages of both worlds?

Can fast/slow inference be converted to a more precise latency inference, such as latency ranges, percentile buckets , or precise latency with high accuracy?
