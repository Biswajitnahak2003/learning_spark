# Day 3 - GroupBy, Aggregations & Date Functions

## 📖 Topics Covered

1. **Reading Parquet Files**
2. **Basic Aggregations (without GroupBy)**
3. **GroupBy with Aggregations**
4. **Understanding Shuffle Operations**
5. **Date and Time Functions**
6. **Stop Session**

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

## 5. Date and Time Functions

### 5.1 Current Date and Timestamp

```python
from pyspark.sql import functions as F

date = F.current_date()
spark.range(1).select(date).show()

time = F.current_timestamp()
spark.range(1).select(time).show()
```

**Key Points:**
- `F.current_date()` returns today's date (yyyy-MM-dd)
- `F.current_timestamp()` returns current timestamp with time
- Both are useful for default values and time-based filtering

### 5.2 Parsing and Formatting Dates

```python
date_1 = "2024-12-08"
F.to_date(F.lit(date_1), "yyyy-MM-dd")
F.date_format(F.lit(date_1), "yyyy-MM-dd")
```

**Key Points:**
- `F.to_date(col, format)`: Parses text string → pure date (yyyy-MM-dd)
- `F.to_timestamp(col, format)`: Parses text → full timestamp with hours, minutes, seconds
- `F.date_format(col, format)`: Reverse operation, date/timestamp → text string

### 5.3 Extracting Date Components

```python
path = r"../dataset/Orders.csv"
df_3 = spark.read.csv(path, header=True)
year = df_3.select(F.year(F.col('CreationTime')))
month = df_3.select(F.month(F.col('CreationTime')))
month.show()
```

**Key Points:**
- `F.year(col)`: Extracts 4-digit year (e.g., 2026)
- `F.quarter(col)`: Returns fiscal quarter (1 to 4)
- `F.month(col)`: Extracts month number (1 to 12)
- `F.dayofmonth(col)`: Day of month (1 to 31)
- `F.dayofweek(col)`: Day of week index (1 Sunday, 7 Saturday)
- `F.dayofyear(col)`: Day number of year (1 to 366)
- `F.weekofyear(col)`: Calendar week number (1 to 53)
- `F.hour(col)`: Hour block (0 to 23)
- `F.minute(col)`: Minute block (0 to 59)
- `F.second(col)`: Seconds block (0 to 59)

### 5.4 Date Arithmetic

```python
df_3.select(F.to_date("CreationTime"), F.date_add("CreationTime", 1).alias("next_dat")).show()
```

**Key Points:**
- `F.date_add(col, days)`: Adds specific number of days forward
- `F.date_sub(col, days)`: Subtracts specific number of days backward
- `F.add_months(col, months)`: Shifts date by full months
- `F.datediff(end_col, start_col)`: Calculates exact days between two dates
- `F.months_between(end_col, start_col)`: Calculates months between dates (returns decimal)

### 5.5 Truncating Dates

```python
day_1_of_each_month = df_3.select(F.date_trunc("month", "CreationTime"))
day_1_of_each_month.show()
```

**Key Points:**
- `F.date_trunc(format, col)`: Resets timestamp down to start of specified unit (e.g., "MM" resets to first day of month at 00:00:00)
- `F.last_day(col)`: Returns final valid calendar date of month
- `F.next_day(col, "dayOfWeek")`: Finds exact date of next specified day of week

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
| `groupBy()` | Groups rows by column(s) for aggregation |
| `agg()` | Applies aggregation functions per group |
| `count()` | Counts non-null values |
| `sum()` | Sum of values |
| `avg()` / `mean()` | Average of values |
| `min()` / `max()` | Minimum/maximum values |
| **Shuffle** | Data redistribution across partitions |
| `current_date()` | Returns today's date |
| `current_timestamp()` | Returns current timestamp |
| `to_date()` | Parses text to date |
| `date_format()` | Formats date/timestamp to text |
| `year()`, `month()`, etc. | Extract date components |
| `date_add()`, `date_sub()` | Date arithmetic |
| `date_trunc()` | Truncates timestamp to specified unit |

---

## 🔗 Resources

- [PySpark GroupBy Documentation](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.GroupedData.html)
- [Spark Aggregation Functions](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html#aggregation-functions)
- [Understanding Spark Shuffle](https://spark.apache.org/docs/latest/sql-performance-tuning.html#shuffle-tuning)
- [PySpark Date Functions](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html#date-functions)