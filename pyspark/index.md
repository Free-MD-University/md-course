# Spark 
distribute data agent. like thread for data . 
a computer netework is called a cluster . 

## Architecture 
the archi is like : 
- Cluster Manager: is one program that will manage the all system it will create a master/friver node 
- Driver Node : contains the context and the called code with the entire flow send the data to cluster manager
- Worker Node : a node created by the cluster manager after the driver node ask for this will have task and the compute . 

## benefit : 
- in memory computation : the entire data will be in the ram divided in many worker 
- Lazy evaluation : will create the code after the plan is created this mean that the transformation flow can be changed to be more fast .  
- Partioning: partitionne the data to be more fast 
- Fault Tolerant : handle errors 


a job is a flow. a job is divided in stages that are subdivided in task . 

Functions are executed in parallel across the cluster's worker nodes when they are applied to a Resilient Distributed Dataset (RDD), DataFrame, or Dataset using transformations like map, filter, groupBy, join, or any other operation that takes advantage of the distributed nature of the data.

Key Factors for Parallel Execution:
Distributed Data Structures:

RDDs/DataFrames: The data itself is already partitioned across the worker nodes.

PySpark Transformations: These are functions applied to RDDs or DataFrames that define the operations to be performed on the data.

map, filter, flatMap: The function provided is applied to each element or row of the partitioned data independently on a worker node.

groupByKey, reduceByKey, join: These operations involve data shuffling (moving data between nodes) but the actual grouping or joining logic is executed in parallel across the partitions.

Lazy Evaluation: PySpark operations are not executed until an Action (like collect, show, count, save) is called. When an action is triggered, PySpark's Driver program creates a Directed Acyclic Graph (DAG) of the operations and schedules the parallel tasks to run on the Executors (worker nodes).

In short: If you are calling a function inside a PySpark transformation (like df.withColumn('new_col', my_udf(col('old_col'))) or rdd.map(my_function)), that function's code will be serialized, sent to the worker nodes, and run in parallel on the local data partitions.

🖥️ When Functions Run on a Single Computer (Serial Execution)
Functions are executed serially on the Driver program (the machine where you launched the PySpark application, like your Jupyter notebook or command line) in a few main scenarios:

1. Operations on Local Python Objects
Any function applied to standard Python objects—like lists, dictionaries, single variables, or Pandas DataFrames that haven't been converted to a Spark DataFrame—will run locally on the Driver machine, not distributedly.


# RDD VS Dataframe

DataFrames and their successor, Datasets (available in Scala/Java, but often used interchangeably with DataFrames in PySpark discussions), offer significant advantages.
You can easily get an RDD (Resilient Distributed Dataset) from a DataFrame in PySpark by using the .rdd attribute or method.
The mapPartitions function in PySpark (and Spark in general) is a transformation used on RDDs (Resilient Distributed Datasets) that allows you to apply a function to the entire partition of data as an iterator, rather than applying the function to a single element at a time.
mapPartitions offers significant performance benefits over the standard map transformation in specific scenarios:

Resource Initialization: This is the primary reason to use it. If your function needs to open a database connection, initialize a heavy object, or load a pre-trained model, map would do this for every single row. mapPartitions does it only once per partition, drastically reducing overhead.

Example: If you have 1 million records across 10 partitions:

map initializes the resource 1,000,000 times.

mapPartitions initializes the resource only 10 times.

Batch Processing: It allows you to process data in batches within the function, which can be more efficient for operations like bulk insertion into a database or vectorized computations (though Pandas UDFs and mapInPandas are now the preferred way to do this with DataFrames).
While mapPartitions is a native RDD transformation, its equivalent functionality in DataFrames is achieved primarily through the use of Pandas UDFs (User Defined Functions), specifically the mapInPandas function.

You generally won't find a direct .mapPartitions() method on a PySpark DataFrame because DataFrames are optimized to work with column-based operations.

🐼 mapInPandas: The DataFrame Equivalent
The function that provides the same partition-level control and efficiency as RDD's mapPartitions is mapInPandas (available since Spark 3.0, also sometimes referred to as a "Grouped Map Pandas UDF" in older documentation).