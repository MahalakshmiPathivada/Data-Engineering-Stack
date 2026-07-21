# JOB ?
A Job is created when you perform an action in Spark. (One action = one job)
Ex:  collect() 
  count() 
  show() 
  write()

# STAGE ? 
A Stage is a subset of a job that can be executed without shuffle.  (Stage = group of tasks)
Stages are divided based on wide transformations (shuffle boundaries)
- ##### Narrow transformations → same stage 
- #####  Wide transformations → new stage

# TASK?
A Task is the smallest unit of work in Spark. Each task processes one partition
 
High-level hierarchy
Databricks Job (Workflow level)
    ↓
Spark Job
    ↓
Stages 
    ↓
Tasks ( 1 or more)

 


# AQE (Adaptive Query Execution)
AQE is a Spark optimization feature that dynamically changes the execution plan at runtime based on actual data statistics.
Why AQE is needed?
👉 Normally, Spark creates a plan before execution 👉 But data characteristics may differ at runtime
👉 AQE fixes this by optimizing during execution
### Key Features of AQE
1. Coalesce Shuffle Partitions
•	Reduces number of small partitions after shuffle
•	Improves performance
Switch Join Strategy
•	Converts Sort Merge Join → Broadcast Join
•	If one dataset becomes small at runtime
 

## Spark Optimizations Comparison

| Feature | Catalyst | AQE (Adaptive Query Execution) | Tungsten |
|----------|----------|----------|----------|
| **Stage** | Planning | Runtime Adjustment | Execution |
| **Type** | Logical/Physical Optimizer | Adaptive Optimizer | Execution Engine |
| **Focus** | Query Transformation | Runtime Improvements | Performance |
| **Handles Skew** | ❌ | ✅ | ❌ |
| **Code Generation** | ❌ | ❌ | ✅ |
| **Memory Optimization** | ❌ | ❌ | ✅ |

### Summary

- **Catalyst** is Spark's query optimizer responsible for logical and physical plan optimization during the planning phase.
- **AQE (Adaptive Query Execution)** improves query performance at runtime by adapting execution plans based on actual data statistics.
- **Tungsten** is Spark's execution engine focused on performance improvements through optimized memory management and code generation.


# JOINS
### Sort Merge Join (SMJ)
A Sort Merge Join is used when both datasets are large.It works by sorting and then merging the data on the join key

*How it works*
1.	Shuffle both datasets based on join key
2.	Sort each partition
3.	Merge matching keys

#### Key Characteristics
•	✅ Works well for large datasets
•	✅ Requires shuffle + sort
•	❗ Expensive (network + disk + CPU)
•	✅ Default join in many cases

df1.join(df2, "id")

### Shuffle Hash Join
A Shuffle Hash Join builds a hash table on one dataset after shuffle and probes with the other dataset.

 
How it works
1.	Shuffle both datasets by key
2.	Build hash table on smaller partition
3.	Match with larger dataset
#### Key Characteristics
•	✅ Faster than Sort Merge Join (no sorting)
•	❗ Requires shuffle
•	❗ Needs enough memory for hash table
### When  - One side is smaller but not small enough to broadcast
👉 Spark prefers Sort Merge Join over this by default unless configured

### Broadcast Join (Map-Side Join)
A Broadcast Join is used when one dataset is small.The small dataset is broadcast to all executors, avoiding shuffle.  df1.join(broadcast(df2), "id")

#### Key Characteristics
•	✅ No shuffle (very fast)
•	✅ Best performance
•	✅ Used when one dataset is small (< ~10MB default)
•	❗ Limited by memory


Spark decides based on:
•	Data size
•	Config (spark.sql.autoBroadcastJoinThreshold)
•	AQE optimization
spark.conf.get("spark.sql.autoBroadcastJoinThreshold")   -- Default 10MB

In Databricks projects:
•	✅ Always try for Broadcast Join (best performance)
•	✅ Use repartition() before SMJ if data is skewed
•	✅ Enable AQE → improves join strategy automatically





