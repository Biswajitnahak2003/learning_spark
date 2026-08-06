# Day 4 - Joins and Unions in PySpark

## 📖 Topics Covered

1. **Reading CSV Files**
2. **Joins**
   - Inner Join
   - Broadcast Join
3. **Unions**
4. **Stop Session**

---

## 1. Reading CSV Files

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder\
    .appName("pyspark_day4")\
    .getOrCreate()

path_1 = r"../dataset/Customers.csv"
path_2 = r"../dataset/Orders.csv"

df_1 = spark.read.csv(path_1, header=True)
df_2 = spark.read.csv(path_2, header=True)
print(df_1)
print(df_2)
```

**Key Points:**
- Use `spark.read.csv()` with `header=True` to read CSV files
- Verify schemas by printing DataFrames
- Ensure column names match for joins; rename if necessary

---

## 2. Joins

### 2.1 Inner Join

```python
inner_df = df_1.join(df_2, on="CustomerID", how="inner")
inner_df.show()
```

**Key Points:**
- `join()` combines two DataFrames based on a common column
- `on` specifies the join column(s)
- `how` defines join type: `"inner"`, `"left"`, `"right"`, `"full"`, `"cross"`, `"left_semi"`, `"left_anti"`
- Inner join returns only matching rows from both DataFrames
- Ensure both DataFrames have the same column name for join key; rename if needed

### 2.2 Broadcast Join

```python
from pyspark.sql.functions import broadcast

broadcast_df = df_1.join(broadcast(df_2), on="CustomerID", how="inner")
broadcast_df.show()
```

**Key Points:**
- Broadcast join is used when one DataFrame is small enough to fit in memory
- `broadcast()` hints Spark to send the small DataFrame to all executors
- Avoids expensive shuffle operations
- Useful when joining large dataset with small dataset (e.g., 500GB with 1GB)
- For right join with broadcast, swap DataFrames and use left join
- For full join, combine left join + anti-left join then union

---

## 3. Unions

```python
union_df = df_1.unionByName(df_2, allowMissingColumns=True)
union_df.show()
```

**Key Points:**
- `unionByName()` combines two DataFrames by column name (handles missing columns)
- `allowMissingColumns=True` fills missing columns with NULL
- `union()` requires same number of columns in same order
- In PySpark, `union()` does **not** remove duplicates (same as `unionAll()`)
- Use `.distinct()` to remove duplicate rows after union

---

## 4. Stop Session

```python
spark.stop()
```

Always stop the session to release resources.

---

## 📝 Summary

| Concept | Description |
|---------|-------------|
| `join()` | Combines two DataFrames based on a key |
| `how="inner"` | Returns only matching rows |
| `how="left"` | Returns all rows from left, matching from right |
| `how="right"` | Returns all rows from right, matching from left |
| `how="full"` | Returns all rows from both DataFrames |
| `broadcast()` | Sends small DataFrame to all executors for efficient joins |
| `unionByName()` | Combines DataFrames by column name (handles missing columns) |
| `union()` | Combines DataFrames by position (same column order required) |
| `.distinct()` | Removes duplicate rows |

---

## 🔗 Resources

- [PySpark Join Documentation](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.join.html)
- [PySpark Broadcast Hints](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html#broadcast)
- [PySpark Union Documentation](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.union.html)