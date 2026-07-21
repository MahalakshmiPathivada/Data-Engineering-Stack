
# What is Spark?
Apache Spark is a distributed data processing engine that uses DAG-based execution, in-memory computation, and parallel processing to efficiently handle large-scale data workloads.
or
Spark is a super-fast cluster computing technology. It is a distributed data processing engine that can be used to solve a variety of complex problems and perform highly complex computations
or
Apache Spark is an open-source distributed data processing framework designed for large-scale data processing. It provides in-memory computation, making it much faster than traditional Hadoop MapReduce for many workloads.


# Why is Spark faster than Hadoop (MapReduce)?
Spark is faster because:
It processes data in memory (RAM) instead of writing intermediate results to disk.
It has an optimized execution engine.
It supports in-memory caching.


Hadoop (MapReduce) |	Apache Spark
-----------------------------------------------
	Disk-based	   | Memory-based
	Slow	       |     Fast
Multiple disk I/O  | Less disk I/O
High latency	   |  Low latency


#  What are the Feature of spark?
  -	Speed          -->   Spark runs up to 100 times faster than Hadoop MapReduce for large-scale data processing. It is also able to achieve this speed through controlled partitioning.
  -	Powerful Caching   -->   Simple programming layer provides powerful caching and disk persistence capabilities.
  -  Deployment     -->   It can be deployed through Mesos, Hadoop via YARN, or Spark’s own cluster manager.
  -  Real-Time      -->   It offers Real-time computation & low latency because of in-memory computation.
  -   Polyglot     -->    Spark provides high-level APIs in Java, Scala, Python, and R. Spark code can be written in any of these four languages. It also provides a shell in Scala and Python.

#  Name the components in the Spark ecosystem ?  or  Spark Terminologies?  or What are the main components of Spark?
 The components in the Spark ecosystem are: 
    ⇒ Spark Streaming – It is responsible for processing the real-time streaming data.
    ⇒ GraphX – It implements graphs and parallel computations on the graphs.
    ⇒ Spark SQL – It is responsible for integrating Spark’s functional programming API with relational database processing. 
    ⇒ MLib – This is the library used for machine learning.
    ⇒ Spark Core – It is a distributed execution engine that can perform large-scale parallel and distributed data processing.

# what is RDD?
 Resilient Distributed Dataset   stands for 

	Resilient: It’s fault-tolerant and can build data in case of a any node failure

	Distributed: The data is distributed among multiple nodes in a cluster

	Dataset: Data is partitioned based on values

 RDD is immutable, meaning that it cannot be modified once created, but it can be transformed at any time. Every Dataset in RDD is divided into multiple logical partitions, and this distribution is done by Spark.
 An RDD can be created from existing data in your program or from external storage like HDFS/ ADLS. It supports two types of operations: transformations and actions
Example:

rdd = spark.sparkContext.parallelize([1,2,3,4])
----------

# what is Transformations?
  RDD transformations are lazy operations that create a new RDD from an existing one, such as map and filter.


# What are Narrow and Wide Transformations?
  Narrow transformation: A transformation where each input partition is used to create only one output partition
    •	Data stays within one partition (no shuffle)
    •	Faster execution
 *Examples:*
    •	map() → apply function to each element
    •	filter() → select some elements
    •	flatMap() → flatten data

 ### Wide transformation: A transformation where data from multiple partitions is shuffled and combined to create new partitions
     •	Data moves across partitions (shuffle happens)
    •	More expensive
*Examples:*
    •	groupByKey()
    •	reduceByKey()
    •	join()
    •	distinct()
    •  repartition()

# what is Actions ?
  Actions are applied on an RDD, which instructs Spark to apply computation and sent the result to the driver
   --------------------------------------
     Action         |  Meaning
  ---------------------------------------
    Collect()       | Returns all data to driver
    Count()         | Counts no of elements

# What is a Lineage Graph ?
 A Lineage Graph (also known as the RDD Operator Graph) is a record of all the parent RDDs used to create a child RDD. Since RDDs are immutable, Spark doesn't change data; it creates new RDDs. If a node fails and a partition of data is lost, Spark looks at the lineage graph to see exactly which transformations were applied to the original data source and re-runs them to recover the lost partition.
Example:

rdd1 -> map -> filter -> reduce

If data is lost, Spark rebuilds it using lineage information.

# What is a DAG in Spark?
 DAG (Directed Acyclic Graph) represents the sequence of transformations. Spark's DAG Scheduler divides jobs into stages and tasks for efficient execution.
 It is a execution blue print of  data pipelines

# Difference between DAG and Lineage Graph
+--------------+----------------------------------+--------------------------------------+
| Feature      | Lineage                          | DAG (Directed Acyclic Graph)         |
+--------------+----------------------------------+--------------------------------------+
| Meaning      | History of transformations       | Execution graph                      |
+--------------+----------------------------------+--------------------------------------+
| Level        | Logical                          | Physical / Execution                 |
+--------------+----------------------------------+--------------------------------------+
| Structure    | Linear chain                     | Graph (nodes + edges)                |
+--------------+----------------------------------+--------------------------------------+
| Purpose      | Fault tolerance                  | Execution planning                   |
+--------------+----------------------------------+--------------------------------------+
| Created When | During transformations           | During action                        |
+--------------+----------------------------------+--------------------------------------+
| Used For     | Recomputing data                 | Scheduling tasks                     |
+--------------+----------------------------------+--------------------------------------+

# What is Lazy Evaluation?
 Spark does not execute transformations immediately. It builds a DAG (Directed Acyclic Graph) and executes only when an action is called.

*Example:*
df.filter(df.age > 30)   
No execution occurs until:  df.count()

# What is the difference between RDD, DataFrame, and Dataset?

DataFrames and Datasets are high-level APIs built on top of RDDs, where Spark uses Catalyst optimizer and Tungsten engine to convert operations into optimized RDD execution

RDD → Control + Flexibility, but slow     --> rarely used
DataFrame → Fast + Easy, but no type safety      --> 90% use cases
Dataset → Best of both (but limited language support)  → used in Scala-heavy projects


------------------+----------------------+--------------------------+----------------------------+
| Feature          | RDD                  | DataFrame                | Dataset                   |
+------------------+----------------------+--------------------------+----------------------------+
| Full Form        | Resilient Distributed| Distributed collection   | Typed distributed         |
|                  | Dataset              | of data with schema      | collection (RDD + DF)     |
+------------------+----------------------+--------------------------+----------------------------+
| Type Safety      | Yes                  | No                       | Yes                       |
+------------------+----------------------+--------------------------+----------------------------+
| Schema           | No schema            | Has schema               | Has schema                |
+------------------+----------------------+--------------------------+----------------------------+
| Optimization     | No                   | Catalyst optimizer       | Catalyst optimizer        |
+------------------+----------------------+--------------------------+----------------------------+
| Performance      | Lowest               | High                     | High                      |
+------------------+----------------------+--------------------------+----------------------------+
| API Level        | Low-level            | High-level               | High-level                |
+------------------+----------------------+--------------------------+----------------------------+
| Language Support | Java, Scala, Python  | Java, Scala, Python      | Scala, Java only          |
+------------------+----------------------+--------------------------+----------------------------+
| Ease of Use      | Harder               | Easy (SQL-like)          | Moderate                  |
+------------------+----------------------+--------------------------+----------------------------+
| Serialization    | Java/Python objects  | Tungsten (optimized)     | Encoders (optimized)      |
+------------------+----------------------+--------------------------+----------------------------+

Dataset = DataFrame + Type Information

# What is Catalyst Optimizer and Tungsten ?
 Catalyst is Spark’s query optimizer that transforms logical plans into optimized physical plans, while Tungsten is the execution engine that improves runtime performance using memory management and code generation

Optimizations Does It Do?
Predicate pushdown ✅
Column pruning ✅
Constant folding ✅
Join optimization ✅

DataFrame Query
        ↓
Catalyst Optimizer (plans & optimizes)
        ↓
Tungsten Engine (executes efficiently)
        ↓
RDD execution on cluster

# What is Tungsten?
 Tungsten improves Spark performance through:

Better memory management
CPU optimization
Binary processing
Whole-stage code generation


What are the different cluster managers in Spark? 
Ans: There are three different cluster managers in Spark – YARN, Spache Mesos, and standalone deployments. 

What is Shuffling?
Ans: Shuffling is the redistribution of data across partitions. Data transfer across stages happens during shuffling. It may lead to data movement across the Java Virtual Machines processes. Data redistribution between executors on separate machines is also called shuffling. 

================================================================================
# What is DRIVER Program?
The Driver is the process where your main() program runs. It creates the SparkContext / SparkSession and controls how tasks are distributed across the cluster.
Creates Spark Context
Builds Execution Plan
Schedules Tasks
Communicates with Cluster Manager
Collects Results


Driver Program
     |
     |-----> Cluster Manager (allocates resources)
     |
Executors (run tasks on worker nodes)

⚠️ Important Points

The driver runs on a single node (can be local machine or cluster node)
If the driver crashes → the entire Spark application fails

# What are Executors in Spark?
Executors are worker processes that run on cluster nodes and are responsible for executing tasks and storing data.
 ### Key Responsibilities 
1. Execute Tasks  -- Driver sends tasks → Executors run them in parallel
2. Store Data  -- Hold cached data in memory (RDD/DataFrame caching)
3. Return Results --  Send results back to the Driver


# What is Resource Manager?
The Resource Manager (also called Cluster Manager) is responsible for allocating resources (CPU, memory) to your Spark application.
### Key Responsibilities
		Resource Allocation   -- 1. Decides how many executors you get    2. 	Allocates memory & cores
		Launching Executors   --  Starts executor processes on worker nodes
		Managing Cluster     --   Tracks resource usage  & 	Handles failures


*Simple Spark Execution Flow*
1.	User submits Spark application
2.	Driver creates logical plan (DAG)
3.	DAG is divided into stages
4.	Each stage contains multiple tasks
5.	Tasks are distributed to executors
6.	Results are returned to the driver


# --> what is RePartition and Coalesce ?
repartition() and coalesce() are used to change the number of partitions in a DataFrame/RDD—but they behave very differently.
*Repartition*  --> repartition() is used to increase or decrease partitions and always performs a full shuffle.
Key Characteristics

✅ Can increase OR decrease partitions
✅ Always triggers shuffle
✅ Data is evenly distributed
❗ Slower due to shuffle
ex : df = df.repartition(10)    --> Spark redistributes data across 10 partitions evenly.

*Coalesce*  --> coalesce() is used to reduce the number of partitions with minimal shuffle (or no shuffle)
Key Characteristics

✅ Only reduces partitions
✅ Avoids shuffle (more efficient)
❗ Data may be unevenly distributed
✅ Faster than repartition
ex: df = df.coalesce(2)   --> Spark merges partitions without full data movement.
When to Use
When you need balanced data distribution
Before joins, aggregations
To fix data skew

When to Use
Before writing output (like fewer files)
When reducing partitions for performance optimization
When you don’t care about perfect balance
