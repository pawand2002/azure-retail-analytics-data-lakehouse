# azure-retail-analytics-data-lakehouse
End-to-end retail sales data analytics pipeline on Azure, demonstrating a modern data lakehouse architecture.
# Azure Retail Sales Data Analytics: Modernizing ETL with a Data Lakehouse Approach

## Project Overview

This project demonstrates the modernization of a retail sales data pipeline on Microsoft Azure, showcasing a shift from traditional data warehousing and ETL methods to a scalable, cost-effective, and cloud-native data lakehouse architecture. It covers end-to-end batch ingestion, multi-layered data transformations (Bronze, Silver, Gold), and analytical reporting.

## Architecture

The solution implements a Medallion Architecture pattern leveraging key Azure services:

```mermaid
graph TD
    A[Raw Sales Data (Local Python)] --> B(Azure Data Lake Storage Gen2 - Bronze Layer)
    B --> C{Azure Data Factory - Data Flow (Bronze to Silver Transformation)}
    C --> D(Azure Data Lake Storage Gen2 - Silver Layer)
    D --> E{Azure Data Factory - Data Flow (Silver to Gold Aggregation)}
    E --> F(Azure Data Lake Storage Gen2 - Gold Layer)
    F --> G[Azure Synapse Analytics - Serverless SQL Pool (Views)]
    G --> H[Power BI Desktop & Service (Dashboard)]
```
