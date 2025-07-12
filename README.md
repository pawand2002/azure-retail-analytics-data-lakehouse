🛍️ Retail Sales Data Engineering Project – Azure 
Overview
This project simulates a retail sales pipeline leveraging synthetic data, cloud-native orchestration, and dimensional modeling principles (Kimball style). It's designed to demonstrate scalable ETL, governance, and analytic workflows using Azure Data Factory, Azure Synapse analytics, and visualization tools like Power BI.

📦 Dataset Details
- sales_data.csv: Synthetic dataset with 1000+ transactions
- Fields include: TransactionID, CustomerID, ProductID, Quantity, Price, TransactionDate, Region
- Covers a 6-month window with randomized records across multiple dimensions

💡 Key Features
- Python script to generate realistic sales data for pipeline testing
- Modular ETL design adaptable across Azure and GCP
- Dimensional modeling aligned with Kimball standards (SCDs, fact tables, bridge tables)
- Power BI dashboards with time-series logic and regional segmentation
- Parquet integration via ADLS Gen2 and Synapse SQL views

🧠 Learning Outcomes
- Design reusable ETL pipelines across cloud platforms
- Apply Kimball modeling in real-world retail contexts
- Automate staging zones: Bronze → Silver → Gold
- Integrate Parquet and SQL Serverless for Power BI consumption
- Experiment with Gen AI (LangChain, Vertex AI) for insights and reporting

📐 Architecture
[Python Data Generator] → [ADLS Gen2 / GCS]
      ↓                          ↓
[ADF / Dataflow] → [Synapse / BigQuery] → [Power BI / Looker]



🛠️ Stack
| Component | Tech | 
| Data Generation | Python (Pandas, Random, Datetime) | 
| Storage | ADLS Gen2, Parquet | 
| Orchestration | Azure Data Factory, Cloud Composer | 
| Modeling | Synapse Serverless SQL, BigQuery | 
| Visualization | Power BI | 
| Gen AI (Future) | Vertex AI, LangChain | 



📁 File Structure
├── generate_sales_data.py
├── sales_data.csv
├── notebooks/
│   └── exploratory_analysis.ipynb
├── sql/
│   └── create_gold_views.sql
├── powerbi/
│   └── dashboard.pbix
├── README.md



🚀 To Run Locally
python generate_sales_data.py


Make sure the path in the script matches your local directory. Dataset will be saved to /Downloads/Data Engineering Projects/SalesData_Azure/.
