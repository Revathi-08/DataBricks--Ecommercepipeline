# 🛒 DataBricks E-Commerce Medallion Pipeline

End-to-End E-commerce Data Engineering project implemented in Databricks Lakehouse using Medallion Architecture (Bronze → Silver → Gold).

Built by Revathi-08 | Databricks + PySpark + Delta Lake

### 🏗️ Architecture

Raw Source -> 1_setup -> Bronze Layer -> Silver Layer -> Gold Layer -> Analytics Ready Tables

### 📁 Project Structure
- 1_setup/ - DB & Volume setup
- 1_medallion_processing_dim/ - Dim Bronze, Silver, Gold
    - 1_dim_bronze.ipynb
    - 1_dim_silver.ipynb
    - 1_dim_gold.ipynb
- 3_medallion_proceessing_fact/ - Fact Bronze, Silver, Gold
    - 1_fact_bronze.ipynb
    - 2_fact_silver.ipynb
    - 3_fact_gold.ipynb

### ⚙️ Tech Stack
- Platform: Databricks
- Language: PySpark, Spark SQL
- Storage: Delta Lake
- Design: Medallion Architecture

### 🔄 What Each Layer Does
Bronze: Raw ingestion as-is into Delta
Silver: Cleaning, deduplication, quality checks
Gold: Star Schema - Fact & Dimensions for BI

### 🚀 How To Run
1. Import repo into Databricks
2. Run notebooks in order: setup -> bronze -> silver -> gold
3. Check Gold tables in Catalog

### Author
Revathi - Aspiring Data Engineer
