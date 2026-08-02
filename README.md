# PySpark Learning Journey

A hands-on learning repository for Apache PySpark, covering concepts from basics to advanced topics.

## 📚 Structure

| Day | Topic | Description |
|-----|-------|-------------|
| [Day 1](./day1/) | Spark Basics | SparkSession, RDDs, DataFrames, CSV & Parquet |

## 🛠️ Setup

```bash
# Create virtual environment and install dependencies
uv sync

# Or install manually
uv pip install pyspark jupyter
```

## 🚀 How to Run

```bash
# Activate virtual environment
source .venv/bin/activate

# Launch Jupyter
jupyter notebook
```

1. Navigate to the respective day folder
2. Run the notebook cells sequentially

## 📁 Repository Structure

```
learning_spark/
├── README.md
├── LICENSE
├── day1/
│   ├── README.md
│   └── spark_day1.ipynb
├── dataset/
│   └── Customers.csv
├── pyproject.toml
├── uv.lock
└── .python-version
```

## 📝 Summary

This repository tracks my PySpark learning journey, covering:

- **Day 1**: Spark basics - SparkSession, RDDs, DataFrames, reading CSV, writing Parquet

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
