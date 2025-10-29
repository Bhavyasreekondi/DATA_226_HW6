# Homework 6 – Weekly Active User (WAU) Pipeline

## 📘 Overview
This project demonstrates a complete **data pipeline** built using **Apache Airflow**, **Snowflake**, and **Preset (Superset)** for visualizing **Weekly Active Users (WAU)**.  
It covers the full workflow — from **data extraction (ETL)** to **data transformation (ELT)** to **business intelligence (BI) visualization**.

---

## 🎯 Objective
The goal of this assignment was to:
1. Extract session data from Snowflake into Airflow.
2. Transform and join datasets using SQL logic to build an analytical table.
3. Visualize Weekly Active Users (WAU) using Preset.
4. Automate the process using Airflow DAGs.

---

## 🧱 Project Structure


---

## ⚙️ Step-by-Step Implementation

### 1️⃣ ETL DAG – Import Data from Snowflake
- File: `session_to_snowflake.py`
- Purpose: Extract data from two Snowflake tables:
  - `user_session_channel`
  - `session_timestamp`
- Tasks are handled by `PythonOperator`.
- DAG scheduled to run daily.
- Output: Raw data staged in Airflow for transformation.

### 2️⃣ ELT DAG – Create Joined Summary Table
- File: `build_summary.py`
- Purpose: Join `user_session_channel` and `session_timestamp` into a summary table `analytics.session_summary`.
- Logic includes:
  - Removing duplicate sessions using `ROW_NUMBER()`.
  - Aggregating sessions per user.
- Output: Clean, analytical dataset ready for visualization.

### 3️⃣ Preset Setup
- Connected Preset to the Snowflake database.
- Imported the `session_summary` table as a dataset.
- Verified schema and connectivity in Preset.

### 4️⃣ WAU Chart Creation
- Dataset: `analytics.session_summary`
- Metric: Weekly Active Users (WAU)
- Time Aggregation: `WEEK(session_timestamp)`
- Visualization Type: Line Chart
- Purpose: Display weekly user engagement trends.

---

## 🧩 Extra Credit
An additional SQL condition was added to remove duplicate session records:
```sql
ROW_NUMBER() OVER (PARTITION BY user_id, session_id ORDER BY session_timestamp DESC) = 1
