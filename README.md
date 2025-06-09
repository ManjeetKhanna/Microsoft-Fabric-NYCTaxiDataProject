# 🗽 Microsoft Fabric NYC Taxi Data Project

Welcome to this comprehensive Microsoft Fabric data project, which demonstrates an **end-to-end data warehousing pipeline** using **New York City TLC Taxi data**. This project integrates **Data Lakehouse**, **Data Factory**, **Data Warehouse**, and **Power BI** to showcase a modern, scalable analytics solution.

---

## 📌 Project Overview

This project processes and analyzes **NYC Yellow Taxi trip records** by building a modern data architecture using Microsoft Fabric. The solution demonstrates an end-to-end pipeline, moving data from raw files to insightful dashboards through layered transformations and modeling.

We leverage the following components:

- **Data Lakehouse** – to store both raw and curated data  
- **Data Warehouse** – to host structured, query-optimized data for reporting and analytics  
- **Data Factory Pipelines** – for orchestrating ETL workflows across layers  
- **Dataflows and Stored Procedures** – for transforming data between staging and presentation layers  
- **Power BI** – for semantic modeling and building interactive reports

---

## 📂 Data Source

We use publicly available **TLC Trip Record Data (Yellow Taxi)** from the NYC Taxi & Limousine Commission.  
🔗 [NYC TLC Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

---

## 🔁 Project Workflow

The project follows a structured workflow across the data lifecycle:

### 1️⃣ Raw Data Landing  
- NYC taxi CSV files are stored in a **Data Lakehouse** in the `landing` zone

### 2️⃣ Staging Layer  
- A **Data Factory Pipeline** moves and formats raw data to the `staging` zone  
- Transformations are applied using **Notebooks** and **Dataflows**

### 3️⃣ Presentation Layer  
- Cleaned data is moved to the `presentation` zone  
- Further transformations are handled by **Dataflows** or **Stored Procedures**

### 4️⃣ Power BI Modeling  
- A **semantic model** is created on top of the presentation data  
- **Power BI** reports and dashboards are built to derive insights

---

## 📦 Project Components

| Name                         | Type                  | Description                                                              |
|------------------------------|-----------------------|---------------------------------------------------------------------------|
| `NYC Taxi Data Pipelines`    | Folder                | Contains orchestration pipelines for data movement (Landing ➝ Staging)   |
| `df_pres_processing_nyctaxi` | Dataflow Gen2 (CI/CD) | Transforms data from staging to presentation layer                        |
| `ProjectLakehouse`           | Lakehouse             | Stores raw, staging, and curated data                                     |
| ├── Semantic model (default) | Power BI Model        | Used for reporting on curated presentation data                           |
| └── SQL analytics endpoint   | SQL Endpoint          | Enables SQL-based access to Lakehouse tables                              |
| `ProjectWarehouse`           | Warehouse             | Optional or additional structured storage                                 |
| └── Semantic model (default) | Power BI Model        | Alternate model for querying warehouse tables                             |

![Project Structure](https://github.com/user-attachments/assets/718e18ee-3fe8-4a1a-8a98-e2bb88e75893)

---

## ✅ Prerequisites

Before starting the project, ensure you have:

- Access to a **Microsoft Fabric** environment  
- Basic knowledge of **SQL**, **ETL**, **data warehousing**, and **Power BI**  
- Familiarity with concepts like **Lakehouse**, **Warehousing**, and **Semantic Models**

## Pipeline: pl_stg_processing_nyctaxi

Overall Pipeline:
![pl_stg_processing_pipeline](https://github.com/user-attachments/assets/2287cf80-e992-4a9e-a458-e78ee8eb9933)

### 📌 Latest Processed Data

**Script Activity**: `Latest Processed Data`  
This activity retrieves the most recent processed pickup date for the table `staging_nyctaxi_yellow`.

```sql
SELECT TOP 1 
latest_processed_pickup 
FROM metadata.processing_log 
WHERE table_processed = 'staging_nyctaxi_yellow'
ORDER BY latest_processed_pickup DESC;

