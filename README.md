🌩️ Retail Sales Analytics Pipeline – Powered by Azure
📌 Project Summary
This repository showcases a fully cloud-native retail sales analytics pipeline built entirely on Microsoft Azure. It leverages synthetic sales data generated with Python and flows through a multi-zone architecture—Bronze → Silver → Gold—using Azure Data Factory, Azure Synapse Serverless SQL, and visualized through Power BI dashboards.

🧰 Tech Stack
| Layer | Technology | 
| Data Source | Synthetic data via Python | 
| Storage | Azure Data Lake Storage Gen2 (ADLS) | 
| Orchestration | Azure Data Factory (ADF) | 
| Modeling | Azure Synapse Analytics | 
| Visualization | Power BI | 



🗂️ Dataset Overview
- Generator Script: generate_sales_data.py
- Format: CSV + optional Parquet
- Fields: TransactionID, CustomerID, ProductID, Quantity, Price, TransactionDate, Region
- Scope: ~6 months of synthetic retail activity across regions and products

🎯 Core Objectives
- Build reusable ETL flows using ADF pipelines
- Ingest and transform zone-based data in ADLS Gen2
- Apply Kimball-style dimensional modeling via Synapse Serverless SQL
- Create dynamic Power BI dashboards for sales insights
- Explore parameterized views, SCD types, and semantic layers

📐 Architecture Diagram
[Python Generator]
       ↓
[ADLS Gen2 — Bronze Zone]
       ↓
[ADF — Data Movement & Cleansing]
       ↓
[ADLS Gen2 — Silver Zone → Synapse Views]
       ↓
[Power BI Dashboard — DirectQuery / Import]



📁 Repository Structure
├── generate_sales_data.py         # Python script to create synthetic sales data
├── sales_data.csv                 # Output dataset (can be saved as Parquet)
├── sql/                           # Synapse SQL scripts for gold views
├── powerbi/                       # PBIX files and visuals
└── README.md                      # Project documentation




