# Day 3 - GroupBy and Aggregations (The Shuffles)

## 📖 Topics Covered

1. **Reading Parquet Files**
2. **Basic Aggregations (without GroupBy)**
3. **GroupBy with Aggregations**
4. **Understanding Shuffle Operations**

---

## 1. Reading Parquet Files

```python
parquet_path = r"../dataset/parquet_output"
df = spark.read.parquet(parquet_path)
```

**Key Points:**
- Parquet is a columnar storage format (created in Day 1)
- `spark.read.parquet()` reads parquet files efficiently
- Schema is preserved from the original data

---

## 2. Basic Aggregations (without GroupBy)

```python
from pyspark.sql import functions as F

df_1 = df.select(
    F.count("ID").alias("total_customers"),
    F.min("Score").alias("min_score"),
    F.avg("Score").alias("avg_score"),
    F.max("Score").alias("max_score")
)
df_1.show()
```

**Key Points:**
- Aggregations without `groupBy()` compute over the entire DataFrame
- Returns a single row with aggregated values
- Common functions: `count`, `sum`, `avg`, `min`, `max`, `stddev`

---

## 3. GroupBy with Aggregations

```python
df_2 = df.groupBy("Country").agg(
    F.count("ID").alias("total_customers"),
    F.min("Score").alias("min_score"),
    F.avg("Score").alias("avg_score"),
    F.max("Score").alias("max_score")
)
df_2.show()
```

**Key Points:**
- `groupBy()` groups rows by one or more columns
- `.agg()` applies aggregation functions to each group
- **Triggers a Shuffle** - data is redistributed across partitions by group key
- Each group becomes one row in the result

---

## 4. Understanding Shuffle Operations

### What is a Shuffle?
A shuffle is the process of redistributing data across partitions so that rows with the same key end up in the same partition. This happens during:
- `groupBy()` operations
- `join()` operations
- `repartition()` / `coalesce()`
- `orderBy()` / `sort()`

### Shuffle Phases:
1. **Map Phase** - Each partition processes its data, writes to local disk
2. **Shuffle Phase** - Data is transferred across network to target partitions
3. **Reduce Phase** - Target partitions read and aggregate data

### Performance Impact:
- **Network I/O** - Data moves between executors
- **Disk I/O** - Spill to disk if memory insufficient
- **Serialization** - Data must be serialized for transfer

### Optimization Tips:
- Filter early (before groupBy)
- Use `coalesce()` instead of `repartition()` when reducing partitions
- Consider broadcast joins for small DataFrames
- Tune `spark.sql.shuffle.partitions` (default 200)

---

## 5. Stop Session

```python
spark.stop()
```

Always stop the session to release resources.

---

## 📝 Summary

| Concept | Description |
|---------|-------------|
| `groupBy()` | Groups rows by column(s) for aggregation |
| `agg()` | Applies aggregation functions per group |
| `count()` | Counts non-null values |
| `sum()` | Sum of values |
| `avg()` / `mean()` | Average of values |
| `min()` / `max()` | Minimum/maximum values |
| **Shuffle** | Data redistribution across partitions |

---

## 🔗 Resources

- [PySpark GroupBy Documentation](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.GroupedData.html)
- [Spark Aggregation Functions](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html#aggregation-functions)
- [Understanding Spark Shuffle](https://spark.apache.org/docs/latest/sql-performance-tuning.html#shuffle-tuning)