# 🛒 DataBricks E-Commerce Medallion Pipeline

End-to-End E-commerce Data Engineering project implemented in **Databricks Lakehouse** using **Medallion Architecture (Bronze → Silver → Gold)** for real-time e-commerce analytics.

> Built by **Revathi-08** | Databricks + PySpark + Delta Lake | Unity Catalog

### 🏗️ Architecture

**Flow:** `External Volume (Raw CSV) → 1_setup → Bronze (Raw Delta) → Silver (Cleaned) → Gold (Star Schema) → Analytics`

### 📁 Project Structure & What Each File Does

#### 1. `1_setup/` - Foundation Layer
This is the starting point. Creates Database, Schema and Volumes in Unity Catalog if not exists.
- Creates `ecommerce_db` database
- Sets up 3 schemas: `bronze`, `silver`, `gold`
- Handles path creation for landing raw files

#### 2. `1_medallion_processing_dim/` - Dimension Processing

**a) `1_dim_bronze.ipynb` - Raw Ingestion for Dimensions**
- Reads raw customer/product CSVs from `/Volumes`
- Ingests AS-IS without any transformation
- Writes to Delta Table `bronze.dim_customers`, `bronze.dim_products` with `overwrite` + `mergeSchema = true`
- Keeps ingestion timestamp: `_ingest_timestamp`
- Purpose: Single source of truth, handles schema drift

**b) `1_dim_silver.ipynb` - Cleaning & Deduplication for Dimensions**
- Reads from Bronze Delta tables
- **Cleaning Steps:**
    - Null handling: Drops rows where customer_id/product_id is null
    - Trim & Lower for email, standardizes country codes
    - Deduplication: `dropDuplicates(["customer_id"])` - keeps latest record based on timestamp
    - Data type casting: String to Date for `dob`
- Writes to `silver.dim_customers`, `silver.dim_products` with SCD Type 1 logic
- Adds data quality flag: `is_valid`

**c) `1_dim_gold.ipynb` - Curated Dimension for BI**
- Reads from Silver
- Creates final Dimension tables optimized for reporting
- Applies `SELECT DISTINCT` and business rules
- Optimized with `OPTIMIZE` and `ZORDER BY customer_id`
- Output: `gold.dim_customer_final`, `gold.dim_product_final` - Used directly in Power BI / Databricks SQL

#### 3. `3_medallion_proceessing_fact/` - Fact Processing (Transactional Data)

**a) `1_fact_bronze.ipynb` - Raw Ingestion for Facts**
- Ingests raw orders/sales transactions CSV
- High volume, append-only ingestion
- Writes to `bronze.fact_orders_raw` as Delta
- No joins, no filters - just landing

**b) `2_fact_silver.ipynb` - Fact Cleaning & Validation**
- Reads from `bronze.fact_orders_raw` + joins with Silver Dims for validation
- **Validations:**
    - Removes orders with invalid `customer_id` (not in dim)
    - Calculates `total_amount = quantity * unit_price`
    - Handles negative quantity, filters cancelled orders
    - Standardizes order_status
- Writes to `silver.fact_orders_cleaned`

**c) `3_fact_gold.ipynb` - Business Aggregates & Star Schema**
- Final Fact table creation: `gold.fact_sales`
- **Aggregations:**
    - Daily Sales, Monthly Revenue
    - Top 10 Products by Revenue
    - Customer Lifetime Value
- Creates Star Schema: Fact table linked to `dim_customer_final` and `dim_product_final`
- Partitioned by `order_date` for faster queries
- Ready for `Databricks SQL Dashboard`

### ⚙️ Tech Stack
- **Compute:** Databricks Runtime 14.3 LTS, PySpark
- **Storage:** Delta Lake (ACID, Time Travel), Unity Catalog Volumes
- **Language:** PySpark, Spark SQL
- **Pattern:** Medallion Architecture + Star Schema
- **Ops:** OPTIMIZE, VACUUM, Z-Ordering, Databricks Repos + GitHub

### 🚀 How To Run
1. Clone repo: Databricks > Repos > Add Repo > `https://github.com/Revathi-08/DataBricks--Ecommercepipeline`
2. Attach cluster and run in order: `1_setup` → `dim_bronze` → `fact_bronze` → `dim_silver` → `fact_silver` → `dim_gold` → `fact_gold`
3. Query Gold layer: `SELECT * FROM gold.fact_sales`

### 👩‍💻 Author
**Revathi** | Aspiring Data Engineer
