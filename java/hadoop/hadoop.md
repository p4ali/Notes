## Comparison with other
### Relational RDBMS
MapReduce (Hadoop) is very efficient to read/write and process large dataset. Traditional DB use B-Tree is more efficient to 
update a small portion of large dataset.

RDBMS is good at processing **structured data**. Hadoop is good at process **semi-structured data** or **unstructured data**

### Grid computing(HPC)
Hadoop tries to co-locate the data with the compute nodes (`data locality`). HPC use MPI to passing message in between nodes,
which is usually bottlenecked by network bandwidth.
