# PySpark Learning Journey

A hands-on learning repository for Apache PySpark, covering concepts from basics to advanced topics.

## 📚 Structure

| Day | Topic | Description |
|-----|-------|-------------|
| [Day 1](./day1/) | Spark Basics | SparkSession, RDDs, DataFrames, CSV & Parquet |

## 🛠️ Setup

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

## 🚀 How to Run

1. Navigate to the respective day folder
2. Select the kernel: `.venv` (Python 3.11)
3. Run the notebook cells sequentially

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

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
