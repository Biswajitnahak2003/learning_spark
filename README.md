# PySpark Learning Journey

A hands-on learning repository for Apache PySpark, covering concepts from basics to advanced topics.

## 📚 Structure

| Day | Topic | Description |
|-----|-------|-------------|
| [Day 1](./day1/) | Spark Basics | SparkSession, RDDs, DataFrames, CSV & Parquet |

## 🛠️ Setup

```bash
# Create virtual environment
python -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install pyspark jupyter
```

## 🚀 How to Run

1. Activate the virtual environment
2. Launch Jupyter: `jupyter notebook`
3. Navigate to the respective day folder
4. Run the notebook cells sequentially

## 📁 Repository Structure

```
pyspark-learning/
├── README.md
├── day1/
│   ├── README.md
│   └── spark_day1.ipynb
├── dataset/
│   └── Customers.csv
├── parquet_path/
├── pyproject.toml
└── .python-version
```

## 📝 Topics Covered

### Day 1 - Spark Basics
- Creating SparkSession & SparkContext
- RDD operations (parallelize, map, collect)
- Reading CSV files (multiple methods)
- DataFrame operations (show, take, printSchema)
- Column renaming with `withColumnRenamed`
- Writing data to Parquet format

## 📄 License

This project is for learning purposes.
