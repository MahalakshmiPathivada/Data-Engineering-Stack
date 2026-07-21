# Memory architecture
# JVM Heap Memory

```text
JVM Heap Memory
│
├── Reserved Memory (300 MB)
│   └── Fixed memory reserved for the Spark Engine
│
├── User Memory (40% of Total Memory after reserving 300 MB)
│   ├── User Defined Functions (UDFs)
│   ├── Metadata
│   └── User-created data structures
│
└── Spark Memory (60% of Total Memory after reserving 300 MB)
    │
    ├── Execution Memory (50%)
    │   ├── Shuffle Operations
    │   ├── Joins
    │   ├── Sorts
    │   └── Transformations
    │
    └── Storage Memory (50%)
        ├── Cache
        ├── Persisted Data
        ├── Broadcast Variables
        └── Long-Term Memory Storage
```

## Memory Breakdown

### Reserved Memory
- **300 MB**
- Fixed memory reserved internally by the Spark Engine.

### User Memory
- **40% of (Total Memory - 300 MB)**
- Used for:
  - UDFs (User Defined Functions)
  - Metadata
  - User-created objects

### Spark Memory
- **60% of (Total Memory - 300 MB)**
- Managed by Spark's **Unified Memory Manager**.

#### Execution Memory
- **50% of Spark Memory**
- Used for short-term execution tasks such as:
  - Transformations
  - Shuffle operations
  - Joins
  - Sorting

#### Storage Memory
- **50% of Spark Memory**
- Used for long-term storage such as:
  - Caching
  - Persisted DataFrames/RDDs
  - Broadcast variables

Executer memory can evict storage Memory  but storage memory does not evict executor memory

``` spark.memory.fraction ```

## Explain Storage Memory?
Storage memory is used for: 
                Cached DataFrames
                Cached RDDs
                Broadcast variables

--> If memory is insufficient:

          Execution Memory
          ↓
          Spill to Disk
          ↓
          Performance Degradation

## What is EVIC? (Eviction Mechanism)?
When storage memory becomes full: 
                        Spark Storage Memory Full
                        ↓
                        Old cached blocks removed
                        ↓
                        New blocks cache
it uses LRU (Least Recently Used)


###  Can Execution Memory Evict Storage Memory?
Yes -Execution can borrow memory from storage.

### Can Storage Memory Evict Execution Memory?
No --Execution Memory is never forcefully evicted by Storage Memory

Execution → Can Evict Storage
Storage → Cannot Evict Execution

### Explain Memory Fraction Parameters?
spark.memory.fraction=0.6      -->  60% of Heap Memory

40%
User + Reserved Memory

# Storage Fraction  ``` spark.memory.storageFraction=0.5 ```

## Spark Memory
````
   |-- 50% Storage
   |-- 50% Execution
````

## What Happens When Memory Becomes Full?
*process :* 
     - Execution Needs Memory
     - Reserve More Memory
      - Spill to Disk
      - Continue Processing
*Symptoms:*
      Slow jobs
      Long GC
      Disk I/O increase

### What is Memory Spill?
When task memory exceeds available execution memory. and it stores in disk temporarily called Data spill
            Data
            ↓
            Memory Full
            ↓
            Temporary Disk Storage     

### Difference Between Spill and Eviction.
Spill   (Free execution memory)
        Execution Memory
        ↓
        Data written to disk

Eviction  (Free storage memory)
        Storage Memory
        ↓
        Cached blocks removed

### How Do You Identify Memory Issues in Spark UI?
 Executors Tab  
            -  Storage Memory
            - GC Time
            -  Peak Memory
Tasks   
       - Memory Spill
       - Disk Spill

### Explain Tungsten Memory Management.
Spark Tungsten introduced:   
Off-heap memory
Cache-friendly computation
Binary processing

### What is Off-Heap Memory?
Think of Spark memory in two areas:
            1. On-Heap Memory  (Inside JVM Heap)
            2. Off-Heap Memory (Outside JVM Heap)

On-Heap Memory -- By default Spark stores data inside JVM Heap. (mainly we are using)

```` JVM Heap
|
|-- Spark Objects
|-- Cached Data
|-- Shuffle Data
|-- User Objec ````

suppose -Cached data is stored inside the JVM heap. as we might have millions of java objects it will be the problem
result 
        Long Garbage Collection
        Slow Jobs
        Executor OOM
Off-Heap Memory
Off-Heap memory is memory allocated outside JVM Heap but still used by Spark.  spark can access it directly
Benefits:        
        Reduced GC
        Better memory utilization

spark.memory.offHeap.enabled=true
spark.memory.offHeap.size=8g



### Difference Between cache() and persist() from Memory Perspective?
cache()  (Stores only in RAM.)
          MEMORY_ONLY
persist()  (flexible)
          MEMORY_ONLY
          MEMORY_AND_DISK
          DISK_ONLY
          OFF_HEAP

### How Does AQE Help in Memory Management?
AQE:  Adaptive Query Execution
Benefits:
Dynamically
        Converts sort merge join
        Creates broadcast joins
        Handles skew
results  
        Reduced Shuffle
        Reduced Memory Usage

### Which Join Consumes Maximum Memory?
Sort Merge Join

### Production Scenario: Executor OOM ?
Job failing because of 
          ExecutorLostFailure
          OutOfMemoryError

### what is Data Skew?
Data skew in Spark occurs when data is unevenly distributed across partitions, causing some tasks to process significantly more data than others. This leads to straggler tasks, poor resource utilization, and longer job execution times. It commonly occurs during joins, aggregations, and shuffle operations

### How to Identify Data Skew?
if one task =30mints and remaing tasks 20 sec  (One task will be significantly larger.)
then 
Check Key Distribution df.groupBy("customer_id").count() 
if any on of the id have  million records other have less records then it clearly skew

### How do you handle data skew in Spark?
Data skew happens when a few keys contain much more data than others, causing some partitions to process significantly more records and become bottlenecks.
Common ways to handle skew:

#### Broadcast Join
      If one table is small, broadcast it.
      Avoids shuffle and reduces skew impact.

Pythondf1.join(broadcast(df2), "id")

#### Salting  
      Add random values to skewed keys to distribute records across multiple partitions.

Example:
Plain TextA → A_1, A_2, A_3, A_4


#### Adaptive Query Execution (AQE) 
    Enable AQE so Spark automatically detects and splits skewed partitions.
``` Pythonspark.conf.set("spark.sql.adaptive.enabled", "true")spark.conf.set("spark.sql.adaptive.skewJoin.enabled", "true") ```


#### Repartition Data
        Increase partitions to distribute data more evenly.        
        Pythondf.repartition(500, "customer_id")


#### Process Skewed Keys Separately
      Filter heavily skewed keys and handle them in a separate pipeline.

#### Bucketing
        Pre-organize data by join keys to reduce shuffle during joins.

### Salting Method ?
The most commonly used skew-handling technique.
Salting is a skew mitigation technique where a highly skewed key is split into multiple keys using random suffixes. The dimension side is duplicated with matching suffixes. This distributes the workload across partitions and prevents a single executor from processing all records associated with a hot key.

 # Advanced Architect-Level Questions 

### Why does Storage Memory use LRU eviction?
Answer: To remove least recently accessed blocks first, maximizing cache hit ratio while making room for new cached data.

### Why can Execution borrow from Storage but not vice versa?
Answer: Active tasks cannot safely lose execution memory during computation. Cached data can be recomputed, so Spark allows storage eviction but protects execution memory.

### What causes excessive spill despite sufficient executor memory?
Answer:
      Data skew
      Large partitions
      Poor partitioning
      Wide transformations
      Sort Merge Joins
      Inefficient aggregations

### How do you reduce GC pressure?
Answer:
      Kryo Serialization
      Broadcast joins
      Reduce object creation
      Use DataFrames instead of RDDs
      Enable Tungsten
      Use Off-Heap Memory

### Explain a real production memory tuning exercise.
We observed 35% execution time spent in GC and significant disk spill. Spark UI showed skewed partitions and sort-merge joins. 
We enabled AQE, switched to Broadcast Join for dimension tables under 500MB, increased shuffle partitions from 200 to 1200, enabled Kryo serialization, and optimized caching strategy. 
Runtime reduced from 3.5 hours to 48 minutes while GC dropped below 5%."


### What's the difference between On-Heap and Off-Heap Memory?

 On-Heap vs Off-Heap Memory

| Feature | On-Heap Memory | Off-Heap Memory |
|----------|----------|----------|
| **Location** | Inside JVM Heap | Outside JVM Heap |
| **Management** | Managed by Garbage Collector (GC) | Not Managed by GC |
| **GC Overhead** | Higher GC Overhead | Lower GC Overhead |
| **Default Usage** | Default Memory Area | Optional Configuration |
| **Storage Type** | Stores Java Objects | Stores Binary Data |

*On-Heap Memory*

- Located inside the JVM Heap.
- Managed by Java Garbage Collection (GC).
- Used by default in Spark and Java applications.
- Stores objects in Java object format.
- May experience higher GC pauses when memory usage is high.

* Off-Heap Memory*

- Located outside the JVM Heap.
