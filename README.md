# azure-retail-analytics-data-lakehouse
End-to-end retail sales data analytics pipeline on Azure, demonstrating a modern data lakehouse architecture.
# Azure Retail Sales Data Analytics: Modernizing ETL with a Data Lakehouse Approach

## Project Overview

This project demonstrates the modernization of a retail sales data pipeline on Microsoft Azure, showcasing a shift from traditional data warehousing and ETL methods to a scalable, cost-effective, and cloud-native data lakehouse architecture. It covers end-to-end batch ingestion, multi-layered data transformations (Bronze, Silver, Gold), and analytical reporting.

## Architecture

The solution implements a Medallion Architecture pattern leveraging key Azure services. Data flows through distinct layers:

* **1. Raw Data Ingestion (Local Python):** Sales data is generated locally using a Python script.
* **2. Azure Data Lake Storage Gen2 (Bronze Layer):** The raw sales data (CSV) is ingested into the Bronze layer of ADLS Gen2. This layer acts as a immutable landing zone.
* **3. Azure Data Factory - Data Flow (Bronze to Silver Transformation):** An Azure Data Factory pipeline utilizes a Mapping Data Flow to read data from the Bronze layer. It performs data cleaning, validation, and schema enforcement.
* **4. Azure Data Lake Storage Gen2 (Silver Layer):** The transformed and conformed data is stored in the Silver layer of ADLS Gen2 in Parquet format. This layer is ready for further aggregation and feature engineering.
* **5. Azure Data Factory - Data Flow (Silver to Gold Aggregation):** Another Data Flow reads from the Silver layer, performs aggregations (e.g., daily sales, unique customers), and prepares the data for business consumption.
* **6. Azure Data Lake Storage Gen2 (Gold Layer):** The aggregated and highly optimized data is stored in the Gold layer of ADLS Gen2 in Parquet format.
* **7. Azure Synapse Analytics - Serverless SQL Pool (Views):** A Serverless SQL Pool in Azure Synapse Analytics provides a SQL interface over the Gold layer Parquet files, allowing for ad-hoc querying and creating logical views (like `gold_daily_sales_summary`) without provisioning dedicated compute resources.
* **8. Power BI Desktop & Service (Dashboard):** Power BI connects to the Synapse Serverless SQL Pool view using DirectQuery mode, enabling interactive dashboards and reports directly on the business-ready data in the data lakehouse.
