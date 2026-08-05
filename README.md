# PySpark Learning Journey

A hands-on learning repository for Apache PySpark, covering concepts from basics to advanced topics. Each day includes a Jupyter notebook with explanations, code examples, and practical exercises.

---

## Table of Contents

- [About](#about)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [How to Run](#how-to-run)
- [Course Content](#course-content)
- [Repository Structure](#repository-structure)
- [Spark Architecture](#spark-architecture)
- [Spark UI](#spark-ui)
- [License](#license)

---

## About

This repository documents my journey of learning PySpark. Each day focuses on specific concepts with:
- Detailed explanations in markdown
- Hands-on code examples
- Practical exercises

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| **Python 3.11** | Programming language |
| **PySpark** | Apache Spark Python API |
| **Jupyter Notebook** | Interactive development environment |
| **uv** | Fast Python package manager |

---

## Getting Started

### Prerequisites

- Python 3.11+
- [uv](https://docs.astral.sh/uv/) package manager

### Installation

```bash
# Clone the repository
git clone https://github.com/Biswajitnahak2003/learning_spark.git
cd learning_spark

# Create virtual environment and install dependencies
uv venv
source .venv/bin/activate
uv init
uv add pyspark ipykernel
```

---

## How to Run

1. Navigate to the respective day folder
2. Select the kernel: `.venv` (Python 3.11)
3. Run the notebook cells sequentially

---

## Course Content

| Day | Topic | Description |
|-----|-------|-------------|
| [Day 1](./day1/) | Spark Basics | SparkSession, RDDs, DataFrames, CSV & Parquet |
| [Day 2](./day2/) | Slicing, Dicing, Mutating & Regular Exp. | Select, Filter, withColumn, when/otherwise, regexp_extract & regexp_replace |
| [Day 3](./day3/) | Groupby and Aggregation | groupBy and agg |


---

## Repository Structure

```
learning_spark/
├── README.md
├── LICENSE
├── day1/
│   ├── README.md
│   └── spark_day1.ipynb
├── day2/
│   ├── README.md
│   └── spark_day2.ipynb
├── dataset/
│   └── Customers.csv
├── pyproject.toml
├── uv.lock
└── .python-version
```

---

## Spark Architecture

```
                    ┌─────────────────────────────────────────┐
                    │              DRIVER PROGRAM              │
                    │         (Your PySpark Code)             │
                    │    SparkSession / SparkContext           │
                    └─────────────────┬───────────────────────┘
                                      │
                                      ▼
                    ┌─────────────────────────────────────────┐
                    │            CLUSTER MANAGER               │
                    │      (YARN / Mesos / Kubernetes)        │
                    └─────────────────┬───────────────────────┘
                                      │
              ┌───────────────────────┼───────────────────────┐
              │                       │                       │
              ▼                       ▼                       ▼
    ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
    │   EXECUTOR 1    │     │   EXECUTOR 2    │     │   EXECUTOR 3    │
    │                 │     │                 │     │                 │
    │  ┌───────────┐  │     │  ┌───────────┐  │     │  ┌───────────┐  │
    │  │  Task 1   │  │     │  │  Task 2   │  │     │  │  Task 3   │  │
    │  └───────────┘  │     │  └───────────┘  │     │  └───────────┘  │
    │  ┌───────────┐  │     │  ┌───────────┐  │     │  ┌───────────┐  │
    │  │  Task 4   │  │     │  │  Task 5   │  │     │  │  Task 6   │  │
    │  └───────────┘  │     │  └───────────┘  │     │  └───────────┘  │
    │  ┌───────────┐  │     │  ┌───────────┐  │     │  ┌───────────┐  │
    │  │   CACHE   │  │     │  │   CACHE   │  │     │  │   CACHE   │  │
    │  └───────────┘  │     │  └───────────┘  │     │  └───────────┘  │
    └─────────────────┘     └─────────────────┘     └─────────────────┘
              │                       │                       │
              └───────────────────────┼───────────────────────┘
                                      │
                                      ▼
                    ┌─────────────────────────────────────────┐
                    │            STORAGE SYSTEM                │
                    │   (HDFS / S3 / Local File System)       │
                    └─────────────────────────────────────────┘
```

### Key Components

| Component | Description |
|-----------|-------------|
| **Driver** | Runs your main() program, creates SparkContext, coordinates tasks |
| **Cluster Manager** | Allocates resources across applications (YARN, Mesos, K8s) |
| **Executor** | JVM process on worker node, runs tasks and stores data |
| **Task** | Smallest unit of work, one task per partition |
| **RDD/DataFrame** | Distributed data structure, partitioned across executors |
| **DAG** | Directed Acyclic Graph of stages (created by Catalyst optimizer) |

---

## Spark UI

The Spark UI is a web interface for monitoring and debugging Spark applications.

### Accessing Spark UI

| Mode | URL |
|------|-----|
| Local | `http://localhost:4040` |
| YARN | ResourceManager UI → Application → Tracking URL |
| Standalone | `http://<master-host>:8080` |

> **Note:** The UI is only available while the application is running. After `spark.stop()`, the UI is no longer accessible.

### Spark UI Tabs

| Tab | Description |
|-----|-------------|
| **Jobs** | List of all jobs, status (succeeded/failed), duration |
| **Stages** | Stages within each job, task metrics, shuffle read/write |
| **Storage** | Cached RDDs and DataFrames, memory usage |
| **Executors** | Executor details, memory, GC time, active tasks |
| **SQL** | SQL query plan, DAG visualization, physical plan |
| **Environment** | Spark config, system properties, classpath |
| **Containers** | (YARN) Container allocation and status |

### Useful Metrics to Monitor

| Metric | Why It Matters |
|--------|----------------|
| **Task Duration** | Identify skew (some tasks take longer) |
| **Shuffle Read/Write** | High shuffle = potential optimization needed |
| **GC Time** | High GC = memory pressure, tune heap size |
| **Spill (Disk/Memory)** | Data spilling to disk = need more memory or partitioning |
| **Broadcast Size** | Large broadcasts may cause OOM on executors |

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<p align="center">Made with PySpark</p>
