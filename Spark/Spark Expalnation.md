# Apache Spark

## What is Spark?

![Spark](images/HadoopVsSpark.jpg)

Apache Spark is a distributed data processing engine that uses DAG-based execution, in-memory computation, and parallel processing.

---

## RDD (Resilient Distributed Dataset)

![Spark](images/RDD.png)

### RDD Characteristics

- Resilient
- Distributed
- Dataset
- Immutable
- Fault-Tolerant

---

## Lazy Evaluation

![Lazyges/lazy-evaluation.png

RDD operations are categorized into:

- Transformations
- Actions

---

## Transformations

![Transformationsrmations.png

### Narrow Transformations

../images/narrow-transformation.png

Examples:

- map()
- filter()
- flatMap()

### Wide Transformations

../images/wide-transformation.png

Examples:

- reduceByKey()
- join()
- distinct()
- groupByKey()

---

## Actions

![RDD Actions](../images/actions

```python
count()
collect()
first()
max()
reduce()
```

---

## DAG (Directed Acyclic Graph)

![DAG ges/dag.png

### Components

- Directed
- Acyclic
- Graph

---

## Spark Features

![Spark Featuresfeatures.png

### Features

- Speed
- In-Memory Processing
- Caching
- Real-Time Processing
- Scalability
- Multi-Language Support

---

## Spark Driver

![/images/spark-driver.png

Responsibilities:

- Creates SparkContext
- Schedules Jobs
- Monitors Execution
- Collects Results

---

## Spark Ecosystem

![Spark Ecosystemcosystem.png

Components:

- Spark Core
- Spark SQL
- Spark Streaming
- MLlib
- GraphX

---

## Spark Architecture

![Spark Architectureitecture.png

### Components

- Driver
- Cluster Manager
- Worker Nodes
- Executors

---

## Spark Execution Flow

![Sparkes/spark-execution-flow.png

```text
User Application
        ↓
      Driver
        ↓
     DAG Plan
        ↓
      Stages
        ↓
      Tasks
        ↓
    Executors
        ↓
      Result
```

---

## Hadoop YARN

![YARN Architecture](../images/yarn-N acts as:

- Resource Manager
- Job Scheduler
- Cluster Coordinator

---

# JVM Heap Memory

![JVM Memory](../images/jvm-memory.pngHeap Memory
│
├── Reserved Memory
├── User Memory
└── Spark Memory
    ├── Execution Memory
    └── Storage Memory
```

---

# On-Heap vs Off-Heap Memory

![On Heap vs Off Heap](../images/onheap-offheap.png Off-Heap |
|----------|----------|----------|
| Location | JVM Heap | Outside JVM Heap |
| GC | Managed | Not Managed |
| Performance | Higher GC | Lower GC |

---

# Cache vs Persist

![Cache vs Persist](../.png

### cache()

```python
df.cache()
```

Stores data in:

- MEMORY_ONLY

### persist()

```python
df.persist()
```

Supports:

- MEMORY_ONLY
- MEMORY_AND_DISK
- DISK_ONLY
- OFF_HEAP

---

# Catalyst, AQE and Tungsten

![Catalyst AQE Tungsten](../images/cataleature | Catalyst | AQE | Tungsten |
|----------|----------|----------|----------|
| Stage | Planning | Runtime | Execution |
| Focus | Optimization | Adaptation | Performance |

---

# Unity Catalog

![Unityes/unity-catalog.png

```text
Metastore
│
└── Catalog
    │
    └── Schema
        ├── Tables
        ├── Views
        ├── Volumes
        └── Functions
```

---

# Volumes

![Unity Catalog Volumes-volumes.png

Used for:

- CSV Files
- JSON Files
- PDFs
- Images
- ML Models

---

# Lineage

![Unity Catalog Lineage]lineage.png

```text
Source Table
      ↓
Silver Table
      ↓
Gold Table
      ↓
Dashboard
```

---

# Enterprise Unity Catalog Design

![Enterprise Unity Catalog](../imagesg

```text
Metastore
│
├── sales_catalog
├── finance_catalog
└── marketing_catalog
```

---

# Summary

![Spark Summary](..ng

Apache Spark provides:

- Distributed Processing
- DAG Execution
- Fault Tolerance
- In-Memory Computing
- Scalability
- High Performance

Unity Catalog provides:

- Governance
- Security
- Lineage
- Auditing
- Data Sharing
