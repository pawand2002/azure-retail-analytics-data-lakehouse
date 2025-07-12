# 🛍️ Retail Sales Analytics on Azure

An end-to-end Retail Sales Analytics project built on Azure, showcasing modern data engineering practices using Python, Azure Data Factory (ADF), Azure Data Lake Storage Gen2, Azure Synapse Analytics, and Power BI. 

This project demonstrates how raw retail sales data can be ingested, transformed, analyzed, and visualized using a scalable cloud-based architecture.

---

## 🚀 Project Architecture

```mermaid
graph TD
    A[Python Script] --> B[Azure Data Factory]
    B --> C[Azure Data Lake - Bronze/Silver/Gold]
    C --> D[Azure Synapse Analytics]
    D --> E[Power BI Reports]

🧱 Tech Stack
Component	Technology
Language	Python
Orchestration	Azure Data Factory
Storage	Azure Data Lake Gen2
Data Processing	Azure Synapse Analytics
Visualization	Power BI

📂 Project Structure
bash
retail-sales_analytics/
│
├── data/                       # Raw CSV files (input datasets)
├── python_ingestion/          # Python scripts for initial data push
├── adf_pipelines/             # JSON exports of ADF pipelines
├── synapse_queries/           # SQL scripts for DDL + transformations
├── powerbi_reports/           # PBIX files / screenshots
├── README.md
└── architecture.png           # Architecture Diagram
