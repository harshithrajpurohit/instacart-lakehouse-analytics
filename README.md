# 🛒 Instacart Lakehouse Analytics - Databricks

A Medallion Architecture (Bronze → Silver → Gold) data pipeline built with **PySpark** and **Delta Lake**, analyzing the [Instacart Market Basket](https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis/data?utm_source=chatgpt.com) dataset to uncover purchasing patterns, top products, and department-level reorder behavior.

## 📐 Architecture

```
Raw CSVs → Bronze (raw ingest) → Silver (cleaned & joined) → Gold (business aggregates)
```

- **🥉 Bronze** — Raw CSVs (`orders`, `order_products__prior`, `order_products__train`, `products`, `aisles`, `departments`) loaded as-is into Delta tables, preserving source fidelity.
- **🥈 Silver** — Tables joined into a single denormalized fact table, type casting, duplicate/null checks, and a `quality_flag` column for data trust classification.
- **🥇 Gold** — Business-ready aggregates: department-wise sales, top 20 products, top aisles, and reorder rate by department.

## 🔑 Key Findings

- **Produce dominates volume** — over 9.47M products sold, more than 1.7x the next closest department (dairy & eggs at 5.41M).
- **Organic wins** — 5 of the top 6 most-ordered products are explicitly organic (bananas, strawberries, spinach, avocados).
- **Fresh aisles lead traffic** — fresh fruits and fresh vegetables aisles each pull 3.4M+ orders.
- **Dairy & eggs has the highest loyalty** — ~67% reorder rate, followed by beverages (~65.3%) and produce (~65%).

## 🛠️ Tech Stack

| Component | Tool |
|---|---|
| Processing engine | Apache Spark (PySpark) |
| Storage format | Delta Lake |
| Environment | Databricks |
| Language | Python |

## 📁 Repository Structure

```
instacart-lakehouse-analytics/
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
├── instacart_lakehouse_analytics.ipynb

```

## 📊 Dataset

This project uses the [Instacart Market Basket Analysis](https://www.kaggle.com/c/instacart-market-basket-analysis) dataset (~3M orders, 200K users). Due to size and licensing, raw data is **not included** in this repo. To reproduce:

1. Download the dataset from Kaggle.
2. Upload the CSVs to your Spark environment (e.g. a Databricks Volume or DBFS path).
3. Update the `base_path` variable in the notebook to point to your data location.

## 🚀 How to Run

1. Import `instacart_lakehouse_analytics.ipynb` into a Databricks workspace (or any Spark environment with Delta Lake support).
2. Attach the notebook to a cluster with Spark 3.x+ and Delta Lake enabled.
3. Set `base_path` to your CSV directory.
4. Run all cells sequentially — Bronze tables are created first, then Silver, then Gold aggregates.

## 📈 Sample Outputs

| Analysis | Output |
|---|---|
| Department sales | Bar chart / table of total products sold per department |
| Top 20 products | Ranked list of most frequently ordered items |
| Top aisles | Ranked list of aisles by order volume |
| Reorder rate | Average reorder rate by department |

## 🔮 Future Improvements

- Add a `gold` layer table for customer-level purchase frequency segmentation
- Automate the pipeline with Databricks Workflows / Airflow
- Add unit tests for data quality checks
- Visualize results with a BI dashboard (Power BI / Tableau / Databricks SQL)

## 📄 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
