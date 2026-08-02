# Day 1 - PySpark Basics

## 📖 Topics Covered

1. **SparkSession & SparkContext**
2. **RDD Operations**
3. **Reading CSV Files**
4. **DataFrame Operations**
5. **Writing to Parquet**

---

## 1. SparkSession & SparkContext

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName('first session') \
    .getOrCreate()
sc = spark.sparkContext
```

**Key Points:**
- `SparkSession` is the entry point for DataFrame API (Spark 2.0+)
- `SparkContext` is the entry point for RDD API (older API)
- `local[*]` runs Spark locally using all available CPU cores
- `appName` sets the name visible in Spark UI

---

## 2. RDD Operations

### Parallelize - Creating RDD from list
```python
lst = [1, 2, 3, 4, 5]
new_1 = sc.parallelize(lst)
```

### Map - Transform each element
```python
lst_2 = new_1.map(lambda x: x * 10)
```

### Collect - Bring results to driver
```python
result = lst_2.collect()  # [10, 20, 30, 40, 50]
```

**Key Points:**
- RDDs are immutable, distributed collections
- `parallelize()` converts Python collections to RDD
- `map()` applies function to each element (lazy transformation)
- `collect()` triggers computation and returns results to driver

---

## 3. Reading CSV Files

### Method 1: Using spark.read.csv()
```python
df = spark.read.csv(file_path, header=True, inferSchema=True)
```

### Method 2: Using format() API
```python
df = spark.read.format('csv') \
    .option('header', 'True') \
    .option('inferSchema', 'True') \
    .load(file_path)
```

**Key Points:**
- `header=True` - First row is column names
- `inferSchema=True` - Auto-detect data types (slower for big data)
- `inferSchema=False` (default) - All columns read as strings

---

## 4. DataFrame Operations

### View Data
```python
df.show(5)        # Pretty print table
df.take(5)        # Return first 5 rows as list of Row objects
df.printSchema()  # Display column names and data types
```

### Rename Column
```python
df_1 = df.withColumnRenamed('CustomerID', 'ID')
```

---

## 5. Writing to Parquet

```python
df.write.mode('overwrite').parquet('parquet_path')
```

**Key Points:**
- Parquet is a columnar storage format
- `mode('overwrite')` replaces existing data
- Parquet is compressed and efficient for analytics

---

## 6. Stop Session

```python
spark.stop()
```

Always stop the session to release resources.

---

## 📝 Summary

| Concept | Description |
|---------|-------------|
| SparkSession | Entry point for DataFrame operations |
| SparkContext | Entry point for RDD operations |
| RDD | Low-level distributed collection |
| DataFrame | High-level distributed table |
| Lazy Evaluation | Transformations only compute when action is called |
| Parquet | Efficient columnar storage format |

---

## 🔗 Resources

- [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
- [Spark SQL Guide](https://spark.apache.org/docs/latest/sql-programming-guide.html)
