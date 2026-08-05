# Day 2 - Slicing, Dicing & Mutating in PySpark

## 📖 Topics Covered

1. **Reading Parquet Files**
2. **Slicing (Selecting Columns)**
3. **Dicing (Filtering Rows)**
4. **Mutating (Adding/Transforming Columns)**
5. **Practical Exercise: Parsing Log Files**
6. **Parsing Log Files with Regexp**

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

## 2. Slicing (Selecting Columns)

### Method 1: Direct Column Selection
```python
df_1 = df.select('ID', 'FirstName', 'Country')
```

### Method 2: Using col() with Alias
```python
from pyspark.sql.functions import col

df_2 = df.select(
    col('ID').alias('CustomerID'),
    col('FirstName'),
    col('Country')
)
```

**Key Points:**
- `select()` picks specific columns (like SQL SELECT)
- `col()` gives column reference for transformations
- `alias()` renames columns on the fly

---

## 3. Dicing (Filtering Rows)

```python
df_3 = df.filter(
    (col('Country') == 'USA') & (col('Score') > 500)
)
df_3.show()
```

**Key Points:**
- `filter()` keeps rows matching the condition
- Use `&` for AND, `|` for OR, `~` for NOT
- Always wrap conditions in parentheses due to operator precedence

---

## 4. Mutating (Adding/Transforming Columns)

```python
from pyspark.sql.functions import when

df_4 = df.withColumn(
    "Performance",
    when(col("Score") >= 800, "Excellent")
    .when(col("Score") >= 500, "Average")
    .otherwise("Needs Improvement")
)
df_4.show()
```

**Key Points:**
- `withColumn()` adds a new column or replaces existing one
- `when()` works like if-else (similar to CASE WHEN in SQL)
- Chain multiple `.when()` for multiple conditions
- `.otherwise()` is the default/fallback case

---

## 5. Practical Exercise: Parsing Log Files

### Problem Statement
Parse a raw text log file into a clean DataFrame with 4 columns:
- `timestamp` (Proper Timestamp type)
- `log_level` (Only "ERROR", "INFO", or "WARN")
- `user_id` (Integer type, replacing null strings with 0)
- `message` (The final text sentence)

### Solution
```python
from pyspark.sql.functions import split, concat, lit

txt = spark.read.text(txt_path)
txt = txt.withColumn('timestamp', concat(split('value', ' ')[0], lit(' '), split('value', ' ')[1]))\
        .withColumn('log_level', split('value', ' ')[2])\
        .withColumn('user_id', split('value', ' ')[3])\
        .withColumn('message', concat(split('value', ' ')[5], lit(' '), split('value', ' ')[6], lit(' '), split('value', ' ')[7], lit(' '), split('value', ' ')[8]))
txt = txt.select('timestamp', 'log_level', 'user_id', 'message')
```

**Key Points:**
- `split()` breaks a string by delimiter into an array
- `concat()` joins multiple strings/columns together
- `lit()` adds a literal string value
- Array indices start from 0

---

## 6. Parsing Log Files with Regexp

### Alternative Approach
Instead of `split()`/`concat()`, we can use regular expressions with `regexp_extract()` and `regexp_replace()` for a cleaner, more robust solution.

### Solution
```python
from pyspark.sql import functions as F

pattern = r"(\w.{19})"                     ## timestamp
pattern_2 = r"\[(\w+)\]"                   ## log_level
pattern_3 = r"User_ID:(\d{4}|null)"        ## user_id
pattern_4 = r"-\s(.*)$"                    ## message

txt = txt.withColumn('timestamp', F.regexp_extract('value', pattern, 1))\
        .withColumn('log_level', F.regexp_extract('value', pattern_2, 1))\
        .withColumn('user_id', F.regexp_extract('value', pattern_3, 1))\
        .withColumn('message', F.regexp_extract('value', pattern_4, 1))
txt = txt.withColumn('message', F.regexp_replace(F.col('message'), r'"$', ''))
txt.select('timestamp', 'log_level', 'user_id', 'message').show(truncate=False)
```

**Key Points:**
- `regexp_extract(str, pattern, idx)` extracts a capture group from a regex pattern
- `regexp_replace(str, pattern, replacement)` replaces regex matches
- Groups are captured with `()` and referenced by 1-based index
- More robust than split/concat for complex log formats

---

## 7. Stop Session

```python
spark.stop()
```

Always stop the session to release resources.

---

## 📝 Summary

| Concept | Description |
|---------|-------------|
| select() | Pick specific columns (Slice) |
| col().alias() | Reference and rename columns |
| filter() | Keep rows matching condition (Dice) |
| withColumn() | Add or transform columns (Mutate) |
| when().otherwise() | Conditional logic (like CASE WHEN) |
| split() | Break string into array |
| concat() | Join strings together |
| regexp_extract() | Extract capture group using regex |
| regexp_replace() | Replace regex matches |

---

## 🔗 Resources

- [PySpark DataFrame Operations](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.html)
- [PySpark Functions](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html)
