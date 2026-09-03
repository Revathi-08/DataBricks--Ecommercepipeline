markdown# 🛒 DataBricks E-Commerce Medallion Pipeline

End-to-End E-commerce Data Engineering project implemented in **Databricks Lakehouse** using **Medallion Architecture (Bronze → Silver → Gold)**.

> Built by Revathi | Databricks + PySpark + Delta Lake

### 🏗️ ArchitectureRaw E-commerce Source
      ↓
[ 1_setup ] - Database & Schema Creation
      ↓
[ Bronze Layer ] - Raw Ingestion (dim & fact)
      ↓
[ Silver Layer ] - Cleaning, Deduplication, Data Quality
      ↓
[ Gold Layer ] - Star Schema, Business Aggregates
      ↓
Analytics Ready Tablesjavascript
### 📁 Project Structureproject_ecommerce/
├── 1_setup/ - DB & Volume setup
├── 1_medallion_processing_dim/
│   ├── 1_dim_bronze.ipynb
│   ├── 1_dim_silver.ipynb
│   └── 1_dim_gold.ipynb
├── 3_medallion_proceessing_fact/
│   ├── 1_fact_bronze.ipynb
│   ├── 2_fact_silver.ipynb
│   └── 3_fact_gold.ipynbjavascript
### ⚙️ Tech Stack
- **Platform:** Databricks
- **Language:** PySpark, Spark SQL
- **Storage:** Delta Lake, Unity Catalog
- **Design Pattern:** Medallion Architecture
- **Version Control:** Databricks Repos + GitHub

### 🔄 What Each Layer Does

**1. Bronze:** Ingests raw CSV/JSON as-is into Delta tables with `Auto Loader` concept.
**2. Silver:** Removes nulls/duplicates, standardizes columns, applies data quality checks.
**3. Gold:** Creates Star Schema (Dim_Customers, Dim_Products, Fact_Sales) for BI reporting. Optimized with `OPTIMIZE`.

### 🚀 How To Run
1. Import this repo into Databricks: Repos > Add Repo > `https://github.com/Revathi-08/DataBricks--Ecommercepipeline`
2. Run notebooks in order: `1_setup` -> `1_dim_bronze` -> `1_fact_bronze` -> Silver -> Gold
3. Check Gold tables in Catalog.

### 📊 Key Learnings
- Implemented Medallion architecture for both Dimensions and Facts
- Handled SCD Type 1 for Dimensions
- Used Delta Lake features - Schema Evolution, Time Travel

### 👩‍💻 Author
**Revathi** - Aspiring Data Engineer

