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

This activity retrieves the most recent processed pickup date for the table `staging_nyctaxi_yellow`.

```sql
SELECT TOP 1 
latest_processed_pickup 
FROM metadata.processing_log 
WHERE table_processed = 'staging_nyctaxi_yellow'
ORDER BY latest_processed_pickup DESC;
```

**v_date**
Pipeline expression for v_date Set Variable activity
```
@formatDateTime(addToTime(activity('Latest Processed Date').output.resultSets[0].rows[0].latest_processed_pickup, 1, 'Month'), 'yyyy-MM')
```

**v_end_date**
Pipeline expression for v_end_date Set Variable activity
```
@addToTime(concat(variables('v_date'), '-01'),1,'Month')
```

**SP Removing Outlier Dates**
For the Stored Procedure Activity “SP Removing Outlier Dates”.
Create the Stored Procedure stg.data_cleaning_stg in the Data Warehouse using the code below.
```
create procedure stg.data_cleaning_stg
@end_date datetime2,
@start_date datetime2
as
delete from stg.nyctaxi_yellow where tpep_pickup_datetime < @start_date or tpep_pickup_datetime > @end_date;
```
## Pipeline: pl_pres_processing_nyctaxi
![pl_pres_processing_nyctaxi](https://github.com/user-attachments/assets/49652cef-8bf3-4d5a-b2c7-4129e02786ce)

## data Flow
![Data Flow](https://github.com/user-attachments/assets/d4fe70ef-ba32-4541-9cc5-7c6ccad360d9)

## pl_orchestrate_nyctaxi
![pl_orchestrate_nyctaxi](https://github.com/user-attachments/assets/d250a88a-ece9-41ac-8955-29a7cfb9128c)
