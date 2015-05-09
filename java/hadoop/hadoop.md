# BigData (V3)
* Volume - too big
* Velocity - too quick
* Variety - too variable

## Compoments
* MapReduce + HBase + HDFS <==> MapReduce + BigTable + GFS

## Map -> Shuffle -> Reduce
* Map - Split input and deliver to nodes; send tasks to specific nodes based on where the input was delivered.
* Shuffle - Merge result from mappers and move results to the reducers
* Reduce - Combine the results into a final output

# Chapter 1
## Comparison with other
### Relational RDBMS
MapReduce (Hadoop) is very efficient to read/write and process large dataset. Traditional DB use B-Tree is more efficient to 
update a small portion of large dataset.

RDBMS is good at processing **structured data**. Hadoop is good at process **semi-structured data** or **unstructured data**

### Grid computing(HPC)
Hadoop tries to co-locate the data with the compute nodes (`data locality`). HPC use MPI to passing message in between nodes,
which is usually bottlenecked by network bandwidth.

MapReduce is **shared-nothing** architecture, so it easy to handle partial failure. In such case, it can rerun the failure node. By contrast, MPI programs have to explicitly manage checkpoint and recovery, which bring more duty to developer.

### Volunteer computing (such as SETI)
Volunteer computing is CPU-intensive, while MapReduce is running jobs that last minutes to hours on trusted, dedicated hardware
running in a single data center with very high aggregate bandwith interconnects.

## Hadoop at Yahoo
### Internet-scale search engine
* Crawler - download pages
* Webmap - build graph of known web
* Indexer - builds a reverse index to the best pages
* Runtime - answer user's query

# Chapter 2. MapReduce
