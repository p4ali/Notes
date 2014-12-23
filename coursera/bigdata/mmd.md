## Distributed File Systems
* Distributed File System
* Computational Model
* Scheduling and Data flow
* Refinements

### Why Map-Reduce
* classical data mining:
 * Single node architecture: CPU<=>Memory<=>Disk
 * Bottleneck: reading data from disk to memory. e.g., reading 200TB by 50Mb/sec disk takes 50 days.
 * solution: split data into chunks, and using multiple computers(clusters) to read
 * Cluster -> (switch) -> racks -> nodes (16-64 linux nodes, google has 1M nodes 2011)
* cluster computing challenges
 * Node failure (hardware)
  * How to store data persistently and keep it available if nodes can fail
  * Ho wot deal with node failure during a long running computation
 * Network bottlenect (1Gbps), 10Tb need 1 day
 * Distributed programming is hard - need a simple model
* Map-reduce solution
 * store data redundantly - distributed file system GFS,HDFS
 * move computation close to data - minimum network bottleneck
 * simple programming model
